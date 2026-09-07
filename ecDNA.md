
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


