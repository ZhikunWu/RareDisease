
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

