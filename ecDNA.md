
## Comparative analysis of methodologies for detecting extrachromosomal circular DNA

Study design
Assessment of analysis pipelines in eccDNA identification
Impact of sequencing depth on eccDNA identification
Computational resources consumed by different analysis pipelines
Impact of eccDNA enrichment steps on eccDNA identification
Detection efficiency of ecDNA by different experimental methods
EccDNA profiles showed heterogeneity across experimental methods

## tools
```
AmpliconArchitect
Circle-Map
Circle_finder
ECCsplorer
ecc_finder
eccDNA_RCA_nanopore
NanoCircle
CReSIL
```

## papers
```

[AmpliconArchitect](https://github.com/virajbdeshpande/AmpliconArchitect)

CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing
[CoRAL](https://github.com/AmpliconSuite/CoRAL)

CReSIL: accurate identification of extrachromosomal circular DNA from long-read sequences 
[cresil](https://github.com/visanuwan/cresil)

eccDNA-pipe: an integrated pipeline for identification, analysis and visualization of extrachromosomal circular DNA from high-throughput sequencing data 

FLED: a full-length eccDNA detector for long-reads sequencing data 

Fast and accurate resolution of ecDNA sequence using Cycle-Extractor

ECCFP A consecutive full pass-based bioinformatic analysis for eccDNA identification from long-read sequencing data

High-confidence structural predictions of extrachromosomal DNA with ecDNAInspector

Landscape of extrachromosomal DNA characteristics in high-grade serous ovarian cancer via long-read sequencing
```


# 利用三代测序数据检测ecDNA的工具有哪些，他们对应的文章是哪些？

可以。这里先明确一个很重要的概念：

> **“利用三代测序检测 ecDNA”目前实际上分成两条路线：**
>
> 1. **Long-read WGS（PacBio HiFi / ONT WGS）直接从肿瘤基因组中推断 ecDNA**——最适合你现在的 **Tumor PacBio HiFi BAM**。
> 2. **先富集/纯化 eccDNA，再进行 ONT/PacBio 测序**——更适合系统检测全基因组各种大小的 eccDNA，但不一定适合你现有的普通 Tumor WGS。

近年的benchmark已经把主要 long-read 方法归纳为 **CReSIL、NanoCircle、eccDNA_RCA_nanopore/Flec、ecc_finder**，另外针对肿瘤复杂ecDNA结构，又出现了 **CoRAL 和 Decoil**。([Nature][1])

---

# 一、目前真正值得关注的三代测序 ecDNA 工具

| 工具                             | 测序类型                      |   是否需要eccDNA富集 |                       主要用途 |  PacBio |   ONT | 我对肿瘤ecDNA的推荐 |
| ------------------------------ | ------------------------- | -------------: | -------------------------: | ------: | ----: | -----------: |
| **CReSIL**                     | Long-read                 |           可不需要 |             eccDNA检测+定位+结构 |       △ | **✓** |        ⭐⭐⭐⭐⭐ |
| **Decoil**                     | Long-read WGS             |        **不需要** |          **复杂ecDNA重构/去卷积** |       △ | **✓** |        ⭐⭐⭐⭐⭐ |
| **CoRAL**                      | Long-read                 |        不需要/可结合 |            **癌症ecDNA结构重构** |   **✓** | **✓** |        ⭐⭐⭐⭐⭐ |
| **ecc_finder**                 | Long-read                 |           通常需要 |                   eccDNA检测 |       △ | **✓** |         ⭐⭐⭐⭐ |
| **NanoCircle**                 | ONT long-read             |     Circle-Seq |                   eccDNA检测 |       × | **✓** |         ⭐⭐⭐⭐ |
| **Flec / eccDNA_RCA_nanopore** | ONT long-read             |          RCA富集 |                 完整eccDNA序列 |       × | **✓** |         ⭐⭐⭐⭐ |
| **ECCFP**                      | Long-read                 | 主要针对TGS eccDNA | full-pass eccDNA detection |       △ |     ✓ |          ⭐⭐⭐ |
| **ecDNAFinder**                | Long-read single-cell WGS |            不一定 |                   单细胞ecDNA | ✓/ONT相关 |     ✓ |          ⭐⭐⭐ |
| **CIDER-Seq/CIDER-Seq2**       | PacBio SMRT               |       eccDNA富集 |         full-length eccDNA |   **✓** |     × |          ⭐⭐⭐ |

需要特别注意：**eccDNA ≠ cancer ecDNA**。很多工具最初针对的是几十 bp–几十 kb 的一般eccDNA，而癌症ecDNA往往可以达到 **100 kb–Mb级别**，而且经常包含多个染色体来源片段。因此，对于你现在的肿瘤PacBio HiFi数据，不能简单把所有“eccDNA caller”都当成ecDNA caller。([PubMed Central (PMC)][2])

---

# 二、最推荐：CReSIL

## CReSIL

全称：

> **Construction-based Rolling-circle-amplification for eccDNA Sequence Identification and Location**

论文：

**Wanchai et al. CReSIL: accurate identification of extrachromosomal circular DNA from long-read sequences. Briefings in Bioinformatics. 2022.**

这是目前非常重要的long-read eccDNA工具。论文benchmark显示，在模拟数据中CReSIL的F1可以达到 **0.98**，并且能够处理long-read数据中的复杂eccDNA。([PubMed Central (PMC)][3])

### 它的核心思路

对于RCA产生的long read：

```text
eccDNA
   ↓
rolling circle amplification
   ↓
eccDNA-eccDNA-eccDNA-eccDNA
   ↓
ONT long read
   ↓
tandem repeat / breakpoint
   ↓
识别circle
   ↓
定位到reference
   ↓
重建eccDNA
```

现在的CReSIL已经增加了：

```text
identify
identify_wgls
annotate
visualize
```

其中尤其值得注意的是：

> **identify_wgls**

可以直接分析 **whole-genome long-read sequencing**。

官方文档明确提供了WGLS模式。([GitHub][4])

### 例如你的数据

```text
Tumor PacBio HiFi BAM
        ↓
FASTQ
        ↓
CReSIL
        ↓
candidate circular DNA
        ↓
eccDNA structure
```

但是：

**CReSIL更偏向“检测circle”，而不是专门解决复杂癌症ecDNA amplicon。**

---

# 三、对你现在的数据，我认为最重要的是 Decoil

## Decoil

论文：

> **Giurgiu et al. Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil. Genome Research. 2024.**

这篇和你的数据类型非常匹配。

Decoil的核心不是简单回答：

> “有没有circle？”

而是回答：

> **这个复杂的ecDNA到底由哪些片段组成？不同ecDNA结构如何共存？**

它使用：

```text
Long-read BAM
     +
SV
     +
coverage
     ↓
breakpoint graph
     ↓
LASSO regression
     ↓
ecDNA reconstruction
     ↓
deconvolution
```

它特别解决：

> **多个ecDNA共享相同genomic footprint的问题。**

也就是说，如果：

```text
ecDNA-A
chr7 → chr8 → chr7 → chr8

ecDNA-B
chr7 → chr8 → chr7
```

两个ecDNA高度重叠，普通方法很容易把它们混在一起。

Decoil就是专门针对这个问题。([PubMed Central (PMC)][5])

而且它可以：

> **直接输入 BAM**

或者：

```text
BAM
+
VCF(SV)
+
coverage
```

官方实现甚至可以从BAM内部计算SV、coverage和ecDNA reconstruction。([PubMed Central (PMC)][5])

### 对你的意义

你之前的流程：

```text
PacBio HiFi
      ↓
ClairS / DeepSomatic
      ↓
SNV/Indel

Severus
NanoMops
Sniffles
Savana
      ↓
Somatic SV
```

实际上可以非常自然地接：

```text
Somatic SV
    +
Tumor HiFi BAM
    +
coverage
    ↓
  Decoil
    ↓
ecDNA
```

这是我目前**最推荐你测试的方案之一**。

---

# 四、CoRAL——目前最适合“癌症ecDNA结构重建”的工具之一

## CoRAL

论文：

> **Zhu et al. CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing. Genome Research. 2024;34:1344–1354.** ([PubMed][6])

CoRAL的重点是：

> **利用long reads解决短读长无法phase的复杂ecDNA结构。**

尤其解决：

* breakpoint phasing
* internal duplications
* complex rearrangements
* ecDNA heterogeneity
* multiple structural paths

短读长AA：

```text
short reads
   ↓
breakpoint graph
   ↓
可能存在多个cycle
```

而CoRAL利用long reads：

```text
long reads
    ↓
breakpoint evidence
    ↓
long-range phasing
    ↓
ecDNA path
    ↓
完整结构
```

所以：

### 如果你的目标是：

> **“我的PacBio HiFi数据里面有没有ecDNA，以及ecDNA具体由哪些SV连接起来？”**

那么：

**CoRAL + Decoil**

应该放在第一梯队。

---

# 五、ecc_finder

论文：

> **Zhang et al. ecc_finder: A Robust and Accurate Tool for Detecting Extrachromosomal Circular DNA From Sequencing Data. Frontiers in Plant Science. 2021.**

它比较特别，因为同时支持：

* Illumina
* ONT long reads
* mapping
* de novo assembly

而且支持：

> **reference-free模式**

这是它相对于很多工具的优势。([PubMed Central (PMC)][7])

Long-read mapping模式主要利用：

```text
circular read
     ↓
same genomic region
重复映射
     ↓
tandem pattern
     ↓
eccDNA
```

不过对于**癌症Mb级复杂ecDNA**，我不会把ecc_finder作为唯一caller。

更适合：

> **辅助验证。**

---

# 六、NanoCircle

NanoCircle主要针对：

> **ONT long-read + Circle-Seq**

它利用：

* coverage
* soft-clipped reads
* breakpoint
* long-read alignment

来识别circle。

经典应用之一是：

> **Circular DNA in the human germline and its association with recombination**

Molecular Cell, 2022。该研究使用NanoCircle分析ONT long-read数据，并提供了NanoCircle和CReSIL workflow。([科学直通车][8])

但需要注意：

**NanoCircle更偏向eccDNA检测，而不是癌症复杂ecDNA结构重构。**

---

# 七、Flec / eccDNA_RCA_nanopore

这个工具非常有意思。

Flec：

> **Full-length eccDNA caller**

论文：

> **Purification, full-length sequencing and genomic origin mapping of eccDNA. Nature Protocols, 2023.**

它针对：

> **RCA + Nanopore**

数据。

原理：

```text
eccDNA
 ↓
RCA
 ↓
tandem copies
 ↓
ONT long read
 ↓
多个相同eccDNA串联
 ↓
consensus
 ↓
full-length eccDNA
```

所以特别适合：

> **“我想得到完整eccDNA sequence”**

而不是单纯：

> “我想在普通WGS里发现ecDNA”。

论文明确说明Flec通过RCA产物中的多个tandem eccDNA copies构建consensus sequence，再定位其genomic origin。([PubMed][9])

---

# 八、ECCFP——比较新的TGS方法

2026年还有一个比较新的：

> **ECCFP — Consecutive Full Pass-based eccDNA identification**

论文：

> **ECCFP: A consecutive full pass-based bioinformatic analysis for eccDNA identification from long-read sequencing data. iMetaOmics, 2026.**

它针对Flec/CReSIL/FLED的一些问题进行改进。

作者认为：

* Flec过度依赖single-read tandem repeats
* CReSIL可能过度估计eccDNA边界
* FLED阈值较严格

ECCFP通过：

> **consecutive full pass**

识别eccDNA。初步benchmark显示它在junction定位、检测数量和运行效率方面有优势。([Wiley Online Library][10])

不过如果你的重点是**肿瘤ecDNA**，目前我仍然会把：

**Decoil / CoRAL > ECCFP**

放在前面。

---

# 九、CIDER-Seq / CIDER-Seq2

这是PacBio路线必须知道的。

CIDER-Seq的思想是：

```text
eccDNA enrichment
       ↓
PacBio long-read
       ↓
full-length circular molecule
       ↓
consensus
       ↓
genomic mapping
```

早期工作是PacBio/SMRT平台的代表性方法。

后续ecc_finder论文也明确把：

> **CIDER-Seq2**

列为基于PacBio long reads的eccDNA分析方法。([PubMed Central (PMC)][7])

但是：

### 如果你现在已经有普通Tumor PacBio HiFi WGS

不要为了CIDER-Seq重新做实验。

因为它更适合：

> **专门提取eccDNA → PacBio测序**

而不是：

> **普通Tumor WGS → ecDNA calling**

---

# 十、一个非常重要的区别：普通PacBio HiFi WGS vs eccDNA-enriched long reads

这是你现在最需要注意的。

## 情况A：你现在的数据

```text
Tumor
 ↓
PacBio HiFi WGS
 ↓
普通DNA提取
 ↓
HiFi sequencing
```

那么推荐：

### 第一梯队

**CoRAL**

**Decoil**

**CReSIL WGLS**

然后：

**Sniffles2 / Severus / pbsv → SV evidence**

---

## 情况B：专门提取eccDNA

```text
Tumor DNA
 ↓
exonuclease / Circle-Seq / RCA
 ↓
eccDNA enrichment
 ↓
ONT/PacBio
```

那么：

### 推荐

**CReSIL**

**Flec**

**NanoCircle**

**ecc_finder**

**ECCFP**

这些方法会更合适。benchmark显示，在long-read eccDNA-enriched数据中，CReSIL表现尤其突出；2024年的系统比较中，CReSIL在long-read类别取得最高F1之一，而ecc_finder的assembly模式表现明显较弱。([Nature][1])

---

# 十一、如果你的目标明确是“肿瘤ecDNA”，我建议这样组合

对于你目前的**Tumor + Normal PacBio HiFi**，我不会只跑一个工具。

推荐：

```text
                Tumor PacBio HiFi
                       │
                       ▼
                 minimap2/pbmm2
                       │
                       ▼
                    Tumor BAM
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Sniffles2     Severus      pbsv
          │            │            │
          └────────────┼────────────┘
                       ▼
                Somatic SV
                       │
                       ▼
             Coverage / CNV
                       │
            ┌──────────┴──────────┐
            ▼                     ▼
         CoRAL                  Decoil
            │                     │
            └──────────┬──────────┘
                       ▼
                  ecDNA candidates
                       │
                       ▼
                    CReSIL
                       │
                       ▼
                 circular evidence
                       │
                       ▼
             consensus ecDNA set
                       │
          ┌────────────┼─────────────┐
          ▼            ▼             ▼
       oncogene      enhancer        TE
          │            │             │
          └────────────┼─────────────┘
                       ▼
                Hi-C / Pore-C
                       │
                       ▼
               ecDNA 3D structure
                       │
                       ▼
                Fiber-seq / RNA
```

---

# 十二、我最推荐的“3工具交叉验证”

如果你现在让我给你的**PacBio HiFi肿瘤数据**选择3个：

| 排名 | 工具         | 作用             | 为什么选                                 |
| -- | ---------- | -------------- | ------------------------------------ |
| 🥇 | **Decoil** | 复杂ecDNA重构      | 直接利用long-read + SV + coverage，特别适合肿瘤 |
| 🥈 | **CoRAL**  | ecDNA结构解析      | 专门针对long-read癌症ecDNA                 |
| 🥉 | **CReSIL** | circular DNA检测 | 可以分析WGLS，提供独立circle证据                |

然后采用：

> **至少2个工具支持 + SV breakpoint支持 + CN amplification支持**

定义高可信ecDNA。

例如：

```text
Candidate ecDNA
      │
      ├── CoRAL ✓
      ├── Decoil ✓
      ├── CReSIL ✓
      ├── CN amplification ✓
      ├── ≥2 circular junctions ✓
      └── long-read spanning junction ✓
                ↓
          High-confidence ecDNA
```

这样会比单独用AmpliconArchitect更有说服力。

---

# 十三、尤其注意：AmpliconArchitect不要因为“经典”就直接用于HiFi

**AmpliconArchitect (AA)** 是ecDNA研究中最经典的工具之一，但它本质上是：

> **short-read WGS → breakpoint graph → amplicon/circle reconstruction**

不是三代测序工具。([PubMed Central (PMC)][11])

不过它非常有价值，因为你可以做：

```text
PacBio HiFi
      │
      ├── CoRAL
      ├── Decoil
      └── CReSIL
            │
            ▼
         ecDNA
            ▲
            │
      AmpliconArchitect
      （独立证据）
```

甚至：

> **AA/AmpliconSuite + CoRAL + Decoil**

会形成很强的交叉验证体系。

---

## 十四、针对你现在的数据，我会这样选

如果你手里的是：

> **Tumor PacBio HiFi BAM + Normal PacBio HiFi BAM**

那么我建议优先级：

### 第一层：发现

**Sniffles2 / Severus / pbsv**

↓

### 第二层：ecDNA

**CoRAL + Decoil**

↓

### 第三层：独立验证

**CReSIL WGLS**

↓

### 第四层

**AmpliconArchitect / AmpliconSuite**

↓

### 第五层

**Hi-C / Pore-C**

验证：

> **ecDNA是否形成独立3D interaction domain / enhancer hub**

↓

### 第六层

你现在还有 **Fiber-seq** 的话，可以进一步做：

> **ecDNA上的chromatin accessibility / nucleosome / TF occupancy / FIRE**

这一步实际上很有创新空间。

---

### 一个关键结论

**如果你的目标是“从现有Tumor PacBio HiFi BAM中直接检测癌症ecDNA”，目前最值得优先尝试的不是NanoCircle/Flec，而是：**

> **Decoil + CoRAL + CReSIL(WGLS)**

因为前两者针对的是**癌症ecDNA复杂结构重建**，而CReSIL提供独立的circular-DNA证据；而NanoCircle/Flec/ecc_finder更偏向**eccDNA-enrichment + long-read**场景。2024年的系统benchmark也支持这种区分。([Nature][1])

如果你愿意，我下一步可以直接给你整理一套**“PacBio HiFi BAM → Somatic SV → CoRAL + Decoil + CReSIL → 高可信ecDNA”的完整可运行流程**，包括 **conda安装、输入BAM要求、每一步命令、参数、结果文件解释，以及最后如何画ecDNA circular structure图**。

[1]: https://www.nature.com/articles/s41467-024-53496-8?utm_source=chatgpt.com "Comparative analysis of methodologies for detecting extrachromosomal circular DNA | Nature Communications"
[2]: https://pmc.ncbi.nlm.nih.gov/articles/PMC13380329/?utm_source=chatgpt.com "Targeting extrachromosomal DNA in human cancers - PMC"
[3]: https://pmc.ncbi.nlm.nih.gov/articles/PMC10144670/?utm_source=chatgpt.com "CReSIL: accurate identification of extrachromosomal circular DNA from long-read sequences - PMC"
[4]: https://github.com/visanuwan/cresil?utm_source=chatgpt.com "GitHub - visanuwan/cresil: CReSIL: Accurate Identification of Extrachromosomal Circular DNA from Long-read Sequences · GitHub"
[5]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11529853/?utm_source=chatgpt.com "Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil - PMC"
[6]: https://pubmed.ncbi.nlm.nih.gov/38981681/?utm_source=chatgpt.com "CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing - PubMed"
[7]: https://pmc.ncbi.nlm.nih.gov/articles/PMC8672306/?utm_source=chatgpt.com "ecc_finder: A Robust and Accurate Tool for Detecting Extrachromosomal Circular DNA From Sequencing Data - PMC"
[8]: https://www.sciencedirect.com/science/article/pii/S1097276521010297?utm_source=chatgpt.com "Circular DNA in the human germline and its association with recombination - ScienceDirect"
[9]: https://pubmed.ncbi.nlm.nih.gov/36517607/?utm_source=chatgpt.com "Purification, full-length sequencing and genomic origin mapping of eccDNA - PubMed"
[10]: https://onlinelibrary.wiley.com/doi/10.1002/imo2.70080?utm_source=chatgpt.com "ECCFP: A consecutive full pass‐based bioinformatic analysis for eccDNA identification from long‐read sequencing data - Li - 2026 - iMetaOmics - Wiley Online Library"
[11]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9671848/?utm_source=chatgpt.com "Extrachromosomal DNA amplifications in cancer - PMC"


---

# 最近三年有做ecDNA检测方面benchmark的文章吗？具体有哪些高水平的文章

有，而且如果你把范围严格限定为**近3年（2024–2026）“ecDNA/eccDNA检测或结构重建方法的系统benchmark”**，其实数量并没有30篇那么多；真正称得上**高水平、直接比较多个方法**的文章主要集中在 **Nature Communications、Genome Research，以及 CoRAL 的高水平方法论文**。

我建议把它们分成两类理解：

* **A类：真正的“benchmark/方法比较”论文**——直接比较多个ecDNA/eccDNA caller。
* **B类：方法论文中包含系统benchmark**——虽然标题不是benchmark，但对多个方法、模拟数据和真实癌细胞进行了比较。

下面按这个标准整理。

---

# 1. 最重要的一篇：Nature Communications 2024

### ① Comparative analysis of methodologies for detecting extrachromosomal circular DNA

**Nature Communications, 2024, 15:9208**

这是目前近三年我认为**最标准、最完整的一篇ecDNA/eccDNA detection benchmark**。

[Nature Communications 原文](https://www.nature.com/articles/s41467-024-53496-8?utm_source=chatgpt.com)

它做了两层benchmark：

### 第一层：计算工具benchmark

比较了 **7个pipeline、11种运行模式**：

**Short-read：**

* Circle-Map
* Circle_finder
* ECCsplorer
* ecc_finder

**Long-read：**

* CReSIL
* eccDNA_RCA_nanopore
* NanoCircle
* ecc_finder

同时使用：

* 7组模拟数据
* 不同测序深度
* 不同比例chimeric reads
* F1-score
* sequence identity
* base-pair difference
* duplication rate
* CPU
* memory

进行评价。([Nature][1])

### 最重要的结果

在 **50× long-read**模拟数据：

| Long-read工具         |        F1 |
| ------------------- | --------: |
| **CReSIL**          | **0.918** |
| NanoCircle          |     0.905 |
| eccDNA_RCA_nanopore |     0.859 |
| ecc_finder asm-ont  |     0.179 |

因此作者的结论是：

> **CReSIL是long-read eccDNA detection中表现最好的方法之一，尤其在>10×覆盖度时。**

([Nature][1])

---

# 2. 这篇论文还有一个非常重要的实验benchmark

它不只是benchmark软件。

还比较了：

* WGS-SR
* WGS-LR
* Circle-Seq-SR
* Circle-Seq-LR
* 3SEP-SR
* 3SEP-LR
* ATAC-seq

共 **7种实验策略**，用了 **21个真实测序数据集**。([Nature][1])

尤其重要的是：

> **Circle-Seq + long-read 对 >10 kb、copy-number amplified eccDNA的检测效率明显较高。**

所以这篇论文实际上回答了两个问题：

```text
实验方法：
WGS / Circle-Seq / 3SEP / ATAC
             ↓
        哪种最好？

计算方法：
CReSIL / NanoCircle / ecc_finder / ...
             ↓
        哪种最好？
```

因此如果你要设计一个**ecDNA detection benchmark**，这篇是最应该参考的框架。

---

# 3. 第二篇：CoRAL，Genome Research

### ② CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing

**Genome Research, 2024**

这篇严格来说不是“所有ecDNA检测工具的大benchmark”，但它是**目前long-read癌症ecDNA结构重建benchmark中非常重要的一篇**。

[Genome Research / CoRAL 原文](https://genome.cshlp.org/content/34/9/1344?utm_source=chatgpt.com)

CoRAL：

> **Complete Reconstruction of Amplifications with Long reads**

同时支持：

* Oxford Nanopore
* PacBio

这是你目前**PacBio HiFi数据**特别值得关注的一篇。([PubMed Central (PMC)][2])

它进行了：

### 模拟数据benchmark

比较：

* CoRAL
* Decoil
* AmpliconArchitect
* de novo assembly相关方法

评价：

* breakpoint detection
* segment ordering
* cycle reconstruction
* copy-number explanation

结果显示：

> **CoRAL在复杂ecDNA结构的breakpoint检测和segment order inference方面优于Decoil和short-read AA。**

([PubMed Central (PMC)][2])

而且作者进一步在**10个ecDNA癌细胞系**中进行了验证，并使用AmpliconClassifier重新确认cyclic/ecDNA结构。([PubMed Central (PMC)][2])

### 对你特别重要

如果你的数据是：

> **Tumor PacBio HiFi WGS**

那么我会把这篇的优先级放到非常高。

因为CoRAL明确设计成：

> **PacBio/ONT long-read → ecDNA amplicon reconstruction**

而不是单纯eccDNA检测。

---

# 4. 第三篇：Decoil，Genome Research 2024

### ③ Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil

**Genome Research, 2024**

[Genome Research / Decoil 原文](https://genome.cshlp.org/content/34/9/1355?utm_source=chatgpt.com)

这篇也属于：

> **method + benchmark**

而且benchmark非常有价值。

Decoil作者建立了自己的：

> **ecDNA simulation benchmark dataset**

模拟：

* simple circularization
* multi-region
* multichromosomal
* nested duplication
* foldback
* complex rearrangement

等不同复杂度的ecDNA topology。([PubMed Central (PMC)][3])

然后比较：

* **Decoil**
* **CReSIL**
* **Shasta**

在不同复杂度和coverage下的表现。

### 结果

对于简单ecDNA topology：

> Decoil可以高保真重建。

对于复杂topology：

> 在超过1900个复杂模拟中，超过70%的模拟可以达到normalized largest contig >0.6。

而且总体上：

> **Decoil优于Shasta和CReSIL。**

([PubMed Central (PMC)][3])

这篇特别适合你，因为Decoil直接利用：

```text
BAM
+
SV
+
coverage
        ↓
breakpoint graph
        ↓
ecDNA reconstruction
```

所以和你目前的：

> **PacBio HiFi + SV calling**

非常匹配。

---

# 5. CReSIL本身：虽然不是近3年严格意义上的新benchmark，但非常重要

### ④ CReSIL: accurate identification of extrachromosomal circular DNA from long-read sequences

**Briefings in Bioinformatics, 2023**

[CReSIL全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC10144670/?utm_source=chatgpt.com)

这篇本身也是：

> **method + comparative benchmark**

作者比较了：

* CReSIL
* NanoCircle
* eccDNA_RCA_nanopore
* ecc_finder
* Flye

并在不同测序深度：

* 100×
* 50×
* 10×
* 5×
* 3×

下进行测试。([PubMed Central (PMC)][4])

特别值得注意：

> CReSIL在模拟数据中即使降到3×仍保持很高的F1，而其他工具随coverage下降明显。([PubMed Central (PMC)][4])

所以：

### 如果你要做long-read ecDNA benchmark：

**CReSIL一定应该作为baseline。**

---

# 6. 近3年真正值得放进“benchmark对比矩阵”的工具

综合这几篇论文，我建议你不要只看工具名称，而是分成两个问题。

## 第一类：eccDNA detection

| 工具                      | Long-read |    需要富集 | 主要用途                              |
| ----------------------- | --------: | ------: | --------------------------------- |
| **CReSIL**              |         ✓ |     否/可 | eccDNA detection + reconstruction |
| **NanoCircle**          |         ✓ |    通常需要 | simple/complex eccDNA             |
| **eccDNA_RCA_nanopore** |         ✓ |     RCA | circular DNA                      |
| **ecc_finder**          |         ✓ |    通常需要 | mapping/assembly                  |
| **Flec**                |         ✓ |     RCA | full-length eccDNA                |
| **ECCFP**               |         ✓ | 主要针对TGS | full-pass eccDNA                  |

其中2024 Nature Communications benchmark直接比较的是前四组。([Nature][1])

---

# 7. 第二类：癌症ecDNA结构重建

这个类别和普通eccDNA检测一定要分开。

| 工具                       | Long-read | 癌症ecDNA |  复杂结构 |        推荐 |
| ------------------------ | --------: | ------: | ----: | --------: |
| **CoRAL**                |         ✓ |       ✓ | ⭐⭐⭐⭐⭐ | **★★★★★** |
| **Decoil**               |         ✓ |       ✓ | ⭐⭐⭐⭐⭐ | **★★★★★** |
| **CReSIL**               |         ✓ |      部分 |   ⭐⭐⭐ |      ★★★★ |
| **AA/AmpliconArchitect** |         × |       ✓ |  ⭐⭐⭐⭐ |      ★★★★ |
| **AmpliconSuite**        |      ×/间接 |       ✓ |  ⭐⭐⭐⭐ |      ★★★★ |

所以不能简单说：

> “CReSIL是benchmark第一名，所以一定比CoRAL好。”

这是**错误的比较**。

因为它们解决的问题不完全一样。

---

# 8. 一个非常关键的区别

### Nature Communications 2024 benchmark测的是：

> **eccDNA detection**

而：

### CoRAL / Decoil benchmark测的是：

> **ecDNA structure reconstruction**

这两个概念不同。

例如：

```text
一个癌细胞：

chr7
  │
  ├── MYC
  ├── enhancer
  │
  └── SV
       │
       ▼
      ecDNA
       │
       ├── 500 kb
       ├── 1.2 Mb
       ├── multiple duplications
       ├── foldback
       └── multi-chromosomal
```

你真正想知道的可能不是：

> “有没有circle？”

而是：

> **这个ecDNA到底由哪些segment组成？**

那么应该使用：

**CoRAL / Decoil**

而不是只用CReSIL。

---

# 9. 近3年benchmark文章的重要性排序

如果你的目的就是：

> **“我要做一个新的ecDNA detection benchmark”**

我会这样排序：

### 🥇 第一名

**Comparative analysis of methodologies for detecting extrachromosomal circular DNA**

**Nature Communications, 2024**

这是最标准的：

> **eccDNA detection benchmark**

([Nature][1])

---

### 🥈 第二名

**CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing**

**Genome Research, 2024**

最重要的：

> **long-read cancer ecDNA reconstruction benchmark**

([PubMed Central (PMC)][2])

---

### 🥉 第三名

**Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil**

**Genome Research, 2024**

特别重要的是：

> **复杂ecDNA topology + mixture + long-read benchmark**

([PubMed Central (PMC)][3])

---

### 第4名

**CReSIL: accurate identification of extrachromosomal circular DNA from long-read sequences**

**Briefings in Bioinformatics, 2023**

这是理解：

> **long-read eccDNA caller如何benchmark**

最重要的基础方法论文之一。([PubMed Central (PMC)][4])

---

# 10. 但有一个很大的研究空白

这个其实对你非常重要。

2024 Nature Communications那篇benchmark自己就明确指出：

> 目前可用于**non-enriched WGS long-read**的eccDNA detection pipeline仍然比较有限，因此当时的benchmark主要针对**eccDNA-enriched long-read data**。([Nature][1])

这意味着：

## 目前还缺一个真正系统的：

> **PacBio HiFi/ONT WGS → cancer ecDNA detection benchmark**

尤其是：

```text
PacBio HiFi WGS
       vs
ONT WGS
       ↓
CoRAL
Decoil
CReSIL
AA/AmpliconSuite
SV callers
       ↓
统一ground truth
       ↓
Sensitivity
Specificity
F1
breakpoint accuracy
cycle accuracy
copy-number accuracy
length accuracy
```

这样的系统benchmark。

这对你目前的数据非常有意义。

---

# 11. 如果你准备做自己的benchmark，我建议直接按照下面这个框架

你现在如果是：

> **Tumor PacBio HiFi + Normal PacBio HiFi**

我建议不要简单重复2024 Nature Communications的工作，而是做：

### Benchmark 1：不同caller

```text
CoRAL
Decoil
CReSIL
AmpliconArchitect
AmpliconSuite
```

### Benchmark 2：不同SV输入

```text
pbsv
Sniffles2
cuteSV
Severus
Savana
```

### Benchmark 3：不同测序深度

```text
5×
10×
15×
20×
30×
40×
60×
```

### Benchmark 4：不同ecDNA复杂度

```text
Simple circle
       ↓
Single chromosome
       ↓
Multi-region
       ↓
Nested duplication
       ↓
Foldback
       ↓
Multi-chromosomal
       ↓
Multiple co-existing ecDNA
```

### Benchmark 5：不同ecDNA长度

```text
10 kb
50 kb
100 kb
500 kb
1 Mb
2 Mb
5 Mb
10 Mb
```

最后评价：

```text
                    ┌─ detection
                    ├─ breakpoint
                    ├─ cycle
                    ├─ structure
Ground truth ───────┼─ copy number
                    ├─ length
                    ├─ gene content
                    └─ enhancer content
```

---

## 12. 对你目前项目，我尤其推荐的组合

如果你准备拿自己的**PacBio HiFi肿瘤数据**做ecDNA研究，我会把这4篇作为核心benchmark文献：

| 文献                                           |   年份 | 期刊                          | 主要价值                                |
| -------------------------------------------- | ---: | --------------------------- | ----------------------------------- |
| **CReSIL**                                   | 2023 | Briefings in Bioinformatics | long-read eccDNA detection baseline |
| **Comparative analysis of methodologies...** | 2024 | **Nature Communications**   | **系统eccDNA detection benchmark**    |
| **Decoil**                                   | 2024 | **Genome Research**         | **复杂ecDNA结构benchmark**              |
| **CoRAL**                                    | 2024 | **Genome Research**         | **PacBio/ONT ecDNA结构重建benchmark**   |

其中**2024年这三篇放在一起看非常重要**：Nature Communications回答“**谁能检测到circle**”，Decoil回答“**复杂circle怎么重构**”，CoRAL进一步回答“**如何利用long reads提高复杂ecDNA结构的准确性**”。([Nature][1])

另外，截至我这次检索，**2025–2026年还没有发现一篇已经在主流高水平期刊正式发表、且覆盖CReSIL + CoRAL + Decoil + 其他新工具、并专门针对PacBio HiFi/ONT非富集WGS做全面ecDNA detection benchmark的论文**；2026年已经出现新的Cycle-Extractor预印本，并与CoRAL、Decoil、AA进行了比较，但目前仍是bioRxiv预印本，因此不建议把它和上述正式发表的文章等量齐观。([生物预印本][5])

**所以，如果你的目的是寻找一个可以进一步做文章的benchmark空白点，我认为“PacBio HiFi非富集WGS条件下的ecDNA detection + structure reconstruction系统benchmark”仍然有明显空间。**

[1]: https://www.nature.com/articles/s41467-024-53496-8?utm_source=chatgpt.com "Comparative analysis of methodologies for detecting extrachromosomal circular DNA | Nature Communications"
[2]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11529860/?utm_source=chatgpt.com "CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing - PMC"
[3]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11529853/?utm_source=chatgpt.com "Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil - PMC"
[4]: https://pmc.ncbi.nlm.nih.gov/articles/PMC10144670/?utm_source=chatgpt.com "CReSIL: accurate identification of extrachromosomal circular DNA from long-read sequences - PMC"
[5]: https://www.biorxiv.org/content/10.64898/2026.03.10.710955v1?utm_source=chatgpt.com "Fast and accurate resolution of ecDNA sequence using Cycle-Extractor | bioRxiv"



# 如果想做一个更新的关于ecDNA方面的benchmark研究，如何规划

如果你想做一个**比 2024 年 Nature Communications 那篇 eccDNA benchmark 更“新”、更容易冲高水平期刊的 ecDNA benchmark**，我建议不要再做简单的“几个软件跑一遍、比较 F1”了。

目前领域已经有两个重要基准：

* 2024 年 *Nature Communications* 做了 **7 个 pipeline + 7 种实验方法**的 eccDNA benchmark，但重点仍然是模拟 eccDNA、Circle-Seq/3SEP 等富集数据，长读长部分主要比较 CReSIL、eccDNA_RCA_nanopore、NanoCircle 等。作者自己也指出，**非富集 WGS-LR 的 pipeline 比较仍然不足**。([Nature][1])
* 2024 年 *Genome Research* 的 **CoRAL** 和 **Decoil** 已经把问题推进到“long-read ecDNA structure reconstruction”，而不只是 detection。CoRAL 支持 PacBio/ONT，Decoil 重点解决复杂 ecDNA 和共享 genomic footprint 的解卷积。([PubMed][2])

所以我认为现在最有价值的方向是：

> **从“谁能检测 ecDNA”升级到“什么数据条件下，什么算法能够可靠地检测、重建、定量和解析 ecDNA 的结构异质性”。**

---

# 一、我最推荐你的课题定位

可以把题目设计成：

### **Benchmarking long-read sequencing strategies and computational methods for comprehensive reconstruction of extrachromosomal DNA in cancer**

或者更有冲击力：

### **Systematic benchmarking of extrachromosomal DNA detection and structural reconstruction from long-read cancer genomes**

中文：

> **长读长测序条件下癌症 ecDNA 检测与结构重建方法的系统性基准研究**

这个题目比单纯：

> CReSIL vs CoRAL vs Decoil

强很多。

核心不是比较软件，而是建立一个：

**ecDNA benchmark framework**

---

# 二、真正值得做的创新点

我建议你把 benchmark 拆成 **5 个层次**。

```text
                         ecDNA Benchmark
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
   Detection              Structure              Quantification
       │                      │                      │
       │                      │                      │
  是否存在ecDNA          环结构是否正确          copy number
  breakpoint recall       segment order           abundance
  false positive          orientation             heterogeneity
       │                      │                      │
       └──────────────────────┼──────────────────────┘
                              │
                       Biological validity
                              │
                    oncogene / enhancer
                    expression / 3D contacts
                              │
                              ▼
                       Clinical relevance
```

这是和 2024 benchmark 最大的区别。

---

# 三、第一大创新：重点做 PacBio HiFi + ONT

这是我最建议你做的。

目前大量 ecDNA 方法实际上是：

```text
Illumina WGS
     ↓
AmpliconArchitect
     ↓
ecDNA
```

而 long-read 方面已经出现：

```text
CReSIL
Decoil
CoRAL
```

但还没有一个特别系统的：

> **PacBio HiFi vs ONT + 多种 depth + 多种 read length + 多种 ecDNA topology + 多种 tumor purity 的全面 benchmark。**

CoRAL 明确支持 PacBio 和 ONT，而 Decoil 的设计主要针对 ONT WGS。([PubMed][2])

这正好和你现在手上的 **PacBio HiFi tumor data** 非常匹配。

---

# 四、第二大创新：不要只模拟“简单圆环”

这是整个 benchmark 最关键的地方。

建议设计 **8–10 类 ecDNA topology**。

例如：

### Type 1：Simple circle

```text
A → B → C → A
```

---

### Type 2：Inverted circle

```text
A → B → C → B' → A
```

---

### Type 3：Tandem duplication

```text
A → B → C → B → C → A
```

---

### Type 4：Nested duplication

```text
A → B → C → B → D → A
```

---

### Type 5：Fold-back

```text
A → B → C → C' → B' → A
```

---

### Type 6：Chromosome-shattering derived ecDNA

例如：

```text
chr7: A
chr7: D
chr8: B
chr7: F
chr8: C
```

最终：

```text
A → B → D → C → F → A
```

---

### Type 7：Multiple co-existing ecDNA

这是非常重要的。

例如：

```text
ecDNA-1 = A → B → C → A

ecDNA-2 = A → D → E → A
```

两个 ecDNA 有：

```text
shared region = A
```

这正是 Decoil 特别想解决的问题。([Genome Research][3])

---

### Type 8：ecDNA + HSR

模拟：

```text
ecDNA
   ↓
chromosomal HSR
```

这是非常接近真实癌症基因组的。

---

### Type 9：ecDNA with SNV/SV

例如：

```text
ecDNA:
A -- SV1 -- B -- SV2 -- C
```

同时加入：

* SNV
* Indel
* SV
* CNV

测试 long-read 是否能够同时：

> phase SNV + SV + ecDNA

---

### Type 10：超复杂 ecDNA

最终可以做：

```text
5–20 genomic segments
+
inversion
+
duplication
+
foldback
+
multiple ecDNA
+
shared segments
```

这才是真正能拉开 benchmark 层次的地方。

---

# 五、第三大创新：建立“真实 ground truth”

这是我认为你这个项目最可能产生高水平文章的地方。

**不要只靠模拟。**

建议：

## Level 1：in silico

完全知道：

```text
True ecDNA structure
True breakpoint
True copy number
True abundance
```

用于：

* F1
* precision
* recall

---

# 六、Level 2：合成 ecDNA ground truth

这个比纯模拟强很多。

例如构建：

```text
10–50 个已知结构的 circular DNA
```

每一个：

```text
known sequence
known size
known breakpoint
known orientation
known copy number
```

然后混入 genomic DNA。

例如：

```text
genomic DNA       99%
ecDNA-A            1%
ecDNA-B            0.1%
ecDNA-C            0.01%
```

这样可以测试：

### detection limit

```text
50%
20%
10%
5%
1%
0.1%
0.01%
```

这是非常漂亮的一组 benchmark。

---

# 七、Level 3：真实 cell line

选择有：

> **已知 ecDNA structure**

的癌细胞系。

例如：

* MYC
* MYCN
* EGFR
* MDM2
* CDK4
* MET

相关 ecDNA。

然后用：

```text
PacBio HiFi
ONT
Illumina WGS
Hi-C
FISH
optical mapping
```

构建 multi-platform ground truth。

CoRAL 和 Decoil 已经利用了一些已表征 cell lines 做 benchmark，所以你需要在此基础上进一步扩大真实 ground truth 的类型和平台。([PubMed][2])

---

# 八、Level 4：真实 tumor

这是最终最有价值的。

建议：

```text
20–50 tumors
```

最好：

```text
Tumor PacBio HiFi
Tumor ONT
Tumor Illumina WGS
```

部分样本：

```text
Hi-C
RNA-seq
FISH
```

然后建立：

```text
Consensus ecDNA truth set
```

---

# 九、第四大创新：做 depth benchmark

这个非常适合你的 PacBio 数据。

例如：

```text
PacBio HiFi

60X
40X
30X
20X
15X
10X
5X
2X
1X
```

测试：

```text
CReSIL
CoRAL
Decoil
AmpliconArchitect
AmpliconSuite
```

然后回答：

> **PacBio HiFi 至少需要多少深度才能可靠检测 ecDNA？**

例如最终可能得到：

```text
                         Minimum depth
Detection                  10X
Breakpoint                15X
Structure                 20X
Complex structure         30X
Heterogeneity             40X
```

这会比单纯比较 F1 有意义得多。

---

# 十、第五大创新：Tumor purity benchmark

这个我非常建议加入。

模拟：

```text
100% tumor
75%
50%
25%
10%
5%
1%
```

例如：

```text
Tumor ecDNA
     ↓
Normal DNA
```

然后测试：

```text
ecDNA detection
breakpoint detection
copy number
structure reconstruction
```

最终回答：

> **ecDNA 在临床肿瘤样本中最低 tumor purity 到多少还能被 long-read 检测？**

这是非常实际的问题。

---

# 十一、一定要加入 read length

特别是 PacBio vs ONT。

例如：

### PacBio HiFi

```text
10 kb
15 kb
20 kb
25 kb
30 kb
```

### ONT

```text
10 kb
25 kb
50 kb
100 kb
200 kb
```

然后测试：

```text
read length
      ↓
breakpoint spanning
      ↓
structural reconstruction
```

这样就能回答：

> **复杂 ecDNA 结构到底需要多长的 reads？**

---

# 十二、真正值得做的一个指标：Structure Recovery Score

我建议不要只用 F1。

可以设计：

## 1. Breakpoint Recall

```text
BR = recovered true breakpoints / true breakpoints
```

---

## 2. Segment Recall

```text
SR = correctly reconstructed segments / true segments
```

---

## 3. Orientation Accuracy

```text
OA =
correct segment orientations /
total segment orientations
```

---

## 4. Order Accuracy

例如真实：

```text
A → B → C → D → A
```

预测：

```text
A → C → B → D → A
```

虽然 segment 都找到了，但：

```text
order ≠ correct
```

所以应该单独评分。

---

# 十三、最终可以定义一个 EcDNA Structural Recovery Score

例如：

$$
ESRS =
w_1 BR+
w_2 SR+
w_3 OA+
w_4 SOA+
w_5 CN_{accuracy}
$$

其中：

* BR = breakpoint recall
* SR = segment recall
* OA = orientation accuracy
* SOA = segment order accuracy
* CN = copy-number accuracy

这会比传统：

```text
F1 = 0.82
```

更加符合 ecDNA 的生物学问题。

---

# 十四、还应该加入“定量能力”

这个非常容易被忽略。

例如真实：

```text
ecDNA-A = 70%
ecDNA-B = 20%
ecDNA-C = 10%
```

算法预测：

```text
A = 68%
B = 23%
C = 9%
```

应该评估：

```text
abundance correlation
RMSE
MAE
R²
```

尤其是：

**Decoil 的优势之一就是对共享 genomic footprint 的 ecDNA elements 做 deconvolution。** ([Genome Research][3])

所以你可以专门做：

> **ecDNA isoform deconvolution benchmark**

这可能比普通 ecDNA detection 更有新意。

---

# 十五、算法组怎么选？

我建议至少：

| 类别          | 方法                        |
| ----------- | ------------------------- |
| Short-read  | AmpliconArchitect         |
| Short-read  | AmpliconSuite             |
| Long-read   | CReSIL                    |
| Long-read   | CoRAL                     |
| Long-read   | Decoil                    |
| Long-read   | eccDNA_RCA_nanopore       |
| Long-read   | NanoCircle                |
| Assembly    | Flye                      |
| Assembly    | Shasta                    |
| SV-assisted | SV + graph reconstruction |

但**不要为了数量堆软件**。

真正核心：

```text
CReSIL
CoRAL
Decoil
AA/AmpliconSuite
```

因为这四类方法代表：

```text
eccDNA detection
      ↓
long-read detection
      ↓
long-read reconstruction
      ↓
short-read amplicon reconstruction
```

CReSIL 是 WGS-LR eccDNA detection；CoRAL 和 Decoil 已经进入复杂结构重建层面。([Nature][1])

---

# 十六、你现在特别适合加入 Fiber-seq

这实际上可能成为你项目的一个**非常独特的卖点**。

你之前已经在做：

> Tumor PacBio HiFi + Fiber-seq + FIRE

那么可以把 benchmark 从：

```text
ecDNA sequence
```

升级成：

```text
ecDNA sequence
       ↓
ecDNA structure
       ↓
ecDNA chromatin
       ↓
ecDNA regulatory activity
```

例如：

```text
ecDNA
 │
 ├── sequence
 │
 ├── SV
 │
 ├── CN
 │
 ├── methylation
 │
 ├── chromatin accessibility
 │
 ├── nucleosome occupancy
 │
 └── FIRE
```

然后进一步测试：

> **正确重建 ecDNA 后，能不能正确解释 ecDNA 上的 enhancer/promoter/chromatin architecture？**

这就明显超出传统 benchmark。

---

# 十七、甚至可以加入 3D genome

2026 年已经出现 **ec3D**，专门从 Hi-C 重建 ecDNA 的三维结构，并且可以处理 duplicated segments 和 multi-way interactions。([Nature][4])

因此你可以设计：

```text
Long-read
   ↓
ecDNA sequence reconstruction
   ↓
Hi-C
   ↓
ec3D
   ↓
3D structure
   ↓
enhancer–oncogene interaction
```

最终形成：

> **Sequence → Structure → Chromatin → 3D → Function**

这个故事就非常完整了。

---

# 十八、我建议你的整体 benchmark 设计

可以直接画成：

```text
                         EC-DNA BENCHMARK
                               │
              ┌────────────────┴────────────────┐
              │                                 │
        DATA GENERATION                    REAL DATA
              │                                 │
     ┌────────┼────────┐               ┌────────┼────────┐
     │        │        │               │        │        │
   Sim      Spike-in  Cell lines      Tumor    Cellline  Hi-C
     │        │        │               │
     └────────┴────────┘               │
              │                        │
              ▼                        ▼
      PacBio HiFi / ONT         PacBio HiFi / ONT
              │                        │
              └──────────┬─────────────┘
                         ▼
                  EC-DNA CALLERS
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
    CReSIL             CoRAL             Decoil
       │                 │                 │
       └─────────────────┼─────────────────┘
                         ▼
                STRUCTURE EVALUATION
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
   Detection         Reconstruction     Quantification
       │                 │                 │
       ▼                 ▼                 ▼
     F1/PR          topology score      CN accuracy
  breakpoint       segment order       abundance
     recall          orientation        deconvolution
                         │
                         ▼
                  BIOLOGICAL VALIDATION
                         │
              ┌──────────┼──────────┐
              │          │          │
            RNA-seq    Fiber-seq    Hi-C
              │          │          │
              └──────────┼──────────┘
                         ▼
                 RECOMMENDATION
                         │
           "Which method for which data?"
```

---

# 十九、最后形成一个“ecDNA detection atlas”

这个我觉得是文章最漂亮的结果之一。

比如做一个二维/三维矩阵：

| Data        | Depth | Purity | Best method | Detection | Structure |
| ----------- | ----: | -----: | ----------- | --------: | --------: |
| PacBio HiFi |   10X |   100% | CoRAL       |       ★★★ |        ★★ |
| PacBio HiFi |   20X |   100% | CoRAL       |       ★★★ |       ★★★ |
| PacBio HiFi |   30X |    50% | CoRAL       |       ★★★ |       ★★★ |
| ONT         |   20X |    50% | Decoil      |       ★★★ |       ★★★ |
| ONT         |   10X |    10% | CReSIL      |        ★★ |         ★ |
| Illumina    |  100X |   100% | AA          |       ★★★ |        ★★ |

最终给出一个：

> **ecDNA method-selection guideline**

这会比简单说“CoRAL最好”有价值得多。

---

# 二十、如果结合你目前的数据，我会这样定制

你现在已经有：

**Tumor/Normal PacBio HiFi + Fiber-seq**

所以我反而不建议你从纯计算模拟开始。

可以直接做：

### Phase I — Benchmark

```text
PacBio HiFi
ONT
Illumina
        ↓
CReSIL
CoRAL
Decoil
AA
AmpliconSuite
        ↓
Detection + reconstruction
```

### Phase II — Controlled simulation

```text
ecDNA topology
×
depth
×
read length
×
tumor purity
×
ecDNA abundance
```

### Phase III — Ground truth

```text
validated cell lines
+
synthetic circular DNA
+
real tumors
```

### Phase IV — Multi-omics

```text
ecDNA sequence
       ↓
Fiber-seq
       ↓
FIRE / accessibility / methylation
       ↓
RNA-seq
       ↓
Hi-C / ec3D
```

### Phase V — 建立最终模型

```text
                 ecDNA Benchmark Score
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
 Detection            Structure         Function
       │                  │                  │
   sensitivity       topology          chromatin
   specificity       breakpoint        regulation
   breakpoint        phasing           expression
       │                  │                  │
       └──────────────────┼──────────────────┘
                          ↓
                Optimal method selection
```

---

## 我认为最有潜力的“文章核心问题”

不要把文章问题写成：

> **Which ecDNA caller is the best?**

而应该是：

> **How reliably can long-read sequencing resolve the existence, architecture, abundance and regulatory state of heterogeneous ecDNA across sequencing depths and tumor purities?**

这就从一个普通的 software benchmark，变成了一个**测序技术 + 算法 + 肿瘤基因组学 + ecDNA biology 的系统 benchmark**。

而且这个设计正好卡在现有工作的空白：2024 年的 Nature Communications benchmark 已经覆盖了 eccDNA 检测，但作者明确指出非富集 WGS-LR 的 pipeline 比较不足；2024 年 CoRAL/Decoil 又把问题推进到长读长结构重建。([Nature][1]) **因此下一步最有价值的不是重复一次软件比较，而是建立“PacBio HiFi/ONT + 非富集 WGS + 复杂拓扑 + purity/depth + 多 ecDNA 共存 + 结构定量 + biological validation”的 benchmark。**

如果按你现在的 **PacBio HiFi Tumor/Normal + Fiber-seq** 条件，我会优先把它规划成 **“PacBio HiFi ecDNA benchmark”**，并把 **CReSIL、CoRAL、Decoil、AmpliconSuite/AA + SV/CNV** 作为第一版核心算法，然后设计一套 **10种 ecDNA topology × 7个测序深度 × 6个 tumor purity × 4种 read-length** 的模拟矩阵，再用真实 tumor/cell line 做外部验证。这样已经足以形成一篇比较完整的方法学文章。

[1]: https://www.nature.com/articles/s41467-024-53496-8?utm_source=chatgpt.com "Comparative analysis of methodologies for detecting extrachromosomal circular DNA | Nature Communications"
[2]: https://pubmed.ncbi.nlm.nih.gov/38981681/?utm_source=chatgpt.com "CoRAL accurately resolves extrachromosomal DNA genome structures with long-read sequencing - PubMed"
[3]: https://genome.cshlp.org/content/early/2024/10/03/gr279123124?utm_source=chatgpt.com "Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil | Genome Research"
[4]: https://www.nature.com/articles/s41467-025-67614-7?utm_source=chatgpt.com "Reconstructing the three-dimensional architecture of extrachromosomal DNA with ec3D | Nature Communications"

---

# 如何准备ecDNA benchmark的数据


可以。对于你前面规划的 **“新一代 ecDNA benchmark”**，数据准备其实是整个项目最核心的部分。我的建议是不要只准备一套模拟数据，而是建立一个 **四层 benchmark data system**：

> **Simulation → controlled spike-in → validated cell lines → real tumors**

这样才能同时回答“算法在理想条件下怎么样”和“真实 PacBio HiFi 肿瘤中到底怎么样”。

现有 2024 年 *Nature Communications* benchmark 已经做了 10,000 个 circular + 10,000 个 linear DNA 的模拟，并测试了不同深度和 chimeric DNA 比例；但它明确主要针对 eccDNA/enriched 数据，且作者指出 **non-enriched WGS-LR 的方法比较仍不足**。([Nature][1]) 这正是你可以往前推进的地方。

---

# 一、我建议最终建立 4 类数据集

```text
                         ecDNA Benchmark Dataset
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
      Tier 1                  Tier 2                  Tier 3/4
    Simulation              Spike-in / Mix         Real biological
          │                       │                       │
    完全已知答案             半真实 ground truth       Cell line / Tumor
          │                       │                       │
          └───────────────┬───────┴───────────────────────┘
                          ↓
                PacBio HiFi / ONT / Illumina
                          ↓
             CReSIL / CoRAL / Decoil / AA
                          ↓
                    Benchmark
```

其中我认为**最重要的是 Tier 2 + Tier 3**，因为单纯 simulation 很容易被审稿人质疑。

---

# 二、Tier 1：建立标准化 ecDNA simulation library

第一步不要直接生成 FASTQ。

先建立一个：

> **ecDNA structural truth library**

也就是一个“已知答案”的 ecDNA 数据库。

---

## 1. 先设计 10 类 ecDNA topology

建议第一版至少：

| ID  | topology                        |    难度 |
| --- | ------------------------------- | ----: |
| T01 | simple circle                   |     ★ |
| T02 | inversion                       |    ★★ |
| T03 | tandem duplication              |    ★★ |
| T04 | deletion + circle               |    ★★ |
| T05 | fold-back                       |   ★★★ |
| T06 | nested duplication              |   ★★★ |
| T07 | multi-chromosomal               |   ★★★ |
| T08 | multiple ecDNA sharing segments |  ★★★★ |
| T09 | ecDNA + HSR                     |  ★★★★ |
| T10 | highly complex rearranged ecDNA | ★★★★★ |

例如 T01：

```text
chr8:A ─ chr8:B ─ chr8:C
             │
             └──────────────┐
                            ↓
                     ecDNA-A
                     A → B → C → A
```

T07：

```text
chr7:A
   ↓
chr8:B
   ↓
chr12:C
   ↓
chr7:D
   ↓
ecDNA
```

T08 更重要：

```text
ecDNA-1:
A → B → C → D → A

ecDNA-2:
A → B → E → F → A
```

两个 ecDNA **共享 A/B**。

这类结构正是结构解卷积最困难的情况，Decoil 的工作也专门讨论了复杂、异质 ecDNA 重建。([Genome Research][2])

---

# 三、不要只生成“圆形 FASTA”

每个 simulated ecDNA 最好保存完整 truth。

例如：

```text
ecDNA_T08_001/
├── truth.fa
├── truth.bed
├── truth.tsv
├── junctions.tsv
├── segments.tsv
└── metadata.json
```

`truth.tsv`：

```text
ecDNA_ID    topology    length    copy_number
E001        simple      125000    20
E002        inversion   180000    15
E003        complex     420000    30
```

`segments.tsv`：

```text
ecDNA_ID    order    chr     start       end       strand
E003        1         chr7    1000000     1050000   +
E003        2         chr8    5000000     5080000   +
E003        3         chr7    2000000     2050000   -
E003        4         chr12   8000000     8200000   +
```

`junctions.tsv`：

```text
ecDNA_ID    junction    chr1    pos1    strand1    chr2    pos2    strand2
E003        J1           chr7    1050000 +          chr8    5000000 +
E003        J2           chr8    5080000 +          chr7    2000000 -
E003        J3           chr7    2050000 -          chr12   8000000 +
E003        J4           chr12   8200000 +          chr7    1000000 +
```

**后面所有 benchmark 都基于这些 truth，而不是根据软件结果反推答案。**

---

# 四、Tier 1 的模拟因素

不要只改变 sequencing depth。

建议做一个正交设计：

### A. ecDNA complexity

```text
simple
moderate
complex
very_complex
```

### B. ecDNA size

```text
1 kb
5 kb
10 kb
50 kb
100 kb
500 kb
1 Mb
5 Mb
```

尤其要重点增加：

> **>10 kb、>100 kb、>1 Mb**

因为 2024 benchmark 自己也指出其模拟数据中 >10 kb ecDNA 比例偏低，这限制了对长 ecDNA 的评价。([Nature][1])

---

# 五、最重要：不要只模拟 ecDNA abundance

建议增加：

```text
ecDNA abundance
```

例如：

```text
100 copies
50 copies
20 copies
10 copies
5 copies
2 copies
1 copy
```

或者使用 VAF-like：

```text
50%
20%
10%
5%
1%
0.1%
```

这样你可以回答：

> **ecDNA copy number 到什么程度才能被检测？**

---

# 六、加入 tumor purity

这个对于你的项目非常重要。

例如：

```text
100% tumor
75%
50%
25%
10%
5%
1%
```

构造：

```text
Tumor genome
     +
Normal genome
```

例如：

```text
50X total sequencing
```

在：

```text
50% tumor purity
```

时：

```text
25X tumor
25X normal
```

在：

```text
10% tumor purity
```

时：

```text
5X tumor
45X normal
```

这会让 benchmark 非常接近临床真实情况。

---

# 七、测序深度矩阵

针对你的 PacBio HiFi：

```text
60X
40X
30X
20X
15X
10X
5X
2X
1X
```

我建议重点：

```text
10X
15X
20X
30X
40X
```

因为你最终很可能需要回答：

> **PacBio HiFi 做 ecDNA detection 至少需要多少 coverage？**

---

# 八、read length 也要控制

这是 PacBio HiFi benchmark 非常值得做的地方。

可以从真实 PacBio HiFi reads 中 downsample/read-length filter：

```text
5 kb
10 kb
15 kb
20 kb
25 kb
30 kb
40 kb
50 kb
```

然后：

```text
same ecDNA
        ↓
different read length
        ↓
CReSIL / CoRAL / Decoil
```

最终可以得到：

> **复杂 ecDNA 结构需要多长的 HiFi read 才能可靠恢复？**

这个结果会比单纯的 F1 更有意义。

---

# 九、Tier 2：做“半真实”Spike-in 数据

这是我最推荐你增加的一层。

不要完全依赖模拟。

可以使用：

```text
真实 tumor genomic DNA
          +
已知结构 ecDNA
          ↓
混合
          ↓
PacBio HiFi sequencing
```

这样：

* genomic DNA 是真实的
* repeat 是真实的
* sequencing error 是真实的
* GC bias 是真实的
* mapping ambiguity 是真实的

只有 ecDNA 是你知道答案的。

---

# 十、Spike-in ecDNA 从哪里来？

有三个选择。

### 方案 A：合成 circular DNA

最好。

例如：

```text
10 kb
50 kb
100 kb
500 kb
1 Mb
```

构建具有已知：

```text
sequence
junction
orientation
copy number
```

的 circular DNA。

---

### 方案 B：已有 plasmid/circular DNA

成本较低，可以做第一版 proof-of-concept。

---

### 方案 C：真实 cancer ecDNA

最好，但实验难度最高。

可以选择已经有明确 ecDNA 结构的癌细胞系。

---

# 十一、Spike-in 最重要的是设计“真实背景”

不要：

```text
ecDNA + clean reference genome
```

这太简单。

应该：

```text
Real tumor DNA
      +
ecDNA
```

最好进一步：

```text
Real tumor DNA
      +
Normal DNA
      +
ecDNA
```

形成：

```text
tumor purity
×
ecDNA abundance
```

两个维度同时变化。

---

# 十二、Tier 3：真实 cell line benchmark

这一层非常重要。

选择：

> **已经被多种实验手段证明存在 ecDNA 的癌细胞系**

然后收集：

```text
PacBio HiFi
ONT
Illumina WGS
```

最好再有：

```text
Hi-C
FISH
optical mapping
RNA-seq
```

这样建立：

```text
Experimental consensus truth
```

而不是：

> “CReSIL 认为有 ecDNA，所以它就是真实 ecDNA”。

---

# 十三、真实 cell line 的 ground truth 怎么定义？

我建议采用：

## Level 1

至少：

```text
FISH positive
+
long-read spanning junction
```

---

## Level 2

```text
FISH
+
SV
+
CNV
+
long-read
```

---

## Level 3

最强：

```text
FISH
+
optical mapping
+
PacBio/ONT
+
Hi-C
+
RNA
```

定义为：

> **Gold-standard ecDNA**

---

# 十四、Tier 4：真实 tumor cohort

最后才进入真正的 cancer cohort。

建议第一版：

```text
20–50 tumors
```

最好有：

```text
Tumor PacBio HiFi
Normal PacBio HiFi
```

你现在正好可以利用这个体系。

每个 tumor：

```text
Tumor HiFi
Normal HiFi
       │
       ├── SV
       ├── CNV
       ├── CReSIL
       ├── CoRAL
       └── Decoil
```

最后得到：

```text
Consensus ecDNA
```

---

# 十五、你现有的 Tumor/Normal PacBio HiFi 特别适合这样利用

你之前已经规划了：

```text
Tumor
 ↓
DeepSomatic / ClairS
 ↓
SNV/Indel

Tumor + Normal
 ↓
Severus
NanoMonSV
Sniffles
Savana
 ↓
somatic SV
```

这个不要丢。

直接把它变成 benchmark 的辅助 truth：

```text
                 Tumor HiFi
                     │
        ┌────────────┼────────────┐
        │            │            │
       CNV          SV          ecDNA
        │            │            │
        └────────────┼────────────┘
                     ↓
              structural truth
```

---

# 十六、Fiber-seq 可以作为第二层验证

你已经有 Fiber-seq，这个非常有价值。

对于确定的 ecDNA：

```text
ecDNA sequence
      ↓
Fiber-seq
      ↓
m6A
      ↓
chromatin accessibility
      ↓
FIRE
```

这样可以验证：

> 一个软件重建出来的 ecDNA，是否真的具有独立的染色质调控结构？

这已经从：

**algorithm benchmark**

升级成：

**functional benchmark**。

---

# 十七、建议建立统一的数据目录

最终项目建议设计成：

```text
ecDNA-Benchmark/
│
├── 00_reference/
│   ├── GRCh38.fa
│   ├── GRCh38.fa.fai
│   └── GRCh38.mmi
│
├── 01_simulation/
│   ├── topology/
│   │   ├── T01_simple/
│   │   ├── T02_inversion/
│   │   ├── T03_duplication/
│   │   └── ...
│   │
│   ├── depth/
│   ├── purity/
│   ├── abundance/
│   └── read_length/
│
├── 02_spikein/
│   ├── 10kb/
│   ├── 50kb/
│   ├── 100kb/
│   ├── 500kb/
│   └── 1Mb/
│
├── 03_cellline/
│   ├── PacBio/
│   ├── ONT/
│   ├── Illumina/
│   ├── HiC/
│   └── FISH/
│
├── 04_tumor/
│   ├── tumor/
│   ├── normal/
│   ├── HiFi/
│   ├── ONT/
│   ├── Illumina/
│   ├── FiberSeq/
│   └── RNA/
│
├── 05_truth/
│   ├── ecDNA_truth.tsv
│   ├── segments.tsv
│   ├── junctions.tsv
│   └── copy_number.tsv
│
└── 06_results/
    ├── CReSIL/
    ├── CoRAL/
    ├── Decoil/
    ├── AmpliconSuite/
    └── benchmark/
```

---

# 十八、数据量不要一开始就做得特别大

我建议先做一个 **Pilot benchmark**。

### Pilot：

```text
10 topology
×
5 depth
×
4 purity
×
3 abundance
```

就是：

```text
10 × 5 × 4 × 3 = 600 datasets
```

如果每个 dataset：

```text
3 replicates
```

就是：

```text
1,800 datasets
```

已经非常够用了。

---

# 十九、然后再扩展

正式版：

```text
Topology       10
Depth           8
Purity          6
Abundance       6
Read length     6
Platform        2
Replicate       3
```

理论上：

```text
10 × 8 × 6 × 6 × 6 × 2 × 3
```

会达到几十万组合，完全没必要全部做。

所以应该使用：

> **factorial design / Latin hypercube sampling**

而不是暴力组合。

---

# 二十、一个非常重要的设计：训练集和测试集必须分开

如果未来你开发自己的 benchmark tool 或算法，这一点非常重要。

应该：

```text
Training / Development
        │
        ├── T01
        ├── T02
        ├── T03
        └── ...
        
Hidden Test Set
        │
        ├── T08
        ├── T09
        └── T10
```

尤其不要：

> 用同一批 simulated ecDNA topology 调参，再用同样的数据宣布方法最好。

否则 benchmark 很容易被认为存在 overfitting。

---

# 二十一、最好做一个“blind benchmark”

这是我特别推荐你加入的。

例如：

```text
Benchmark dataset
       ↓
隐藏 truth
       ↓
给 CReSIL / CoRAL / Decoil 开发者
       ↓
提交结果
       ↓
服务器自动评分
```

最终：

> **ecDNA Benchmark Challenge**

甚至可以做成 GitHub + Docker/Singularity：

```text
ecDNA-Benchmark
        ↓
run_benchmark.sh
        ↓
results.tsv
        ↓
automatic scoring
```

这会让数据集具有长期价值，而不仅仅是论文 supplement。

---

# 二十二、最终建议的“Gold-standard”数据组成

如果是我来设计你的项目，我会这样分配：

| 数据层    | 数据                            | 目的                    |   重要性 |
| ------ | ----------------------------- | --------------------- | ----: |
| Tier 1 | Simulation                    | 精确计算 F1               |   ★★★ |
| Tier 2 | Real genome + synthetic ecDNA | controlled benchmark  | ★★★★★ |
| Tier 3 | ecDNA cell lines              | biological truth      | ★★★★★ |
| Tier 4 | real tumors                   | clinical relevance    | ★★★★★ |
| Tier 5 | Fiber-seq/Hi-C/RNA            | functional validation |  ★★★★ |

**不要把 2024 benchmark 的 simulation 数据直接作为你的核心数据集。** 它可以作为 baseline/reproducibility dataset，因为其代码和 template 已经公开；但你的核心创新应该是 **non-enriched PacBio HiFi/ONT WGS + complex ecDNA + controlled spike-in + tumor/normal + multi-omic validation**。2024 年 benchmark 的代码和模板已经公开在 `QuKunLab/eccDNABenchmarking`，因此你完全可以先复现它作为 baseline，再在此基础上扩展。([Nature][1])

---

# 二十三、如果结合你现在的实验条件，我最推荐的实际路线

你现在已经有 **Tumor/Normal PacBio HiFi + Fiber-seq**，所以我不会建议你先花几个月做大量纯模拟。

我会按下面顺序：

```text
             Phase 1
        复现2024 benchmark
                │
                ↓
       获得 baseline results
                │
                ↓
             Phase 2
       建立10种ecDNA topology
                │
                ↓
       PacBio HiFi simulation
                │
       ┌────────┼─────────┐
       ↓        ↓         ↓
     depth    purity    abundance
       │        │         │
       └────────┼─────────┘
                ↓
             Phase 3
       Real DNA + ecDNA spike-in
                │
                ↓
             Phase 4
        真实ecDNA cell lines
                │
                ↓
             Phase 5
       你的Tumor/Normal HiFi
                │
        ┌───────┼────────┐
        ↓       ↓        ↓
       CNV     SV     ecDNA callers
        │       │        │
        └───────┼────────┘
                ↓
          Consensus truth
                │
        ┌───────┼─────────┐
        ↓       ↓         ↓
    Fiber-seq  Hi-C     RNA-seq
        │       │         │
        └───────┼─────────┘
                ↓
       ecDNA sequence → structure
                ↓
          chromatin → function
```

### 最关键的一点

**你的 benchmark 的“核心数据集”最好不是 FASTQ，而是一个有明确 ground truth 的 `ecDNA structural truth set`。**

也就是说，每一个 benchmark case 从一开始就必须知道：

```text
1. ecDNA 是否存在
2. ecDNA 有多长
3. 来自哪些 chromosome
4. segment 顺序
5. segment orientation
6. 所有 circular junction
7. copy number
8. ecDNA abundance
9. tumor purity
10. 是否存在多个共存 ecDNA
11. 是否共享 genomic segments
12. 是否存在 HSR
```

然后再根据这个 truth 去生成 **PacBio HiFi / ONT / Illumina FASTQ**。

这样你的文章最后就不是：

> “我们跑了 5 个软件，CoRAL F1 最高。”

而是：

> **“我们建立了一个具有已知结构、丰度、复杂度和肿瘤背景的 ecDNA benchmark，并系统定义了 long-read ecDNA detection 和 reconstruction 在不同测序深度、tumor purity、read length 和 structural complexity 下的可靠性边界。”**

这才是一个真正有机会成为**领域标准 benchmark dataset**的设计。

如果你下一步要实际开始做，我建议先从 **“10种 ecDNA topology 的 truth 文件怎么构建 + 如何自动生成 PacBio HiFi FASTQ”** 开始；这一部分我可以直接给你设计成一套 **Python + samtools + minimap2 + PBSIM2/Badread 的可重复 benchmark 数据生成 pipeline**，包括 `truth.tsv`、SV VCF、ecDNA FASTA、PacBio HiFi FASTQ 和 600 个 benchmark case 的自动生成规则。

[1]: https://www.nature.com/articles/s41467-024-53496-8?utm_source=chatgpt.com "Comparative analysis of methodologies for detecting extrachromosomal circular DNA | Nature Communications"
[2]: https://genome.cshlp.org/content/early/2024/10/03/gr279123124?utm_source=chatgpt.com "Reconstructing extrachromosomal DNA structural heterogeneity from long-read sequencing data using Decoil | Genome Research"


