# 如何识别microexon

Microexon（微外显子）的识别，核心是从 RNA-seq 或长读长转录组数据中，准确鉴定长度通常为 3–30 bp 的短外显子，确定其基因组位置、剪接位点、表达量及不同样本中的包含比例（PSI）。

与普通可变剪接（Alternative Splicing，AS）相比，microexon 识别有几个特殊难点：

1. 长度非常短：部分 microexon 只有 3–6 bp，常规 RNA-seq 比对和剪接分析容易漏检。
2. 剪接位点识别困难：需要准确识别 microexon 两侧的剪接连接（splice junctions）。
3. 低表达和组织特异性：很多 microexon 只在特定组织、发育阶段或细胞类型中表达。
4. 转录本组合复杂：同一个基因可能存在多个 microexon 组合，需要确定其真实转录本结构。

因此，建议将 microexon 识别分为三个层次：

- 已知 microexon 的鉴定与定量。
- 新型 microexon 的发现。
- microexon 的差异剪接及功能分析。

## 一、常用的 microexon 识别工具

目前比较值得使用的工具如下。

| 工具            | 数据类型                   | 主要功能               | 推荐用途                    |
| ------------- | ---------------------- | ------------------ | ----------------------- |
| MicroExonator | Bulk RNA-seq、scRNA-seq | 从头鉴定及定量microexon   | 首选，尤其是新型microexon       |
| VAST-TOOLS    | Bulk RNA-seq           | 已知可变剪接及microexon定量 | 已知microexon的PSI分析       |
| OLego         | Short-read RNA-seq     | 特殊剪接比对，增强微外显子发现能力  | 辅助鉴定新型microexon         |
| Whippet       | Bulk RNA-seq           | 可变剪接事件识别和差异分析      | microexon差异剪接分析，需先建立事件图 |
| IsoQuant      | Long-read RNA-seq      | 转录本重建、定量和新型剪接异构体鉴定 | 长读长转录本中的microexon识别     |
| SQANTI3       | Long-read RNA-seq      | 转录本结构质控和分类         | 对microexon所在转录本进行验证     |

这里需要特别区分：

- MicroExonator和VAST-TOOLS是针对microexon问题设计的工具。
- IsoQuant和SQANTI3是长读长转录本分析工具，并非专门的microexon识别软件。
- rMATS、MAJIQ等常规剪接工具可以用于补充分析，但不建议单独依赖它们发现极短外显子。

MicroExonator采用特殊的剪接连接序列比对策略，可以发现普通比对流程容易遗漏的新型microexon。其研究在小鼠中鉴定到2,984个microexon，其中部分此前没有被注释。

[image](https://www.google.com/s2/favicons?domain=https://pmc.ncbi.nlm.nih.gov\&sz=32)

PMC+2



## 二、基于短读长 RNA-seq 识别 microexon

推荐方案：MicroExonator

如果有 Illumina RNA-seq 数据，建议使用 MicroExonator 作为主要分析工具。

### Step 1. 数据准备

需要准备：

- RNA-seq FASTQ文件。
- 参考基因组，例如 hg38。
- 基因注释文件，例如 GENCODE GTF。
- 已知microexon数据库（可选，例如 VASTDB）。

### Step 2. MicroExonator 从头鉴定

其基本原理是：

普通剪接连接

上游外显子 ───────── 下游外显子

常规剪接，两个外显子直接连接

Microexon 剪接连接

上游外显子 ── microexon（3–30 bp）── 下游外显子

同时存在上游与下游两侧的剪接连接证据

核心识别策略

1. 建立已知外显子之间的剪接连接序列库。
2. 将RNA-seq reads比对到剪接连接序列库。
3. 搜索无法直接匹配的短序列片段。
4. 将短序列重新定位到基因组内含子区域。
5. 检查候选区域两侧的剪接位点。
6. 综合reads支持数、剪接信号和序列保守性筛选microexon。

[image](https://www.google.com/s2/favicons?domain=https://pmc.ncbi.nlm.nih.gov\&sz=32)

PMC+1

### Step 3. 运行和参数设置

MicroExonator采用Snakemake工作流。建议根据官方文档准备配置文件。

官方资源：

- [MicroExonator官方文档](https://microexonator.readthedocs.io/en/latest/?utm_source=chatgpt.com)
- [安装与环境配置](https://microexonator.readthedocs.io/en/latest/setup.html?utm_source=chatgpt.com)
- [Discovery and Quantification](https://microexonator.readthedocs.io/en/latest/discovery_and_quantification.html?utm_source=chatgpt.com)
- [GitHub源码](https://github.com/hemberg-lab/MicroExonator?utm_source=chatgpt.com)

重点参数：

| 参数               | 推荐设置         | 说明                                  |
| ---------------- | ------------ | ----------------------------------- |
| `min_reads_PSI`  | 5            | 支持新型microexon的最少reads数，样本量充足时建议5或更高 |
| `ME_DB`          | VASTDB BED12 | 已知microexon数据库，可选                   |
| `paired_samples` | 按实际情况        | 配对样本信息                              |
| `Single_Cell`    | T/F          | 是否进行单细胞分析                           |

其中，`min_reads_PSI`默认值为3；如果有足够测序深度和样本，建议提高至5或以上。

[image](https://www.google.com/s2/favicons?domain=https://microexonator.readthedocs.io\&sz=32)

MicroExonator documentation+1



### Step 4. 结果提取

重点关注以下两个结果文件：

| 文件                        | 作用                   |
| ------------------------- | -------------------- |
| `out.high_quality.txt`    | 高可信度microexon鉴定结果    |
| `out_filtered_ME.PSI.txt` | 各样本microexon PSI定量结果 |

`out.high_quality.txt`中值得关注的字段：

- `ME`：microexon基因组坐标。
- `Transcript`：所属转录本。
- `Total_coverage`：支持microexon的总覆盖情况。
- `ME_length`：microexon长度。
- `ME_seq`：microexon序列。
- `U2_score`：U2剪接信号评分。
- `Mean_conservation`：保守性评分。
- `ME_type`：microexon类型。

这些信息可以用于后续建立microexon注释数据库。

## 三、基于长读长 RNA-seq 识别 microexon

特别推荐用于全长转录本研究

如果使用 PacBio HiFi Iso-Seq、ONT cDNA 或 direct RNA sequencing，识别策略与短读长有所不同。

长读长的优势是：一条read可以跨越上游外显子、microexon和下游外显子，能够直接提供完整的剪接结构证据。

### Step 1. 长读长数据比对

推荐使用 minimap2。

PacBio HiFi Iso-Seq数据示例：

```
minimap2 \
    -ax splice:hq \
    -uf \
    --secondary=no \
    -t 32 \
    hg38.fa \
    sample.flnc.fastq.gz \
    | samtools sort -@ 16 -o sample.sorted.bam

samtools index sample.sorted.bam
```

注意：

- `-ax splice:hq`：针对高准确度长读长剪接比对。
- `-uf`：针对正向转录本方向的剪接比对模式。
- `--secondary=no`：不输出secondary alignment。
- `-t 32`：使用32线程。

对于其他类型的ONT数据，需要根据数据类型选择合适的minimap2剪接比对模式，不能直接照搬PacBio参数。

### Step 2. 转录本组装和鉴定

可以采用以下工具：

| 工具         | 主要功能          |
| ---------- | ------------- |
| IsoQuant   | 长读长转录本结构鉴定及定量 |
| TALON      | 新型转录本鉴定和注释    |
| FLAIR      | 转录本纠正、组装和定量   |
| StringTie2 | 长读长转录本组装      |
| SQANTI3    | 转录本质量控制和分类    |

推荐流程：

```
Long-read RNA-seq
        |
        v
minimap2
        |
        v
Sorted BAM
        |
        v
IsoQuant / TALON / FLAIR
        |
        v
Transcript GTF
        |
        v
Extract all exon coordinates
        |
        v
Filter exon length <= 30 bp
        |
        v
Candidate microexons
```

这里最重要的一点是：不要直接将所有长度小于等于30 bp的GTF exon都认定为microexon。

还需要检查其剪接结构、reads支持情况和转录本完整性。

### Step 3. 从GTF中提取microexon

假设已经获得IsoQuant输出的GTF文件。

首先提取所有exon：

```
awk '$3=="exon" {
    len=$5-$4+1;
    if(len>=3 && len<=30){
        print $1,$4,$5,$7,len,$9
    }
}' sample.gtf > candidate_microexons.txt
```

这里提取的是候选microexon，后续还需要进一步过滤。

建议增加以下条件：

1. microexon位于两个正常长度的外显子之间。
2. microexon两侧存在明确的剪接连接。
3. 两侧剪接位点具有合理的剪接信号。
4. 至少有一定数量的独立长读长支持。
5. 不属于比对错误、低复杂度序列或重复区域引起的假阳性。
6. 在不同样本或不同建库批次中具有可重复性。

对于一个高可信度microexon，理想情况下应该有完整的：

```
Exon 1 → Microexon → Exon 2
```

而不是只有一侧连接证据。

### Step 4. 计算microexon的包含比例（PSI）

microexon的核心定量指标是PSI（Percent Spliced In）。

定义：

\\[ PSI=\frac{N\_{inclusion}} {N\_{inclusion}+N\_{skipping}} \\]

其中：

- `N_inclusion`：支持microexon包含的reads。
- `N_skipping`：支持跳过microexon的reads。

长读长数据可以直接统计完整跨越三个外显子的reads，以及直接连接上下游外显子的跳跃reads。

例如：

| 样本      | 包含reads | 跳跃reads | PSI  |
| ------- | ------- | ------- | ---- |
| Sample1 | 80      | 20      | 0.80 |
| Sample2 | 30      | 70      | 0.30 |
| Sample3 | 60      | 40      | 0.60 |

如果研究疾病组与正常组的差异，可以进一步计算：

\\[ \Delta PSI=PSI\_{Case}-PSI\_{Control} \\]

建议使用生物学重复作为统计单位，而不是将每条read视为独立生物学重复。

## 四、如何提高microexon鉴定的准确性？

建议将以下证据结合起来。

| 证据      | 判断标准                       | 目的            |
| ------- | -------------------------- | ------------- |
| 长度      | 3–30 bp                    | 确定候选microexon |
| 剪接位点    | 典型GT-AG等剪接信号               | 验证剪接合理性       |
| Reads支持 | 两侧剪接连接均有支持                 | 排除错误组装        |
| 转录本结构   | 完整的上游exon-microexon-下游exon | 确定真实剪接结构      |
| 跨样本重复性  | 多个样本可重复检出                  | 提高可靠性         |
| 保守性     | 跨物种序列或剪接结构保守               | 评估功能重要性       |
| 蛋白影响    | 是否改变阅读框或氨基酸序列              | 评估潜在功能        |

还需要特别注意：3的倍数长度的microexon可能直接增加或删除氨基酸，而非3的倍数长度可能造成移码，但最终还需要结合转录本阅读框和NMD情况判断。

## 五、针对你的长读长转录组数据，我最推荐的分析方案

考虑到你已经有约100个样本的长读长RNA-seq数据，并且已经使用IsoQuant完成转录本分析，我建议不必重新从头构建整个转录组流程，而是基于现有结果开展microexon专项分析。

## 推荐的 Microexon 专项分析流程

1. 现有IsoQuant结果

   BAM、GTF、转录本表达量矩阵
2. 候选microexon提取

   提取长度3–30 bp的exon，统一基因组坐标和转录本注释
3. microexon结构验证

   检查两侧剪接位点、长读长跨越支持、比对质量及重复序列
4. 已知microexon数据库比对

   与VASTDB、GENCODE等进行交叉注释
5. microexon定量

   计算每个样本的包含reads、跳跃reads和PSI
6. 差异剪接分析

   比较疾病组与对照组，筛选差异microexon
7. 功能及遗传关联

   结合WGS/WES，分析microexon相关变异、sQTL、蛋白结构及疾病表型

建议重点关注三个创新点：

1. 新型microexon发现：长读长是否发现了GENCODE和VASTDB中没有注释的microexon？
2. 复杂剪接异构体：多个microexon是否在同一个转录本中组合出现，形成疾病特异性isoform？
3. 遗传变异调控：NDD患者中的SNV、InDel或SV是否影响microexon的剪接和包含比例？


---

# 我的数据类型：ONT RNA-seq；研究目标：发现新的microexon，并鉴定已知microexon；参考基因组：hg38。请设计完整可执行的microexon鉴定流程，包括软件、安装、参数、命令、结果过滤和PSI分析

# ONT RNA-seq 数据鉴定新型及已知 Microexon 的完整分析流程

针对你的数据类型（ONT RNA-seq）、研究目标（新型 microexon 发现 + 已知 microexon 鉴定）和参考基因组（hg38），我建议采用：

minimap2 + IsoQuant + FLAIR + SQANTI3 + 自定义microexon提取与验证 + PSI定量

其中，IsoQuant负责长读长转录本结构鉴定，FLAIR用于独立验证剪接结构，SQANTI3用于转录本质量控制，最后通过自定义脚本提取3–30 bp的候选microexon，并结合长读长reads进行定量。

需要强调一个技术问题：ONT reads的单碱基错误率、剪接位点偏移和短外显子比对不确定性，都可能造成假microexon。因此，不能简单地从GTF中筛选长度3–30 bp的exon，就认为发现了新的microexon。

我建议将结果分成三个层次：

1. Known microexons：与已有数据库或基因组注释一致的microexon。
2. Novel microexons：已有注释中不存在，但有可靠剪接连接和长读长支持的microexon。
3. High-confidence novel microexons：具有完整上下游剪接证据、多样本支持、较高比对质量，并通过独立分析流程验证的候选microexon。

# 一、总体分析流程

## ONT Microexon identification pipeline

ONT RNA-seq

FASTQ / POD5 / BAM

Step 1. Read QC

NanoPlot / pycoQC / NanoFilt

Step 2. Genome alignment

minimap2 + samtools

ONT RNA-seq → hg38 BAM

Step 3. Transcript identification

IsoQuant + FLAIR

Step 4. Microexon extraction

GTF exon length = 3–30 bp

Known + Novel candidates

Step 5. Structural validation

Splice junction / Read support / Alignment quality / SQANTI3

Step 6. Annotation and quantification

GENCODE + VASTDB + PSI matrix

Step 7. Differential microexon analysis

ΔPSI / Statistical testing / Multiple testing correction

Final results

Known microexon + Novel microexon + PSI matrix + Differential microexon

# 二、软件及环境准备

建议建立独立的conda环境，避免IsoQuant、FLAIR和SQANTI3之间的Python依赖冲突。

## 1. 软件清单

| 软件       | 推荐版本    | 用途            |
| -------- | ------- | ------------- |
| minimap2 | 2.28或以上 | ONT RNA-seq比对 |
| samtools | 1.20或以上 | BAM处理         |
| NanoPlot | 1.42或以上 | 测序质量统计        |
| IsoQuant | 3.6.3   | 转录本鉴定、定量      |
| FLAIR    | 2.0或以上  | 转录本结构校正及鉴定    |
| SQANTI3  | 5.x     | 转录本结构QC       |
| bedtools | 2.31    | 区间交集及注释       |
| gffread  | 0.12.7  | GTF/GFF处理     |
| pysam    | 0.22或以上 | BAM解析         |
| R        | 4.3或以上  | PSI统计分析       |

已知microexon数据库建议准备：

- GENCODE v49或与你的分析版本一致的hg38 GTF。
- VASTDB microexon注释。
- 可选：GENCODE综合注释、Ensembl注释。

建议所有分析统一使用同一版hg38参考基因组和基因注释，避免不同版本的坐标不一致。

## 2. 环境安装

```
conda create -n microexon_ont \
    python=3.10 \
    -y

conda activate microexon_ont

conda install -c bioconda \
    minimap2 \
    samtools \
    bedtools \
    gffread \
    nanoplot \
    pysam \
    pybedtools \
    -y

pip install isoquant==3.6.3
```

FLAIR和SQANTI3建议分别安装：

```
conda create -n flair \
    -c conda-forge -c bioconda \
    python=3.10 flair \
    -y

conda create -n sqanti3 \
    -c conda-forge -c bioconda \
    python=3.10 sqanti3 \
    -y
```

注意：以上是建议安装方案，实际使用时应根据各软件当前release及依赖约束锁定环境，并记录软件版本。不要将不同软件环境强行合并。

# 三、ONT 数据预处理和比对

## 1. 准备参考基因组及注释

推荐使用：

- GRCh38/hg38 reference genome。
- GENCODE v49 comprehensive annotation。
- VASTDB hg38 microexon annotation。

目录建议：

```
Microexon_Project/
├── reference/
│   ├── hg38.fa
│   ├── hg38.fa.fai
│   ├── gencode.v49.annotation.gtf
│   └── vastdb_microexon.bed12
├── fastq/
├── bam/
├── isoquant/
├── sqanti3/
├── microexon/
├── psi/
└── scripts/
```

参考资源：

- [GENCODE Human Annotation](https://www.gencodegenes.org/human/?utm_source=chatgpt.com)
- [VASTDB Downloads](https://vastdb.crg.eu/wiki/Downloads?utm_source=chatgpt.com)
- [UCSC hg38 Reference Genome](https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/?utm_source=chatgpt.com)

VASTDB建议下载human hg38的Alternative Splicing Event Information及相关事件注释。对于microexon，重点提取cassette exon事件，并保留外显子坐标、链方向及上下游剪接连接信息。其数据库还提供事件PSI和保守性等信息。

[image](https://www.google.com/s2/favicons?domain=https://vastdb.crg.eu\&sz=32)

VastDB+1



## 2. ONT FASTQ质量控制

推荐NanoPlot。

安装：

```
conda activate microexon_ont

NanoPlot \
    --fastq fastq/sample01.fastq.gz \
    --threads 16 \
    --outdir qc/sample01
```

重点查看：

- Read length distribution
- Mean read quality
- Read N50
- Total bases
- Read number
- Length distribution

建议初步过滤：

| 指标           | 建议                |
| ------------ | ----------------- |
| Read quality | Q10以上，条件允许建议Q12以上 |
| Read length  | 50 bp以上           |
| 低质量reads     | 过滤                |
| 极短reads      | 过滤                |
| Poly(A)尾     | 不建议提前全部去除         |

这里的Q10/Q12是分析起点，不是microexon特异性的质量保证。对于3–6 bp的microexon，仍需更严格的结构证据。

## 3. ONT RNA-seq比对到hg38

这一步必须区分ONT的两种建库类型：

- ONT cDNA sequencing
- ONT Direct RNA sequencing

两种数据的minimap2参数不同。

A. ONT cDNA-seq

```
minimap2 \
    -ax splice \
    --secondary=no \
    -t 32 \
    reference/hg38.fa \
    fastq/sample01.fastq.gz \
    | samtools sort -@ 16 \
        -o bam/sample01.sorted.bam
```

B. ONT Direct RNA-seq

```
minimap2 \
    -ax splice \
    -k14 \
    -uf \
    --secondary=no \
    -t 32 \
    reference/hg38.fa \
    fastq/sample01.fastq.gz \
    | samtools sort -@ 16 \
        -o bam/sample01.sorted.bam
```

Direct RNA使用较短k-mer以提高较高错误率reads的比对敏感性；-uf用于正向转录本方向的比对模式。

这些参数来自minimap2针对ONT cDNA和Direct RNA的官方比对建议。

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



然后建立索引：

```
samtools index bam/sample01.sorted.bam

samtools flagstat bam/sample01.sorted.bam \
    > bam/sample01.flagstat.txt

samtools stats bam/sample01.sorted.bam \
    > bam/sample01.stats.txt
```

建议不要在这里直接过滤掉所有低MAPQ reads并覆盖原始BAM，保留原始比对结果，后续再根据候选microexon进行针对性过滤。

# 四、利用IsoQuant鉴定转录本

这是整个流程的核心步骤。

你之前已经使用过IsoQuant 3.6.3，因此可以沿用这个版本，避免不同版本的算法变化影响结果可比性。

## 1. 运行IsoQuant

如果每个样本都有独立FASTQ，建议将所有样本放在同一个experiment中，以便建立统一的转录本注释，同时保留样本级定量结果。

创建配置文件：

`isoquant/samples.yaml`

```
data format: fastq

data:
  - name: HIE_ONT
    long read files:
      - /project/fastq/sample01.fastq.gz
      - /project/fastq/sample02.fastq.gz
      - /project/fastq/sample03.fastq.gz
    labels:
      - sample01
      - sample02
      - sample03
```

实际使用时将其扩展到所有样本。

运行：

```
isoquant \
    --yaml isoquant/samples.yaml \
    --reference reference/hg38.fa \
    --genedb reference/gencode.v49.annotation.gtf \
    --data_type nanopore \
    --complete_genedb \
    --threads 32 \
    --output isoquant/results
```

如果已有比对BAM，也可以使用：

```
isoquant \
    --bam bam/sample01.sorted.bam \
         bam/sample02.sorted.bam \
         bam/sample03.sorted.bam \
    --labels sample01 sample02 sample03 \
    --reference reference/hg38.fa \
    --genedb reference/gencode.v49.annotation.gtf \
    --data_type nanopore \
    --complete_genedb \
    --threads 32 \
    --output isoquant/results
```

注意：如果采用已经比对的BAM输入，确保BAM中保留了IsoQuant需要的read tags，尤其是涉及poly(A)信息时。直接从FASTQ输入通常更省心。

[image](https://www.google.com/s2/favicons?domain=https://ablab.github.io\&sz=32)

IsoQuant+2



## 2. 重点关注IsoQuant输出文件

重点查看：

| 输出文件                        | 作用               |
| --------------------------- | ---------------- |
| `*.transcript_models.gtf`   | 检测到的已知和新型转录本结构   |
| `*.extended_annotation.gtf` | 原有注释与新发现转录本的整合结果 |
| `*.transcript_counts.tsv`   | 转录本read counts   |
| `*.gene_counts.tsv`         | 基因表达定量           |
| `*.transcript_tpm.tsv`      | 转录本TPM           |
| `*.read_assignments.tsv`    | read与转录本的分配信息    |

不同版本的文件名和输出组织可能有变化，以实际运行结果为准。

[image](https://www.google.com/s2/favicons?domain=https://ablab.github.io\&sz=32)

IsoQuant+1



这里建议优先使用：

```
*.transcript_models.gtf
```

提取microexon。

# 五、提取候选microexon

## 1. 基本筛选原则

建议将microexon定义为：

\\[ 3 \leq L\_{\text{exon}} \leq 30\ \text{bp} \\]

其中：

\\[ L\_{\text{exon}}=end-start+1 \\]

可以进一步分成：

| 长度       | 分类          |
| -------- | ----------- |
| 3–6 bp   | 极短microexon |
| 7–15 bp  | 短microexon  |
| 16–30 bp | microexon   |

需要说明：3–30 bp是本项目建议采用的操作性定义，不代表所有文献对microexon的长度边界完全一致。

## 2. 提取所有3–30 bp exon

下面给出一个可以直接运行的Python脚本。

文件：`scripts/extract_microexons.py`

```
#!/usr/bin/env python3import reimport sysfrom collections import defaultdictgtf = sys.argv[1]out = sys.argv[2]transcripts = defaultdict(list)with open(gtf) as f:    for line in f:        if line.startswith("#"):            continue        fields = line.rstrip().split("\t")        if len(fields) < 9 or fields[2] != "exon":            continue        chrom = fields[0]        start = int(fields[3])        end = int(fields[4])        strand = fields[6]        attr = fields[8]        gene = re.search(            r'gene_id "([^"]+)"', attr        )        transcript = re.search(            r'transcript_id "([^"]+)"', attr        )        if not gene or not transcript:            continue        gid = gene.group(1)        tid = transcript.group(1)        length = end - start + 1        transcripts[tid].append(            (chrom, start, end, strand, gid, tid)        )results = []for tid, exons in transcripts.items():    exons.sort(key=lambda x: x[1])    for i, exon in enumerate(exons):        chrom, start, end, strand, gid, tid = exon        length = end - start + 1        if not 3 <= length <= 30:            continue        if i == 0 or i == len(exons) - 1:            continue        prev_exon = exons[i-1]        next_exon = exons[i+1]        # 检查相邻外显子是否位于同一染色体        if prev_exon[0] != chrom or next_exon[0] != chrom:            continue        # 记录候选microexon及其两侧外显子        results.append([            chrom, start, end, length,            strand, gid, tid,            prev_exon[1], prev_exon[2],            next_exon[1], next_exon[2]        ])with open(out, "w") as f:    f.write(        "chrom\tstart\tend\tlength\tstrand\t"        "gene_id\ttranscript_id\t"        "prev_start\tprev_end\tnext_start\tnext_end\n"    )    for x in results:        f.write("\t".join(map(str, x)) + "\n")print("Candidate microexons:", len(results))
```

运行：

```
python scripts/extract_microexons.py \
    isoquant/results/HIE_ONT.transcript_models.gtf \
    microexon/candidate_microexons.tsv
```

注意：脚本按照基因组坐标排列外显子，输出的是候选microexon。负链基因的转录顺序与基因组坐标顺序相反，因此后续必须统一按照基因组坐标处理剪接连接。

此外，这个脚本保留了转录本级别的候选结果，后面需要进行跨转录本和跨样本合并，避免同一个microexon重复计数。

## 3. 候选microexon标准化

建议按照以下字段建立统一ID：

```
chrom:start:end:strand
```

例如：

```
chr1:100001:100012:+
chr1:200001:200009:-
```

但是，同一microexon可能对应不同的上下游外显子组合，因此建议同时保留：

- Microexon genomic coordinates
- Strand
- Gene ID
- Transcript ID
- Upstream exon coordinates
- Downstream exon coordinates
- Splice junction coordinates

不能只按照microexon本身的坐标合并所有剪接事件。

# 六、已知和新型microexon的注释

这一部分需要将候选microexon与两个数据库进行比较。

## 1. 与GENCODE注释比较

提取GENCODE中长度3–30 bp的exon：

```
awk -F'\t' '
BEGIN{OFS="\t"}
$3=="exon" {
    len=$5-$4+1
    if(len>=3 && len<=30)
        print $1,$4-1,$5,$9,$6
}' reference/gencode.v49.annotation.gtf \
> microexon/gencode_microexons.bed
```

注意：BED采用0-based、half-open坐标，而GTF采用1-based、closed坐标，所以这里起始坐标减1。

候选microexon转换为BED：

```
awk 'BEGIN{OFS="\t"} NR>1 {
    print $1,$2-1,$3,$7,$4,$5
}' microexon/candidate_microexons.tsv \
> microexon/candidate_microexons.bed
```

比较：

```
bedtools intersect \
    -a microexon/candidate_microexons.bed \
    -b microexon/gencode_microexons.bed \
    -wa -wb -s \
    > microexon/gencode_overlap.txt
```

这里建议采用严格坐标匹配，并同时检查链方向。

## 2. 与VASTDB比较

VASTDB是microexon研究中非常值得使用的资源。

其human hg38数据可以提供：

- Alternative exon坐标。
- Microexon包含和跳跃信息。
- 不同组织、细胞类型的PSI。
- 进化保守性。
- 蛋白结构影响。

官方入口：[VASTDB Download](https://vastdb.crg.eu/wiki/Downloads?utm_source=chatgpt.com)

建议将VASTDB中的cassette exon事件转换为统一的BED或TSV格式，然后与候选microexon进行匹配。

建议设置两种匹配等级：

- Exact match：染色体、起止坐标、链方向完全一致。
- Near match：边界相差1–2 bp，作为待验证匹配，不能直接认定为已知microexon。

## 3. 建立最终注释分类

建议最终建立以下分类：

| 分类             | 判定方式                 |
| -------------- | -------------------- |
| Known-GENCODE  | GENCODE精确匹配          |
| Known-VASTDB   | VASTDB精确匹配           |
| Known-both     | 两个数据库均匹配             |
| Novel          | 两个数据库均无匹配            |
| Novel-boundary | 与已知microexon接近但边界不一致 |

这里有一个非常重要的区别：

新型microexon不等于新型转录本。

例如，一个已知基因中的新microexon，即使所在转录本被IsoQuant标记为NIC，也可能只是新的microexon组合；反过来，一个FSM转录本也可能含有已知注释中的microexon。

因此，最终应该分别报告microexon新颖性和转录本新颖性。

# 七、使用SQANTI3验证候选转录本

IsoQuant鉴定到的转录本可能存在错误剪接、错误的转录起始位点或不完整转录本。建议使用SQANTI3进行独立质量评估。

SQANTI3能够根据剪接连接结构将转录本分为：

| 类型         | 含义                      | 对microexon研究的意义   |
| ---------- | ----------------------- | ----------------- |
| FSM        | Full Splice Match       | 所有剪接位点匹配已知转录本     |
| ISM        | Incomplete Splice Match | 已知转录本的不完整形式       |
| NIC        | Novel In Catalog        | 新的外显子组合，但剪接位点已有注释 |
| NNC        | Novel Not in Catalog    | 存在新的剪接位点          |
| Antisense  | 反义转录本                   | 需要单独评估            |
| Intergenic | 基因间区转录本                 | 需要谨慎验证            |

其中，NIC和NNC尤其值得关注，但不能直接将NNC等同于新型microexon。

[image](https://www.google.com/s2/favicons?domain=https://www.nature.com\&sz=32)

Nature Methods+1



运行：

```
conda activate sqanti3

sqanti3_qc.py \
    --isoforms isoquant/results/HIE_ONT.transcript_models.gtf \
    --refGTF reference/gencode.v49.annotation.gtf \
    --refFasta reference/hg38.fa \
    --skipORF \
    -o HIE_ONT \
    -d sqanti3/results \
    -t 32 \
    --report both
```

重点检查：

```
HIE_ONT_classification.txt
HIE_ONT_junctions.txt
HIE_ONT_corrected.gtf
```

实际输出文件前缀和目录以运行版本为准。

建议重点检查：

- `structural_category`
- `subcategory`
- `RTS_stage`
- `all_canonical`
- `min_sample_cov`
- `FL`
- `length`

对于microexon，需要特别检查其两侧剪接连接是否被SQANTI3识别为canonical。

注意：SQANTI3主要评价转录本质量，不是专门的microexon检测工具。它的FSM/NIC/NNC分类应作为辅助证据，而不是microexon新颖性的唯一判断标准。

[image](https://www.google.com/s2/favicons?domain=https://www.nature.com\&sz=32)

Nature Methods+1



# 八、基于ONT BAM进行microexon reads验证

这是我认为整个流程中最重要的一步。

单纯通过GTF提取3–30 bp外显子会产生不少假阳性，需要回到原始BAM，直接确认microexon两侧的剪接连接。

## 1. Microexon的结构证据

假设有一个microexon：

```
chr1:100001-100012
```

其正常剪接结构应当是：

```
上游Exon
    |
    |  Junction 1
    v
Microexon (12 bp)
    |
    |  Junction 2
    v
下游Exon
```

我们需要找到同时支持两个剪接连接的reads：

- Junction 1：上游exon → microexon。
- Junction 2：microexon → 下游exon。

另外还需要统计直接连接上下游exon、跳过microexon的reads。

Microexon inclusion

Exon 1 ── Microexon ── Exon 2

两个剪接连接均有支持

Microexon skipping

Exon 1 ───────────── Exon 2

一个跳跃剪接连接

## 2. 使用pysam统计microexon剪接支持

建议建立一个专门的Python程序，直接解析BAM中的CIGAR信息。

核心逻辑：

| CIGAR信息 | 解释          |
| ------- | ----------- |
| M、=、X   | 比对到参考基因组的序列 |
| N       | 跨越内含子的剪接连接  |
| I       | 插入          |
| D       | 缺失          |

对于一个候选microexon：

- inclusion：同一条read中同时出现两个预期的N连接。
- skipping：同一条read中出现跨越整个microexon的预期N连接。
- 同一read只计数一次。
- 排除secondary和supplementary alignment。
- 建议MAPQ至少20，严格分析可采用30。

下面给出可以运行的核心脚本。

文件：`scripts/count_microexon_psi.py`

```
#!/usr/bin/env python3import argparseimport csvimport pysamparser = argparse.ArgumentParser()parser.add_argument("--candidate", required=True)parser.add_argument("--bam", required=True)parser.add_argument("--sample", required=True)parser.add_argument("--out", required=True)parser.add_argument("--mapq", type=int, default=20)args = parser.parse_args()bam = pysam.AlignmentFile(args.bam, "rb")def get_junctions(read):    junctions = set()    if read.cigartuples is None:        return junctions    ref_pos = read.reference_start    for op, length in read.cigartuples:        # M, =, X        if op in (0, 7, 8):            ref_pos += length        # D
```

这里采用严格的剪接连接坐标匹配，目的是避免将边界不确定的ONT reads错误地认定为microexon支持证据。

注意两个细节：

1. 代码假定候选表中的`prev_start/prev_end`和`next_start/next_end`分别是microexon所在转录本的相邻外显子。对于负链转录本，程序通过基因组坐标重新确定左右侧外显子，因此不会直接把转录方向当作基因组方向。
2. 如果同一read在不同候选转录本中被重复记录，需要先按事件坐标和上下游剪接边界去重。该脚本统计的是read数，不等同于经过UMI去重的独立RNA分子数。

运行：

```
python scripts/count_microexon_psi.py \
    --candidate microexon/candidate_microexons.tsv \
    --bam bam/sample01.sorted.bam \
    --sample sample01 \
    --mapq 20 \
    --out psi/sample01.psi.tsv
```

其他样本依次运行即可。

对于实际项目，建议进一步增加以下检查：

- 检查microexon内部是否有连续比对的M/= /X片段。
- 检查read是否同时覆盖两个剪接连接。
- 检查候选事件的上下游外显子是否属于同一基因。
- 检查skip和inclusion reads是否存在明显的嵌合比对。
- 对于极短microexon，检查比对是否受重复序列或低复杂度区域影响。

另外，上面的脚本适合先对候选事件进行严格的junction-level计数。如果一个microexon只有一侧剪接证据，不能把它作为完整inclusion事件。

# 九、PSI矩阵构建及差异microexon分析

## 1. PSI计算

对于每个microexon，使用：

\\[ PSI=\frac{I}{I+S} \\]

其中：

- \\(I\\)：包含microexon的reads数量。
- \\(S\\)：跳过microexon的reads数量。

例如：

| Microexon | Sample    | Inclusion | Skipping | PSI  |
| --------- | --------- | --------- | -------- | ---- |
| ME001     | Case01    | 80        | 20       | 0.80 |
| ME001     | Case02    | 75        | 25       | 0.75 |
| ME001     | Control01 | 20        | 80       | 0.20 |
| ME001     | Control02 | 25        | 75       | 0.25 |

如果病例组和对照组之间存在明显PSI差异，则该microexon可能与疾病相关。

## 2. 建立PSI矩阵

最终建议建立三个矩阵：

Matrix 1：Inclusion counts

```
Microexon    Case01    Case02    Control01    Control02
ME001        80        75        20           25
ME002        35        40        60           65
ME003        100       90        30           25
```

Matrix 2：Skipping counts

```
Microexon    Case01    Case02    Control01    Control02
ME001        20        25        80           75
ME002        65        60        40           35
ME003        10        10        70           75
```

Matrix 3：PSI

```
Microexon    Case01    Case02    Control01    Control02
ME001        0.80      0.75      0.20         0.25
ME002        0.35      0.40      0.60         0.65
ME003        0.91      0.90      0.30         0.25
```

建议将PSI矩阵用于后续聚类、PCA、差异分析和机器学习。

## 3. 差异microexon分析

建议使用ΔPSI：

\\[ \Delta PSI=Mean(PSI\_{Case})-Mean(PSI\_{Control}) \\]

例如：

| Microexon | Case PSI | Control PSI | ΔPSI  | 变化          |
| --------- | -------- | ----------- | ----- | ----------- |
| ME001     | 0.78     | 0.22        | +0.56 | Inclusion增加 |
| ME002     | 0.32     | 0.63        | -0.31 | Inclusion降低 |
| ME003     | 0.90     | 0.28        | +0.62 | Inclusion增加 |

推荐筛选标准：

| 指标                     | 建议阈值                |
| ---------------------- | ------------------- |
| Mean PSI               | 至少一个组达到0.05         |
| ΔPSI                   | 绝对值≥0.10            |
| FDR                    | 0.05                |
| Total supporting reads | 每个样本至少有一定数量的支持reads |

注意：不建议简单使用t-test对每个样本的PSI进行检验，因为PSI的分母和测序深度存在差异。

对于病例–对照研究，更推荐使用Beta-binomial模型：

\\[ (I,S)\sim BetaBinomial \\]

R中可以使用`glmmTMB`：

```
library(glmmTMB)

# dat包含：
# inclusion, skipping, group, age, sex, batch

fit <- glmmTMB(
    cbind(inclusion, skipping) ~
    group + age + sex +
    (1 | batch),
    family = betabinomial(link = "logit"),
    data = dat
)

summary(fit)
```

这里假设batch具有足够的重复水平。如果有配对样本，应将subject作为随机效应纳入模型；如果有多个测序批次，也需要考虑批次效应。

对于每个microexon单独建模，最后进行Benjamini-Hochberg多重检验校正。

# 十、最终microexon过滤标准

建议建立三级过滤体系，而不是一次性删除所有低支持事件。

## Microexon quality classification

Level 1：Candidate microexons

- Exon length：3–30 bp
- 有转录本注释
- 有至少一侧剪接证据

初步候选

Level 2：High-confidence microexons

- 两侧剪接连接均有支持
- 同一read同时支持两个连接
- MAPQ≥20
- 每个候选事件有至少3条完整inclusion reads
- 具有合理的剪接位点

高可信候选

Level 3：Validated novel microexons

- 不在GENCODE和VASTDB中
- 多个独立样本重复发现
- 两侧剪接连接稳定
- 通过独立工具或短读长数据验证
- 必要时通过RT-PCR验证

重点研究对象

我建议进一步设置以下具体阈值：

| 过滤项目            | 初步标准     | 严格标准        |
| --------------- | -------- | ----------- |
| Microexon长度     | 3–30 bp  | 3–30 bp     |
| MAPQ            | ≥20      | ≥30         |
| Inclusion reads | ≥3       | ≥5          |
| 检出样本数           | ≥1       | ≥2或更多       |
| 两侧剪接位点          | 均有支持     | 均有重复支持      |
| 剪接信号            | 典型GT-AG等 | 优先canonical |
| ΔPSI            | ≥0.10    | ≥0.20       |
| FDR             | 0.05     | 0.05        |

这里的样本数阈值应结合队列规模、测序深度和病例组/对照组比例调整。尤其是3–6 bp的microexon，不建议仅依赖上述reads数量阈值判定其真实性。

对于非典型剪接位点的microexon，不应直接全部删除，而应单独标记为低可信候选，结合序列、比对和实验验证进行判断。

# 十一、建议增加的功能分析

鉴定到microexon后，建议不要只停留在PSI差异层面。

| 分析方向   | 推荐工具                      | 研究目的                    |
| ------ | ------------------------- | ----------------------- |
| 蛋白结构影响 | TransDecoder、InterProScan | 判断microexon是否影响蛋白结构域    |
| 阅读框分析  | 自定义Python、Biopython       | 判断是否移码                  |
| 保守性分析  | phastCons、phyloP          | 评估进化保守性                 |
| 剪接调控   | RBPmap、motif分析            | 寻找SRRM3、SRRM4等调控因子的结合特征 |
| 遗传变异分析 | bcftools、VEP              | 识别影响microexon的SNV/InDel |
| sQTL分析 | LeafCutter、QTLtools等      | 分析遗传变异与剪接比例的关联          |
| 蛋白互作   | STRING                    | 研究microexon相关基因的功能网络    |
| 疾病关联   | HPO、OMIM                  | 关联NDD临床表型               |

特别值得关注：

- SRRM3
- SRRM4
- RBFOX1
- PTBP2
- NOVA1
- NOVA2
- RBM24

这些基因涉及神经元相关RNA剪接调控，适合用于microexon调控机制研究的候选基因分析。

# 十二、一个需要特别注意的问题：MicroExonator是否适合ONT？

前面推荐MicroExonator作为首选工具，这里需要针对你的数据类型进一步修正：

MicroExonator不建议直接作为ONT RNA-seq的主分析工具。

它原始设计主要针对短读长RNA-seq，通过剪接连接序列库和BWA-MEM等策略发现微外显子，不能直接把它当作ONT长读长专用分析流程。

[image](https://www.google.com/s2/favicons?domain=https://microexonator.readthedocs.io\&sz=32)

MicroExonator documentation+2



对于你的项目，我更推荐：

| 任务            | 首选工具                       |
| ------------- | -------------------------- |
| 新型转录本鉴定       | IsoQuant                   |
| 独立转录本结构验证     | FLAIR                      |
| 转录本质量控制       | SQANTI3                    |
| 候选microexon提取 | 自定义Python                  |
| 剪接连接验证        | pysam                      |
| 已知microexon注释 | GENCODE + VASTDB           |
| PSI定量         | 自定义junction-based counting |
| 差异microexon分析 | Beta-binomial模型            |
| 功能注释          | VEP + InterProScan         |

如果有同一样本的Illumina RNA-seq，可以额外使用MicroExonator进行短读长microexon发现和验证，特别是对3–6 bp的极短microexon，短读长与长读长的互补证据非常有价值。

# 十三、最终建议：针对你的项目形成可发表的研究框架

如果你的目标是构建一个大规模人类microexon图谱，我建议最终形成以下几个结果：

1. Human microexon landscape：鉴定所有已知和新型microexon，构建统一坐标和注释数据库。
2. Microexon expression landscape：计算不同样本、基因和转录本的PSI，建立microexon表达图谱。
3. Novel microexon discovery：筛选高可信度新型microexon，分析其组织特异性和保守性。
4. Microexon isoform diversity：分析多个microexon组合形成的复杂转录本结构。
5. Microexon functional impact：研究microexon对蛋白结构、蛋白互作和信号通路的影响。
6. Disease-associated microexons：分析疾病相关microexon及其遗传调控机制。

## 推荐最终成果

# 1

Microexon Catalog

已知及新型microexon数据库

# 2

PSI Atlas

样本级microexon PSI图谱

# 3

Isoform Atlas

microexon组合转录本图谱

# 4

Functional Map

蛋白功能和疾病关联图谱

## 十四、我建议你特别关注的两个技术问题

第一，ONT是否足以准确鉴定3 bp的microexon？

对于3 bp极短外显子，ONT单独识别的可靠性需要谨慎评估。建议采用严格双侧junction支持，并尽量结合Illumina RNA-seq或RT-PCR验证。尤其不能仅凭IsoQuant GTF中出现一个3 bp exon就认定它是真实microexon。

第二，是否需要将所有样本合并进行microexon鉴定？

建议采用：

- 所有样本联合发现候选microexon。
- 每个样本独立进行microexon PSI定量。
- 按照病例和对照进行差异分析。
- 对新型microexon进行跨样本重复验证。

这样既可以提高低表达microexon的发现能力，也能保留样本之间的差异。

如果这是你之前使用IsoQuant 3.6.3分析的约100个长读长样本项目，那么可以直接从已有的BAM和GTF开始，不必重新比对和组装。

最终最推荐的技术组合是：IsoQuant 3.6.3（联合发现）+ SQANTI3（结构质控）+ pysam（逐样本junction验证及PSI）+ VASTDB（已知microexon注释）+ Beta-binomial（差异分析）。

这套方案比较适合构建高可信度的ONT microexon图谱，也便于后续结合WGS/WES开展遗传调控和疾病机制研究。



---

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

---

# 近三年发表的关于泛转录本的高水平文章有哪些

如果你这里说的“**泛转录本**”是指 **pan-transcriptome / pan-transcriptomics（泛转录组）**，而且结合你现在正在做的 **100个样本 IsoQuant + 长读长 RNA-seq + 建立非冗余 population reference GTF**，近三年（**2023–2026**）确实有几篇非常值得重点看。

我按“**与你当前研究最相关程度**”整理如下。

---

## 一、与你现在100个样本 IsoQuant 项目最直接相关

### 1. 人群多样性 + 长读长 + pan-transcriptome —— 强烈推荐

**Clavell-Revelles et al.
“Long-read transcriptomics of a diverse human cohort reveals ancestry bias in gene annotation”
Nature Communications, 2025**

这是目前和你现在的研究设计**最接近的一篇**。

研究使用：

* 43个具有遗传多样性的人群样本
* 8个人群
* ONT long-read RNA-seq
* 超过 **800 million full-length reads**
* FLAIR
* IsoQuant
* ESPRESSO
* LyRic

构建跨人群 transcript annotation。研究发现非欧洲人群存在大量当前参考注释没有充分覆盖的 transcript，并指出个人基因组 assembly 可以进一步提高 transcript discovery。作者明确提出需要进一步建立 **human pantranscriptome**。([Nature][1])

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-66096-x?utm_source=chatgpt.com)

**对你的项目最值得借鉴的地方：**

```text
多个人群/个体
      ↓
long-read RNA-seq
      ↓
IsoQuant / FLAIR / ESPRESSO
      ↓
transcript discovery
      ↓
cross-sample annotation
      ↓
population-level transcript annotation
      ↓
pan-transcriptome
```

这和你现在：

```text
100 samples
   ↓
IsoQuant
   ↓
transcript_models.gtf
   ↓
collapse
   ↓
population reference GTF
```

几乎是同一个技术路线。

---

## 二、最值得你学习“如何建立 Pan-transcriptome”的文章

### 2. Barley pan-transcriptome —— Nature Genetics 2025

**Guo et al.
“A barley pan-transcriptome reveals layers of genotype-dependent transcriptional complexity”
Nature Genetics, 2025, 57:441–450**

这是我认为**最值得你仔细研究方法部分的一篇**。([Nature][2])

研究：

* 20个不同 barley genotype
* 多组织
* short-read RNA-seq
* PacBio Iso-Seq
* genotype-specific reference transcript datasets
* 最终建立 **pan-RTD**

他们没有简单把所有 transcript GTF `cat` 在一起，而是：

```text
20 genomes
   ↓
20 genotype-specific transcript datasets
   ↓
integration
   ↓
pan-RTD
   ↓
core / shell / cloud transcript
```

这是一个非常重要的概念。

他们把 transcript 分成类似：

```text
Core transcripts
Shell transcripts
Cloud transcripts
```

因此不仅仅是：

> “所有样本检测到的 transcript 集合”

而是进一步研究：

> **哪些 transcript 在所有基因型中存在，哪些是群体/基因型特异的。**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-02069-y?utm_source=chatgpt.com)

### 对你的100个样本特别有启发

你完全可以借鉴成：

```text
100 samples
      ↓
non-redundant transcriptome
      ↓
                   ┌── Core
                   │
                   ├── Common
                   │
                   ├── Population-enriched
                   │
                   └── Sample-specific
```

例如定义：

| 类别        | 100样本中出现比例 |
| --------- | ---------: |
| Core      |       ≥90% |
| Common    |     20–90% |
| Rare      |      2–20% |
| Singleton |         1% |

当然具体阈值应该根据你的样本量、组织类型和测序深度决定。

---

# 三、另一个非常重要的 Nature Genetics：大规模群体 Pan-transcriptome

### 3. Yeast pan-transcriptome

**Caudal et al.
“Pan-transcriptome reveals a large accessory genome contribution to gene expression variation in yeast”
Nature Genetics, 2024**

这是非常漂亮的**大群体 pan-transcriptome**研究。

他们分析约：

> **1,000个 yeast natural isolates**

建立：

* 4,977 core genes
* 1,468 accessory genes

然后进一步研究：

```text
pan-transcriptome
       ↓
core / accessory genome
       ↓
gene expression
       ↓
population structure
       ↓
GWAS
       ↓
genetic determinants of expression
```

研究发现 accessory genome 对表达差异贡献很大，而且 accessory genes 与较大的遗传效应相关。([Nature][3])

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01769-9?utm_source=chatgpt.com)

### 对你最大的启发

你以后如果100个样本有 WGS，那么你的项目可以从：

```text
Population transcriptome
```

进一步升级成：

```text
Population pan-transcriptome
             ↓
transcript presence/absence
             ↓
isoform usage
             ↓
expression variation
             ↓
eQTL / isoQTL
             ↓
SV / SNV / indel
             ↓
genotype → transcript → phenotype
```

这比单纯建立一个 GTF 有明显更大的研究空间。

---

# 四、2023 Nature：Pan-grass transcriptome

### 4. Guillotin et al.

**“A pan-grass transcriptome reveals patterns of cellular divergence in crops”
Nature, 2023, 617:785–791**

这是近三年 pan-transcriptome 领域非常值得看的 Nature 文章。([Nature][4])

研究：

* maize
* sorghum
* Setaria
* single-cell / single-nucleus RNA-seq
* comparative transcriptomics

重点不是“建立一个 GTF”本身，而是：

```text
multiple species
      ↓
pan-transcriptome
      ↓
cell types
      ↓
orthologous transcript/gene programs
      ↓
cellular divergence
```

对你以后如果考虑：

> 不同组织/不同疾病状态/不同人群的 transcriptome diversity

这个思路非常有参考价值。

[Nature 原文](https://www.nature.com/articles/s41586-023-06053-0?utm_source=chatgpt.com)

---

# 五、2025 Nature：Oat pan-genome + pan-transcriptome

### 5. Avni et al.

**“A pangenome and pantranscriptome of hexaploid oat”
Nature, 2025/2026**

这篇也非常值得关注。

研究同时构建：

```text
pangenome
+
pantranscriptome
```

并把基因组多样性与 transcript diversity 联系起来。该论文发表于 **Nature 649, 131–139（2026 issue）**。([Nature][5])

[Nature 原文](https://www.nature.com/articles/s41586-025-09676-7?utm_source=chatgpt.com)

这篇对你以后做：

> **Pan-genome → Pan-transcriptome → functional variation**

非常有参考意义。

---

# 六、虽然不是“pan-transcriptome”，但你的项目一定应该看

### 6. LRGASP：长读长 transcriptome benchmark

**“Systematic assessment of long-read RNA-seq methods for transcript identification and quantification”
Nature Methods, 2024**

这是 **LRGASP** 项目，非常重要。([Nature][6])

它系统比较了：

* transcript identification
* transcript quantification
* de novo transcript discovery
* PacBio
* ONT
* cDNA
* direct RNA
* 多种分析工具

数据超过：

> **427 million long-read sequences**

涉及 human、mouse、manatee。([Nature][6])

[Nature Methods 原文](https://www.nature.com/articles/s41592-024-02298-3?utm_source=chatgpt.com)

对于你正在比较：

```text
IsoQuant
TAMA
gffcompare
FLAIR
SQANTI3
```

这篇非常重要。

---

# 七、SQANTI3：建立高质量 Pan-transcriptome 时尤其重要

### 7. Pardo-Palacios et al.

**“SQANTI3: curation of long-read transcriptomes for accurate identification of known and novel isoforms”
Nature Methods, 2024**

SQANTI3 主要解决：

> 长读长发现出来的 novel transcript 到底是不是真实 transcript？

它可以评估：

* splice junction
* transcript ends
* FSM
* ISM
* NIC
* NNC
* antisense
* intronic
* intergenic
* RT switching
* internal priming
* junction quality

([Nature][7])

[Nature Methods 原文](https://www.nature.com/articles/s41592-024-02229-2?utm_source=chatgpt.com)

因此你的流程最好不是：

```text
100 GTF
 ↓
merge
 ↓
GTF
```

而是：

```text
100 IsoQuant
      ↓
cross-sample collapse
      ↓
SQANTI3
      ↓
novel transcript QC
      ↓
high-confidence pan-transcriptome
```

---

# 八、2025 Nature Methods：长读长 transcript quantification benchmark

### 8. ONT long-read RNA-seq benchmark

**“A systematic benchmark of Nanopore long-read RNA sequencing for transcript-level analysis in human cell lines”
Nature Methods, 2025**

研究比较了：

* short-read cDNA
* ONT direct RNA
* ONT direct cDNA
* PCR cDNA
* PacBio Iso-Seq

并使用多个 human cell lines 和 spike-in 对 transcript-level quantification 进行评价。([Nature][8])

[Nature Methods 原文](https://www.nature.com/articles/s41592-025-02623-4?utm_source=chatgpt.com)

这个对你后面：

```text
population_reference.gtf
          ↓
重新IsoQuant
          ↓
transcript TPM/count
```

特别重要。

---

# 九、如果你最终想做“人类 Pan-transcriptome”，我建议重点看这几篇

按照与你当前项目的**相关性**，我会这样分：

| 优先级   | 文章                                                                                               |   年份 | 期刊                    | 主要价值                                       |
| ----- | ------------------------------------------------------------------------------------------------ | ---: | --------------------- | ------------------------------------------ |
| ★★★★★ | **Long-read transcriptomics of a diverse human cohort reveals ancestry bias in gene annotation** | 2025 | Nature Communications | **人群 + long-read + transcript annotation** |
| ★★★★★ | **A barley pan-transcriptome reveals layers of genotype-dependent transcriptional complexity**   | 2025 | Nature Genetics       | **Pan-transcriptome构建方法**                  |
| ★★★★★ | **Pan-transcriptome reveals a large accessory genome contribution...**                           | 2024 | Nature Genetics       | **大群体 + pan-transcriptome + genetics**     |
| ★★★★★ | **Systematic assessment of long-read RNA-seq methods...**                                        | 2024 | Nature Methods        | **IsoQuant等工具benchmark**                   |
| ★★★★☆ | **SQANTI3**                                                                                      | 2024 | Nature Methods        | **Novel isoform QC**                       |
| ★★★★☆ | **A pangenome and pantranscriptome of hexaploid oat**                                            | 2026 | Nature                | **Pan-genome + pan-transcriptome**         |
| ★★★★☆ | **A pan-grass transcriptome reveals patterns of cellular divergence in crops**                   | 2023 | Nature                | **跨物种/细胞类型pan-transcriptome**              |
| ★★★★☆ | **A systematic benchmark of Nanopore long-read RNA sequencing...**                               | 2025 | Nature Methods        | **transcript-level quantification**        |

---

# 十、对你目前100个 IsoQuant 样本，我尤其建议借鉴两篇

其实你现在正在做的事情已经非常接近这两个方向的结合：

### 文章A

**Clavell-Revelles 2025**

解决：

> **不同人群 → long-read transcript discovery → population annotation**

### 文章B

**Guo 2025**

解决：

> **多个 genotype → genotype-specific transcriptome → pan-transcriptome → core/shell/cloud**

因此你的项目完全可以设计成：

```text
                 100 individuals
                       │
                       ▼
                Long-read RNA-seq
                       │
                       ▼
                    IsoQuant
                       │
            ┌──────────┴──────────┐
            │                     │
       known isoforms        novel isoforms
            │                     │
            │             cross-sample collapse
            │                     │
            │                     ▼
            │              non-redundant
            │              novel transcriptome
            │                     │
            └──────────┬──────────┘
                       ▼
              Population Transcriptome
                       │
             ┌─────────┼─────────┐
             ▼         ▼         ▼
           Core      Common     Rare
        transcript  transcript transcript
             │
             ▼
        Pan-transcriptome
             │
     ┌───────┼─────────┐
     ▼       ▼         ▼
  Isoform   AS       APA/TSS
  usage
     │
     ▼
  WGS / SV
     │
     ▼
 isoQTL / sQTL / apaQTL
```

这实际上已经不仅仅是“合并100个GTF”，而是可以形成一个比较完整的 **population long-read pan-transcriptome** 研究框架。

特别值得注意的是，2025年的人群研究明确指出，即使只分析43个来自不同遗传背景的人群样本，也能发现大量当前参考注释没有充分覆盖的 transcript；作者因此提出扩大不同人群、组织、细胞类型和发育阶段的数据来构建更完整的人类 pantranscriptome。([Nature][1])

**如果你的100个样本是人类样本，并且有 WGS，我建议下一步不要只做“100 GTF → 一个 GTF”。** 可以进一步按照这些文章的思路，把它设计成 **“人群长读长泛转录组 + transcript PAV + isoform diversity + WGS/SV关联”**。这和你现在的 IsoQuant 数据非常匹配。

[1]: https://www.nature.com/articles/s41467-025-66096-x?utm_source=chatgpt.com "Long-read transcriptomics of a diverse human cohort reveals ancestry bias in gene annotation | Nature Communications"
[2]: https://www.nature.com/articles/s41588-024-02069-y?utm_source=chatgpt.com "A barley pan-transcriptome reveals layers of genotype-dependent transcriptional complexity | Nature Genetics"
[3]: https://www.nature.com/articles/s41588-024-01769-9?utm_source=chatgpt.com "Pan-transcriptome reveals a large accessory genome contribution to gene expression variation in yeast | Nature Genetics"
[4]: https://www.nature.com/articles/s41586-023-06053-0?utm_source=chatgpt.com "A pan-grass transcriptome reveals patterns of cellular divergence in crops | Nature"
[5]: https://www.nature.com/articles/s41586-025-09676-7?utm_source=chatgpt.com "A pangenome and pantranscriptome of hexaploid oat | Nature"
[6]: https://www.nature.com/articles/s41592-024-02298-3?utm_source=chatgpt.com "Systematic assessment of long-read RNA-seq methods for transcript identification and quantification | Nature Methods"
[7]: https://www.nature.com/articles/s41592-024-02229-2?utm_source=chatgpt.com "SQANTI3: curation of long-read transcriptomes for accurate identification of known and novel isoforms | Nature Methods"
[8]: https://www.nature.com/articles/s41592-025-02623-4?utm_source=chatgpt.com "A systematic benchmark of Nanopore long-read RNA sequencing for transcript-level analysis in human cell lines | Nature Methods"



