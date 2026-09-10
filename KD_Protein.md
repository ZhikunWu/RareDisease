# 查找近三年发表的关于川崎病蛋白质组学研究得到文章

可以。按**近三年（2023年9月—2026年9月）**检索，目前川崎病（KD）蛋白质组学文献数量并不算多，但其中有几篇与你现在的**DIA血浆蛋白组 + 急性/恢复期 + CAL分层**研究非常直接相关。

我建议重点关注下面 **7篇**。

### 1. 最值得重点参考：SERPINE1与CAL

**Wang N, et al.**
**Plasma proteomic profiling reveals that SERPINE1 is a potential biomarker associated with coronary artery lesions in Kawasaki disease.**
*International Immunopharmacology*, 2024;139:112698.
DOI: 10.1016/j.intimp.2024.112698. ([PubMed][1])

这是目前与你研究设计**最接近的一篇**。

* 样本：健康对照、KD、KD-CAL、KD-nCAL
* 技术：**DIA-MS plasma proteomics**
* 设计：包括**IVIG前后**
* CAL相关分析
* 后续采用独立队列 **ELISA验证**
* 还进行了 **CAWS小鼠模型 + HCAEC细胞实验**
* 关键蛋白：**SERPINE1（PAI-1）**
* CAL组SERPINE1升高
* ROC：**AUC = 0.824**
* IVIG后，nCAL组SERPINE1下降，而CAL组仍维持较高水平。 ([PubMed][1])

**对你的启发非常大：**

> 急性期蛋白 → IVIG后变化 → CAL持续异常

正好可以对应你现在的：

> **acute DIA → recovery DIA → Δprotein → CAL**

所以这篇建议作为你论文/基金中的**直接文献依据**。

---

### 2. 2025：HRG与CAL

**Ohnishi Y, et al.**
**Quantitative serum proteomics to identify candidate biomarkers of coronary artery lesions in Kawasaki disease.**
*Pediatrics International*, 2025;67:e70164.
DOI: 10.1111/ped.70164. ([PubMed][2])

研究来自日本山口大学。

设计非常值得你注意：

* 从299例KD患者中选取
* **5例CAL + 5例non-CAL**
* LC/MS serum proteomics
* 找到6个CAL相关蛋白
* 其中 **HRG（histidine-rich glycoprotein）**差异最显著
* CAL患者HRG水平降低

作者认为：

> **HRG可能成为KD-CAL预测生物标志物。** ([PubMed][2])

不过它最大的局限是发现队列非常小（5 vs 5）。

所以如果你的样本量约200例，那么你的研究在**统计功效和分层能力上明显更有优势**。

---

### 3. 2026：BST1 + 4D-DIA + CAL

**Zhang S, et al.**
**Identification of Serum BST1 as a Biomarker to Predict Coronary Artery Lesions in Children with Kawasaki Disease Based on 4D-DIA Quantitative Proteomics.**
*Journal of Inflammation Research*, 2026;19:570480.
DOI: 10.2147/JIR.S570480. ([PubMed][3])

这篇与你的技术路线也**非常接近**。

### Discovery cohort

* KD-CAL：8
* KD-nCAL：7
* Healthy control：4
* **4D-DIA quantitative proteomics**
* 共鉴定 **2575 proteins**
* CAL vs nCAL得到 **213 DEPs**

其中：

> **BST1在CAL组中特异性升高。**

随后：

### Validation cohort

* KD-CAL：35
* KD-nCAL：61
* HC：30
* ELISA验证

BST1：

* CAL组升高
* 与冠脉 **Z-score正相关**
* ROC **AUC = 0.9052**

富集到：

* innate immune response
* cell adhesion
* complement and coagulation cascades
* extracellular exosome
* cellular metabolism。 ([PubMed][3])

**这篇对你的项目尤其重要。**

因为它说明：

> **DIA蛋白组 → CAL差异蛋白 → 通路 → 单蛋白验证 → Z-score/ROC**

已经成为目前KD-CAL蛋白组学研究比较典型的路线。

---

### 4. 2025：KD-CAL外泌体蛋白组

**Plasma Exosomal-Derived SERPINA1 and GNAI2 Downregulation as Potential Diagnostic Biomarkers of Kawasaki Disease with Coronary Artery Aneurysms.**
*International Journal of Molecular Sciences*, 2025. ([PubMed Central (PMC)][4])

这篇比较有意思，因为不是普通血浆蛋白，而是：

> **plasma exosomal proteomics**

设计包括：

* HC
* KD without CAA
* KD + small/medium CAA
* KD + giant CAA
* febrile controls

发现：

* KD患者共有 **104 DEPs**
* CAA特异性DEPs：**91**
* 这些蛋白主要涉及：

  * NETs
  * complement
  * platelet activation
* 小/中型CAA：102 DEPs
* giant CAA：34 DEPs
* 重点候选：

  * **SERPINA1**
  * **GNAI2**

而且随着CAA严重程度，还可以看到不同的蛋白表达/通路模式。 ([PubMed Central (PMC)][4])

这篇与你现在想做的：

> **normal → dilation → aneurysm**

这种**CAL严重程度梯度分析**特别值得参考。

---

### 5. 2025：KD急性期 vs 恢复期蛋白组

**Proteomic insights into molecular alterations associated with Kawasaki disease in children.**
2025. ([PubMed][5])

这个研究与你的**急性期/恢复期配对DIA**思路尤其接近。

样本：

| 组别                         |  n |
| -------------------------- | -: |
| Acute KD                   | 20 |
| Febrile bacterial controls | 20 |
| Recovered KD               |  8 |

蛋白组结果：

* Acute KD vs control：

  * 92 proteins ↑
  * 101 proteins ↓
* Recovered KD：

  * 537 proteins ↑
  * 231 proteins ↓
* 有 **56 proteins**在acute/recovery之间呈现相反变化模式。

重点通路：

* **Complement and coagulation cascades**
* AMPK
* PI3K-Akt

候选蛋白：

* **C3**
* **C6**
* **A1AT/SERPINA1**。 ([PubMed][5])

这篇对你非常有参考价值，因为你现在的设计可以进一步做成：

> **paired acute–recovery longitudinal proteomics**

而不仅仅是：

> acute KD vs control。

---

### 6. 2025：KD shock syndrome蛋白组

**Up-regulated vitronectin in Kawasaki disease shock syndrome serves as a potential biomarker.**
*Translational Pediatrics*, 2025. ([转化医学小儿科][6])

研究：

> KD vs Kawasaki disease shock syndrome（KDSS）

采用：

> **TMT-based plasma proteomics**

共分析455个血浆蛋白：

* 58 ↑
* 52 ↓
* 13个重点DEPs
* **Vitronectin (VTN)**得到验证

主要涉及：

* cell activation signaling
* inflammatory cascades
* endopeptidase activity
* endothelial barrier dysfunction。 ([转化医学小儿科][6])

这篇更适合参考KD**重症/炎症表型**，对CAL不是最直接。

---

### 7. 2025：牙龈沟液蛋白组

**Fan X, et al.**
**New biomarkers of Kawasaki disease identified by gingival crevicular fluid proteomics.**
*Frontiers in Molecular Biosciences*, 2025;12:1597412. ([PubMed][7])

样本：

* KD：27
* HC：18

方法：

> **DIA quantitative proteomics + MRM-MS**

发现：

> 197 DEPs

其中：

* 174 ↑
* 23 ↓

MRM验证12个蛋白：

**IFIT3、UB2L6、HP、A1AT、HSP90AA1、HNRPC、HSP90AB1、SAA1、MX1、B2M、FKBP4、TRAP1**

主要富集：

* NOD-like receptor signaling
* ER protein processing
* influenza pathway

这篇更偏向**KD诊断标志物**，而不是CAL预测。 ([PubMed][7])

---

# 按你的研究方向排序

如果你的目的是给目前的**“KD急性期/恢复期DIA + CAL分组 + WGS + 多模态机器学习”**项目找文献，我建议优先级如下：

| 优先级   | 研究                       | 技术                 | 主要目标               | 对你价值           |
| ----- | ------------------------ | ------------------ | ------------------ | -------------- |
| ⭐⭐⭐⭐⭐ | Wang 2024                | **DIA-MS**         | CAL                | **最接近**        |
| ⭐⭐⭐⭐⭐ | Zhang 2026               | **4D-DIA**         | CAL                | **非常接近**       |
| ⭐⭐⭐⭐⭐ | Ohnishi 2025             | LC/MS              | CAL                | CAL蛋白标志物       |
| ⭐⭐⭐⭐⭐ | 2025 Proteomic insights  | Proteomics         | **acute/recovery** | **非常适合你的纵向设计** |
| ⭐⭐⭐⭐  | 2025 Exosomal proteomics | Exosome proteomics | CAA严重程度            | 适合CAL梯度        |
| ⭐⭐⭐   | 2025 KDSS                | TMT                | 重症/KDSS            | 重症机制           |
| ⭐⭐⭐   | 2025 GCF                 | DIA + MRM          | KD诊断               | 诊断方向           |

---

# 最值得你重点关注的蛋白

把近三年这些研究放在一起，可以看到一些比较有意思的重复出现的生物学轴：

### ① Complement / coagulation

反复出现：

**C3、C6、SERPINA1/A1AT、SERPINE1**

这与你之前发现的 **Complement and coagulation cascades** 非常吻合。2025年的血清蛋白组研究也直接观察到了急性→恢复期的补体/凝血级联变化。 ([PubMed Central (PMC)][8])

---

### ② Endothelial injury / vascular remodeling

重点：

**SERPINE1 → coagulation/fibrinolysis/endothelial dysfunction**

**BST1 → inflammation/cell adhesion**

**HRG → CAL**

**VTN → endothelial barrier**

因此可以形成一个比较漂亮的KD-CAL机制框架：

> **Systemic inflammation → complement activation → endothelial injury → coagulation/platelet activation → vascular remodeling → CAL**

---

### ③ Neutrophil / NETs

尤其是CAA外泌体蛋白组：

> CAA-specific proteins → **NET formation + complement**

这与你现在如果考虑：

> neutrophil / lymphocyte + proteomics + CAL

会非常契合。 ([PubMed Central (PMC)][4])

---

# 对你目前200例DIA数据，我认为真正有价值的创新点

目前文献最大的共同问题其实很明显：

**很多发现队列非常小。**

例如：

* SERPINE1研究虽然有多队列验证，但蛋白组发现仍属于候选筛选模式；
* HRG研究发现队列只有 **5 CAL vs 5 non-CAL**；
* BST1研究发现队列只有 **8 CAL + 7 nCAL + 4 HC**。 ([PubMed][2])

所以如果你有约**200例KD患者，每个人都有急性期 + 恢复期DIA**，你的优势不是简单地“再找几个DEPs”，而应该明确做成：

> **Large longitudinal KD proteomic cohort**

进一步做：

**① Acute vs Recovery**

↓

**② CAL normal / dilation / aneurysm**

↓

**③ Time × CAL interaction**

↓

**④ CAL severity trend**

↓

**⑤ WGCNA / protein modules**

↓

**⑥ pQTL / WGS**

↓

**⑦ Protein–genotype integration**

↓

**⑧ multimodal ML**

↓

**⑨ CAL prediction + early intervention decision**

这会比单纯重复“DIA找SERPINE1/BST1/HRG”明显更有创新性。

另外，你前面关注的 **SERPINA3、SERPINC1、ENPP2、SERPINE1** 其实可以进一步组成一个很有意思的**炎症–凝血–内皮–脂质信号轴**，这比单独做DEG/DEP列表更适合作为你后续文章的机制主线。

如果你愿意，我下一步可以继续给你整理一张 **“2023–2026年川崎病蛋白质组学全部核心文章（约15–20篇）”**，按 **年份、期刊、样本量、样本类型、DIA/TMT/iTRAQ、KD/CAL分组、蛋白数量、核心蛋白、验证方式、AUC、主要通路、与你200例DIA研究的可借鉴点** 做成一张完整表格。

[1]: https://pubmed.ncbi.nlm.nih.gov/39029232/?utm_source=chatgpt.com "Plasma proteomic profiling reveals that SERPINE1 is a potential biomarker associated with coronary artery lesions in Kawasaki disease - PubMed"
[2]: https://pubmed.ncbi.nlm.nih.gov/40851226/?utm_source=chatgpt.com "Quantitative serum proteomics to identify candidate biomarkers of coronary artery lesions in Kawasaki disease - PubMed"
[3]: https://pubmed.ncbi.nlm.nih.gov/41877833/?utm_source=chatgpt.com "Identification of Serum BST1 as a Biomarker to Predict Coronary Artery Lesions in Children with Kawasaki Disease Based on 4D-DIA Quantitative Proteomics."
[4]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11942354/?utm_source=chatgpt.com "Plasma Exosomal-Derived SERPINA1 and GNAI2 Downregulation as Potential Diagnostic Biomarkers of Kawasaki Disease with Coronary Artery Aneurysms - PMC"
[5]: https://pubmed.ncbi.nlm.nih.gov/39984993/?utm_source=chatgpt.com "Proteomic insights into molecular alterations associated with Kawasaki disease in children."
[6]: https://tp.amegroups.org/article/view/139820/html?utm_source=chatgpt.com "Up-regulated vitronectin in Kawasaki disease shock syndrome serves as a potential biomarker - Wei - Translational Pediatrics"
[7]: https://pubmed.ncbi.nlm.nih.gov/40421421/?utm_source=chatgpt.com "New biomarkers of Kawasaki disease identified by gingival crevicular fluid proteomics."
[8]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11846444/?utm_source=chatgpt.com "Proteomic insights into molecular alterations associated with Kawasaki disease in children - PMC"



---

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

---

# SERPINA3和SERPINC1基因有什么功能，他们有什么关系


**SERPINA3 和 SERPINC1 是两个非常值得放在一起看的基因**：它们都属于 **SERPIN（serine protease inhibitor）家族**，都是分泌到血浆中的丝氨酸蛋白酶抑制剂，但**主要靶点和生物学功能完全不同**。

如果结合你现在做的 **KD/CAL + DIA proteomics**，这两个蛋白放在一起尤其值得关注，因为它们分别连接了 **炎症/中性粒细胞蛋白酶** 和 **凝血系统**。

---

## 1. 两个基因分别是什么？

| 基因           | 蛋白                  | 常用名称                  | 主要功能                  |
| ------------ | ------------------- | --------------------- | --------------------- |
| **SERPINA3** | α1-antichymotrypsin | AACT / ACT            | 炎症、急性期反应、抑制中性粒细胞蛋白酶   |
| **SERPINC1** | Antithrombin        | AT / Antithrombin III | 抗凝血、抑制 thrombin、FXa 等 |

两者都是 SERPIN 家族成员，但属于不同的 SERPIN clade：SERPINA3 属于 **clade A**，SERPINC1 属于 **clade C**。([PubMed Central (PMC)][1])

---

# 2. SERPINA3：主要是“炎症蛋白酶抑制”

### SERPINA3 → α1-antichymotrypsin

它是一种**急性期蛋白（acute-phase protein）**，在炎症状态下通常升高。

它可以抑制：

* **Cathepsin G**
* **Chymotrypsin**
* mast cell chymase 等

尤其值得注意的是 **neutrophil cathepsin G**。([PubMed][2])

可以简单画成：

```text
感染 / 炎症
     ↓
中性粒细胞活化
     ↓
Cathepsin G ↑
     ↓
组织蛋白水解 / 炎症反应
     ↑
     │
 SERPINA3
     │
     └── 抑制 Cathepsin G
```

所以 SERPINA3 可以理解成：

> **炎症环境下，对中性粒细胞蛋白酶的一种“刹车”。**

不过 SERPINA3 并不是单纯的“抗炎蛋白”。它的作用具有明显的**疾病和组织环境依赖性**，目前其完整生理功能仍没有完全阐明。([PubMed][2])

---

# 3. SERPINC1：主要是“凝血系统的刹车”

SERPINC1 编码：

> **Antithrombin（抗凝血酶，AT，旧称 Antithrombin III）**

这是凝血系统非常核心的天然抗凝蛋白。

主要抑制：

* **Thrombin / FIIa**
* **Factor Xa**
* Factor IXa
* Factor XIa
* Factor XIIa

其中 thrombin 和 FXa 是最重要的。([国家生物技术信息中心][3])

所以：

```text
Coagulation cascade
       ↓
   Thrombin ↑
       ↓
     Fibrin
       ↓
     血栓
       
SERPINC1 / Antithrombin
       ↓
抑制 thrombin + FXa
       ↓
   抑制凝血
```

而且 **heparin / heparan sulfate 可以显著增强 Antithrombin 的抗凝活性**。([PubMed Central (PMC)][4])

因此 SERPINC1 可以简单理解为：

> **血液凝固系统的天然“刹车”。**

SERPINC1 缺陷会增加血栓形成风险，这是临床上非常经典的遗传性血栓倾向之一。([国家生物技术信息中心][3])

---

# 4. 那么 SERPINA3 和 SERPINC1 有什么关系？

这里最重要的一点：

### 它们不是直接的上下游基因。

也就是说，不应该简单理解成：

```text
SERPINA3 → SERPINC1
```

或者：

```text
SERPINC1 → SERPINA3
```

目前更合理的理解是：

> **它们是同一家族的不同成员，在不同的 protease systems 中发挥抑制作用，但共同参与“炎症—蛋白水解—凝血”的稳态调控。**

SERPIN 家族本身就是一个非常大的蛋白酶调控系统，参与：

* inflammation
* coagulation
* fibrinolysis
* complement
* extracellular proteolysis

等过程。([PubMed][5])

---

# 5. 从疾病机制上，它们可以形成一个很有意思的“炎症—凝血轴”

这个对于你的 **KD/CAL** 项目特别重要。

可以把它理解成：

```text
             Infection / inflammation
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
      Neutrophil activation   Endothelial injury
             ↓                   ↓
       Cathepsin G ↑          Tissue factor
             ↓                   ↓
       SERPINA3 ─┤          Coagulation
                                 ↓
                              Thrombin
                                 ↓
                         Fibrin / platelet
                                 ↓
                              Thrombosis
                                 ↑
                         SERPINC1 ─┤
```

所以：

### SERPINA3

主要反映：

> **炎症 / neutrophil protease regulation**

### SERPINC1

主要反映：

> **coagulation / anticoagulant regulation**

而二者之间的共同生物学背景就是：

> **炎症与凝血系统之间的 cross-talk。**

---

# 6. 这对你的 KD/CAL 研究特别有意思

因为 Kawasaki disease，尤其是 CAL，涉及：

```text
系统性炎症
      ↓
中性粒细胞活化
      ↓
血管内皮损伤
      ↓
血小板/凝血系统活化
      ↓
血管重塑
      ↓
Coronary artery lesions
```

所以如果你的 DIA 数据里面出现：

```text
SERPINA3 ↑
SERPINC1 ↓
```

或者：

```text
SERPINA3 ↑
SERPINC1 ↑
```

都值得进一步研究。

**不要仅仅看它们单独的 fold change。**

我反而建议你计算：

$$
\text{SERPINA3/SERPINC1}
$$

即：

$$
\log_2(\text{SERPINA3})-
\log_2(\text{SERPINC1})
$$

把它作为一个：

> **inflammation–coagulation balance phenotype**

来分析。

---

# 7. 如果你在你的数据里看到 SERPINA3/SERPINC1 ratio，很值得进一步做

比如：

| CAL组 | SERPINA3 | SERPINC1 | SERPINA3/SERPINC1 |
| ---- | -------: | -------: | ----------------: |
| CAL0 |        低 |        高 |                 低 |
| CAL1 |        ↑ |        高 |                 ↑ |
| CAL2 |       ↑↑ |        ↓ |                ↑↑ |
| CAL3 |      ↑↑↑ |       ↓↓ |               ↑↑↑ |

如果出现这种趋势：

$$
\text{SERPINA3}/\text{SERPINC1}
\uparrow
$$

随着 CAL severity 增加，那么它就可能代表：

> **炎症性蛋白酶活性调控相对于抗凝能力发生失衡。**

这比单独说：

> SERPINA3 是 CAL biomarker

会有更强的机制解释。

---

# 8. 更进一步：你可以把它和 pQTL / rQTL 接起来

这其实和你上一条问的 **protein ratio pQTL** 非常契合。

你可以构造：

$$
R=\log_2(SERPINA3)-\log_2(SERPINC1)
$$

然后：

### Step 1

分别做：

```text
SNP → SERPINA3
SNP → SERPINC1
```

### Step 2

再做：

```text
SNP → SERPINA3/SERPINC1
```

也就是：

```text
                   SNP
                /       \
               ↓         ↓
          SERPINA3    SERPINC1
               \         /
                \       /
                 ↓     ↓
             SERPINA3/
              SERPINC1
                   ↓
                  CAL
```

如果发现某个 SNP：

```text
SERPINA3 ↑
SERPINC1 ↓
```

同时：

```text
SERPINA3/SERPINC1 ↑
```

并且：

```text
SERPINA3/SERPINC1 ↑ → CAL severity ↑
```

那么就可以进一步形成一个非常漂亮的：

> **genetic → proteomic balance → coronary lesion**

机制链。

---

## 9. 但有一个需要特别注意的地方

**SERPINA3/SERPINC1 ratio 并不能直接等同于“炎症/凝血活性比值”。**

因为蛋白浓度 ≠ 蛋白实际 enzymatic activity。

例如 SERPINC1 的抗凝功能还受到：

* heparin/heparan sulfate
* thrombin
* FXa
* protein modification
* consumption

等因素影响。([PubMed Central (PMC)][4])

因此论文里更严谨的表述应该是：

> **SERPINA3/SERPINC1 protein ratio reflects the relative circulating abundance of an inflammation-associated protease inhibitor and a major anticoagulant protein**

而不要直接说：

> “SERPINA3/SERPINC1 ratio represents inflammation/coagulation activity.”

---

### 对你目前的 KD 项目，我会把这两个蛋白放进一个更大的 **“inflammation–proteolysis–coagulation–endothelial injury” module** 中，而不仅仅作为两个孤立蛋白分析。SERPINA3 和 SERPINC1 恰好位于这个轴的两个不同位置，具有很好的机制互补性。([PubMed][2])

[1]: https://pmc.ncbi.nlm.nih.gov/articles/PMC3880077/?utm_source=chatgpt.com "Update of the human and mouse SERPIN gene superfamily - PMC"
[2]: https://pubmed.ncbi.nlm.nih.gov/36672665/?utm_source=chatgpt.com "SERPINA3: Stimulator or Inhibitor of Pathological Changes - PubMed"
[3]: https://www.ncbi.nlm.nih.gov/books/NBK545295/?utm_source=chatgpt.com "Biochemistry, Antithrombin III - StatPearls - NCBI Bookshelf"
[4]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9110684/?utm_source=chatgpt.com "Anticoagulant SERPINs: Endogenous Regulators of Hemostasis and Thrombosis - PMC"
[5]: https://pubmed.ncbi.nlm.nih.gov/1452519/?utm_source=chatgpt.com "Structure and mechanism of action of serpins - PubMed"


