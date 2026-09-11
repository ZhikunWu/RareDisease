# 现在有一个川崎病GWAS的结果，如何进行PRS分析


可以。**如果你现在已经有一份“川崎病（KD）GWAS summary statistics”，下一步完全可以基于这份 GWAS 构建 KD-PRS，然后在你自己的 WGS/基因型队列中计算每个人的 PRS。**

如果你把 **KD GWAS 结果文件上传**，我可以进一步帮你检查列名、效应等位基因、build，并直接给你写成可运行的命令。

---

# 一、先明确你的数据结构

PRS 至少需要两套数据：

### ① Base GWAS：川崎病 GWAS summary statistics

例如：

```text
CHR    BP        SNP       A1    A2    BETA       SE       P
1      12345     rs123     A     G     0.125      0.03     2e-8
1      23456     rs456     T     C    -0.082      0.02     3e-5
...
```

这里：

* `A1` = effect allele
* `A2` = non-effect allele
* `BETA` = effect size
* `P` = GWAS P value

**如果原始 GWAS 给的是 OR，而不是 BETA：**

$$
\beta=\ln(OR)
$$

PRSice 等工具可以根据相应格式处理，但我更推荐先明确转换成 BETA。

---

### ② Target genotype：你自己的 WGS

例如你现在 KD 队列有：

```text
200 KD patients
+
controls
```

如果你有 PLINK：

```text
target.bed
target.bim
target.fam
```

或者 PLINK2：

```text
target.pgen
target.pvar
target.psam
```

就可以计算 PRS。

---

# 二、PRS的核心原理

最基本的 PRS：

$$
PRS_i=\sum_j \beta_jG_{ij}
$$

也就是：

> 每个人每个 SNP 的 effect allele dosage × GWAS effect size，然后所有 SNP 加起来。

例如：

```text
rs1   dosage=2   beta=0.10
rs2   dosage=1   beta=0.05
rs3   dosage=0   beta=-0.20
```

那么：

$$
PRS=2\times0.10+1\times0.05+0\times(-0.20)
=0.25
$$

所以最终每个人得到一个：

```text
Sample    KD_PRS
ID001     0.82
ID002    -0.13
ID003     1.47
...
```

---

# 三、最推荐你使用 PRSice-2

对于你现在这个项目，我建议首先使用 **PRSice-2**。

它的标准流程就是：

```text
KD GWAS summary statistics
             ↓
        QC / harmonization
             ↓
     SNP clumping + P threshold
             ↓
       KD PRS construction
             ↓
      每个样本一个 PRS
             ↓
   KD/CAL phenotype association
```

PRSice-2 的输入就是 **base GWAS summary statistics + target genotype**，并可以自动进行 clumping 和不同 P-value threshold 的 PRS 构建。([PRSice-2](https://choishingwan.github.io/PRSice/quick_start/)) 

---

# 四、第一步：整理KD GWAS结果

建议最终整理成：

```text
SNP    A1    A2    BETA    P
rs123  A     G     0.125   2e-8
rs456  T     C    -0.082   3e-5
rs789  C     T     0.041   0.002
```

如果你的 GWAS 是：

```text
SNP
EA
NEA
OR
SE
P
```

那么：

```bash
awk 'BEGIN{OFS="\t"} NR==1{print "SNP","A1","A2","BETA","P";next} \
{print $1,$2,$3,log($4),$6}' KD_GWAS.txt > KD_GWAS_PRS.txt
```

但**不要直接照这个命令跑**，要根据你实际 GWAS 列名调整。

---

# 五、非常重要：先统一 genome build

这是 PRS 最容易出问题的地方之一。

比如：

```text
KD GWAS：GRCh37
你的 WGS：GRCh38
```

那么不能直接匹配。

需要：

```text
GRCh37
   ↓
liftover
   ↓
GRCh38
   ↓
allele harmonization
   ↓
PRS
```

PGS Catalog 目前也提供 GRCh37/GRCh38 的 harmonized scoring files，并特别强调 variant position、rsID 和 effect allele 的协调。([PGS Catalog][1])

---

# 六、第二步：Target WGS QC

你的 WGS 如果是 PLINK2：

```bash
plink2 \
  --pfile KD_WGS \
  --maf 0.01 \
  --geno 0.01 \
  --hwe 1e-6 \
  --make-pgen \
  --out KD_WGS_QC
```

不过这里有一个重要区别：

**不要简单照搬 GWAS 的 QC。**

PRS target genotype 的 QC 主要是保证：

* genotype quality
* missingness
* allele consistency
* ancestry consistency
* variant matching

---

# 七、第三步：PRSice-2计算PRS

一个基本例子：

```bash
Rscript PRSice.R \
  --base KD_GWAS_PRS.txt \
  --target KD_WGS_QC \
  --snp SNP \
  --A1 A1 \
  --A2 A2 \
  --beta \
  --stat BETA \
  --pvalue P \
  --clump-kb 250 \
  --clump-r2 0.1 \
  --clump-p 1 \
  --pheno phenotype.txt \
  --pheno-col KD \
  --cov PC1,PC2,PC3,PC4,PC5,PC6,PC7,PC8,PC9,PC10 \
  --binary-target T \
  --out KD_PRS
```

PRSice 默认可以进行 LD clumping；例如 `--clump-kb 250`、`--clump-r2 0.1` 是常用设置，也可以使用外部 LD reference。对于小样本 target，PRSice 文档特别指出可以考虑使用相同人群的外部 LD reference 来改善 clumping。([PRSice command details](https://choishingwan.github.io/PRSice/command_detail/)) 

---

# 八、P值阈值不要只做一个

这是你这个项目里非常重要的一点。

不要只做：

```text
P < 5e-8
```

建议做一系列 threshold：

```text
5e-8
1e-6
1e-5
1e-4
1e-3
0.01
0.05
0.1
0.2
0.5
1
```

最终得到：

```text
PRS_5e-8
PRS_1e-6
PRS_1e-5
PRS_1e-4
PRS_1e-3
PRS_0.01
PRS_0.05
PRS_0.1
PRS_0.2
PRS_0.5
PRS_1
```

然后比较哪个 PRS 对你的 phenotype 解释能力最好。

PRSice-2 本身就是按照这种思路对多个 P-value thresholds 构建 PRS 并进行评估。([PRSice-2](https://choishingwan.github.io/PRSice/quick_start/)) 

---

# 九、但你的KD项目有一个非常关键的问题

你前面说你的队列大约：

> **200个KD患者**

如果你现在只有：

```text
200 KD
```

而没有：

```text
healthy controls
```

那么：

### ❌ 不能很好地验证“KD susceptibility PRS”

因为所有人都是 KD：

```text
KD vs healthy
```

这个 phenotype 根本不存在。

但是你完全可以研究：

### ✅ KD severity PRS

例如：

```text
KD PRS
   ↓
CAL
```

或者：

```text
KD PRS
   ↓
normal
dilation
aneurysm
```

甚至：

```text
KD PRS
   ↓
maximum Z-score
```

---

# 十、这其实非常适合你现在的项目

你的研究可以设计成：

```text
                  KD GWAS
                     │
                     ↓
                KD genetic PRS
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
       CAL risk             CAL severity
          │                     │
          ↓                     ↓
      dilation              max Z-score
      aneurysm              aneurysm size
```

然后再和你已有的：

```text
WGS
+
DIA proteomics
+
clinical biochemical data
+
ECG/echo
```

进行整合。

这就比单纯计算一个 KD-PRS 更有价值。

---

# 十一、我尤其推荐你做“PRS → PROTEIN → CAL”

结合你前面已经发现的 **PROZ** 等蛋白，可以进一步构建：

```text
KD GWAS
   ↓
KD-PRS
   ↓
PROZ pQTL / PROZ protein
   ↓
coagulation
   ↓
CAL
```

或者更完整：

```text
GWAS
 │
 ├── KD-PRS
 │       ↓
 │     CAL
 │
 └── pQTL
       ↓
    protein
       ↓
      CAL
```

然后可以进一步做：

### ① PRS-CAL association

```R
glm(
  CAL ~ PRS + age + sex + PC1 + PC2 + ...,
  family = binomial
)
```

### ② PRS-Z-score

```R
lm(
  max_Z ~ PRS + age + sex + PC1 + PC2 + ...
)
```

### ③ PRS-protein

```R
lm(
  PROZ ~ PRS + age + sex + PC1 + PC2 + ...
)
```

### ④ Mediation

进一步探索：

```text
KD-PRS
   ↓
PROZ
   ↓
CAL
```

这会形成一个非常漂亮的：

> **genetic risk → molecular phenotype → clinical phenotype**

框架。

---

# 十二、如果你的KD GWAS只有几十个显著SNP怎么办？

这个情况也很常见。

不要只使用：

```text
P < 5e-8
```

因为这样可能只有几十个 SNP，PRS 信息量太少。

应该让：

```text
P threshold
```

作为一个参数进行优化。

例如：

```text
PRS_5e-8
PRS_1e-6
PRS_1e-5
PRS_1e-4
PRS_1e-3
PRS_0.01
PRS_0.05
PRS_0.1
PRS_0.2
PRS_0.5
```

然后在**独立验证集**中选择最佳 threshold。

---

# 十三、如果KD GWAS本身样本量比较小，要特别谨慎

如果这个 KD GWAS 本身只有：

```text
Ncase = 几百
Ncontrol = 几百
```

那么直接建立 genome-wide PRS 可能比较不稳定。

这时我更建议：

### 方案A：已知KD GWAS风险位点 PRS

只使用：

```text
P < 5e-8
```

的可靠 loci。

### 方案B：clumping + thresholding

例如：

```text
P < 0.05
r² < 0.1
window = 250 kb
```

### 方案C：LD-aware方法

如果你的 GWAS 样本量比较大，可以进一步考虑：

* LDpred2
* PRS-CS
* SBayesR

这些通常比简单的 clumping + thresholding 更充分利用 GWAS summary statistics 和 LD 信息。

---

# 十四、你现在最应该先检查GWAS文件的这几个字段

如果你准备把 GWAS 结果给我，最重要的是看有没有：

```text
SNP / rsID
CHR
POS
EA / A1
NEA / A2
BETA 或 OR
SE
P
EAF
N
```

尤其是：

> **效应等位基因 + BETA/OR + genome build**

这三个最关键。

PGS Catalog 的标准 scoring file 也以 variant ID/位置、effect allele、other allele 和 effect weight 为核心字段。([PGS Catalog][1])

---

## 对你目前KD项目，我建议最终做成这条完整路线

```text
             KD GWAS summary statistics
                       │
                 QC / harmonization
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
       genome-wide PRS      known-locus PRS
             │                   │
             └─────────┬─────────┘
                       ↓
                  KD cohort
                       │
                ┌──────┴──────┐
                ↓             ↓
              CAL          CAL severity
                │             │
                ↓             ↓
          Logistic/Ordinal   Z-score
                       │
                       ↓
             Proteomics integration
                       │
                       ↓
             pQTL / protein / WGCNA
                       │
                       ↓
               PRS → protein → CAL
                       │
                       ↓
               causal mechanism
```

**如果你把现在这份川崎病 GWAS 的前几行（header + 5–10行）或者整个 summary statistics 文件上传，我可以直接按你的实际列名，给你写一套从 `GWAS结果 → harmonization → LD clumping → PRSice-2 → PRS计算 → CAL关联分析 → 最佳P阈值选择` 的完整 Linux 命令。**

[1]: https://www.pgscatalog.org/downloads/?utm_source=chatgpt.com "PGS Catalog - Download Information"

