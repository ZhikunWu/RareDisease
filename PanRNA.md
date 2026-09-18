# data

[2025_LR-RNA-seq_GeneAnnotationBias_NatComm](https://github.com/Mele-Lab/2025_LR-RNA-seq_GeneAnnotationBias_NatComm/tree/master)

[TRAILS](https://github.com/juninamo/TRAILS): Long-read sequencing for 29 immune cell subsets reveals disease-linked isoforms
PRJDB17558

A systematic benchmark of Nanopore long-read RNA sequencing for transcript-level analysis in human cell lines
https://www.ebi.ac.uk/ena/browser/view/PRJEB44348

High-throughput RNA isoform sequencing using programmed cDNA concatenation
https://github.com/broadinstitute/mas-seq-paper-data

Sources of gene expression variation in a globally diverse human cohort
https://www.ncbi.nlm.nih.gov/bioproject/PRJNA851328

Mapping medically relevant RNA isoform diversity in the aged human frontal cortex with deep long-read RNA-seq
https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1008058



# Haplotype-aware pantranscriptome analyses using spliced pangenome graphs

# PanGraphRNA An efficient and flexible bioinformatics platform for graph pangenome‐based RNA‐seq data analysis

# pangenome graph如何转为可以用于RNA比对的spliced pangenome graph
将普通的 **Pangenome Graph**（通常基于 DNA 序列构建，包含 SNP 和 SV）转换为 **Spliced Pangenome Graph**，核心在于**将已知的转录组结构（外显子和内含子剪接位点）整合进图形结构中**。

如果不进行这一步，长读段 RNA-seq 比对工具（如 `vg mpmap`）将无法识别跨越巨大内含子间隙的 Read，从而将其误判为结构变异或比对失败。

以下是实现这一转换的标准流程，主要使用 `vg` (Variation Graph toolkit) 工具链：



### 1. 核心逻辑与算法
要生成支持剪接的图形，必须执行以下操作：
1.  **坐标映射**：将线性参考基因组的 GFF/GTF 注释信息投影到图的路径（Paths）上。
2.  **节点拆分**：如果剪接位点（Splice Site）出现在一个长节点的中间，必须在该位点处断开节点，以创建新的边缘（Edges）。
3.  **插入剪接边**：在代表同一个转录本的相邻外显子节点之间增加跨越内含子的直接边缘（Spliced Edges）。




### 2. 具体操作步骤 (使用 `vg`)

#### Step 1: 准备输入文件
你需要三个基础文件：
* **Graph文件**：例如 `pangenome.xg` 或 `pangenome.vg`。
* **注释文件**：标准 `genes.gtf` 或 `genes.gff3`。
* **单倍型信息**（可选但推荐）：`pangenome.gbwt`，用于限制剪接路径仅存在于已知的单倍型中。

#### Step 2: 提取剪接位点和外显子
使用 `vg` 内置工具从 GTF 中提取信息并将其转译为图形坐标。
```bash
# 从 GTF 中提取剪接位点，需要参考序列的路径名（如 "GRCh38"）
vg rna -x pangenome.xg -g genes.gtf -n > pangenome_spliced.vg
```
*注意：此命令会自动在图上添加代表内含子剪接的新边。*

#### Step 3: 构建转录本单倍型索引 (GBWT)
为了让比对工具知道哪些剪接路径是生物学上真实的（即属于同一个转录本），需要构建转录本索引。
```bash
# 构建包含转录本路径的 GBWT 索引
vg rna -x pangenome.xg -g genes.gtf -p --gbwt-out transcripts.gbwt > spliced_graph.vg
```
* `-p`: 将转录本作为路径添加到图中。
* `--gbwt-out`: 生成用于比对的单倍型索引。

#### Step 4: 建立比对索引 (Distance & Minimizer)
`vg mpmap`（用于 RNA 比对）需要距离索引来加速计算。
```bash
# 1. 建立剪接图的 xg 索引
vg index -x spliced_graph.xg spliced_graph.vg

# 2. 建立距离索引 (Snarls)
vg snarls spliced_graph.xg > spliced_graph.snarls
vg index -s spliced_graph.snarls -x spliced_graph.xg spliced_graph.dist

# 3. 建立 Minimizer 索引 (用于快速种子匹配)
vg minimizer -g transcripts.gbwt -d spliced_graph.dist -o spliced_graph.min spliced_graph.xg
```



### 3. 如何进行比对？

一旦你拥有了上述索引（`.xg`, `.dist`, `.min`, `.gbwt`），就可以运行 `vg mpmap` 进行长读段或短读段 RNA 比对：

```bash
# -S 参数表示 Spliced alignment
vg mpmap -x spliced_graph.xg -m spliced_graph.min -d spliced_graph.dist -f rna_reads.fastq.gz -S > aligned_rna.gam
```



### 4. 关键注意事项

* **节点 ID 变化**：在执行 `vg rna` 后，原来的节点可能会被切分，节点 ID 会改变。务必在后续分析（如鉴定 TE-基因嵌合）中使用新生成的 `spliced_graph.xg` 坐标系统。
* **TE 的特殊处理**：由于你的目标是鉴定 TE 嵌合转录本，你的 GTF 文件可能只包含常规基因。如果你想让图直接支持 TE 剪接，**建议将 TE 的坐标也伪装成“外显子”加入到 GTF 文件中**，这样 `vg rna` 就会在 TE 节点和相邻基因节点间预建剪接边。

**您目前的图形规模（节点数或物种数）大概多大？如果图形非常巨大（如人类全基因组级别），我们可能需要讨论更节省内存的 `pggb` 或 `minigraph-cactus` 转换方案。**


---

# IsoQuant 的结果主要有哪些

IsoQuant 的结果可以理解为 **4个核心层级：read → transcript/isoform → gene → exon/splicing**。如果你现在做的是 **PacBio Iso-Seq / ONT 三代转录组**，最值得关注的是 `transcript_models.gtf`、`transcript_counts/tpm`、`read_info` 和各种 novel isoform 分类结果。

根据 IsoQuant 当前官方文档，主要输出如下。([Ablab][1])

### 1. 最核心：`*.transcript_models.gtf`

这是我认为你做三代转录组时**最重要的结果文件**。

例如：

```text
sample.transcript_models.gtf
```

里面是 IsoQuant 最终构建出的 transcript model，包括：

* 已知 transcript
* novel transcript
* exon 坐标
* transcript_id
* gene_id
* strand
* transcript classification
* 支持该 transcript 的 reads 等信息

也就是说，它回答：

> **这个样本最终检测到了哪些转录本？每个转录本的 exon 结构是什么？**

如果你后面要做：

```text
novel isoform
alternative splicing
transcript structure
SQANTI3
CPC2
CPAT
RNA-seq support
```

这个 GTF 都是核心输入。

---

### 2. `*.extended_annotation.gtf`

例如：

```text
sample.extended_annotation.gtf
```

它相当于：

```text
reference annotation
        +
IsoQuant发现的novel transcripts
```

因此特别适合构建：

> **样本特异性 transcriptome annotation**

官方说明中，该文件包含完整的 reference annotation 加上发现的新 transcript。([Ablab][1])

---

### 3. `*.transcript_counts.tsv`

这是**已知 reference transcript 的 read count**。

例如：

```text
transcript_id    count
ENST000001       125
ENST000002       37
...
```

用于：

* transcript abundance
* differential transcript usage
* isoform switching
* transcript-level DE
* downstream statistical analysis

---

### 4. `*.transcript_tpm.tsv`

这是 transcript-level TPM：

```text
transcript_id    TPM
ENST000001       12.53
ENST000002       4.82
...
```

如果你有多个样本，通常可以整理成：

```text
             sample1 sample2 sample3 sample4
isoform1       12.3    14.2    3.2     5.6
isoform2        2.1     1.2    8.4     9.1
```

然后用于：

* PCA
* clustering
* heatmap
* isoform expression
* isoform switching
* correlation

IsoQuant 官方也提供 combined transcript count/TPM 矩阵。([Ablab][1])

---

### 5. `*.gene_counts.tsv`

gene-level read counts。

也就是：

```text
gene
 ↓
多个 transcript
 ↓
gene count
```

例如：

```text
gene_id       count
GENE1         1250
GENE2          823
GENE3          421
```

---

### 6. `*.gene_tpm.tsv`

gene-level TPM。

因此最基本的表达结果实际上有四个：

```text
gene_counts.tsv
gene_tpm.tsv

transcript_counts.tsv
transcript_tpm.tsv
```

可以简单理解成：

| 层级         | Count               | TPM              |
| ---------- | ------------------- | ---------------- |
| Gene       | `gene_counts`       | `gene_tpm`       |
| Transcript | `transcript_counts` | `transcript_tpm` |

---

# 7. `*.read_info.tsv.gz`

这个文件非常重要，但很多人容易忽略。

它是：

> **每一条 long read 到 gene/transcript 的 assignment 信息。**

官方当前格式包含例如：

```text
read_id
chr
strand
gene_id
gene_assignment_type
isoform_id
isoform_assignment_type
assignment_events
classification
exons
polyA
```

([Ablab][2])

所以它可以回答：

> **某一条 PacBio/ONT read 到底支持哪个 transcript？**

例如：

```text
read001 → GeneA → TranscriptA.1
read002 → GeneA → TranscriptA.1
read003 → GeneA → TranscriptA.3
read004 → GeneA → novel_isoform_001
```

这对于验证 novel isoform 很有价值。

---

# 8. `*.transcript_model_reads.tsv.gz`

这个文件和上面的 `read_info` 有一点区别。

它记录：

> **哪些 reads 支持 IsoQuant 最终构建出的 transcript model。**

例如：

```text
transcript_model_001
    ├── read001
    ├── read005
    ├── read008
    └── read023
```

所以你可以进一步计算：

```text
novel_isoform
       ↓
supporting reads
       ↓
read number
       ↓
isoform confidence
```

官方将这个文件定义为 discovered transcript models 与 reads 的对应关系。([Ablab][1])

---

# 9. Novel transcript classification

这是 IsoQuant 非常值得关注的一部分。

例如：

```text
FSM
ISM
NIC
NNC
intergenic
antisense
genic genomic
```

它和 SQANTI 的分类思想类似。

大致可以理解：

| 类型            | 含义                      |
| ------------- | ----------------------- |
| FSM           | Full Splice Match       |
| ISM           | Incomplete Splice Match |
| NIC           | Novel In Catalog        |
| NNC           | Novel Not in Catalog    |
| intergenic    | 基因间                     |
| antisense     | 反义                      |
| genic genomic | 基因区域内但结构异常              |

因此你可以统计：

```text
Known isoforms
      ↓
FSM / ISM

Novel isoforms
      ↓
NIC / NNC / intergenic / antisense
```

IsoQuant 也可以输出 SQANTI-like classification。([Ablab][3])

---

# 10. `*.novel_vs_known.SQANTI-like.tsv`

如果运行：

```bash
--sqanti_output
```

会得到类似：

```text
sample.novel_vs_known.SQANTI-like.tsv
```

这个非常适合你后面分析：

> **novel isoform 与哪个已知 transcript 最相似，以及到底发生了什么结构变化。**

例如：

```text
novel transcript
       ↓
similar reference transcript
       ↓
alternative exon
       ↓
novel splice site
       ↓
novel 5'/3' end
```

官方说明中，该文件用于比较 discovered novel transcripts 与 reference transcripts。([Ablab][2])

---

# 11. PolyA site

IsoQuant 还可以输出：

```text
*.polyA_prediction.tsv
```

用于预测：

> **transcript 的 polyadenylation site**

这对于 PacBio Iso-Seq 特别有价值。

例如：

```text
GeneA
 ├── isoform1 ──────── polyA1
 ├── isoform2 ───────────── polyA2
 └── isoform3 ─────────────────── polyA3
```

因此可以研究：

**alternative polyadenylation (APA)**。

如果使用 full-length transcript 数据，还可以得到：

```text
*.TSS_prediction.tsv
```

用于 TSS prediction。([Ablab][1])

---

# 12. 如果开启 `exon_quantification`

还会得到：

```text
exon_counts.tsv
exon_splice_site_counts.tsv
splice_junction_counts.tsv
intron_retention_counts.tsv
```

这部分特别适合研究：

### Alternative splicing

```text
exon skipping
alternative 5' splice site
alternative 3' splice site
intron retention
```

以及：

### Splice junction

例如：

```text
chr1:1000-1200
chr1:1000-1500
chr1:1100-1500
```

可以直接进行 splice-junction level 分析。([Ablab][1])

---

# 13. 如果是多样本

IsoQuant 还能生成：

```text
combined_gene_counts.tsv
combined_gene_tpm.tsv

combined_transcript_counts.tsv
combined_transcript_tpm.tsv
```

因此多个 PacBio/ONT 样本可以直接得到：

```text
                 Sample1 Sample2 Sample3 Sample4
GeneA             1250    1432    832     921
GeneB              325     421    512     632

IsoformA.1         532     612    321     422
IsoformA.2         421     523    210     321
IsoformA.3         297     297    301     178
```

([Ablab][1])

---

# 14. 如果你现在是 PacBio Iso-Seq，我建议重点保留这些

如果你的目的就是**发现新的全长转录本/isoform**，我建议至少保留：

```text
├── sample.transcript_models.gtf       ★★★★★
├── sample.extended_annotation.gtf     ★★★★★
├── sample.transcript_model_reads.tsv.gz ★★★★★
├── sample.read_info.tsv.gz            ★★★★
├── sample.discovered_transcript_counts.tsv ★★★★
├── sample.discovered_transcript_tpm.tsv    ★★★★
├── sample.transcript_counts.tsv       ★★★
├── sample.transcript_tpm.tsv          ★★★
├── sample.gene_counts.tsv              ★★★
├── sample.gene_tpm.tsv                 ★★★
├── sample.polyA_prediction.tsv         ★★★
└── sample.TSS_prediction.tsv           ★★★
```

其中最核心的是：

> **`transcript_models.gtf + transcript_model_reads.tsv.gz + discovered_transcript_tpm.tsv`**

这三个文件基本可以构成你的 **IsoQuant novel isoform 分析核心结果**。

---

## 15. 如果你后面准备做“新转录本发现”，推荐完整流程

对于你这种三代转录组数据，我会建议：

```text
PacBio HiFi / Iso-Seq
        │
        ↓
     BAM/FASTQ
        │
        ↓
     IsoQuant
        │
        ├───────────────┐
        ↓               ↓
transcript_models.gtf   read_info
        │               │
        ↓               ↓
  novel isoforms    supporting reads
        │
        ↓
     SQANTI3
        │
        ├── FSM
        ├── ISM
        ├── NIC
        ├── NNC
        ├── RT-switching
        ├── antisense
        └── intergenic
        │
        ↓
  novel isoform filtering
        │
        ├── ≥2/3 supporting reads
        ├── splice junction support
        ├── short-read support
        ├── CAGE/TSS support
        ├── PolyA support
        └── biological relevance
        │
        ↓
   high-confidence
   novel isoforms
```

尤其需要注意：**IsoQuant 的 `transcript_models.gtf` 不是简单的“所有检测到的 read 的集合”**，而是经过 transcript model construction/filtering 后形成的模型；因此后续做 novel isoform 数量统计时，要区分 **raw read-level evidence、constructed transcript model 和最终过滤后的 high-confidence isoform**。([Ablab][1])

如果你现在手里已经有 **IsoQuant 输出目录**，你可以把 `ls -lh` 的结果贴出来，我可以直接逐个告诉你**每个文件是什么、哪些必须保留、哪些可以删除，以及最终做 novel isoform/alternative splicing 应该用哪些文件**。

[1]: https://ablab.github.io/IsoQuant/output.html?utm_source=chatgpt.com "Output files - IsoQuant"
[2]: https://ablab.github.io/IsoQuant/formats.html?utm_source=chatgpt.com "File formats - IsoQuant"
[3]: https://ablab.github.io/IsoQuant/cmd.html?utm_source=chatgpt.com "Command line options - IsoQuant"


---

# 首先如何合并所有样本的isoform，然后按单个样本检测每个isoform的表达量或者tpm

可以。对于 **100 个样本已经分别经过 IsoQuant 分析**的情况，你要做的实际上是建立一个：

> **Population-level non-redundant transcriptome annotation（群体水平非冗余转录组注释）**

即把：

**100 个样本 IsoQuant 发现的 transcript models + 原始参考 GTF**

整合成一个新的、统一的 `population_reference.gtf`。

这里有一个关键点：**不要直接 `cat` 100 个 GTF，也不要直接用 gffcompare 的输出当最终 GTF**。更稳妥的流程是“样本内结果汇总 → transcript model 去冗余 → 与 reference GTF 比对/分类 → 建立统一 transcript ID → 过滤低可信 novel transcript → 生成新的 GTF”。

---

# 一、推荐的总体流程

假设：

```text
100个样本
    │
    ├── S01/transcript_models.gtf
    ├── S02/transcript_models.gtf
    ├── ...
    └── S100/transcript_models.gtf
              │
              ↓
      合并所有 IsoQuant GTF
              │
              ↓
       transcript model 去冗余
              │
              ↓
       population transcriptome
              │
              ↓
      与 reference GTF 比较
              │
       ┌──────┴──────┐
       ↓             ↓
    known          novel
       │             │
       │       ┌─────┴─────┐
       │       ↓           ↓
       │    high-conf    low-conf
       │       novel
       │
       └───────┬───────────┘
               ↓
     population_reference.gtf
```

最终：

```text
reference.gtf
       +
100 samples IsoQuant
       ↓
population_reference.gtf
```

---

# 二、首先要明确：你想建立哪一种“新参考 GTF”

实际上有两种。

## 类型 A：Reference + Novel

这是我最推荐的。

```text
new_reference.gtf
│
├── 原始 reference transcripts
│
└── high-confidence novel transcripts
```

例如：

```text
GENE1
 ├── ENST000001
 ├── ENST000002
 ├── NOVEL000001
 └── NOVEL000002
```

这种适合：

* 后续100个样本重新定量
* transcript expression
* isoform usage
* isoQTL
* sQTL
* APA
* population transcriptome

---

## 类型 B：只保留100个样本实际观察到的 transcript

即：

```text
population_observed.gtf
```

例如 reference 中有：

```text
GENE1
 ├── iso1
 ├── iso2
 ├── iso3
 ├── iso4
 └── iso5
```

但100个人只观察到：

```text
iso1
iso2
iso4
novel1
novel2
```

那么最终只保留：

```text
iso1
iso2
iso4
novel1
novel2
```

这个更适合研究：

> **population transcriptome diversity**

---

# 三、第一步：收集100个 IsoQuant GTF

假设目录：

```bash
project/
├── S01/
│   └── S01.transcript_models.gtf
├── S02/
│   └── S02.transcript_models.gtf
...
└── S100/
    └── S100.transcript_models.gtf
```

先建立列表：

```bash
find project/ \
    -name "*.transcript_models.gtf" \
    > isoquant_gtf.list
```

检查：

```bash
wc -l isoquant_gtf.list
```

应该：

```text
100
```

---

# 四、第二步：先合并100个 GTF

可以先简单合并：

```bash
cat $(cat isoquant_gtf.list) > all_isoquant.gtf
```

但是：

> **这个文件只是临时文件，不是最终 GTF。**

因为不同样本可能产生相同结构但不同 transcript ID。

例如：

```text
S01:
PB.1.1

S02:
PB.23.4

S03:
PB.8.2
```

实际上三者可能是：

```text
chr1:1000-2000
exon1:1000-1100
exon2:1500-1600
exon3:1900-2000
```

即完全相同的 transcript model。

所以必须进行 **collapse / deduplication**。

---

# 五、第三步：非常推荐使用 TAMA 做跨样本 transcript collapse

如果你的目标是建立真正的 **population transcriptome GTF**，TAMA 是非常合适的工具之一。

基本思想：

```text
100 IsoQuant GTF
       ↓
TAMA collapse
       ↓
non-redundant transcript models
```

尤其适合：

* PacBio Iso-Seq
* ONT long RNA
* 多样本
* transcript model collapse
* alternative TSS
* alternative polyA
* transcript end variation

---

# 六、不过这里有一个非常重要的问题

**不要把 reference GTF 和 100 个 IsoQuant GTF 一上来直接 collapse。**

建议分开：

```text
                    ┌── reference.gtf
                    │
100 IsoQuant GTF ───┤
                    ↓
              novel transcript
              identification
```

也就是：

### 第一步

100个样本内部：

```text
100 IsoQuant GTF
      ↓
collapse
      ↓
population IsoQuant GTF
```

### 第二步

```text
population IsoQuant GTF
          +
reference GTF
          ↓
classification
```

### 第三步

```text
known + high-confidence novel
          ↓
new reference GTF
```

这样结构更清楚。

---

# 七、第四步：用 gffcompare 和 reference GTF 比较

这是非常重要的一步。

例如：

```bash
gffcompare \
    -r reference.gtf \
    -o population \
    population_collapsed.gtf
```

会产生：

```text
population.annotated.gtf
population.tracking
population.stats
population.tmap
```

其中：

```text
population.tmap
```

特别重要。

它可以告诉你：

```text
query transcript
       ↓
reference transcript
       ↓
class code
```

---

# 八、重点关注 gffcompare 的 class code

例如：

```text
=
```

表示：

> 与 reference transcript 完全匹配

---

```text
c
```

表示：

> query transcript 包含于 reference transcript / 与已知 transcript 有包含关系

---

```text
j
```

表示：

> 与 reference 有 splice junction match，但结构不是完全一致

---

```text
i
```

表示：

> intronic transcript

---

```text
u
```

表示：

> intergenic transcript

---

```text
x
```

表示：

> antisense transcript

---

所以你可以建立：

```text
Known
 ├── =
 ├── c
 └── j

Novel
 ├── j
 ├── i
 ├── u
 └── x
```

但这里不要机械地把所有 `j/i/u/x` 都当作高可信 novel isoform。

---

# 九、对于100样本，我建议这样定义 High-confidence Novel Isoform

这是整个流程的核心。

例如：

```text
Novel transcript
      │
      ├── ≥3 supporting reads
      │
      ├── ≥2 independent samples
      │
      ├── canonical splice junction
      │
      ├── not identical to reference
      │
      ├── not obvious fragment
      │
      └── SQANTI3 PASS
              │
              ↓
       High-confidence
       novel isoform
```

如果你的数据深度比较高，可以进一步要求：

```text
≥5 supporting reads
```

和：

```text
≥3 individuals
```

---

# 十、为什么一定要加入“样本数”这个条件？

因为你有100个人。

这是非常大的优势。

比如：

### Novel A

```text
100 individuals
85 individuals detected
```

可信度很高。

---

### Novel B

```text
100 individuals
15 individuals detected
```

也可能是真实的低频 isoform。

---

### Novel C

```text
100 individuals
1 individual
1 read
```

很可能是：

* sequencing artifact
* mapping artifact
* incomplete transcript
* random splice
* RT artifact

所以100个样本实际上可以把 novel transcript 分成：

```text
Common
≥50%

Intermediate
10–50%

Rare
1–10%

Private
1%
```

这对于后面的 population transcriptome 分析非常有价值。

---

# 十一、第五步：SQANTI3 建议放在这里

我非常建议：

```text
100 IsoQuant
      ↓
collapse
      ↓
population transcriptome
      ↓
gffcompare
      ↓
SQANTI3
```

SQANTI3 对 novel isoform 做：

* FSM
* ISM
* NIC
* NNC
* antisense
* intergenic
* RT switching
* splice junction QC
* junction support
* ORF
* transcript structural classification

然后：

```text
SQANTI3
    ↓
filter
    ↓
high-confidence novel transcripts
```

这一步对于建立“新参考 GTF”非常重要。

---

# 十二、第六步：建立统一 transcript ID

这个一定要做。

不要最终 GTF 里出现：

```text
S01.PB.1.1
S02.PB.1.1
S03.PB.2.3
```

应该统一成：

```text
ENST...
```

或者：

```text
NOVEL000001
NOVEL000002
NOVEL000003
```

例如：

```text
GENE0001
 ├── ENST000001
 ├── ENST000002
 ├── NOVEL000001
 └── NOVEL000002
```

同时建议建立一个 annotation table：

```text
transcript_id
gene_id
source
class
reference_transcript
n_samples
supporting_reads
chromosome
start
end
strand
```

例如：

| transcript | type  | reference | samples | reads |
| ---------- | ----- | --------- | ------: | ----: |
| ENST001    | known | ENST001   |     100 | 15000 |
| NOVEL001   | NIC   | ENST002   |      63 |   420 |
| NOVEL002   | NNC   | NA        |      21 |    87 |
| NOVEL003   | NNC   | NA        |       3 |    12 |

这张表后面非常有用。

---

# 十三、第七步：最终构建新的 GTF

最终：

```text
new_reference.gtf
```

包含：

```text
reference known transcripts
        +
high-confidence novel transcripts
```

例如：

```text
chr1 source transcript 1000 5000 . + . gene_id "GENE1"; transcript_id "ENST001";

chr1 source exon      1000 1500 . + . gene_id "GENE1"; transcript_id "ENST001";
chr1 source exon      2000 2500 . + . gene_id "GENE1"; transcript_id "ENST001";

chr1 IsoQuant transcript 1000 6000 . + . gene_id "GENE1"; transcript_id "NOVEL000001";

chr1 IsoQuant exon      1000 1500 . + . gene_id "GENE1"; transcript_id "NOVEL000001";
chr1 IsoQuant exon      1800 2200 . + . gene_id "GENE1"; transcript_id "NOVEL000001";
chr1 IsoQuant exon      3000 6000 . + . gene_id "GENE1"; transcript_id "NOVEL000001";
```

---

# 十四、然后一定要做 GTF QC

至少检查：

### transcript 数量

```bash
grep -w transcript new_reference.gtf | wc -l
```

### gene 数量

```bash
grep -w transcript new_reference.gtf \
| sed 's/.*gene_id "\([^"]*\)".*/\1/' \
| sort -u | wc -l
```

### exon 数量

```bash
grep -w exon new_reference.gtf | wc -l
```

---

# 十五、最重要的验证：重新定量100个样本

建立：

```text
new_reference.gtf
```

之后，不要马上拿它做最终群体分析。

应该：

```text
                 new_reference.gtf
                       │
          ┌────────────┼─────────────┐
          ↓            ↓             ↓
         S01          S02          S100
          │            │             │
          └────────────┼─────────────┘
                       ↓
                   IsoQuant
                       ↓
          transcript count / TPM
```

重新跑100个样本。

这样最终得到：

```text
transcript_count_matrix
transcript_TPM_matrix
```

此时：

> **100个样本的每个 transcript 都对应同一个统一 transcript ID。**

这才是真正可以进入群体分析的数据。

---

# 十六、我建议你最终做两个版本的 GTF

非常重要。

### GTF 1：`population_reference.full.gtf`

```text
reference
+
all reasonably supported novel isoforms
```

用于探索。

---

### GTF 2：`population_reference.highconf.gtf`

```text
reference
+
high-confidence novel isoforms
```

用于正式统计分析。

例如：

```text
high-confidence:
≥3 reads
≥2 samples
canonical splice
SQANTI PASS
```

---

# 十七、对于你的100个样本，我更推荐下面这个实际流程

```text
                 100 samples
                      │
                      ↓
            IsoQuant transcript_models
                      │
                      ↓
              sample GTF aggregation
                      │
                      ↓
             TAMA / transcript collapse
                      │
                      ↓
        population_nonredundant.gtf
                      │
                      ↓
              gffcompare -r
                      │
          ┌───────────┴────────────┐
          ↓                        ↓
       Known                     Novel
          │                        │
       = / c / j             j / i / u / x
                                   │
                                   ↓
                              SQANTI3
                                   │
                       ┌───────────┴───────────┐
                       ↓                       ↓
                  high-confidence          low-confidence
                       │
                       ↓
                population novel
                       │
                       ↓
        reference GTF + novel GTF
                       │
                       ↓
          population_reference.gtf
                       │
                       ↓
              100 samples IsoQuant
                 re-quantification
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
        Count matrix          TPM matrix
             │                   │
             └─────────┬─────────┘
                       ↓
             population analysis
```

---

## 十八、但有一个关键技术选择需要先确定

你现在说的“100个样本 IsoQuant 结果”，**如果这100个样本使用的是同一个 reference GTF**，那么我建议流程可以明显简化：

> **已知 transcript 不需要重新 collapse，只需要针对 novel transcript 做跨样本合并；然后将 novel transcript 与 reference GTF 合并。**

这种方式通常比把 **reference + 100个GTF全部重新 collapse** 更安全，因为不会无意间改变原始 reference transcript 的 ID 和结构。

而如果你的100个样本是：

> **没有 reference GTF 的 de novo IsoQuant transcript discovery**

那么就应该走完整的：

**100 GTF → collapse → reference comparison → SQANTI3 → filtering → new GTF**

路线。

**如果你把一个样本的 `transcript_models.gtf` 前20行 + 你的原始 reference GTF 前20行贴出来，并告诉我 IsoQuant 版本（例如 v3.8/v3.9），我可以直接按照你的实际 GTF 格式给你写一套可批量运行的 Bash 流程，从 `100个GTF → 合并 → 去冗余 → gffcompare → SQANTI3 → high-confidence novel isoform → new_reference.gtf → 重新定量100样本`，包括每一步具体命令。**


# isoquant结果的extended_annotation.gtf和transcript_models.gtf有什么区别

在 **IsoQuant** 中，这两个 GTF 都和转录本结构有关，但用途不同。对于你现在 **100 个样本合并、建立 population-level reference GTF** 的任务，这个区别尤其重要。

### 1. `transcript_models.gtf`：IsoQuant 最终建立的转录本模型

这是你最应该关注的文件。

它描述的是 IsoQuant 根据 reads 识别/定量得到的**非冗余 transcript model**，通常包含：

* 已知参考转录本
* IsoQuant 识别到的 novel transcript
* exon 结构
* transcript/gene ID
* 一些 IsoQuant 注释属性

典型结构类似：

```text
chr1  IsoQuant  transcript  1000  5000  .  +  .  gene_id "GENE1"; transcript_id "GENE1-001";
chr1  IsoQuant  exon       1000  1500  .  +  .  gene_id "GENE1"; transcript_id "GENE1-001"; exon_number "1";
chr1  IsoQuant  exon       2000  3000  .  +  .  gene_id "GENE1"; transcript_id "GENE1-001"; exon_number "2";
chr1  IsoQuant  exon       4000  5000  .  +  .  gene_id "GENE1"; transcript_id "GENE1-001"; exon_number "3";
```

所以可以把它理解为：

> **“IsoQuant 最终认为这个样本中有哪些 transcript model。”**

---

### 2. `extended_annotation.gtf`：在原始 annotation 基础上扩展出来的注释

`extended_annotation.gtf` 更偏向于**注释扩展/兼容性注释**。

它通常是在原始 reference GTF 的基础上，加入 IsoQuant 识别出来的新 transcript/gene model，使得后续分析能够把：

```text
Reference transcript
        +
IsoQuant novel transcript
```

放在一个 annotation 中。

也就是说，它更接近：

> **“原来的参考注释 + IsoQuant发现的额外转录本模型。”**

因此它通常比单纯的 `transcript_models.gtf` 更适合用于需要“扩展后的完整 annotation”的场景。

---

## 两者最核心的区别

可以简单记成：

| 文件                        | 核心含义                                 | 主要用途                         |
| ------------------------- | ------------------------------------ | ---------------------------- |
| `transcript_models.gtf`   | IsoQuant 建立的 transcript models       | 看 IsoQuant 识别了哪些转录本          |
| `extended_annotation.gtf` | reference annotation + IsoQuant 扩展模型 | 建立扩展后的 transcript annotation |
| reference.gtf             | 原始参考注释                               | 基础注释                         |

关系可以理解成：

```text
                 Reference GTF
                      │
                      │
                 IsoQuant
                      │
          ┌───────────┴───────────┐
          │                       │
 transcript_models.gtf    extended_annotation.gtf
          │                       │
          │                 Reference annotation
          │                       +
          │                 novel models
          │
      transcript models
```

---

# 对你现在100个样本的情况，应该用哪个？

这个问题非常关键。

如果你的目标是：

> **100个样本的 IsoQuant 结果 → 合并 → 和 reference GTF 整合 → 建立 population-level 新参考 GTF**

我建议**不要直接把100个 `extended_annotation.gtf` 简单 `cat` 在一起**。

也不建议直接把100个 `transcript_models.gtf` 全部 `cat` 后就当成新 reference。

更合理的是：

```text
100 samples
   │
   ├── sample1 transcript_models.gtf
   ├── sample2 transcript_models.gtf
   ├── ...
   └── sample100 transcript_models.gtf
             │
             ↓
       提取 novel transcripts
             │
             ↓
     跨样本 transcript collapse
             │
             ↓
      population novel GTF
             │
             ↓
Reference GTF ─────────┐
                       │
                       ↓
            population_reference.gtf
```

### 为什么？

因为100个样本里面很可能出现这种情况：

```text
Sample1:
chr1:1000-2000-3000

Sample2:
chr1:1000-2000-3000

Sample3:
chr1:1000-2000-3050

Sample4:
chr1:1000-2000-3000
```

如果直接合并：

```text
transcript_A
transcript_B
transcript_C
transcript_D
```

实际上可能只是：

```text
一个共同的 isoform
        +
一个不同3'端的 isoform
```

所以需要先进行 **cross-sample transcript model collapsing / clustering**。

---

# 还有一个非常重要的情况

如果你这100个样本当初都是：

```bash
isoquant.py \
    --reference_gtf reference.gtf \
    ...
```

也就是说**全部使用完全相同的 reference GTF**，那么事情会简单很多。

这时候：

### 已知 transcript

一般不需要重新 collapse。

例如：

```text
reference.gtf

ENST000001
ENST000002
ENST000003
```

100个样本中这些已知 transcript 的 ID/结构本来就是统一的。

真正需要跨样本整合的是：

```text
Sample1 novel transcript
Sample2 novel transcript
Sample3 novel transcript
...
Sample100 novel transcript
```

所以最推荐的策略是：

```text
Reference GTF
     │
     ├── 保留全部 reference transcripts
     │
     │
100 × IsoQuant
transcript_models.gtf
     │
     ↓
提取 novel transcripts
     │
     ↓
跨样本 collapse
     │
     ↓
population novel transcripts
     │
     ↓
Reference + population novel
     │
     ↓
population_reference.gtf
```

---

# `extended_annotation.gtf` 在这里有什么作用？

它可以作为一个很有价值的**辅助结果**。

例如你可以检查：

```bash
grep -v '^#' sample1.extended_annotation.gtf | head
```

以及：

```bash
grep -v '^#' sample1.transcript_models.gtf | head
```

然后比较它们的：

```text
gene_id
transcript_id
source
attributes
```

尤其是 novel transcript 的 ID 和 annotation 信息。

---

## 我更建议你现在先不要急着合并

因为 **IsoQuant 不同版本、不同参数以及是否使用 reference GTF 时，这两个文件中的 attributes 和 transcript 来源可能有所不同**。

你现在最有价值的是把下面两个文件各贴 **20行左右**：

```bash
head -20 sample1.transcript_models.gtf
```

和

```bash
head -20 sample1.extended_annotation.gtf
```

最好再给我：

```bash
grep -v '^#' sample1.transcript_models.gtf | head -5
grep -v '^#' sample1.extended_annotation.gtf | head -5
```

**我可以根据你实际的 IsoQuant GTF 格式，直接给你设计一套 100 个样本的完整流程：**

```text
100 × transcript_models.gtf
          ↓
识别 known / novel
          ↓
100样本 novel transcript collapse
          ↓
统计每个isoform的
supporting reads + sample数
          ↓
高可信 novel isoform筛选
          ↓
Reference GTF + novel GTF
          ↓
population_reference.gtf
          ↓
重新对100个样本统一定量
          ↓
100 × transcript × sample
expression matrix
```

这样最终得到的 GTF 才真正适合作为你后面做 **population-level isoform expression、isoform usage、alternative splicing、eQTL/isoQTL** 的统一参考。


