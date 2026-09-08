# KD_Protein Project

现在有两百个川崎病人血浆样本，有发病急性期和恢复期两个时期，每个时期进行DIA的蛋白组测序，川崎病人根据冠状动脉损伤情况分为四类，同时这些样本都进行了WGS测序，现在想分析与川崎病发病风险及冠状动脉损伤相关的蛋白，制定一个深度、详细、全面的研究方案

可以，而且你这套数据的设计其实非常适合做一个**“纵向蛋白组 + WGS遗传学 + 冠脉分层 + 多模态预测”**的完整研究。相比单纯找 DEPs，我更建议把项目设计成一条完整的生物学链：

> **遗传易感性 → 急性期蛋白改变 → 炎症/血管内皮/ECM重塑 → 冠状动脉损伤 → 恢复期蛋白是否恢复 → 遗传变异是否通过蛋白介导 CAL 风险 → 构建可解释的风险预测模型**

现有 KD 蛋白组研究已经提示炎症、补体、脂蛋白、内皮损伤和 ECM 等方向，但目前多数候选标志物仍需要更大队列和标准化纵向验证；近期综述也特别强调了 IL-1/IL-6–中性粒细胞、FcγR/IVIG 以及内皮/ECM 重塑等多组学轴。([PubMed][1])

下面我按照**可以真正用于论文/基金/课题实施**的方式给你设计。

---

# 一、首先明确：你的研究其实包含 4 个不同问题

不要把所有样本直接扔进一个差异分析。

你的 200 个 KD 患儿、两个时间点、4 个 CAL 分组，可以拆成：

### Aim 1：KD 急性疾病相关蛋白

> 哪些蛋白反映 KD 急性炎症状态？

### Aim 2：CAL 风险相关蛋白

> 哪些蛋白与冠状动脉损伤严重程度相关？

### Aim 3：纵向动态蛋白

> 哪些蛋白在急性期异常，而恢复期恢复？哪些异常持续存在？

### Aim 4：遗传—蛋白—CAL机制

> WGS 中的遗传变异是否通过调控某些血浆蛋白影响 CAL？

最后再增加：

### Aim 5：临床预测

> 能否利用 **临床指标 + 蛋白组 + WGS-derived genetic risk** 构建 CAL 风险预测模型？

---

# 二、一个非常重要的问题：你现在的数据能不能研究“KD发病风险”？

这里需要先区分两个概念。

如果你的 200 人**全部都是 KD 患者，没有健康儿童/发热对照**：

### 可以研究

> KD 内部的疾病严重程度、CAL 风险、恢复情况。

### 不能严格研究

> “谁更容易患 KD”。

因为真正的 KD susceptibility analysis 至少需要：

```text
KD patients
      vs
healthy controls
```

WGS 才能做：

```text
genetic variant
       ↓
KD susceptibility
```

过去 KD 遗传学研究就是将 KD 与健康对照比较；例如已有研究在 KD susceptibility 和 CAA susceptibility 上分别进行 GWAS。([PubMed Central (PMC)][2])

所以你的项目中建议把“发病风险”定义成两个层面：

### 如果没有健康对照

**KD disease activity / CAL risk**

### 如果能补充健康对照

才可以增加：

**KD susceptibility**

---

# 三、我最推荐你的总体研究框架

```text
                         200 KD patients
                               │
                ┌──────────────┴──────────────┐
                │                             │
           Acute phase                  Recovery phase
                │                             │
             DIA-MS                        DIA-MS
                │                             │
                └──────────────┬──────────────┘
                               │
                         paired proteome
                               │
              ┌────────────────┼─────────────────┐
              │                │                 │
             WGS             CAL              Clinical
              │                │                 │
              │          4 phenotype groups     │
              │                │                 │
              └────────────────┼─────────────────┘
                               ↓
                    Integrated multi-omics
                               │
       ┌───────────────────────┼────────────────────────┐
       ↓                       ↓                        ↓
  Disease activity       CAL severity            Longitudinal
       │                       │                    dynamics
       ↓                       ↓                        ↓
   Candidate proteins    CAL biomarkers         persistent proteins
       │                       │                        │
       └───────────────────────┼────────────────────────┘
                               ↓
                         WGS integration
                               │
                    ┌──────────┼──────────┐
                    ↓          ↓          ↓
                  pQTL       PRS        rare variants
                    │          │          │
                    └──────────┼──────────┘
                               ↓
                     genetic → protein → CAL
                               │
                               ↓
                     mediation / MR analysis
                               │
                               ↓
                  multi-modal prediction model
                               │
                               ↓
               external / internal validation
```

---

# 四、第一部分：DIA 蛋白组 QC

这是整个项目的基础。

## 4.1 Protein-level QC

首先建立：

```text
sample × protein
```

矩阵。

检查：

* protein identification number
* peptide number
* missingness
* CV
* precursor intensity
* batch
* retention time
* digestion quality
* technical replicate consistency

建议：

```text
log2 transformation
↓
missing value assessment
↓
sample QC
↓
batch correction
↓
protein filtering
```

---

# 五、样本级 PCA / clustering

首先不要看疾病。

直接：

```text
PCA
UMAP
hierarchical clustering
sample correlation
```

分别看：

```text
Acute
Recovery
```

以及：

```text
CAL group 1
CAL group 2
CAL group 3
CAL group 4
```

尤其要找：

```text
batch effect
outlier
hemolysis
extreme inflammatory sample
```

---

# 六、第二部分：最重要的纵向分析——配对设计

你有一个非常大的优势：

> **同一个患者有 acute + recovery 两个时间点。**

所以千万不要把两个时期当成独立样本。

假设：

```text
Patient 001
    Acute
      │
      └──── Recovery

Patient 002
    Acute
      │
      └──── Recovery
```

应该使用：

### paired analysis

或者更推荐：

### linear mixed model

例如：

```text
Protein ~ Time + CAL + Time:CAL + Age + Sex + (1|Patient)
```

这里：

* `Time`：acute/recovery
* `CAL`：4级 CAL
* `Time:CAL`：关键交互项
* `Patient`：random effect

---

# 七、这个 `Time × CAL` interaction 是你整个项目非常值得做的分析

它可以回答：

> **不同冠脉损伤程度的患者，其蛋白在急性期到恢复期的变化轨迹是否不同？**

例如：

```text
Protein A

                 Acute       Recovery

CAL0             ↑            ↓
CAL1             ↑↑           ↓
CAL2             ↑↑↑          →
CAL3             ↑↑↑↑         ↑
```

这种蛋白比单纯：

```text
Acute CAL3 > Acute CAL0
```

更加有生物学意义。

因为它提示：

> **严重 CAL 患者的炎症/血管损伤蛋白异常不仅更强，而且恢复不完全。**

---

# 八、第三部分：4 类 CAL 不要只做两两比较

你说患者根据冠状动脉损伤分为四类。

建议把它同时作为：

## 分类型变量

```text
CAL0
CAL1
CAL2
CAL3
```

以及：

## 有序变量

```text
0 < 1 < 2 < 3
```

这样做两套分析。

---

# 九、四级 CAL 的蛋白组分析

建议：

### Model 1

```text
Protein ~ CAL_group + Age + Sex + Batch
```

寻找：

```text
CAL0 vs CAL1
CAL0 vs CAL2
CAL0 vs CAL3
CAL1 vs CAL2
CAL1 vs CAL3
CAL2 vs CAL3
```

但是不要只依赖两两比较。

---

# 十、强烈建议增加 CAL severity trend

把 CAL 当作：

```text
0,1,2,3
```

进行：

```text
Protein ~ CAL_score
```

寻找：

```text
CAL severity ↑
        ↓
Protein ↑
```

这种蛋白特别适合做：

> **冠状动脉损伤 severity-associated proteins**

例如：

```text
Protein X

CAL0     █
CAL1     ███
CAL2     █████
CAL3     ███████
```

这类趋势蛋白比单纯某一组显著更稳定。

---

# 十一、第四部分：做“急性期 CAL 蛋白”

这是你最重要的一组 biomarker。

定义：

```text
Acute CAL3
vs
Acute CAL0
```

以及：

```text
Acute CAL severity trend
```

得到：

```text
CAL-associated DEPs
```

推荐统计：

```text
FDR < 0.05
|log2FC| > 0.5
```

具体阈值最好根据蛋白数量和 effect size 分布调整，不建议机械使用 |FC|>2。

---

# 十二、第五部分：恢复期分析——这里很可能有很好的结果

把蛋白分成四种模式：

### Pattern A

```text
Acute ↑
Recovery ↓
```

说明：

> 急性疾病相关，治疗/疾病缓解后恢复。

---

### Pattern B

```text
Acute ↑
Recovery ↑
```

说明：

> persistent abnormality。

非常值得研究。

---

### Pattern C

```text
Acute normal
Recovery abnormal
```

可能提示：

> disease aftermath / vascular remodeling。

---

### Pattern D

```text
Acute ↓
Recovery ↑
```

提示：

> recovery-associated restoration。

---

# 十三、进一步做 trajectory clustering

这是我非常推荐你的分析。

对于每个蛋白：

```text
Acute CAL0
Acute CAL1
Acute CAL2
Acute CAL3
Recovery CAL0
Recovery CAL1
Recovery CAL2
Recovery CAL3
```

得到一个：

```text
8-point protein trajectory
```

然后：

```text
hierarchical clustering
k-means
Mfuzz
```

把蛋白分成：

```text
Cluster 1
acute inflammatory

Cluster 2
vascular injury

Cluster 3
persistent CAL

Cluster 4
recovery

Cluster 5
CAL-specific
```

这会比传统 DEG volcano plot 高一个层次。

---

# 十四、第六部分：蛋白功能富集

对每一个重要 protein cluster：

```text
GO
KEGG
Reactome
Hallmark
WikiPathways
```

我特别建议关注：

### Immunity

* IL-1
* IL-6
* TNF
* NF-κB
* neutrophil activation
* complement
* Fcγ receptor

### Endothelium

* endothelial activation
* angiogenesis
* vascular permeability

### ECM

* collagen
* extracellular matrix organization
* MMP
* TGF-β

### Platelet / coagulation

* coagulation
* platelet activation
* thrombosis

### Lipid metabolism

* APO
* LDL
* HDL
* lipid transport

近期 KD 多组学综述同样将炎症/中性粒细胞、FcγR、T-cell signaling、内皮和 ECM remodeling 作为值得整合的主要轴。([PubMed Central (PMC)][3])

---

# 十五、第七部分：不要只找单个蛋白，要建立 Protein Network

建议：

```text
WGCNA
```

这是你的 DIA 数据非常适合做的。

建立：

```text
protein expression
        ↓
WGCNA
        ↓
modules
```

例如：

```text
Module 1 → inflammation
Module 2 → complement
Module 3 → endothelial
Module 4 → coagulation
Module 5 → lipid metabolism
```

然后计算：

```text
module eigengene
```

与：

```text
CAL severity
CRP
ESR
platelet
albumin
NT-proBNP
ECG
echo
IVIG response
```

进行相关分析。

---

# 十六、WGCNA 可能比单蛋白分析更重要

例如你发现：

```text
Module M7
```

与 CAL：

```text
r = 0.62
P < 0.001
```

里面包含：

```text
SERPINE1
MMP9
VCAM1
ICAM1
...
```

那么可以提出：

> **vascular injury module**

而不是只说：

> SERPINE1 是一个 biomarker。

这会明显增强机制解释。

已有 KD DIA-MS 研究确实已经报道 SERPINE1 与 CAL 相关，并进行了独立 ELISA 验证，因此如果你的数据再次发现 SERPINE1，更适合把它作为一个“已知信号 + 新机制网络”的锚点，而不是简单重复发现。([科学直通车][4])

---

# 十七、第八部分：临床指标整合

把蛋白和临床指标连接起来。

例如：

```text
Protein
│
├── CRP
├── ESR
├── WBC
├── neutrophils
├── platelets
├── albumin
├── ALT/AST
├── bilirubin
├── NT-proBNP
└── ECG/echo
```

推荐：

### Spearman correlation

以及：

### partial correlation

控制：

```text
Age
Sex
Disease duration
IVIG
```

---

# 十八、第九部分：建立蛋白 biomarker panel

不要最后得到：

> 1000 个 DEPs。

而应该得到：

> **5–15 个核心蛋白 panel。**

例如：

```text
Candidate proteins
      ↓
univariate
      ↓
LASSO
      ↓
Random Forest
      ↓
XGBoost
      ↓
recursive feature elimination
      ↓
5–10 protein panel
```

然后：

```text
ROC
AUC
Sensitivity
Specificity
PPV
NPV
Calibration
Decision curve
```

---

# 十九、非常重要：不要用全数据训练模型

你的 n=200 不大。

必须：

```text
Training 70–80%
       ↓
Feature selection
       ↓
Model
       ↓
Test 20–30%
```

或者更推荐：

### nested 5-fold CV

避免：

```text
先全数据找 DEPs
↓
再随机拆 train/test
```

这种 **data leakage**。

---

# 二十、第十部分：WGS——我不建议你现在直接做普通 GWAS

200 人做 genome-wide CAL GWAS：

```text
n = 200
```

统计效能非常有限。

而且你还有：

```text
4 CAL groups
```

进一步降低 power。

已有 KD CAA GWAS 往往使用数百至上千例并设置独立 replication，例如 TIFAB 的研究在发现/复制队列中合计达到数百例以上；近期 WGS CAA 研究也采用数百例病例/对照。([PubMed][5])

所以你的 WGS 最适合走：

# “候选遗传变异 → pQTL → 蛋白 → CAL”

而不是：

> 200 人直接做 genome-wide discovery GWAS。

---

# 二十一、WGS 第一层：标准 variant analysis

首先：

```text
FASTQ
 ↓
alignment
 ↓
BQSR / QC
 ↓
SNV
Indel
SV
CNV
```

然后：

```text
common variants
rare variants
SV
```

---

# 二十二、WGS 第二层：KD/CAL 已知风险位点

建立一个：

```text
KD genetic risk loci
```

candidate list。

包括文献中：

```text
FCGR2A
ITPKC
CASP3
BLK
CD40
TGFBR2
SMAD3
KCNN2
TIFAB
...
```

已有系统综述总结了多个与 KD susceptibility 和 CAL 相关的遗传位点；KCNN2、TIFAB 等也有较明确的 CAA association evidence。([PubMed][6])

---

# 二十三、第三层：建立 KD/CAL PRS

如果能获得：

```text
published GWAS summary statistics
```

可以计算：

```text
KD-PRS
CAL-PRS
CAA-PRS
```

然后：

```text
PRS
 ↓
protein
 ↓
CAL
```

特别有意思。

---

# 二十四、第四层：你真正应该做的是 pQTL

你有：

```text
WGS
+
DIA protein
```

这就是非常好的：

# cis-pQTL / trans-pQTL 数据集

分析：

```text
SNP
 ↓
Protein
```

模型：

```text
Protein ~ SNP + Age + Sex + PCs + Batch
```

重点：

### cis-pQTL

```text
SNP
 │
 │  nearby
 ↓
Gene
 ↓
Protein
```

优先级最高。

---

# 二十五、建立一个“genotype → protein”网络

例如：

```text
rsXXXX
   ↓
SERPINE1
   ↓
CAL
```

或者：

```text
rsXXXX
   ↓
MMP9
   ↓
vascular remodeling
   ↓
CAL
```

这比：

```text
SNP associated with CAL
```

更有机制解释力。

---

# 二十六、第五层：protein-QTL × CAL association

这是非常关键的一步。

先：

```text
SNP → Protein
```

再：

```text
Protein → CAL
```

如果两者连接：

```text
SNP
 ↓
Protein
 ↓
CAL
```

就出现一个候选：

> **genetically anchored protein biomarker**

这种蛋白的证据等级远高于普通 DE protein。

---

# 二十七、进一步做 Mendelian Randomization

如果你的样本量和 pQTL instrument 足够：

```text
SNP
 ↓
Protein
 ↓
CAL
```

做：

### cis-pQTL-based MR

例如：

```text
Exposure = SERPINE1
Outcome = CAL
```

测试：

> genetically predicted SERPINE1 是否影响 CAL risk？

最好使用外部 GWAS summary statistics 来提高 power，而不是仅靠自己的 200 人做 MR。

---

# 二十八、进一步做 mediation analysis

如果你的数据支持，可以建立：

```text
Genotype
   ↓
Protein
   ↓
CAL
```

做：

```text
Total effect
Direct effect
Indirect effect
Mediation proportion
```

最终可以说：

> 某遗传变异对 CAL 的影响部分通过某血浆蛋白介导。

这会成为整个项目中非常漂亮的机制链。

---

# 二十九、我特别推荐一个“证据分级体系”

最后不要把所有蛋白放在一起。

建立：

## Tier 1：强候选

满足：

```text
Acute CAL association
+
longitudinal change
+
WGCNA module
+
pQTL
+
genetic association
```

---

## Tier 2：中等候选

```text
CAL association
+
longitudinal change
+
clinical correlation
```

---

## Tier 3：探索性

```text
only differential protein
```

---

# 三十、最终的核心蛋白可以做一个 Evidence Score

例如：

| Evidence                | Score |
| ----------------------- | ----: |
| Acute CAL association   |    +2 |
| CAL severity trend      |    +2 |
| Time×CAL interaction    |    +2 |
| Recovery persistence    |    +1 |
| WGCNA hub               |    +2 |
| Clinical correlation    |    +1 |
| cis-pQTL                |    +2 |
| genetic CAL association |    +2 |
| MR support              |    +3 |
| independent validation  |    +3 |

最终：

```text
Protein A = 15
Protein B = 11
Protein C = 7
```

这样就能系统地筛出：

> **Top 10 mechanistically supported CAL proteins**

---

# 三十一、然后建立三个模型，而不是一个模型

## Model 1：临床模型

```text
Age
Sex
Disease duration
CRP
ESR
WBC
Platelet
Albumin
etc.
```

得到：

```text
AUC = ?
```

---

## Model 2：Protein model

```text
Clinical
+
Protein panel
```

比较：

```text
ΔAUC
NRI
IDI
Decision curve
```

---

## Model 3：Multi-omics model

```text
Clinical
+
Protein
+
PRS
```

甚至：

```text
Clinical
+
Protein
+
PRS
+
pQTL-informed proteins
```

最终比较：

```text
Clinical
       ↓
Clinical + Protein
       ↓
Clinical + Protein + Genetics
```

如果：

```text
AUC

0.72
 ↓
0.84
 ↓
0.89
```

就可以很好地证明：

> **多组学整合显著提升 CAL 风险预测能力。**

---

# 三十二、还有一个非常值得做的方向：acute → recovery “蛋白恢复指数”

因为你的设计非常特殊。

可以定义：

```text
Recovery Index
=
Protein_recovery - Protein_acute
```

或者标准化：

```text
ΔProtein = Z(recovery) - Z(acute)
```

然后研究：

```text
ΔProtein
       ↓
CAL severity
```

例如：

```text
CAL0:
Protein ↓↓↓

CAL3:
Protein ↓
```

说明：

> CAL 严重患者恢复不完全。

这类 **dynamic biomarker** 可能比单时间点 biomarker 更有价值。

---

# 三十三、进一步：建立“蛋白恢复轨迹”

可以定义：

```text
Responder-like trajectory
```

和：

```text
Persistent-inflammation trajectory
```

例如：

```text
          Acute       Recovery

Normal     ────────────

CAL-low    ↑───────↓────

CAL-high   ↑↑↑────────↑
```

最后筛：

> **persistent CAL-associated proteins**

这个结果很可能成为文章中一个独立章节。

---

# 三十四、如果有 IVIG 时间信息，一定要纳入

这是非常重要的混杂因素。

必须明确：

```text
Acute sample
```

到底是：

```text
IVIG before
```

还是：

```text
IVIG after
```

因为 IVIG 会强烈影响炎症和蛋白组。

最好建立：

```text
Pre-IVIG
Post-IVIG
Recovery
```

如果目前只有：

```text
Acute
Recovery
```

至少把：

```text
IVIG treatment
IVIG resistance
days from fever onset
```

纳入模型。

近期 KD 多组学综述也明确强调，未来 biomarker 研究需要标准化 pre-/post-IVIG sampling。([PubMed Central (PMC)][3])

---

# 三十五、还需要特别处理几个临床混杂因素

至少：

```text
Age
Sex
Fever days
IVIG treatment
IVIG resistance
Steroid
Aspirin
CRP
ESR
WBC
Platelet
Albumin
ALT
AST
NT-proBNP
Troponin
```

尤其：

### Age

KD 患儿年龄跨度可能比较大。

### Sex

CAL 风险存在性别差异；近期研究甚至报道了 sex-specific CAA susceptibility loci。([PubMed Central (PMC)][7])

### Disease duration

这是一个非常大的 confounder：

```text
fever day 3
vs
fever day 8
```

蛋白组可能完全不同。

所以：

> **疾病天数必须进入模型。**

---

# 三十六、建议你把最终研究分成 5 个 Specific Aims

如果让我帮你设计论文/基金，我会写成：

## Aim 1

### Define the longitudinal plasma proteomic landscape of Kawasaki disease

研究：

```text
Acute vs Recovery
```

得到：

> KD dynamic proteome

---

## Aim 2

### Identify proteomic signatures associated with coronary artery lesion severity

研究：

```text
4 CAL groups
```

得到：

> CAL-associated proteins

---

## Aim 3

### Identify genetically anchored proteins associated with CAL susceptibility

整合：

```text
WGS
+
pQTL
+
protein
+
CAL
```

得到：

> genetically anchored CAL proteins

---

## Aim 4

### Elucidate molecular pathways linking genetic susceptibility to coronary injury

建立：

```text
SNP
 ↓
pQTL
 ↓
Protein
 ↓
Pathway
 ↓
CAL
```

重点：

```text
immune
vascular
ECM
coagulation
```

---

## Aim 5

### Develop and validate a multi-omics risk prediction model for CAL

建立：

```text
Clinical
+
Protein
+
Genetic
```

最终得到：

> CAL risk score。

---

# 三十七、最终可以形成一张非常漂亮的主图

我建议最终论文 Figure 1 就画成：

```text
                     200 Kawasaki patients
                              │
               ┌──────────────┴──────────────┐
               │                             │
          Acute plasma                Recovery plasma
               │                             │
               └──────────────┬──────────────┘
                              │
                         DIA proteomics
                              │
                ┌─────────────┼─────────────┐
                ↓             ↓             ↓
            longitudinal    CAL         WGCNA
             proteins       proteins     modules
                │             │             │
                └─────────────┼─────────────┘
                              ↓
                     Candidate proteins
                              │
                              ↓
              ┌───────────────┼──────────────┐
              ↓               ↓              ↓
             WGS             pQTL          PRS
              │               │              │
              └───────────────┼──────────────┘
                              ↓
                     SNP → Protein → CAL
                              │
                              ↓
                         MR/Mediation
                              │
                              ↓
                   Mechanistic candidates
                              │
                              ↓
              Clinical + Protein + Genetic
                              │
                              ↓
                    CAL prediction model
```

---

# 三十八、我认为你这个项目最有价值的“核心创新点”

如果按照上述路线做，我认为不是简单的：

> “发现几个 KD 蛋白 biomarker”。

而是可以形成下面 **4 个创新层次**：

### 创新 1：纵向

**Acute → Recovery longitudinal proteome**

识别真正与疾病活动及恢复相关的动态蛋白。

### 创新 2：CAL 分层

不是简单：

```text
CAL vs no-CAL
```

而是：

```text
CAL0 → CAL1 → CAL2 → CAL3
```

建立连续 severity-associated protein signature。

### 创新 3：遗传锚定

```text
WGS
 ↓
pQTL
 ↓
protein
 ↓
CAL
```

把“相关性 biomarker”提升为“genetically anchored biomarker”。

### 创新 4：临床转化

```text
Clinical
+
Protein
+
Genetic risk
```

建立真正可以用于早期 CAL 风险分层的模型。

---

# 三十九、最重要的一点：200 人怎么分配？

我**不建议**简单：

```text
100 discovery
100 validation
```

因为每组如果再分 4 个 CAL 类别，统计效能会迅速下降。

更合理的是：

### 第一阶段：全部 200 人

做：

```text
QC
paired analysis
CAL association
WGCNA
pQTL
candidate discovery
```

### 第二阶段：内部验证

使用：

```text
nested 5-fold CV
bootstrap
repeated CV
```

而不是简单随机拆半。

### 第三阶段：真正独立验证

如果经费允许，**额外招募 50–100 例 KD 患儿作为 prospective validation cohort**，然后用：

```text
ELISA / targeted MS / PRM
```

验证 5–15 个核心蛋白。

这一点非常重要。已有 KD 蛋白组研究也采用了 discovery → independent ELISA validation 的设计，例如 SERPINE1 的研究就是 DIA-MS 后再用独立队列验证。([科学直通车][4])

---

# 四十、我建议最终把蛋白分成这三个“产品”

最后不要输出一张巨大的 DEG 表，而是形成三个核心 signature：

### Signature A

**KD acute activity signature**

回答：

> 谁处于更强的疾病炎症状态？

### Signature B

**CAL severity signature**

回答：

> 谁更容易出现严重冠脉损伤？

### Signature C

**Persistent CAL / recovery signature**

回答：

> 谁虽然进入恢复期，但仍然存在异常的 vascular/immune state？

然后：

```text
A + B + C
       ↓
Genetic anchoring
       ↓
CAL prediction
```

这会比单纯“DIA 找差异蛋白 + WGS 找 SNP”完整很多。

---

## 最终推荐的统计分析主线

可以直接按这个顺序实施：

```text
01  DIA QC
      ↓
02  PCA / clustering / batch QC
      ↓
03  Acute vs Recovery paired analysis
      ↓
04  4-level CAL analysis
      ↓
05  CAL severity trend analysis
      ↓
06  Time × CAL interaction
      ↓
07  Longitudinal trajectory clustering
      ↓
08  WGCNA protein modules
      ↓
09  GO / KEGG / Reactome / GSEA
      ↓
10  Clinical-protein correlation
      ↓
11  Candidate protein selection
      ↓
12  WGS QC + annotation
      ↓
13  KD/CAL candidate loci
      ↓
14  PRS
      ↓
15  cis/trans-pQTL
      ↓
16  SNP → Protein → CAL
      ↓
17  MR
      ↓
18  Mediation
      ↓
19  Clinical model
      ↓
20  Clinical + Protein model
      ↓
21  Clinical + Protein + Genetic model
      ↓
22  Nested CV / bootstrap
      ↓
23  Independent ELISA/PRM validation
      ↓
24  Final mechanistic network
```

**如果你的目标是发一篇高水平医学/转化医学文章，我最建议把文章的核心故事定成：**

> **“Longitudinal proteogenomic profiling identifies genetically anchored plasma protein signatures associated with coronary artery lesion progression and recovery in Kawasaki disease.”**

而不是传统的：

> “Proteomic analysis identifies differentially expressed proteins in Kawasaki disease.”

前者能够把你现有的 **200例、双时间点 DIA、4级 CAL、WGS** 四个优势全部利用起来。现有 KD 文献已经证明蛋白组可以发现 CAL 相关候选物，但真正把**纵向变化 + CAL severity + WGS/pQTL + 风险预测**系统整合起来，研究空间明显更大。([Taylor & Francis Online][8])

如果你接下来要真正开始做，我建议下一步直接把这个方案进一步落成一份**“从原始 DIA 矩阵 + WGS VCF 到最终论文 Figure 1–8 的完整分析 SOP”**，包括每一步的 **R/Python 软件、统计模型、FDR 阈值、WGCNA 参数、pQTL/PRS 方法、MR、机器学习模型、交叉验证方式以及最终需要输出的表格和图**。

[1]: https://pubmed.ncbi.nlm.nih.gov/41736230/?utm_source=chatgpt.com "Multiomics approaches in Kawasaki disease: insights into pathogenesis and emerging directions for diagnosis and treatment - PubMed"
[2]: https://pmc.ncbi.nlm.nih.gov/articles/PMC7994355/?utm_source=chatgpt.com "Identification of novel locus associated with coronary artery aneurysms and validation of loci for susceptibility to Kawasaki disease - PMC"
[3]: https://pmc.ncbi.nlm.nih.gov/articles/PMC12963946/?utm_source=chatgpt.com "Multiomics approaches in Kawasaki disease: insights into pathogenesis and emerging directions for diagnosis and treatment - PMC"
[4]: https://www.sciencedirect.com/science/article/pii/S1567576924012190?utm_source=chatgpt.com "Plasma proteomic profiling reveals that SERPINE1 is a potential biomarker associated with coronary artery lesions in Kawasaki disease - ScienceDirect"
[5]: https://pubmed.ncbi.nlm.nih.gov/30267110/?utm_source=chatgpt.com "Identification of the TIFAB Gene as a Susceptibility Locus for Coronary Artery Aneurysm in Patients with Kawasaki Disease - PubMed"
[6]: https://pubmed.ncbi.nlm.nih.gov/29098351/?utm_source=chatgpt.com "The Roles of Genetic Factors in Kawasaki Disease: A Systematic Review and Meta-analysis of Genetic Association Studies."
[7]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11361772/?utm_source=chatgpt.com "Sex-Specific Susceptibility Loci Associated With Coronary Artery Aneurysms in Patients With Kawasaki Disease - PMC"
[8]: https://www.tandfonline.com/doi/full/10.1080/1744666X.2024.2383236?utm_source=chatgpt.com "Proteomics approach for biomarker discovery in Kawasaki disease: Expert Review of Clinical Immunology: Vol 20 , No 12 - Get Access"

