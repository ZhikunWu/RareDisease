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


