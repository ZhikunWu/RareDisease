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


# 写一套从 GWAS结果 → harmonization → LD clumping → PRSice-2 → PRS计算 → CAL关联分析 → 最佳P阈值选择 的完整 Linux 命令

可以。下面给你一套**可以直接放到 Linux/HPC 上执行的完整流程**。我按照你目前的场景来设计：

> **KD GWAS summary statistics → harmonization → QC → LD clumping → PRSice-2 → 计算 KD-PRS → CAL 关联 → P-value threshold 选择 → 最终模型**

先强调一个关键点：如果你的 GWAS 是**外部 KD GWAS**，而你的约 200 例是独立的 KD 队列，那么这个流程非常合适。若你的 200 例全部都是 KD，没有健康/非KD对照，那么 **KD-PRS不能用来验证“是否患KD”**，而应主要用于 **CAL风险/严重程度**分析。

---

# 1. 推荐目录结构

```bash
mkdir -p KD_PRS/{00_raw,01_harmonization,02_qc,03_clump,04_prs,05_cal,06_result,log}
cd KD_PRS
```

建议：

```text
KD_PRS/
├── 00_raw/
│   ├── KD_GWAS.txt
│   └── target.pgen/pvar/psam
├── 01_harmonization/
├── 02_qc/
├── 03_clump/
├── 04_prs/
├── 05_cal/
├── 06_result/
└── log/
```

---

# 2. 软件准备

建议至少：

```bash
plink2 --version
R --version
```

以及：

```text
PRSice.R
PRSice_linux
```

PRSice-2 官方文档支持 base GWAS + target genotype，并可以同时进行 LD clumping、不同 P 阈值的 PRS 构建和 phenotype association。([Sam Choi][1])

设置：

```bash
export PRSICE=/path/to/PRSice.R
export PRSICE_BIN=/path/to/PRSice_linux
```

---

# 3. 第一步：检查你的 KD GWAS

假设原始文件：

```text
KD_GWAS.txt
```

例如：

```text
SNP CHR BP A1 A2 BETA SE P EAF N
rs123 1 123456 A G 0.15 0.03 5e-8 0.32 5000
rs456 1 234567 C T -0.08 0.02 4e-5 0.41 5000
```

先：

```bash
head KD_GWAS.txt
```

检查：

```bash
awk 'NR==1{print NF; print $0}' KD_GWAS.txt
```

以及：

```bash
awk 'NR>1 && $7 !~ /^[0-9.eE+-]+$/ {print; exit}' KD_GWAS.txt
```

---

# 4. GWAS harmonization：最重要的一步

PRS最容易出问题的其实不是 PRSice，而是：

> **GWAS effect allele 和 target genotype allele 没有正确对应。**

最终我们希望得到：

```text
SNP
CHR
BP
A1      # effect allele
A2      # other allele
BETA
SE
P
EAF
N
```

例如：

```text
rs123 A G 0.15 0.03 5e-8
```

其中：

```text
A1 = effect allele
BETA = A1对应的效应
```

---

# 5. 如果GWAS给的是 OR

例如：

```text
SNP A1 A2 OR SE P
```

则：

$$
BETA=\ln(OR)
$$

可以：

```bash
awk 'BEGIN{OFS="\t"}
NR==1{
    print "SNP","A1","A2","BETA","SE","P";
    next
}
{
    print $1,$2,$3,log($4),$5,$6
}' KD_GWAS_OR.txt > 01_harmonization/KD_GWAS_PRS.txt
```

**注意：这个命令只适用于你的第4列确实是 OR。**

---

# 6. 去除明显有问题的 SNP

建议先：

```bash
awk 'BEGIN{OFS="\t"}
NR==1{print;next}
$1!="." &&
$4!="." &&
$5!="." &&
$6!="." &&
$7!="." &&
$7>0 &&
$7<=1 &&
$4!=$5 &&
$4~/^[ACGT]$/ &&
$5~/^[ACGT]$/ &&
$6== $6
' KD_GWAS_PRS.txt \
> 01_harmonization/KD_GWAS_PRS.clean.txt
```

不过如果你的列结构不同，必须按实际列号修改。

---

# 7. 特别处理 A/T、C/G palindromic SNP

这一步非常重要。

以下 SNP：

```text
A/T
T/A
C/G
G/C
```

如果没有可靠的 EAF，很难判断 effect allele 是否发生 strand flip。

因此：

### 有 EAF

可以保留非高频 palindromic SNP，例如：

```text
EAF < 0.42
```

### 没有 EAF

建议直接删除：

```bash
awk 'BEGIN{OFS="\t"}
NR==1{print;next}
!(
 ($4=="A"&&$5=="T") ||
 ($4=="T"&&$5=="A") ||
 ($4=="C"&&$5=="G") ||
 ($4=="G"&&$5=="C")
)' KD_GWAS_PRS.clean.txt \
> KD_GWAS_PRS.harmonized.txt
```

---

# 8. Target WGS首先进行 QC

假设你的 target：

```text
target.pgen
target.pvar
target.psam
```

先：

```bash
plink2 \
  --pfile target \
  --geno 0.02 \
  --mind 0.02 \
  --make-pgen \
  --out 02_qc/target.qc
```

如果是高质量 WGS，也可以使用：

```text
--geno 0.01
--mind 0.01
```

但 PRS 的关键是：

> **GWAS中的 SNP 和 target 中的 SNP 能够正确匹配。**

---

# 9. 统一 genome build

这一点必须先确认。

例如：

```text
KD GWAS     GRCh37
Target WGS  GRCh38
```

那么：

```text
GRCh37 GWAS
      ↓
LiftOver
      ↓
GRCh38
      ↓
allele harmonization
```

**不要直接按照 rsID 匹配后就计算 PRS。**

如果两边都是：

```text
GRCh38
```

则继续。

---

# 10. 提取GWAS与target共有SNP

先获得 target SNP：

```bash
plink2 \
  --pfile 02_qc/target.qc \
  --write-snplist \
  --out 02_qc/target
```

然后：

```bash
awk 'NR==FNR{a[$1]=1;next} NR==1 || ($1 in a)' \
    02_qc/target.snplist \
    01_harmonization/KD_GWAS_PRS.harmonized.txt \
    > 02_qc/KD_GWAS.target_overlap.txt
```

检查：

```bash
wc -l 02_qc/KD_GWAS.target_overlap.txt
```

---

# 11. 最重要：allele harmonization

你最好把 GWAS 和 target 的：

```text
SNP
REF
ALT
```

一起比较。

例如 target：

```bash
plink2 \
  --pfile 02_qc/target.qc \
  --freq \
  --out 02_qc/target
```

得到：

```text
target.afreq
```

然后检查：

```text
GWAS A1/A2
      ↕
Target REF/ALT
```

合法情况：

```text
GWAS A1=A
GWAS A2=G

Target A/G
```

或者：

```text
GWAS A1=A
GWAS A2=G

Target G/A
```

第二种需要交换 effect allele 对应关系。

---

# 12. 如果你的GWAS来自公开数据库，推荐直接使用经过harmonization的文件

如果有：

```text
GWAS SSF
PGS Catalog
```

或者作者提供的标准化 summary statistics，优先使用。

因为手动处理：

```text
strand
effect allele
REF/ALT
palindromic SNP
build
```

非常容易出现方向错误。

---

# 13. LD clumping

这里有两种方式。

## 方案A：直接用PRSice-2

我更推荐这个。

例如：

```bash
Rscript $PRSICE \
  --prsice $PRSICE_BIN \
  --base 01_harmonization/KD_GWAS_PRS.harmonized.txt \
  --target 02_qc/target.qc \
  --snp SNP \
  --A1 A1 \
  --A2 A2 \
  --stat BETA \
  --beta \
  --pvalue P \
  --clump-kb 250 \
  --clump-r2 0.1 \
  --clump-p 1 \
  --out 04_prs/KD_PRS
```

PRSice会按照不同 P-value threshold 建立 PRS。

---

# 14. 但是我更推荐你使用一个独立LD reference

你现在只有约200个KD target。

**不要太依赖这200个人估计LD。**

最好使用：

```text
East Asian LD reference
```

例如：

```text
1000 Genomes EAS
```

或者与你研究人群更接近的 EAS reference panel。

PLINK2 官方文档也说明，`--clump` 的 genotype reference 可以与进行关联分析的数据集不同，关键是应包含目标 SNP 并具有相似的群体结构。([COG Genomics][2])

---

# 15. 如果用PLINK2自己clumping

假设你的文件：

```text
SNP A1 A2 BETA P
```

可以：

```bash
plink2 \
  --pfile EAS_LD_reference \
  --clump 01_harmonization/KD_GWAS_PRS.harmonized.txt \
  --clump-id-field SNP \
  --clump-p-field P \
  --clump-a1-field A1 \
  --clump-p1 1 \
  --clump-p2 1 \
  --clump-r2 0.1 \
  --clump-kb 250 \
  --clump-unphased \
  --out 03_clump/KD
```

PLINK2 的 `--clump` 本质上根据 P 值和 LD 把相关 SNP 聚成 clump；`--clump-r2`、`--clump-kb` 控制 LD 和距离。([COG Genomics][3])

---

# 16. PRS threshold建议

不要只用：

```text
P < 5e-8
```

建议：

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
0.3
0.5
1
```

可以让 PRSice 自动搜索。

---

# 17. 完整PRSice-2命令

假设：

```text
KD_GWAS_PRS.harmonized.txt
```

包含：

```text
SNP A1 A2 BETA SE P
```

Target：

```text
target.qc
```

Phenotype：

```text
CAL.pheno
```

例如：

```text
FID IID CAL
KD001 KD001 0
KD002 KD002 1
KD003 KD003 0
...
```

运行：

```bash
Rscript $PRSICE \
  --prsice $PRSICE_BIN \
  --base 01_harmonization/KD_GWAS_PRS.harmonized.txt \
  --target 02_qc/target.qc \
  --snp SNP \
  --A1 A1 \
  --A2 A2 \
  --stat BETA \
  --beta \
  --pvalue P \
  --clump-kb 250 \
  --clump-r2 0.1 \
  --clump-p 1 \
  --lower 5e-8 \
  --upper 1 \
  --interval 1e-5 \
  --pheno 05_cal/CAL.pheno \
  --pheno-col CAL \
  --binary-target T \
  --cov-file 05_cal/covariates.txt \
  --cov-col PC1,PC2,PC3,PC4,PC5,PC6,PC7,PC8,PC9,PC10,AGE,SEX \
  --out 04_prs/KD_PRS
```

---

# 18. 注意一个PRSice参数问题

不同 PRSice-2 版本的参数名称可能略有差异。

所以先：

```bash
Rscript $PRSICE --help
```

确认：

```text
--cov-file
--cov-col
--pheno
--pheno-col
```

等参数。

不要直接复制一个旧版本教程中的全部参数。

---

# 19. PRSice会得到什么？

一般会产生类似：

```text
KD_PRS.best
KD_PRS.summary
KD_PRS.sscore
KD_PRS.log
KD_PRS
```

其中最重要的是：

```text
KD_PRS.best
```

它会告诉你：

```text
最佳P threshold
最佳PRS
R²
P
```

例如假设：

```text
PT = 0.001
R² = 0.087
P = 0.004
```

那么说明：

> P<0.001筛选得到的 KD-PRS 在当前数据中与 CAL 的关联最好。

---

# 20. 但这里有一个非常重要的统计问题

**不能直接把PRSice找到的“最佳P阈值”当成最终无偏结果。**

因为你是在：

```text
同一个数据集
```

里：

```text
尝试12个P threshold
        ↓
选择P值最小的
        ↓
报告这个P值
```

这会产生 **winner's curse / threshold selection bias**。

因此最好：

```text
Discovery
    ↓
选择P threshold
    ↓
Independent validation
    ↓
最终报告
```

如果你现在只有约200个KD患者，可以考虑：

### 5-fold CV

例如：

```text
Fold 1
  train 80%
  test 20%

Fold 2
  train 80%
  test 20%

...
```

在训练集选择最佳 threshold：

```text
P < 5e-8
P < 1e-6
...
P < 1
```

然后在测试集评估。

---

# 21. 你的CAL phenotype如果是四分类

你之前提到你的KD样本现在是**四类CAL状态**。

那么不要简单粗暴地：

```text
CAL = 0/1
```

我建议同时做三种模型。

---

## 模型1：CAL vs non-CAL

如果定义：

```text
0 = no coronary lesion
1 = coronary lesion
```

使用：

```R
glm(
    CAL ~ PRS + AGE + SEX + PC1 + PC2 + ... + PC10,
    family = binomial
)
```

---

# 22. 模型2：四分类CAL

例如：

```text
0 = normal
1 = dilation
2 = aneurysm
3 = severe aneurysm
```

使用：

```R
library(nnet)

fit <- multinom(
    CAL4 ~ PRS + AGE + SEX + PC1 + PC2 + PC3 + PC4 + PC5,
    data = dat
)
```

---

# 23. 模型3：CAL严重程度ordinal

如果四类本身有明确的严重程度顺序：

```text
normal
   ↓
dilation
   ↓
aneurysm
   ↓
giant aneurysm
```

那么**优先推荐 ordinal logistic regression**。

```R
library(MASS)

fit <- polr(
    CAL4 ~ PRS + AGE + SEX + PC1 + PC2 + PC3 + PC4 + PC5,
    data = dat,
    Hess = TRUE
)
```

这样可以直接检验：

$$
PRS \rightarrow CAL\ severity
$$

---

# 24. 如果你有最大Z-score

我反而非常推荐这个。

例如：

```text
Maximum_Z
```

可以：

```R
fit <- lm(
    Maximum_Z ~ PRS + AGE + SEX + PC1 + PC2 + PC3 + PC4 + PC5,
    data = dat
)

summary(fit)
```

这个分析比把连续的 Z-score 强行分类更加充分利用信息。

---

# 25. 最佳P threshold怎么选择？

最终建议同时报告：

| P threshold | SNP数量 | OR/β |  P | AUC | R² |
| ----------: | ----: | ---: | -: | --: | -: |
|        5e-8 |       |      |    |     |    |
|        1e-6 |       |      |    |     |    |
|        1e-5 |       |      |    |     |    |
|        1e-4 |       |      |    |     |    |
|        1e-3 |       |      |    |     |    |
|        0.01 |       |      |    |     |    |
|        0.05 |       |      |    |     |    |
|         0.1 |       |      |    |     |    |
|         0.2 |       |      |    |     |    |
|         0.5 |       |      |    |     |    |
|           1 |       |      |    |     |    |

对于 CAL 二分类：

> 主要看 **OR、95% CI、P、AUC、ΔAUC、Nagelkerke R²、calibration**。

---

# 26. 更重要的是比较“有PRS”和“无PRS”

不要只报告：

```text
PRS P=0.003
```

应该比较：

### Model 1

```text
CAL ~ AGE + SEX + PC1-PC10
```

### Model 2

```text
CAL ~ AGE + SEX + PC1-PC10 + PRS
```

然后比较：

```text
AUC
R²
Likelihood ratio test
AIC
BIC
Calibration
Decision curve
```

例如：

```text
Clinical model:
AUC = 0.71

Clinical + KD-PRS:
AUC = 0.78
```

那么 PRS 才真正体现临床价值。

---

# 27. 如果你的目标是“CAL风险预测”，推荐最终模型

结合你现在的多组学项目，我建议：

```text
Model 0
Clinical
    ↓
CAL

Model 1
Clinical + KD-PRS
    ↓
CAL

Model 2
Clinical + Proteins
    ↓
CAL

Model 3
Clinical + KD-PRS + Proteins
    ↓
CAL

Model 4
Clinical + KD-PRS + Proteins + pQTL
    ↓
CAL
```

这样就能证明：

> **遗传风险 + 蛋白组 + 临床指标**是否能够提高 CAL 风险预测能力。

---

# 28. 你尤其可以进一步做“PRS → PROZ → CAL”

既然你刚才提到了 **PROZ**，如果 PROZ 在你的 pQTL 中比较突出，可以进一步做：

```text
KD-GWAS
   ↓
KD-PRS
   ↓
PROZ
   ↓
CAL
```

分别检验：

### Step 1

```R
PROZ ~ KD_PRS + age + sex + PCs
```

### Step 2

```R
CAL ~ KD_PRS + age + sex + PCs
```

### Step 3

```R
CAL ~ KD_PRS + PROZ + age + sex + PCs
```

如果：

```text
KD-PRS → PROZ
KD-PRS → CAL
```

同时加入 PROZ 后 KD-PRS 效应减弱，那么可以进一步做 mediation analysis。

但这属于**机制探索**，不能仅凭关联就宣称因果。

---

# 29. 最终建议的目录和结果

最后你应该得到：

```text
01_harmonization/
    KD_GWAS_PRS.harmonized.txt

02_qc/
    target.qc.pgen
    target.qc.pvar
    target.qc.psam

03_clump/
    KD.clumps

04_prs/
    KD_PRS.best
    KD_PRS.summary
    KD_PRS.sscore

05_cal/
    CAL.pheno
    covariates.txt

06_result/
    PRS_CAL_association.txt
    PRS_threshold_comparison.txt
    PRS_AUC.pdf
    PRS_CAL_boxplot.pdf
```

---

## 30. 我给你的参数建议

针对你目前这个 KD 项目，我会优先使用：

```text
LD window       = 250 kb
LD r²            = 0.1
P thresholds    = 5e-8 ~ 1
LD reference    = EAS reference
Covariates      = age + sex + PC1-PC10
```

`250 kb + r²=0.1` 是一个比较常见的 PRS clumping 起点；PLINK2 的 clumping 本身支持调整距离和 r² 阈值。([COG Genomics][3])

**但你的 n≈200 比较小，所以不要把“PRSice自动选出的最佳 P threshold”直接作为最终证据。** 最好用 5-fold CV 或独立验证队列。

### 最关键的流程

```text
KD GWAS
   │
   ├── genome build统一
   ├── SNP/allele QC
   ├── A/T、C/G处理
   ├── effect allele统一
   │
   ↓
Harmonized GWAS
   │
   ↓
EAS LD clumping
   │
   ├── r² < 0.1
   └── 250 kb
   │
   ↓
PRSice-2
   │
   ├── 5e-8
   ├── 1e-6
   ├── 1e-5
   ├── 1e-4
   ├── 1e-3
   ├── 0.01
   ├── 0.05
   ├── 0.1
   ├── 0.2
   ├── 0.5
   └── 1
   │
   ↓
KD-PRS
   │
   ├── CAL vs non-CAL
   ├── 4-level CAL
   └── max Z-score
   │
   ↓
Clinical + PRS
   │
   ↓
AUC / OR / R² / calibration
   │
   ↓
Proteomics / pQTL
   │
   ↓
KD-PRS → PROZ/protein → CAL
```

**如果你把你的 KD GWAS 文件的 `header + 前5行` 发给我，我可以下一步直接按你实际的列名（例如 `CHR POS SNP EA NEA OR SE P EAF N`）把上面这套流程改成一份可以直接运行的 `run_KD_PRS.sh`，包括 allele harmonization、palindromic SNP过滤、PLINK2 clumping、PRSice-2 和最后的 CAL logistic/ordinal regression。**

[1]: https://choishingwan.github.io/PRSice/archive/PRSice_MANUAL_v1.25.pdf?utm_source=chatgpt.com "5  Detail on command line options"
[2]: https://www.cog-genomics.org/plink/2.0/tutorials/qwas2?utm_source=chatgpt.com "GWAS Post-Hoc - PLINK 2.0"
[3]: https://www.cog-genomics.org/plink/2.0/postproc?utm_source=chatgpt.com "Report postprocessing - PLINK 2.0"



