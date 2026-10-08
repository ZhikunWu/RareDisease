
## [v5.0q GIAB HG002](https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/AshkenazimTrio/HG002_NA24385_son/v5.0q/NIST_HG002_v5.0q_variant-benchmarksets_README.md)

## [v5.0q GIAB HG002 Assembly-Based Small & Structural Variant Draft Sets](https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/AshkenazimTrio/HG002_NA24385_son/v5.0q/NIST_HG002_v5.0q_variant-benchmarksets_README.md)

## [SV benchmark](https://github.com/RajLabMSSM/AMP_AD_StructuralVariation/tree/main/analysis/benchmarking)

## somatic SVs from long read

[SomaSV](https://github.com/eioyuou/SomaSV)

[Severus]()

[Savana]

[nanomonsv]

delly

Sniffles

pbsv

Sawfish

SVDSS

[smoove](https://github.com/brentp/smoove): based on lumpy, population level



## SVs from NGS
Manta

delly

lumpy

Wham

gridss

---

# 有哪些比较优秀的基于二代WGS检测germline SV的工具

如果限定为**二代 Illumina WGS + germline SV**，我会优先考虑下面这些工具。对于你这种人类 WGS、尤其是后续还要和 PacBio HiFi SV 做交叉验证的场景，建议不要只跑一个 caller，而是采用 **Manta + DELLY + LUMPY/GRIDSS + 专门的 MEI/CNV caller，再做 ensemble**。

近期针对 GIAB/HGSVC 的比较仍然把 **Manta、DELLY、LUMPY、GRIDSS、SvABA**列为代表性的短读长 SV caller；其中 Manta 在 deletion/overall 实用性方面表现尤其稳定。([PMC][1])

### 1. 我最推荐的几个

| 工具              | 主要证据                     |   DEL |   DUP |   INV |       INS | BND/TRA | Germline |        推荐度 |
| --------------- | ------------------------ | ----: | ----: | ----: | --------: | ------: | -------: | ---------: |
| **Manta**       | PE + SR + 局部组装           | ★★★★★ |  ★★★★ |  ★★★★ |      ★★★★ |    ★★★★ |        ✓ |      ⭐⭐⭐⭐⭐ |
| **DELLY**       | PE + SR                  | ★★★★★ |  ★★★★ |  ★★★★ |       ★★★ |   ★★★★★ |        ✓ |      ⭐⭐⭐⭐⭐ |
| **LUMPY**       | PE + SR                  |  ★★★★ |  ★★★★ |  ★★★★ |       ★★★ |    ★★★★ |        ✓ |       ⭐⭐⭐⭐ |
| **GRIDSS**      | break-end assembly + SR  |  ★★★★ |  ★★★★ | ★★★★★ |      ★★★★ |   ★★★★★ |        ✓ |      ⭐⭐⭐⭐⭐ |
| **SvABA**       | local assembly           |  ★★★★ |   ★★★ |   ★★★ |      ★★★★ |    ★★★★ |        ✓ |       ⭐⭐⭐⭐ |
| **GenomeSTRiP** | read-depth + PE/SR       |  ★★★★ | ★★★★★ |     — |         — |       — |        ✓ |       ⭐⭐⭐⭐ |
| **MELT**        | mobile-element insertion |     — |     — |     — | **★★★★★** |       — |        ✓ | ⭐⭐⭐⭐⭐（MEI） |
| **Wham**        | PE/SR                    |  ★★★★ |  ★★★★ |  ★★★★ |       ★★★ |    ★★★★ |        ✓ |        ⭐⭐⭐ |
| **CNVnator**    | read depth               |   ★★★ | ★★★★★ |     — |         — |       — |        ✓ |  ⭐⭐⭐⭐（CNV） |
| **GATK-SV**     | 多caller ensemble         | ★★★★★ | ★★★★★ |  ★★★★ |      ★★★★ |   ★★★★★ |        ✓ |      ⭐⭐⭐⭐⭐ |

不同 benchmark 对工具的排名会有所不同，但一个比较一致的结论是：**没有单一 caller 能很好地覆盖所有 SV 类型**；Manta、GRIDSS、DELLY、LUMPY 等各自存在互补性。([PubMed][2])

---

# 2. Manta：如果只选一个，我首先选它

**Manta** 是二代 WGS germline SV 中非常实用的选择。

它主要利用：

* discordant paired-end reads
* split reads
* 局部组装
* breakpoint refinement

因此对于：

* deletion
* insertion
* inversion
* duplication
* translocation

都有较好的覆盖。

近期短读长 benchmark 中，Manta 对 deletion 的综合表现较好，同时计算资源需求相对低；在一些比较中也是非商业工具中 recall 较好的工具。([PMC][1])

典型输入：

```text
WGS FASTQ
   ↓
BWA-MEM/BWA-MEM2
   ↓
sorted BAM
   ↓
Manta
   ↓
candidateSV.vcf.gz
diploidSV.vcf.gz
```

对于 germline，最终比较重要的是：

```text
diploidSV.vcf.gz
```

---

# 3. DELLY：非常适合做高可信 germline SV

**DELLY** 是另一个我非常推荐的工具。

核心证据：

```text
discordant paired-end
        +
split-read
```

特别适合：

* DEL
* DUP
* INV
* BND/TRA

尤其是**大片段 deletion 和染色体重排**。

DELLY 与 Manta 的算法机制不同，因此两者非常适合组合。

例如：

```text
Manta
   \
    → merge/overlap → high-confidence SV
   /
DELLY
```

在很多 germline/短读长 benchmark 中，DELLY 都属于主流高质量 caller。([PMC][1])

---

# 4. GRIDSS：如果你特别关注 breakpoint，值得加入

GRIDSS 和 Manta/DELLY 的思路不完全一样。

它更加偏向：

> **break-end assembly**

因此对复杂 breakpoint、重排以及一些复杂 SV 比较有价值。

特别适合：

* complex rearrangement
* inversion
* translocation
* breakpoint
* microhomology
* non-templated insertion

早期全面 benchmark 也特别指出，使用 breakpoint assembly、同时利用多种 alignment evidence 的工具通常更有优势，GRIDSS 和 Manta 是其中比较突出的代表。([Nature][3])

缺点是：

**比较吃计算资源。**

近期 benchmark 中 GRIDSS 的运行时间和内存需求通常高于 Manta。([PMC][1])

---

# 5. LUMPY：经典、稳定，适合作为第二/第三 caller

LUMPY 的特点是把多种 alignment signatures 综合起来：

```text
paired-end
    +
split-read
    +
discordant alignment
```

它的优势不是“单项一定第一”，而是：

> **和 Manta、DELLY 的结果具有较好的互补性。**

因此如果你要做：

```text
Manta
DELLY
LUMPY
      ↓
SV intersection / union
      ↓
high-confidence SV
```

是一个比较经典的设计。

---

# 6. SvABA：复杂 SV / 局部组装可以考虑

SvABA 的核心是：

> **windowed local assembly**

也就是说，它会针对异常区域进行局部组装，然后寻找 breakpoint。

因此在一些：

* complex SV
* insertion
* breakpoint
* microhomology

场景有价值。

不过如果是普通的人群 germline WGS，我一般不会把 SvABA 作为唯一 caller，而是作为补充。

---

# 7. GenomeSTRiP：CNV/大型 SV 很值得考虑

如果你的“SV”包括：

> **large CNV / duplication / deletion**

那么 GenomeSTRiP 值得加入。

它和 Manta/DELLY 的一个重要区别是：

```text
Manta / DELLY
        ↓
PE + SR
```

而 GenomeSTRiP 更强调：

```text
read depth
+
population information
+
genotype
```

所以对于：

* large deletion
* large duplication
* CNV
* multi-sample germline SV

尤其有价值。

---

# 8. MELT：如果研究 MEI，一定考虑

如果你的研究涉及：

* Alu
* LINE-1
* SVA
* mobile element insertion

那么不要指望 Manta/DELLY 单独解决得很好。

可以单独跑：

**MELT**

它是专门针对：

> **Mobile Element Insertion, MEI**

设计的。

GATK-SV 的官方流程本身也是一个 ensemble framework，其中包含 Manta、Wham、Scramble，以及 read-depth 的 GATK-gCNV/cn.MOPS；历史 joint callset 中也大量使用 MELT 进行 MEI 检测。([Broad Institute][4])

---

# 9. GATK-SV：如果你有较大的人群队列，我反而最推荐这个思路

如果你不是：

> 一个样本 → 得到 SV

而是：

> **100、500、1000+ 个 WGS 样本 → 建立 germline SV population callset**

那么可以考虑 **GATK-SV**。

它不是单纯一个 caller，而是：

```text
             Manta
               │
             Wham
               │
            Scramble
               │
          GATK-gCNV
               │
            cn.MOPS
               ↓
       ┌────────────────┐
       │ GATK-SV ensemble│
       └────────────────┘
               ↓
       filtering/refinement
               ↓
        final SV callset
```

官方文档明确把它定义为：

> ensemble of SV discovery tools → integration → filtering → refinement → annotation → final callset。([Broad Institute][4])

因此：

### 小队列

```text
Manta
DELLY
GRIDSS
```

就很好。

### 中大型 population WGS

```text
GATK-SV
```

会更加系统。

---

# 10. 如果是你现在这种研究，我建议这样组合

结合你前面一直在做的 **PacBio HiFi + WGS + SV/CNV + population genomics**，我不会只使用一个二代 caller。

我会设计成：

```text
                 Illumina WGS
                      │
                      ↓
                 BWA-MEM2
                      │
                      ↓
                 sorted BAM
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      Manta          DELLY        GRIDSS
        │             │             │
        └─────────────┼─────────────┘
                      ↓
                 SV merge
                      │
             ┌────────┴────────┐
             ↓                 ↓
          MELT             CNVnator/
        (MEI)              GenomeSTRiP
             │                 │
             └────────┬────────┘
                      ↓
              high-confidence
                 germline SV
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
   Illumina-supported       PacBio HiFi
                              SV caller
          │                       │
          └───────────┬───────────┘
                      ↓
              integrated SV set
```

这里尤其适合你后面做：

* **SNV/Indel**
* **germline SV**
* **CNV**
* **MEI**
* **PacBio HiFi SV**
* **population frequency**
* **SV-QTL**
* **基因/转录本影响**

的整合分析。

---

## 11. 如果只让我给你一个推荐组合

### 普通 germline WGS

我会选择：

> **Manta + DELLY + GRIDSS**

然后：

```text
Manta ∩ DELLY
```

作为高可信核心集，

再用：

```text
GRIDSS
```

补充复杂 breakpoint。

---

### 如果是 30× 人群 WGS

推荐：

> **Manta + DELLY + GRIDSS + GenomeSTRiP/CNVnator + MELT**

因为不同工具解决的是不同问题：

| 类型            | 首选                     |
| ------------- | ---------------------- |
| DEL           | Manta / DELLY          |
| DUP           | Manta / GenomeSTRiP    |
| INV           | Manta / DELLY / GRIDSS |
| INS           | Manta / GRIDSS         |
| BND/TRA       | DELLY / GRIDSS         |
| Large CNV     | GenomeSTRiP / CNVnator |
| Alu/L1/SVA    | MELT                   |
| Complex SV    | GRIDSS                 |
| Population SV | GATK-SV                |

---

### 12. 一个非常重要的区别

如果你的数据是：

**Illumina 30× WGS**

那么上述工具非常合适。

但是如果你的最终研究目标是：

> **准确获得人类 germline SV，特别是 50 bp–100 kb 的 insertion、复杂 SV、重复区 SV**

那么**不要把二代 WGS caller 当成最终金标准**。

短读长对重复区域、复杂 insertion 和较大的复杂重排存在明显限制；近期短读长与长读长比较也显示，两者在非重复区域的部分 SV 上可以有相近表现，但重复区域的 SV 检测明显更依赖长读长。([Nature][5])

因此，如果你手里同时有 **Illumina WGS + PacBio HiFi**，非常推荐做：

```text
Illumina
Manta
DELLY
GRIDSS
MELT
GenomeSTRiP
        │
        ↓
   Short-read SV
        │
        ├──────────────┐
        │              │
        ↓              ↓
   PacBio HiFi     population
   Sniffles2       frequency
   pbsv
   cuteSV
        │
        ↓
   SV harmonization
        │
        ↓
 High-confidence germline SV
```

这会比单纯跑 3 个 short-read caller 更有研究价值。

如果你的目标是**“100个人 WGS，建立一个高质量的人群 germline SV catalog，并进一步和你现在的100个 PacBio长读长转录组/WGS数据结合”**，我可以进一步给你设计一套 **Manta + DELLY + GRIDSS + MELT + GenomeSTRiP → Jasmine/Truvari → SV VCF → population AF → PacBio验证** 的完整流程，包括每一步的命令、输入输出文件和过滤标准。

[1]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11801633/?utm_source=chatgpt.com "Comparisons of performances of structural variants detection algorithms in solitary or combination strategy - PMC"
[2]: https://pubmed.ncbi.nlm.nih.gov/31159850/?utm_source=chatgpt.com "Comprehensive evaluation of structural variation detection algorithms for whole genome sequencing - PubMed"
[3]: https://www.nature.com/articles/s41467-019-11146-4?utm_source=chatgpt.com "Comprehensive evaluation and characterisation of short read general-purpose structural variant calling software | Nature Communications"
[4]: https://broadinstitute.github.io/gatk-sv/docs/gs/sv_callers/?utm_source=chatgpt.com "SV/CNV callers | GATK-SV"
[5]: https://www.nature.com/articles/s41439-024-00276-x?utm_source=chatgpt.com "Comparative evaluation of SNVs, indels, and structural variations detected with short- and long-read sequencing data | Human Genome Variation"




---

# 基于pacbio hifi 测序数据检测germline和somatic CNV的工具有哪些

基于 **PacBio HiFi WGS** 做 CNV，我建议把工具分成两类来看：**germline CNV** 和 **tumor/normal somatic CNV**。目前两者的最佳工具并不完全相同。

## 1. 先给结论

| 工具                          |       HiFi | Germline CNV | Somatic CNV |   推荐度 | 主要特点                          |
| --------------------------- | ---------: | -----------: | ----------: | ----: | ----------------------------- |
| **HiFiCNV**                 |          ✅ |            ✅ |          ⚠️ | ⭐⭐⭐⭐⭐ | **PacBio HiFi 专门 CNV caller** |
| **Spectre**                 |          ✅ |            ✅ |          ⚠️ |  ⭐⭐⭐⭐ | HiFi/ONT，适合大 CNV              |
| **CNVkit**                  |          ✅ |            ✅ |           ✅ |  ⭐⭐⭐⭐ | tumor-normal 成熟，深度/BAF        |
| **Purple**                  |         间接 |            ❌ |           ✅ |  ⭐⭐⭐⭐ | tumor purity/ploidy + CNV     |
| **Cobalt + Amber + Purple** |         间接 |            ❌ |           ✅ | ⭐⭐⭐⭐⭐ | 癌症 CNV/纯度/倍体                  |
| **Wakhan**                  | HiFi/ONT相关 |            ❌ |           ✅ |  ⭐⭐⭐⭐ | 长读长 somatic CNV               |
| **SAVANA**                  |       HiFi |            ❌ |           ✅ |  ⭐⭐⭐⭐ | SV + CNV，肿瘤                   |
| **Sniffles2**               |          ✅ |            ✅ |           ✅ |  ⭐⭐⭐⭐ | SV caller，可发现部分 CNV           |
| **pbsv**                    |          ✅ |            ✅ |          ⚠️ |  ⭐⭐⭐⭐ | PacBio 官方 SV caller，可发现部分 CNV |
| **mosdepth**                |          ✅ |            — |           — | ⭐⭐⭐⭐⭐ | 深度计算，不是 CNV caller            |

PacBio 官方目前明确把 **HiFiCNV** 定义为针对 PacBio HiFi 优化的 CNV caller。([PacBio][1])

---

# 2. Germline CNV：首选 HiFiCNV

如果你的数据是：

```text
正常人
PacBio HiFi WGS
30×左右
```

我首推：

> **HiFiCNV**

它专门针对 HiFi reads 的 germline WGS CNV calling，并且包含：

* depth segmentation
* GC bias correction
* CNV calling
* allele-frequency tracks
* bigWig visualization
* VCF/bedGraph 输出

([GitHub][2])

基本流程：

```text
PacBio HiFi FASTQ
        ↓
pbmm2 / minimap2
        ↓
sorted BAM
        ↓
HiFiCNV
        ↓
CNV VCF
        +
depth bigWig
        +
allele-frequency track
```

---

## 3. HiFiCNV 特别适合什么 CNV？

HiFiCNV 最适合：

```text
Deletion
Duplication
Large CNV
Multi-Mb CNV
```

例如：

```text
chr1
────────────────────────────────────
       ┌───────────────┐
       │   DUP         │
───────┴───────────────┴─────────────
       CN=3/4
```

它特别适合从 HiFi 的 read-depth 和 allele-frequency 信息中识别 CNV。PacBio 也明确强调 HiFiCNV 是为了捕获 genome-wide large CNVs 而开发的。([PacBio][3])

---

# 4. 但要注意：CNV 和 SV 不是完全分开的

这是做长读长 CNV 时非常重要的一点。

例如一个：

```text
500 bp deletion
```

既可以被称为：

```text
SV
```

也可以称为：

```text
CNV
```

而一个：

```text
10 Mb duplication
```

更典型地属于 CNV。

因此实际分析最好：

```text
HiFiCNV
   +
pbsv
   +
Sniffles2
```

而不是只跑一个 CNV caller。

例如：

```text
                    PacBio HiFi
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
         HiFiCNV                 pbsv/Sniffles2
             │                       │
        depth-based               SV-based
             │                       │
             └───────────┬───────────┘
                         ↓
                   unified CNV/SV
```

Sniffles2 明确支持 PacBio/ONT，并能够进行 germline、somatic 和 population-level SV calling。([GitHub][4])

---

# 5. pbsv 能不能做 CNV？

可以，但我更建议把 **pbsv 看作 SV caller，而不是主要 CNV caller**。

pbsv 能检测：

* deletion
* insertion
* inversion
* duplication
* translocation

其中 duplication 可以对应一部分 CNV。([GitHub][5])

例如：

```bash id="f9x8m3"
pbsv discover sample.bam sample.svsig.gz
pbsv call reference.fa sample.svsig.gz sample.pbsv.vcf
```

所以：

> **HiFiCNV = CNV 主力**

> **pbsv/Sniffles2 = SV 补充**

这是我更推荐的组合。

---

# 6. Spectre

另一个值得考虑的是：

> **Spectre**

它支持：

```text
PacBio HiFi
ONT
```

主要依靠：

```text
read depth
+
GC correction
+
segmentation
```

识别较大的 CNV。

一篇长读长分析综述指出，目前长读长 CNV caller 中比较突出的就是 **HiFiCNV 和 Spectre**，尤其适合较大的 CNV。([DOI][6])

因此 germline 可以考虑：

```text
HiFiCNV
    +
Spectre
    +
pbsv/Sniffles2
```

做交叉验证。

---

# 7. Somatic CNV：情况完全不同

如果你的数据是：

```text
Tumor HiFi
     +
Normal HiFi
```

就不能简单套用 germline HiFiCNV。

因为 tumor 可能存在：

```text
normal        CN=2

tumor         CN=4
```

或者：

```text
tumor
CN=1
CN=3
CN=6
CN=8
```

更复杂的是：

```text
subclone 1 → CN=4
subclone 2 → CN=7
subclone 3 → CN=10
```

因此 somatic CNV 需要同时考虑：

* tumor/normal depth ratio
* BAF
* tumor purity
* ploidy
* allele-specific CN
* LOH
* subclonal CNV

---

# 8. Tumor-normal HiFi：CNVkit

如果你要做的是：

> **Tumor-normal PacBio HiFi WGS somatic CNV**

可以使用：

**CNVkit**

PacBio 当前的 HiFi somatic workflow 也包含 CNVkit；其 HiFi-somatic-WDL 中明确使用 CNVkit 进行 copy-number segmentation。([GitHub][7])

基本逻辑：

```text
Normal BAM
     │
     ↓
normal depth

Tumor BAM
     │
     ↓
tumor depth
     │
     ↓
Tumor / Normal ratio
     │
     ↓
segmentation
     │
     ↓
somatic CNV
```

---

# 9. 癌症研究：Purple 是非常值得考虑的

如果你是：

```text
Tumor
+
Normal
+
SNV
+
SV
```

那么可以考虑：

```text
Cobalt
   +
Amber
   +
Purple
```

这是 HMFtools 中非常经典的癌症 CNV 分析组合。

大致：

```text
Tumor BAM
    │
    ├── Cobalt → depth / log ratio
    │
    └── Amber → BAF
              │
              ↓
           Purple
              │
       purity + ploidy
              │
              ↓
       allele-specific CN
```

特别适合：

* tumor purity
* ploidy
* LOH
* allele-specific CN
* somatic CNV

---

# 10. Wakhan

如果你特别希望使用：

> **long-read somatic CNV caller**

可以关注：

**Wakhan**

PacBio 的 HiFi somatic workflow 已经把 Wakhan 纳入工具集合，用于 copy-number variation calling。([GitHub][7])

因此：

```text
Tumor/Normal HiFi
       ↓
     Wakhan
       ↓
  somatic CNV
```

是一个值得测试的方案。

---

# 11. SAVANA

另一个比较有意思的是：

> **SAVANA**

它特别适合癌症 long-read：

```text
Somatic SV
+
CNV
```

PacBio 当前的 HiFi somatic workflow 也把 SAVANA 列为 SV/CNV caller。([GitHub][7])

因此：

```text
Tumor HiFi
    ↓
SAVANA
    ↓
SV + CNV
```

对于复杂癌症基因组尤其值得考虑。

---

# 12. 如果是你现在的 Tumor + Normal HiFi 数据

结合你之前的项目背景——你已经在做：

```text
Tumor HiFi
Normal HiFi
Fiber-seq
SV
ecDNA
chromothripsis
```

我不建议只跑一个 CNV caller。

我更推荐：

```text
                 Tumor HiFi
                     │
              ┌──────┴──────┐
              ↓             ↓
          HiFiCNV        SV callers
              │          /    │    \
              │      pbsv Sniffles2 SAVANA
              │             │
              └──────┬──────┘
                     ↓
                CNV/SV merge
                     │
             ┌───────┴────────┐
             ↓                ↓
       Tumor/Normal        Allele-specific
        comparison              CN
             │                │
             ↓                ↓
           CNVkit        Amber/Purple
             │                │
             └───────┬────────┘
                     ↓
              High-confidence
               somatic CNV
```

---

# 13. 如果你的目标包括 ecDNA

这个尤其重要。

你之前做的是 **ecDNA / chromothripsis**，那么单纯 CNV calling 不够。

例如：

```text
chr17
──────────╲
           ╲
            ╲───────┐
                    │
                    │ ecDNA
                    │
            ┌───────┘
           ╱
──────────╱
```

你可能观察到：

```text
CN = 20
```

但是 **CNV caller 只能告诉你“这里 CN 很高”**。

它不能可靠告诉你：

> 这是一个 extrachromosomal circular DNA。

所以应该：

```text
HiFiCNV
   ↓
high-CN regions
   ↓
ecDNA caller / SV
   ↓
CReSIL / AmpliconArchitect等
   ↓
long-read validation
   ↓
Fiber-seq
```

这样才能把：

```text
CN amplification
```

进一步解释成：

```text
ecDNA
```

---

# 14. 我给你的实际推荐

### Germline HiFi WGS

优先级：

```text
⭐⭐⭐⭐⭐ HiFiCNV
⭐⭐⭐⭐  Spectre
⭐⭐⭐⭐  Sniffles2
⭐⭐⭐⭐  pbsv
```

建议：

```text
HiFiCNV
   +
pbsv/Sniffles2
```

---

### Tumor-normal HiFi WGS

优先：

```text
⭐⭐⭐⭐⭐ CNVkit
⭐⭐⭐⭐⭐ Purple
⭐⭐⭐⭐  Wakhan
⭐⭐⭐⭐  SAVANA
⭐⭐⭐⭐  HiFiCNV
```

如果是正式癌症研究：

```text
CNVkit
+
Amber/Cobalt/Purple
+
Wakhan/SAVANA
```

然后进行 consensus。

PacBio 自己的 HiFi somatic workflow 已经将 **CNVkit、Amber、Cobalt、Purple、Wakhan、SAVANA** 等组合在同一套 tumor-normal 分析体系中。([GitHub][7])

---

## 15. 如果你希望做“高可信 CNV”，我建议最终采用这个策略

```text
             PacBio HiFi WGS
                    │
        ┌───────────┴───────────┐
        │                       │
     Germline                 Tumor
        │                       │
   HiFiCNV                   HiFiCNV
   Spectre                  CNVkit
   pbsv                     Wakhan
   Sniffles2                Purple
        │                       │
        └───────────┬───────────┘
                    ↓
              Consensus CNV
                    │
                    ↓
              Long-read SV
           pbsv / Sniffles2
                    │
                    ↓
            Junction validation
                    │
                    ↓
              Fiber-seq / Hi-C
                    │
                    ↓
           ecDNA / chromothripsis
```

**对于你目前的 Tumor + Normal PacBio HiFi 项目，我会把 `HiFiCNV + CNVkit/Purple + Sniffles2/pbsv/SAVANA` 作为主组合，而不是单独依赖一个 caller。** 2026 年新发布的 `nf-core/pacsomatic` 也正是把 HiFi 的 tumor-normal **SNV/Indel + SV + CNV + purity/ploidy** 整合到一个可复现流程中，说明这种多工具组合已经是目前比较合理的实践路线。([academic.oup.com][8])

[1]: https://www.pacb.com/computational-tools/?utm_source=chatgpt.com "Computational tools - PacBio"
[2]: https://github.com/pacificbiosciences/hificnv?utm_source=chatgpt.com "GitHub - PacificBiosciences/HiFiCNV: Copy number variant caller and depth visualization utility for PacBio HiFi reads · GitHub"
[3]: https://www.pacb.com/blog/hificnv/?utm_source=chatgpt.com "Introducing HiFiCNV – copy number calling optimized for HiFi reads - PacBio"
[4]: https://github.com/fritzsedlazeck/Sniffles?utm_source=chatgpt.com "GitHub - fritzsedlazeck/Sniffles: Structural variation caller using third generation sequencing · GitHub"
[5]: https://github.com/PacificBiosciences/pbsv?utm_source=chatgpt.com "GitHub - PacificBiosciences/pbsv: pbsv - PacBio structural variant (SV) calling and analysis tools · GitHub"
[6]: https://doi.org/10.1101/gr.279975.124?utm_source=chatgpt.com "A Hitchhiker's Guide to long-read genomic analysis | Genome Research"
[7]: https://github.com/PacificBiosciences/HiFi-somatic-WDL?utm_source=chatgpt.com "GitHub - PacificBiosciences/HiFi-somatic-WDL: Tumor-normal variant calling workflow using HiFi reads · GitHub"
[8]: https://academic.oup.com/bioinformatics/article/42/9/btag647/8771952?utm_source=chatgpt.com "nf-core/pacsomatic: a scalable somatic analytic pipeline using PacBio HiFi data | Bioinformatics | Oxford Academic"

