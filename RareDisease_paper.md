
可以。你这个问题如果从**“罕见病大型队列 + 基因组学/表型 + 新基因发现/罕见变异 burden/诊断/多组学”**这个方向来找，近四年（约 **2022.9–2026.9**）其实有一批非常值得系统阅读的文章。

我先给你做一个**尽可能全面的第一轮清单**，重点放在 **Nature / Nature Genetics / Nature Medicine / Nature Communications / AJHG / Genetics in Medicine** 等高水平期刊，而且优先选择真正使用了**数千到数万甚至十万级罕见病或相关基因组数据**的研究。

---

# 一、最值得优先看的 TOP 15

如果你只想先读最重要的一批，我建议下面这 15 篇。

| 年份       | 文章                                                                                                                                                                      | 期刊                        |                                 队列规模 | 核心内容                                                      | 推荐度   |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | -----------------------------------: | --------------------------------------------------------- | ----- |
| **2025** | **Rare disease gene association discovery in the 100,000 Genomes Project**                                                                                              | **Nature**                |     34,851 families / 72,690 genomes | rare variant gene burden、Mendelian disease gene discovery | ⭐⭐⭐⭐⭐ |
| **2023** | **Genetic association analysis of 77,539 genomes reveals rare disease etiologies**                                                                                      | **Nature Medicine**       |                               77,539 | Rareservoir + BeviMed + HPO + gene discovery              | ⭐⭐⭐⭐⭐ |
| **2024** | **Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses**                                                                                     | **Nature Medicine**       |   6,004 families / 9,645 individuals | 跨欧洲罕见病大队列系统 reanalysis                                    | ⭐⭐⭐⭐⭐ |
| **2024** | **Rare coding variant analysis for human diseases across biobanks and ancestries**                                                                                      | **Nature Genetics**       |                              748,879 | 多 biobank + rare coding burden + 多祖源                      | ⭐⭐⭐⭐⭐ |
| **2024** | **Pangenome graphs improve the analysis of structural variants in rare genetic diseases**                                                                               | **Nature Communications** |                  GA4K 574 assemblies | HiFi + pangenome + SV                                     | ⭐⭐⭐⭐⭐ |
| **2025** | **GREGoR: accelerating genomics for rare diseases**                                                                                                                     | **Nature**                |                         GREGoR 多中心资源 | 美国大型罕见病基因组研究体系                                            | ⭐⭐⭐⭐⭐ |
| **2026** | **Automated reanalysis of genomic data for rare disease diagnostics at scale**                                                                                          | **Nature Medicine**       |                        4,735 + 1,089 | Talos 自动 reanalysis                                       | ⭐⭐⭐⭐⭐ |
| **2023** | **Integrated multi-omics for rapid rare disease diagnosis on a national scale**                                                                                         | **Nature Medicine**       |                         290 families | WGS + RNA + long-read + proteomics                        | ⭐⭐⭐⭐⭐ |
| **2023** | **Direct haplotype-resolved 5-base HiFi sequencing for genome-wide profiling of hypermethylation outliers in a rare disease cohort**                                    | **Nature Communications** |                    ~1,000 families背景 | PacBio HiFi + methylation + rare disease                  | ⭐⭐⭐⭐⭐ |
| **2025** | **Long read sequencing enhances pathogenic and novel variation discovery in patients with rare diseases**                                                               | **Nature Communications** | 76 positive controls + 51 unresolved | long-read clinical genomics                               | ⭐⭐⭐⭐  |
| **2024** | **Complex trait associations in rare diseases and impacts on Mendelian variant interpretation**                                                                         | **Nature Communications** |                       3,059 probands | rare disease + PGS + penetrance                           | ⭐⭐⭐⭐  |
| **2024** | **Genetic modifiers of rare variants in monogenic developmental disorder loci**                                                                                         | **Nature Genetics**       |                           UK Biobank | rare variant × common variant modifier                    | ⭐⭐⭐⭐⭐ |
| **2024** | **Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings** | **Nature Genetics**       |      5,652 enrolled / 1,577 analyzed | phenotype + sequencing + MDT                              | ⭐⭐⭐⭐  |
| **2024** | **Examining the role of common variants in rare neurodevelopmental conditions**                                                                                         | **Nature**                |                DDD 7,955 + GEL 3,618 | common variant architecture + rare NDD                    | ⭐⭐⭐⭐⭐ |
| **2025** | **Joint, multifaceted genomic analysis enables diagnosis of diverse, ultra-rare monogenic presentations**                                                               | **Nature Communications** |                             UDN 等多队列 | 跨 cohort 联合分析                                             | ⭐⭐⭐⭐  |

下面逐篇展开。

---

# 二、100,000 Genomes Project：目前最值得研究的罕见病大型队列

## 1. Rare disease gene association discovery in the 100,000 Genomes Project

**Nature, 2025**

这是我认为你应该**重点精读**的一篇。

[Nature 原文](https://www.nature.com/articles/s41586-025-08623-w?utm_source=chatgpt.com)

核心规模：

* 34,851 个 probands/families
* 72,690 genomes
* 226 个 rare diseases
* 4,643,230 个 rare candidate variants

最重要的是它不是简单做 variant filtering，而是建立了：

> **gene-based rare variant burden framework → Mendelian disease association**

最终：

* 165 个已知 disease-gene associations
* **141 个新的潜在 disease-gene associations**
* FDR 0.5%
* 69 个进入重点人工/临床 triage

而且文章把方法整理成了 **geneBurdenRD**。

这篇对你以后做：

**Singleton-WES/WGS → rare variant burden → disease gene discovery**

非常有参考价值。([Nature][1])

---

# 三、77,539 genomes：Rareservoir + BeviMed

## 2. Genetic association analysis of 77,539 genomes reveals rare disease etiologies

**Nature Medicine, 2023**

[Nature Medicine 原文](https://www.nature.com/articles/s41591-023-02211-z?utm_source=chatgpt.com)

这是 100KGP 非常经典的一篇。

规模：

* **77,539 individuals**
* 29,741 probands
* 269 disease classes
* 11.9 million rare exonic/splicing variants

方法：

```text
100KGP
   ↓
Rare variants
   ↓
Rareservoir
   ↓
HPO / disease classification
   ↓
BeviMed
   ↓
gene–disease association
   ↓
experimental / pedigree validation
```

最终：

* 241 已知 association
* **19 个新的 association**

特别值得注意的是，它解决了一个非常重要的问题：

> 大型罕见病队列中，如何把 genotype + phenotype 有效组织起来进行统计关联？

所以如果你想做**大型 singleton-WES rare disease burden**，这篇和 2025 Nature 基本应该连着看。([Nature][2])

---

# 四、Solve-RD：欧洲大型罕见病队列

## 3. Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses

**Nature Medicine, 2025**

[Nature Medicine 原文](https://www.nature.com/articles/s41591-024-03420-w?utm_source=chatgpt.com)

这篇非常重要。

规模：

* **6,004 families**
* 9,645 individuals
* 6,447 affected individuals
* 37 expert centers
* 12 个欧洲国家 + Canada
* > 300 collaborators

数据：

```text
WES
WGS
Pedigree
HPO
Clinical information
        ↓
Systematic reanalysis
        ↓
SNV/Indel
CNV/SV
other variant types
        ↓
Expert review
```

结果：

* 506 families 获得新的 genetic diagnosis
* 552 disease-causing variants
* 67 个涉及新 disease genes
* 187 个来自 ClinVar 新证据
* 210 个来自专家重新分类
* 总体 ad hoc + systematic diagnostic yield 达 **12.6%**

尤其值得关注的是：

> **15.9% 的诊断来自非 SNV/short indel。**

也就是：

* CNV
* SV
* 复杂变异
* 其他非标准 variant

这对你做 **SV / repeat / long-read** 特别有意义。([Nature][3])

---

# 五、GA4K：PacBio HiFi 罕见病队列

这个方向与你目前的 long-read 研究非常接近。

## 4. Pangenome graphs improve the analysis of structural variants in rare genetic diseases

**Nature Communications, 2024**

[Nature Communications 原文](https://www.nature.com/articles/s41467-024-44980-2?utm_source=chatgpt.com)

GA4K：

* **574 assemblies**
* 287 parent-offspring trios
* PacBio HiFi
* 平均约 27×

核心：

```text
PacBio HiFi
     ↓
personal assemblies
     ↓
SV discovery
     ↓
94 public assemblies
     ↓
pangenome graph
     ↓
unified SV callset
     ↓
rare/pathogenic SV prioritization
```

尤其值得注意：

> 这个研究直接把 **rare disease + PacBio HiFi + pangenome + SV** 结合起来。

如果你未来考虑：

**Singleton-WGS / HiFi → SV → rare disease association**

这篇非常值得作为方法学参考。([Nature][4])

---

# 六、GA4K：>1000 pediatric rare disease genomes

## 5. Genomic answers for children: Dynamic analyses of >1000 pediatric rare disease genomes

**Genetics in Medicine, 2022**

严格来说它刚好在“近四年”边缘，但**强烈建议保留**。

规模：

* 960 families
* > 1,000 genomes
* pediatric rare disease

用了：

* WES
* short-read WGS
* PacBio HiFi
* SNV
* SV
* repeat variants
* machine learning
* phenotype
* pedigree

最值得注意的结果：

> HiFi-GS 对 rare coding SV 的发现率比 short-read GS 高 **4 倍以上**。

而 SV 为此前未诊断病例增加了最高约 **13%** 的诊断。([科学直通车][5])

---

# 七、GA4K：rare disease + polygenic background

## 6. Complex trait associations in rare diseases and impacts on Mendelian variant interpretation

**Nature Communications, 2024**

[Nature Communications 原文](https://www.nature.com/articles/s41467-024-52407-1?utm_source=chatgpt.com)

规模：

* **3,059 GA4K probands**
* 1,102 PGS

核心思想非常值得关注：

```text
Rare pathogenic/VUS
        +
Polygenic background
        ↓
Phenotypic severity
        ↓
Penetrance / expressivity
```

发现：

* rare disease patients 的 common-disease polygenic liability 并不是随机的
* PGS 可以解释一部分 variable penetrance
* rare variants + polygenic background 可能共同影响 phenotype

这其实非常接近未来罕见病研究的一个重要趋势：

> **从“单个致病变异”转向“rare variant + common variant background”。**

([Nature][6])

---

# 八、DDD + 100KGP：rare disease 中 common variant 的作用

## 7. Examining the role of common variants in rare neurodevelopmental conditions

**Nature, 2024**

[Nature 原文](https://www.nature.com/articles/s41586-024-08217-y?utm_source=chatgpt.com)

用了：

* DDD：**7,955 patients**
* Genomics England：**3,618 patients**

研究 rare neurodevelopmental disorders 中：

> common variants 是否也参与疾病风险？

非常值得注意，因为传统 Mendelian disease analysis 通常默认：

```text
pathogenic variant
        ↓
disease
```

而这篇实际上在推动：

```text
rare pathogenic variant
        +
common polygenic background
        ↓
disease liability
```

这个方向和 2024 Nature Genetics 的 genetic modifiers 那篇可以配套阅读。([Nature][7])

---

# 九、Nature Genetics：rare variant × common variant modifier

## 8. Genetic modifiers of rare variants in monogenic developmental disorder loci

**Nature Genetics, 2024**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01710-0?utm_source=chatgpt.com)

这是非常值得做方法参考的一篇。

研究：

* UK Biobank
* 599 dominant developmental-disorder genes
* 2–5 个 rare damaging variants 的 cumulative burden
* EA polygenic score

主要结论：

> rare variant burden 与 common polygenic score 可以共同决定 phenotype severity。

也就是说：

**Mendelian disease ≠ 完全由一个变异决定。**

而可能是：

```text
Rare pathogenic variant
        ↓
baseline risk

+

Common variants / PGS
        ↓
modifier

=

实际 phenotype
```

([Nature][8])

---

# 十、748,879 人：跨 biobank + 多祖源 rare variant burden

## 9. Rare coding variant analysis for human diseases across biobanks and ancestries

**Nature Genetics, 2024**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01894-5?utm_source=chatgpt.com)

这个规模非常大：

> **748,879 individuals**

包括：

* All of Us
* 多个大型 biobank
* 155,236 非 European ancestry

研究：

* 601 diseases
* gene-based rare variant tests
* 363 significant associations

这个对于你如果考虑：

> **rare variant burden analysis + 多人群**

非常值得学习。

尤其是文章强调：

**pan-ancestry burden testing** 可以提高 inclusive discovery，但 ancestry-specific sensitivity analysis 仍然非常重要。([Nature][9])

---

# 十一、gnomAD：80万人的罕见病 penetrance

## 10. Exploring penetrance of clinically relevant variants in over 800,000 humans from the Genome Aggregation Database

**Nature Communications, 2025**

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-61698-x?utm_source=chatgpt.com)

规模：

> **807,162 individuals**

研究：

* ClinVar pathogenic variants
* 734 predicted LoF variants
* 77 severe early-onset haploinsufficient disease genes

最有意思的是：

734 个看起来应该导致疾病的 LoF variants 中：

> **701/734（95%）可以找到解释其“表型缺失”的原因。**

这篇非常适合研究：

* penetrance
* phenotypic expansion
* incidental pathogenic variants
* variant interpretation
* healthy carriers

([Nature][10])

---

# 十二、GREGoR：美国大型罕见病研究体系

## 11. GREGoR: accelerating genomics for rare diseases

**Nature, 2025**

[Nature 原文](https://www.nature.com/articles/s41586-025-09613-8?utm_source=chatgpt.com)

GREGoR 本身不是单篇“发现一个 gene”的研究，而是一个非常重要的**大型罕见病研究基础设施**。

它整合：

* WES
* WGS
* long-read WGS
* RNA-seq
* family structure
* phenotype
* genetic findings

目前数据持续更新，已经有多个 release。([GREGoR Consortium][11])

如果你想找：

> **未来可以用于 rare variant / gene discovery / variant prioritization 的大型公共队列**

GREGoR 非常值得关注。

---

# 十三、Nature Medicine 2026：自动化 reanalysis

## 12. Automated reanalysis of genomic data for rare disease diagnostics at scale

**Nature Medicine, 2026**

这是目前非常新的，而且我认为**方法学价值极高**的一篇。

[Nature Medicine 原文](https://www.nature.com/articles/s41591-026-04477-5?utm_source=chatgpt.com)

工具：

> **Talos**

数据：

* 1,089 individuals 用于 validation
* **4,735 previously undiagnosed individuals** 用于实际应用

结果：

> 新增 241 diagnoses，额外 diagnostic yield **5.1%**

其中：

* 32%：new gene–disease relationships
* 22%：new variant-level evidence
* 45%：improved analysis strategies

这个研究说明未来罕见病诊断非常可能变成：

```text
第一次 WES/WGS
       ↓
initial diagnosis
       ↓
每月/定期自动 reanalysis
       ↓
新 gene
新 ClinVar evidence
新 phenotype
新 annotation
新 variant caller
       ↓
不断提高 diagnostic yield
```

([Nature][12])

---

# 十四、全国规模多组学罕见病诊断

## 13. Integrated multi-omics for rapid rare disease diagnosis on a national scale

**Nature Medicine, 2023**

[Nature Medicine 原文](https://www.nature.com/articles/s41591-023-02401-9?utm_source=chatgpt.com)

澳大利亚 Acute Care Genomics：

* **290 families**
* critically ill infants/children

整合：

```text
WGS
 ↓
RNA-seq
 ↓
Long-read
 ↓
enzyme assays
 ↓
proteomics
 ↓
diagnosis
```

结果：

* 初始 WGS diagnostic yield：47%
* 多组学后：**54%**
* 额外发现 19 diagnoses
* 120 patients 的 clinical management 被改变

这篇是**rare disease + multi-omics clinical diagnosis**非常好的代表。([Nature][13])

---

# 十五、PacBio HiFi + methylation：非常值得你关注

## 14. Direct haplotype-resolved 5-base HiFi sequencing for genome-wide profiling of hypermethylation outliers in a rare disease cohort

**Nature Communications, 2023**

[Nature Communications 原文](https://www.nature.com/articles/s41467-023-38782-1?utm_source=chatgpt.com)

这个和你现在做 PacBio/long-read 的技术路线非常接近。

GA4K 约 **1,000 families** 的背景数据。

核心：

> PacBio HiFi 不仅用于 SNV/SV，还可以同时研究 methylation。

文章报告：

* HiFi-GS
* haplotype
* methylation
* rare disease
* hypermethylation outlier

特别适合你如果考虑：

```text
HiFi
 ↓
SNV
SV
CNV
repeat
methylation
phasing
 ↓
rare disease mechanism
```

([Nature][14])

---

# 十六、Long-read clinical rare disease

## 15. Long read sequencing enhances pathogenic and novel variation discovery in patients with rare diseases

**Nature Communications, 2025**

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-57695-9?utm_source=chatgpt.com)

核心：

* 76 positive controls
* 57 methylation-positive samples
* 51 previously short-read-negative patients

在 previously negative cases 中：

> long-read 又发现约 **10% additional diagnoses**。

检测范围：

* SNV
* SV
* methylation
* complex variation

所以这篇非常适合放在：

**short-read → long-read clinical genome sequencing**

这一条文献链里面。([Nature][15])

---

# 十七、超罕见病跨队列联合分析

## 16. Joint, multifaceted genomic analysis enables diagnosis of diverse, ultra-rare monogenic presentations

**Nature Communications, 2025**

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-61712-2?utm_source=chatgpt.com)

核心是：

> 把不同 rare disease cohorts 的数据联合起来。

包括 UDN 等。

这类研究特别重要，因为真正的 Mendelian disease 往往：

```text
某个疾病
↓
1–5 个病例
```

单个 cohort 根本没有统计 power。

因此未来越来越多的是：

```text
UK 100K
+
GA4K
+
GREGoR
+
UDN
+
DDD
+
国际 clinical cohorts
        ↓
cross-cohort analysis
        ↓
gene discovery
```

([Nature][16])

---

# 十八、超罕见病 + next-generation phenotyping

## 17. Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings

**Nature Genetics, 2024**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01836-1?utm_source=chatgpt.com)

德国 TRANSLATE NAMSE：

* 5,652 individuals enrolled
* 1,577 进入 exome analysis

重点：

**NGS + HPO + deep phenotyping + multidisciplinary team**

尤其适合研究：

> phenotype-driven gene discovery

([Nature][17])

---

# 十九、一些“不是典型罕见病 cohort，但非常值得纳入”的大型研究

如果你想把范围扩大到：

> **大型 population biobank 中研究 rare disease / rare variants / Mendelian phenotypes**

那么还有一大批。

---

## 18. Whole-genome sequencing of 490,640 UK Biobank participants

**Nature, 2025**

[Nature 原文](https://www.nature.com/articles/s41586-025-09272-9?utm_source=chatgpt.com)

规模：

> **490,640 WGS**

它不是专门的 rare disease cohort，但对于：

* rare variant frequency
* population controls
* penetrance
* rare disease variant filtering
* structural variants

非常重要。([Nature][18])

---

## 19. Whole-genome sequencing analysis identifies rare, large-effect noncoding variants and regulatory regions associated with circulating protein levels

**Nature Genetics, 2025**

规模：

> ~50,000 UK Biobank WGS

分析：

* 1.1 billion variants
* 123 million aggregate tests
* 2,907 proteins

重点是：

> rare **noncoding variants**

所以对于罕见病未来的：

```text
coding
+
noncoding
+
regulatory
```

分析非常值得参考。([Nature][19])

---

## 20. The impact of rare protein coding genetic variation on adult cognitive function

**Nature Genetics, 2023**

规模：

> **485,930 individuals**

虽然不是罕见病患者队列，但使用 rare coding variants 研究人类表型。

发现 8 个重要 genes。

对于：

> rare coding variant burden → phenotype

这个统计框架值得学习。([Nature][20])

---

# 二十、All of Us：未来很重要的 rare disease population resource

## 21. Genomic data in the All of Us Research Program

**Nature, 2024**

[Nature 原文](https://www.nature.com/articles/s41586-023-06957-x?utm_source=chatgpt.com)

All of Us 的优势不是单纯样本量，而是：

```text
Genomics
+
EHR
+
Phenotype
+
Survey
+
Diverse ancestry
```

尤其适合：

> phenotype-first / genotype-first rare disease research

而且它目前已经包括：

* 数十万 WGS
* structural variants
* long-read WGS
* EHR

所以未来几年 All of Us 很可能产生越来越多的 rare disease genotype-first 研究。([Nature][21])

---

# 二十一、另外几个值得放进你的文献库

### 22. Public platform with 39,472 exome control samples enables association studies without genotype sharing

**Nature Genetics, 2024**

这个不是患者 cohort，但对于：

> rare variant burden / case-control

非常有方法学意义。

它提供 **39,472 exome controls**，解决不同机构做 rare variant association 时 control 不足的问题。([Nature][22])

---

### 23. Pangenome graphs improve the analysis of structural variants in rare genetic diseases

**Nature Communications, 2024**

这个前面已经介绍，是：

> **GA4K + HiFi + pangenome + SV**

方向非常值得你重点参考。([Nature][4])

---

### 24. The expanding diagnostic toolbox for rare genetic diseases

**Nature Reviews Genetics, 2024**

这篇不是 cohort research，而是一篇很好的综述。

重点总结：

* WES
* WGS
* long-read
* OGM
* transcriptome
* epigenome
* proteome
* metabolome

([Nature][23])

如果你想搭建一个完整的 rare disease research framework，这篇适合作为综述入口。

---

# 二十二、把这些论文按“研究路线”重新分类

如果你的真正目的是找**可以模仿的大型罕见病队列研究模式**，我建议不要单纯按照期刊分类，而按照研究问题分类。

## A. 最大型：Rare disease gene discovery

### ★★★★★

**100KGP**

```text
77,539 genomes
      ↓
Rareservoir
      ↓
BeviMed
      ↓
gene-disease association
```

2023 Nature Medicine。([Nature][2])

↓

**100KGP**

```text
72,690 genomes
      ↓
rare variant filtering
      ↓
geneBurdenRD
      ↓
burden test
      ↓
141 new associations
```

2025 Nature。([Nature][1])

这两篇应该**一起读**。

---

# 二十三、B. Rare disease + long-read / SV

这是与你目前研究方向最接近的。

推荐顺序：

### ① GA4K >1000 genomes

**Genomic answers for children**
Genetics in Medicine 2022。([科学直通车][5])

↓

### ② GA4K HiFi + methylation

**Direct haplotype-resolved 5-base HiFi sequencing...**
Nature Communications 2023。([Nature][14])

↓

### ③ GA4K pangenome + SV

**Pangenome graphs improve the analysis of structural variants...**
Nature Communications 2024。([Nature][4])

↓

### ④ clinical long-read

**Long read sequencing enhances pathogenic and novel variation discovery...**
Nature Communications 2025。([Nature][15])

↓

### ⑤ clinical LR-WGS framework

**Toward clinical long-read genome sequencing for rare diseases**
Nature Genetics 2025。([Nature][24])

这一条线非常完整。

---

# 二十四、C. Rare disease + phenotype

重点：

**Solve-RD**

```text
6,004 families
      +
HPO
      +
pedigree
      +
WES/WGS
      ↓
systematic reanalysis
      ↓
506 diagnoses
```

Nature Medicine 2025。([Nature][3])

以及：

**TRANSLATE NAMSE**

```text
5,652 patients
      ↓
deep phenotyping
      +
WES
      ↓
ultra-rare disease diagnosis
```

Nature Genetics 2024。([Nature][17])

---

# 二十五、D. Rare variant burden

如果你真正想找的是：

> **“大型队列如何做 rare variant burden analysis？”**

我建议优先看：

### 第一梯队

1. **Rare disease gene association discovery in the 100,000 Genomes Project** — Nature 2025
2. **Genetic association analysis of 77,539 genomes...** — Nature Medicine 2023
3. **Rare coding variant analysis for human diseases across biobanks and ancestries** — Nature Genetics 2024
4. **Genetic modifiers of rare variants in monogenic developmental disorder loci** — Nature Genetics 2024
5. **The impact of rare protein coding genetic variation on adult cognitive function** — Nature Genetics 2023

---

# 二十六、E. Rare disease + penetrance / variable expressivity

这个方向近几年非常火。

推荐：

1. **Exploring penetrance of clinically relevant variants in over 800,000 humans from gnomAD** — Nature Communications 2025
2. **Complex trait associations in rare diseases and impacts on Mendelian variant interpretation** — Nature Communications 2024
3. **Genetic modifiers of rare variants in monogenic developmental disorder loci** — Nature Genetics 2024
4. **Examining the role of common variants in rare neurodevelopmental conditions** — Nature 2024

它们共同指向一个新的模型：

```text
Pathogenic rare variant
          │
          ├── common variant burden
          │
          ├── genetic modifiers
          │
          ├── environment
          │
          └── epigenetic state
                    ↓
              penetrance
                    ↓
               phenotype
```

这个方向我认为比传统的“找一个 pathogenic variant”更有研究空间。

---

# 二十七、F. Rare disease + 自动化诊断

近两年明显开始从：

> “能不能发现更多 variant？”

转向：

> **“如何让整个 rare disease cohort 自动不断 reanalyze？”**

核心文章：

### 2026 Nature Medicine

**Automated reanalysis of genomic data for rare disease diagnostics at scale**

Talos：

```text
Genome
  ↓
variant prioritization
  ↓
gene-disease evidence
  ↓
inheritance
  ↓
HPO
  ↓
clinical evidence
  ↓
automatic reanalysis
  ↓
new diagnosis
```

4,735 undiagnosed individuals → 241 new diagnoses。([Nature][12])

这个方向很可能是接下来几年非常重要的热点。

---

# 二十八、如果按“队列规模”排名

粗略按照规模，可以分成：

### 10万级

| Cohort      |          规模 | 主要用途                           |
| ----------- | ----------: | ------------------------------ |
| 100KGP      |     ~77,539 | rare disease gene discovery    |
| UK Biobank  |   ~500k WGS | rare variant / controls        |
| gnomAD      |       ~807k | variant frequency / penetrance |
| All of Us   |     数十万 WGS | diverse ancestry / phenotype   |
| 三大 biobanks | **748,879** | rare coding burden             |

### 1万级

| Cohort                       |                        规模 |
| ---------------------------- | ------------------------: |
| Solve-RD                     |                     9,645 |
| DDD                          |                     7,955 |
| 100KGP rare disease analysis |  34,851 probands/families |
| GREGoR                       |                   多中心持续扩展 |
| GA4K                         | >1,000 pediatric families |

### 千级

| Cohort              |             规模 |
| ------------------- | -------------: |
| GA4K                |         >1,000 |
| TRANSLATE NAMSE     | 5,652 enrolled |
| RGP                 |   688 families |
| UDN                 |             数百 |
| Acute Care Genomics |   290 families |

---

# 二十九、如果你是为了设计自己的“大型罕见病队列研究”，我最推荐借鉴的模式

实际上可以把这些文章总结成 **5 种“大队列罕见病研究范式”**：

### 模式 1：Phenotype → Gene

```text
Large RD cohort
      ↓
HPO
      ↓
rare variants
      ↓
gene burden
      ↓
new disease genes
```

代表：

**100KGP 2023/2025**

---

### 模式 2：Genome → SV → Disease

```text
HiFi WGS
   ↓
SNV
SV
CNV
STR
MEI
   ↓
pangenome
   ↓
rare/pathogenic variants
   ↓
diagnosis
```

代表：

**GA4K 2022–2025**

这个与你目前的 PacBio HiFi 工作**最接近**。

---

### 模式 3：Rare variant → Modifier → Phenotype

```text
rare pathogenic variant
          +
common variant PGS
          +
genetic modifiers
          ↓
penetrance
          ↓
clinical phenotype
```

代表：

**Nature 2024 + Nature Genetics 2024 + Nat Commun 2024**

---

### 模式 4：Multi-omics diagnosis

```text
WGS
 +
RNA-seq
 +
methylation
 +
proteomics
 +
metabolomics
       ↓
variant interpretation
       ↓
diagnosis
```

代表：

**Nature Medicine 2023**

---

### 模式 5：持续自动 reanalysis

```text
WES/WGS
   ↓
initial analysis
   ↓
database update
   ↓
new gene
new variant
new phenotype
   ↓
automatic reanalysis
   ↓
new diagnosis
```

代表：

**Nature Medicine 2026 Talos**

---

## 三十、如果结合你现在的研究方向，我会重点推荐这条文献链

你现在如果想做的是**大规模 Singleton-WES / WGS 罕见病致病变异发现**，而且希望以后进一步加入 SV、repeat、AI/phenotype，那么我建议优先建立下面这套文献框架：

```text
                   大型 Rare Disease Cohort
                             │
        ┌────────────────────┼────────────────────┐
        ↓                    ↓                    ↓
   100KGP                 GA4K                 Solve-RD
        │                    │                    │
        ↓                    ↓                    ↓
  Rare burden            HiFi / SV             HPO
        │                    │                    │
        ↓                    ↓                    ↓
 Gene discovery         Pangenome              Reanalysis
        │                    │                    │
        └──────────────┬─────┴────────────────────┘
                       ↓
                Multimodal diagnosis
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
     SNV/SV           STR/TE          Methylation
       ↓               ↓                ↓
       └───────────────┼────────────────┘
                       ↓
             Rare disease diagnosis
                       ↓
             Penetrance / modifiers
                       ↓
               Clinical phenotype
```

其中**最值得你精读的 8 篇**是：

1. **Nature 2025 — Rare disease gene association discovery in the 100,000 Genomes Project** ([Nature][1])
2. **Nature Medicine 2023 — Genetic association analysis of 77,539 genomes reveals rare disease etiologies** ([Nature][2])
3. **Nature Medicine 2025 — Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses** ([Nature][3])
4. **Nature Genetics 2024 — Rare coding variant analysis for human diseases across biobanks and ancestries** ([Nature][9])
5. **Nature Communications 2024 — Pangenome graphs improve the analysis of structural variants in rare genetic diseases** ([Nature][4])
6. **Nature 2024 — Examining the role of common variants in rare neurodevelopmental conditions** ([Nature][7])
7. **Nature Communications 2025 — Exploring penetrance of clinically relevant variants in over 800,000 humans from gnomAD** ([Nature][10])
8. **Nature Medicine 2026 — Automated reanalysis of genomic data for rare disease diagnostics at scale** ([Nature][12])

**尤其是第 1 + 2 篇**，非常适合你关注的 **rare variant burden / gene-level association**；**第 4 + 5 篇**适合 **large cohort + SV/long-read**；**第 6 + 7 篇**则是进一步研究 **penetrance / phenotype modifiers**。

如果你的目的是**“找可以直接照着设计自己文章分析框架的文献”**，下一步我可以进一步给你整理成一张更实用的 **“近四年罕见病大型队列 30–50 篇论文表”**，增加 **队列名称、样本量、WES/WGS/HiFi、singleton/trio、SNV/SV/CNV/STR、HPO、rare burden方法、统计模型、是否有新基因、是否有代码/数据、期刊和 DOI**，这样基本可以直接作为你做课题/写基金的 literature matrix。

[1]: https://www.nature.com/articles/s41586-025-08623-w?utm_source=chatgpt.com "Rare disease gene association discovery in the 100,000 Genomes Project | Nature"
[2]: https://www.nature.com/articles/s41591-023-02211-z?utm_source=chatgpt.com "Genetic association analysis of 77,539 genomes reveals rare disease etiologies | Nature Medicine"
[3]: https://www.nature.com/articles/s41591-024-03420-w?utm_source=chatgpt.com "Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses | Nature Medicine"
[4]: https://www.nature.com/articles/s41467-024-44980-2?utm_source=chatgpt.com "Pangenome graphs improve the analysis of structural variants in rare genetic diseases | Nature Communications"
[5]: https://www.sciencedirect.com/science/article/pii/S1098360022006530?utm_source=chatgpt.com "Genomic answers for children: Dynamic analyses of >1000 pediatric rare disease genomes - ScienceDirect"
[6]: https://www.nature.com/articles/s41467-024-52407-1?utm_source=chatgpt.com "Complex trait associations in rare diseases and impacts on Mendelian variant interpretation | Nature Communications"
[7]: https://www.nature.com/articles/s41586-024-08217-y?utm_source=chatgpt.com "Examining the role of common variants in rare neurodevelopmental conditions | Nature"
[8]: https://www.nature.com/articles/s41588-024-01710-0?utm_source=chatgpt.com "Genetic modifiers of rare variants in monogenic developmental disorder loci | Nature Genetics"
[9]: https://www.nature.com/articles/s41588-024-01894-5?utm_source=chatgpt.com "Rare coding variant analysis for human diseases across biobanks and ancestries | Nature Genetics"
[10]: https://www.nature.com/articles/s41467-025-61698-x?utm_source=chatgpt.com "Exploring penetrance of clinically relevant variants in over 800,000 humans from the Genome Aggregation Database | Nature Communications"
[11]: https://gregorconsortium.org/data?utm_source=chatgpt.com "GREGoR Data for the Scientific Community | GREGoR Consortium"
[12]: https://www.nature.com/articles/s41591-026-04477-5?utm_source=chatgpt.com "Automated reanalysis of genomic data for rare disease diagnostics at scale | Nature Medicine"
[13]: https://www.nature.com/articles/s41591-023-02401-9?utm_source=chatgpt.com "Integrated multi-omics for rapid rare disease diagnosis on a national scale | Nature Medicine"
[14]: https://www.nature.com/articles/s41467-023-38782-1?utm_source=chatgpt.com "Direct haplotype-resolved 5-base HiFi sequencing for genome-wide profiling of hypermethylation outliers in a rare disease cohort | Nature Communications"
[15]: https://www.nature.com/articles/s41467-025-57695-9?utm_source=chatgpt.com "Long read sequencing enhances pathogenic and novel variation discovery in patients with rare diseases | Nature Communications"
[16]: https://www.nature.com/articles/s41467-025-61712-2?utm_source=chatgpt.com "Joint, multifaceted genomic analysis enables diagnosis of diverse, ultra-rare monogenic presentations | Nature Communications"
[17]: https://www.nature.com/articles/s41588-024-01836-1?utm_source=chatgpt.com "Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings | Nature Genetics"
[18]: https://www.nature.com/articles/s41586-025-09272-9?utm_source=chatgpt.com "Whole-genome sequencing of 490,640 UK Biobank participants | Nature"
[19]: https://www.nature.com/articles/s41588-025-02095-4?utm_source=chatgpt.com "Whole-genome sequencing analysis identifies rare, large-effect noncoding variants and regulatory regions associated with circulating protein levels | Nature Genetics"
[20]: https://www.nature.com/articles/s41588-023-01398-8?utm_source=chatgpt.com "The impact of rare protein coding genetic variation on adult cognitive function | Nature Genetics"
[21]: https://www.nature.com/articles/s41586-023-06957-x?utm_source=chatgpt.com "Genomic data in the All of Us Research Program | Nature"
[22]: https://www.nature.com/articles/s41588-023-01637-y?utm_source=chatgpt.com "Public platform with 39,472 exome control samples enables association studies without genotype sharing | Nature Genetics"
[23]: https://www.nature.com/articles/s41576-023-00683-w?utm_source=chatgpt.com "The expanding diagnostic toolbox for rare genetic diseases | Nature Reviews Genetics"
[24]: https://www.nature.com/articles/s41588-025-02160-y?utm_source=chatgpt.com "Toward clinical long-read genome sequencing for rare diseases | Nature Genetics"

