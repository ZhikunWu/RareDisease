# 如何基于三代测序数据检测组装基因组中插入的病毒或者微生物基因组

下面给出一个更适用于已经组装好的宿主基因组中寻找外源病毒或微生物 DNA 插入序列的分析框架。需要先区分两种任务：

1. 整合型插入（integration）：病毒或微生物 DNA 已经插入宿主染色体，需要识别宿主—外源序列连接点。
2. 游离或污染序列（contamination / episome）：外源 DNA 可能存在于独立 contig、质粒、游离病毒基因组或混合样本中，并不一定整合进宿主染色体。

这两类问题不能只依靠一个病毒分类工具解决。长读长数据最大的优势是可以让单条读段跨越宿主—外源序列连接点，并帮助还原插入结构；但组装过程中也可能出现污染、嵌合读段或错误连接，因此必须回到原始 reads 验证。

## 一、推荐的整体分析流程

对于已经组装好的宿主基因组，建议采用“外源序列筛查 → 分类鉴定 → 宿主插入定位 → 原始长读段验证 → 插入结构重建 → 生物学证据评估”的流程。

Step 1. 宿主基因组组装

PacBio HiFi / ONT → FASTA contigs

Step 2. 外源序列筛查

Kraken2 / BLASTn / DIAMOND / MMseqs2

Step 3. 病毒或微生物分类鉴定

NCBI nt、RefSeq、病毒数据库；检查分类一致性与序列覆盖度

Step 4. 宿主—外源序列连接点识别

Minimap2、联合参考比对、断点和组装图检查

Step 5. 原始长读段验证

跨越连接点的 reads、断点两侧支持、覆盖深度及独立证据

Step 6. 插入结构与功能分析

插入方向、拷贝数、宿主基因破坏、调控区域和进化来源

## 二、每个步骤推荐哪些工具？

| 分析任务              | 推荐工具               | 主要用途                                              |
| ----------------- | ------------------ | ------------------------------------------------- |
| 组装 contig 分类      | Kraken2、Centrifuge | 快速筛查病毒、细菌、真菌及其他分类群                                |
| 核酸序列同源性           | BLASTn、MMseqs2     | 确认候选 contig 与已知病毒或微生物基因组的相似性                      |
| 蛋白质层面的鉴定          | DIAMOND、BLASTx     | 发现核酸同源性较弱但蛋白质保守的外源序列                              |
| 病毒 contig 识别与质量评估 | CheckV、viralVerify | 判断候选病毒序列、完整度和潜在污染                                 |
| 长读段与组装比对          | minimap2           | 定位 reads 对应的宿主和外源序列区域                             |
| 结构变异辅助检测          | Sniffles2 等        | 在合适的联合参考或自定义比对结果中寻找候选断点；不能直接把普通宿主参考下的所有 SV 当作整合事件 |
| 连接点及组装结构验证        | IGV、BLAST、组装图查看工具  | 检查跨越宿主—外源序列的读段、断点方向和重复结构                          |

其中，viralFlye 是针对长读长宏基因组数据进行病毒组装和宿主关联分析的代表性工具，但它不是专门用于证明病毒已经整合进宿主染色体的工具。

[image](https://www.google.com/s2/favicons?domain=https://pmc.ncbi.nlm.nih.gov\&sz=32)

PMC

+1



## 三、具体怎么从组装基因组中找出插入的病毒或微生物？

### 1. 首先筛查疑似外源 contig

假设你有：

- `assembly.fa`：宿主组装基因组；
- `reads.fastq.gz`：PacBio HiFi 或 ONT 原始 reads；
- 宿主参考基因组（如果存在）。

可以先用 Kraken2 对 contig 进行分类：

```
kraken2 \
  --db /path/to/kraken2_db \
  --fasta-input \
  --threads 32 \
  --report contigs.kraken2.report \
  --output contigs.kraken2.out \
  assembly.fa
```

随后筛选被分类为病毒、细菌、真菌或其他非目标宿主分类群的 contig。

注意： Kraken2 将一个 contig 分类为细菌或病毒，并不等于这段序列真的插入了宿主基因组。宿主基因组中可能存在与微生物同源的保守序列、污染序列或分类错误。因此建议用 BLASTn 对候选 contig 进一步检查。

例如：

```
makeblastdb \
  -in microbial_virus_db.fa \
  -dbtype nucl \
  -out microbial_virus_db

blastn \
  -query candidate_contigs.fa \
  -db microbial_virus_db \
  -outfmt '6 qseqid sseqid pident length qstart qend sstart send evalue bitscore' \
  -evalue 1e-10 \
  -num_threads 32 \
  -out candidate_contigs.blast.tsv
```

这里的 `microbial_virus_db.fa` 应根据研究对象构建，包含相关病毒、细菌、真菌及其他候选微生物参考序列。若研究的是未知微生物，建议结合更广泛的 NCBI nt 数据库或蛋白质数据库，而不是只搜索有限的病原体列表。

### 2. 判断外源序列是否真正插入宿主染色体

这是整个分析中最关键的一步。

假设组装得到如下 contig：

```
宿主染色体左侧序列 ── 病毒序列 ── 宿主染色体右侧序列
```

若该 contig 左右两侧分别与宿主染色体某个位置匹配，中间片段与病毒基因组匹配，就形成了一个候选整合事件。

可以先用 minimap2 将组装 contig 比对到宿主参考基因组：

```
minimap2 -ax asm5 host_reference.fa assembly.fa \
  > assembly_to_host.sam
```

同时，将候选 contig 比对到病毒或微生物参考序列，检查是否存在连续的外源同源片段，以及两侧宿主序列的位置和方向是否合理。

对于更可靠的断点分析，可以建立包含宿主与候选外源基因组的联合参考序列，再将原始长读段比对到联合参考，寻找跨越宿主—外源序列边界的 reads。

### 3. 用原始长读段验证插入事件

对每个候选事件，建议至少检查以下证据：

- 跨断点 reads： 单条原始长读段是否覆盖宿主—外源连接点。
- 两侧断点支持： 插入结构的左、右连接点是否都有独立读段支持。
- 覆盖度： 插入片段的覆盖深度是否与预期一致，是否可能来自高拷贝污染或重复序列。
- 结构一致性： 原始 reads 是否支持组装 contig 中的片段顺序、方向和拷贝数。
- 独立证据： 是否能在独立样本、短读长数据、PCR/Sanger 或其他技术中得到支持。

如果外源序列只出现在独立 contig 中，没有任何可信的宿主—外源连接证据，那么更合理的结论是“检测到候选外源序列”，而不是“检测到宿主基因组整合”。

## 四、最值得参考的高水平研究

[Long-read sequencing unveils high-resolution HPV integration and its oncogenic progression in cervical cancer | Nature Communications](https://images.openai.com/static-rsc-4/YeOFDK18WJjDeQxvJU45xNUB7yG865IS7bovxf77hef05OnGGeWlr7QwI_tONgDH0tWoEEN0aDb2KVyWGcL6y13gxGaVIsTgX1M2RtYlElpLneIUthJK1jB22tF2XvcSRtD5VuGHNRKxjML0bcc_HNqOpmTPfJAZZ02ovWE0P54?purpose=inline)

1\. Long-read sequencing unveils high-resolution HPV integration and its oncogenic progression in cervical cancer

Nature Communications, 2022

利用 ONT 长读长解析 HPV16 在宫颈癌中的整合断点、不同插入结构、染色体易位和病毒—宿主环状 DNA。研究展示了短读长难以完整还原的复杂病毒—宿主结构。

[image](https://www.google.com/s2/favicons?domain=https://www.nature.com\&sz=32)

Nature Communications

+1



[论文全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC9091225/)

[PCIP-seq: simultaneous sequencing of integrated viral genomes and their insertion sites with long reads | Genome Biology | Springer Nature Link](https://images.openai.com/static-rsc-4/OO-7nAwxeQkviCa3IXi0SR1f3JOPhEFLIBhoCQ0O3ThCul7ZMxJqXDMSbKNyv9LHkY09nTcFkzylmNiOLlHF9kKyk1dhineJOdFKvp_q65_QxrIn-RKiPFzFCN8PNUN35VSf7dKPxuqKq628uzEaJD1hOBin02KPbg-aSw2hFjg?purpose=inline)

2\. PCIP-seq: simultaneous sequencing of integrated viral genomes and their insertion sites with long reads

Genome Biology, 2021

通过长读长同时分析病毒整合位点及插入病毒的序列，应用于 HIV、HTLV-1、BLV、HPV 和内源性逆转录病毒等场景。适合理解整合位点检测与病毒序列重建的联合策略。

[image](https://www.google.com/s2/favicons?domain=https://pmc.ncbi.nlm.nih.gov\&sz=32)

PMC



[论文全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC8025556/)

[viralFlye: assembling viruses and identifying their hosts from long-read metagenomics data | Genome Biology | Springer Nature Link](https://images.openai.com/static-rsc-4/MFdwCbRkwixUjkaOPT3991pJGJohd1IpalcJ9z5Q5f6ZjP4mJVtDyv_e2vujqvQ3CI6o6wed1UJHpNW7C5nd31PacPSog0ai6wdFq_Xw3dj0SE-lPD15hg1DbnzdjPlbn_kuI-2lhw1IRbM3Ei4CWE8BfrLDNqX5MhGZHRbUBS4?purpose=inline)

3\. viralFlye: assembling viruses and identifying their hosts from long-read metagenomics data

Bioinformatics, 2022

研究长读长宏基因组中病毒组装、病毒多样性以及病毒—宿主关联。适合发现未知病毒或处理混合微生物样本，但其核心任务不是宿主染色体整合断点鉴定。

[image](https://www.google.com/s2/favicons?domain=https://pmc.ncbi.nlm.nih.gov\&sz=32)

PMC

+1



[论文全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC8862349/)

[Detecting virus integration sites based on multiple related sequencing data by VirTect | BMC Medical Genomics | Springer Nature Link](https://images.openai.com/static-rsc-4/tqE0TsSLRVaXoJPfWJ1PwbjQPce0PJRJRs-piCV5hKo8xXqLTHa6_-oqLSiJlvT6Svoj7eYwYSq4RShpg3FvNEtusUjylP24_dMzF3LYbAx9AmFzXWOBJuuTR3rMLaPk6i1vQwYZt1Aq5BwNKmBcQFlSNcVq9OP9tiMxUMnPyPw?purpose=inline)

4\. VirusFinder: Software for Efficient and Accurate Detection of Viruses and Their Integration Sites in Host Genomes through Next Generation Sequencing Data

PLOS ONE, 2013

经典的病毒检测与整合位点识别流程，整合未比对 reads、病毒序列筛查、组装及宿主—病毒断点检测。主要针对较早期的 NGS 数据，使用于长读长时需重新评估其兼容性。

[image](https://www.google.com/s2/favicons?domain=https://pmc.ncbi.nlm.nih.gov\&sz=32)

PMC

+1



[论文全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC3663743/)

## 五、针对你的研究，最值得做的扩展

如果你研究的是已经完成组装的肿瘤或正常真核生物基因组，我建议不仅筛查病毒，还要系统识别细菌、真菌、质粒及其他微生物来源的候选插入序列，并将其分成三个层次：

1. 序列来源证据： 候选插入片段是否确实与病毒或微生物基因组同源。
2. 整合结构证据： 是否存在可信的宿主—外源序列连接点，并由原始长读段支持。
3. 生物学证据： 插入是否影响宿主基因、调控元件、染色体结构或表达；是否存在正常样本、其他个体或公共数据库中的对应序列。

其中，最容易被忽视的是第三点之外的基础问题：微生物同源序列不一定来自近期微生物整合，也可能是古老的水平基因转移、内源性病毒序列、样本污染或组装错误。 这些情况应在结果中分别标注，避免将所有候选序列都解释成新近发生的整合事件。

如果你提供以下两个信息，我可以进一步给出适用于你数据的完整命令行流程：

- 宿主是人类、其他动物、植物，还是肿瘤基因组？
- 你目前有原始 PacBio HiFi / ONT reads，还是只有组装完成的 FASTA？

---

# 近五年关于真核生物基因组中鉴定病毒或者微生物基因组的高水平的文章有哪些，举出30个文献，列出文献名，发表杂志，时间等

可以。你这个方向实际上对应一个很有潜力的研究主题：**eukaryotic genome 中的外源/内源病毒与微生物 DNA（endogenous viral elements, EVEs；endogenous bacterial elements, EBEs；horizontal gene transfer, HGT）**。

我按 **2021–2026 年**检索，并把“严格意义上的整合/内源化研究”和“病毒/微生物→真核生物基因组 HGT、鉴定方法、演化研究”放在一起。需要说明的是，近五年真正同时满足“**真核基因组 + 明确鉴定病毒/微生物来源序列 + 高水平期刊**”的文章数量并没有 30 篇那么多，所以以下 30 篇中，我用 **★** 标出最值得重点阅读的核心文献，其余是扩展文献/方法/综述。

## 一、最值得优先看的 30 篇

| #       | 文献                                                                                                                                         | 期刊                                                     | 年份       | 类型         | 主要内容                                                            |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ | -------- | ---------- | --------------------------------------------------------------- |
| **1★**  | **Escape of the BC200 gene to a human poxvirus reveals its persistent transposition in primates**                                          | **Science**                                            | **2026** | 病毒/TE跨基因组  | 人类 **BC200** 插入传染性软疣病毒（MCV），提示宿主→病毒的近期基因转移                      |
| **2★**  | **Gene ancestries reveal diverse microbial associations during eukaryogenesis**                                                            | **Nature**                                             | **2026** | 微生物→真核     | 通过系统发育重建 LECA，发现多个细菌来源基因输入，并提出 Nucleocytoviricota 介导的病毒转移       |
| **3★**  | **Genomic catalogue of giant viruses reveals expanded diversity and functional potential**                                                 | **Nature Microbiology**                                | **2026** | 巨型病毒/真核基因组 | 系统搜索 giant endogenous viral elements（GEVEs），在多种真核基因组中发现巨型病毒来源序列 |
| **4★**  | **Endogenous viral elements trace the ancient origins and early evolution of the Caulimoviridae**                                          | **PLOS Pathogens**                                     | **2026** | 植物病毒       | 分析 93 个植物基因组，发现 >47,000 个 Caulimoviridae 来源的内源病毒序列              |
| **5★**  | **Discovery of novel non-retroviral endogenous viral elements reveals their long-term integration history in spiders**                     | **Nature Communications**                              | **2025** | nrEVE      | 在蜘蛛基因组中发现大量非逆转录病毒 EVE，并通过结构预测+序列分析鉴定                            |
| **6★**  | **Blood DNA virome associates with autoimmune diseases and COVID-19**                                                                      | **Nature Genetics**                                    | **2025** | 人类病毒整合     | 6,321 个日本 WGS，研究内源性 HHV-6；PacBio HiFi 确认 HHV-6B 位于 chr22q       |
| **7★**  | **Repressive Cytosine Methylation is a Marker of Viral Gene Transfer Across Divergent Eukaryotes**                                         | **Molecular Biology and Evolution**                    | **2025** | 病毒→真核 HGT  | 不同真核生物中病毒来源基因与 DNA 甲基化/异染色质之间的关系                                |
| **8★**  | **Deep mining reveals the diversity of endogenous viral elements in vertebrate genomes**                                                   | **Nature Microbiology**                                | **2024** | 大规模 EVE    | 对 295 个脊椎动物基因组进行系统搜索，发现 **2,040 个 EVE**                         |
| **9★**  | **Genome mining shows that retroviruses are pervasively invading vertebrate genomes**                                                      | **Nature Communications**                              | **2023** | 活跃 ERV     | 2,004 个脊椎动物基因组，发现 412 个正在侵入宿主基因组的 ERVi                          |
| **10★** | **Large-scale invasion of unicellular eukaryotic genomes by integrating DNA viruses**                                                      | **PNAS**                                               | **2023** | 原生生物/病毒    | 在单细胞真核生物中发现大量 14–40 kb DNA 病毒整合片段                               |
| **11★** | **Widespread endogenization of giant viruses shapes genomes of green algae**                                                               | **Nature**                                             | **2020** | 巨型病毒       | **虽然是 2020 年，略超出你的五年窗口，但非常值得纳入必读**；发现 >1 Mb 巨型病毒内源化             |
| **12★** | **Systematic evaluation of horizontal gene transfer between eukaryotes and viruses**                                                       | **Nature Microbiology**                                | **2022** | 病毒↔真核 HGT  | 201 个真核类群、108,842 个病毒类群，系统识别病毒-真核基因交换                           |
| **13★** | **Accumulation of endosymbiont genomes in an insect autosome followed by endosymbiont replacement**                                        | **Current Biology**                                    | **2022** | 细菌→动物      | **4.9 Mb Wolbachia DNA** 整合到 *Drosophila ananassae* chr4        |
| **14★** | **Endogenous Viral Elements in Shrew Genomes Provide Insights into Pestivirus Ancient History**                                            | **Molecular Biology and Evolution**                    | **2022** | 病毒古生态      | 在鼩鼱基因组中发现 Pestivirus-like EVE，推断约 1,080 万年前的整合事件                |
| **15★** | **Endogenous giant viruses contribute to intraspecies genomic variability in the model green alga Chlamydomonas reinhardtii**              | **Virus Evolution**                                    | **2022** | 巨型病毒       | 发现数百 kb 的 giant virus 内源化序列，影响绿藻 pangenome                      |
| **16**  | **Co-option of a non-retroviral endogenous viral element in planthoppers**                                                                 | **Nature Communications**                              | **2023** | EVE功能化     | 飞虱中的非逆转录病毒 EVE 被宿主“驯化/co-option”                                |
| **17**  | **Endogenous viral elements reveal associations between a non-retroviral RNA virus and symbiotic dinoflagellate genomes**                  | **Communications Biology**                             | **2023** | RNA病毒      | 在珊瑚礁相关甲藻基因组中发现 178 个 dinoRNAV EVE                               |
| **18**  | **A tale of caution: How endogenous viral elements affect virus discovery in transcriptomic data**                                         | **Virus Evolution**                                    | **2024** | 方法学        | 系统评估 EVE 对 RNA-seq 病毒发现的影响                                      |
| **19**  | **Endogenous nege-like viral elements in arthropod genomes reveal virus–host coevolution and ancient history of two plant virus families** | **Journal of Virology**                                | **2024** | nrEVE      | 节肢动物中发现 negevirus-like EVE，并用于研究植物病毒演化                          |
| **20**  | **Evidence of endogenous non-retroviral RNA virus sequences into the genome and transcriptome of the malaria vector Anopheles darlingi**   | **Acta Tropica**                                       | **2024** | nrEVE      | 在 *Anopheles darlingi* 中鉴定 44 个 NIRVS                           |
| **21**  | **EEfinder, a general purpose tool for identification of bacterial and viral endogenized elements in eukaryotic genomes**                  | **Computational and Structural Biotechnology Journal** | **2024** | **方法**     | 自动鉴定真核基因组中的 **病毒+细菌内源序列**                                       |
| **22★** | **ContScout: sensitive detection and removal of contamination from annotated genomes**                                                     | **Nature Communications**                              | **2024** | 基因组质控/HGT  | 对 844 个真核基因组分析外源 DNA，区分 contamination 与真实 HGT                   |
| **23**  | **Population genomics in the arboviral vector Aedes aegypti reveals the genomic architecture and evolution of endogenous viral elements**  | **Molecular Ecology**                                  | **2021** | 群体基因组      | 80 个野生 *Aedes aegypti*，研究 nrEVE 的群体多态性                          |
| **24**  | **Virus-derived variation in diverse human genomes**                                                                                       | **PLOS Genetics**                                      | **2021** | 人类基因组      | 在 1000 Genomes/HGDP 等人类基因组中系统寻找病毒来源序列，包括内源 HHV-6                |
| **25**  | **Genome-wide identification of endogenous viral sequences in alfalfa (Medicago sativa L.)**                                               | **Virology Journal**                                   | **2021** | 植物 EVE     | 苜蓿基因组中的内源病毒序列                                                   |
| **26**  | **Phylogenomics of the Maverick Virus-Like Mobile Genetic Elements of Vertebrates**                                                        | **Molecular Biology and Evolution**                    | **2021** | 病毒样移动元件    | 系统研究脊椎动物 Maverick 类病毒样移动元件                                      |
| **27**  | **Paleovirology of the DNA viruses of eukaryotes**                                                                                         | **Trends in Microbiology**                             | **2022** | 综述         | EVE、病毒古生物学、Maverick/Teratorn 等                                  |
| **28**  | **Endogenous DNA viruses take center stage in eukaryotic genome evolution**                                                                | **PNAS**                                               | **2023** | 综述/观点      | 系统讨论 DNA 病毒内源化对真核基因组演化的作用                                       |
| **29**  | **Horizontal gene transfer in eukaryotes: aligning theory with data**                                                                      | **Nature Reviews Genetics**                            | **2024** | 综述         | 真核 HGT 理论、基因组证据和机制                                              |
| **30**  | **Endogenous viral elements: insights into data availability and accessibility**                                                           | **Virus Evolution**                                    | **2024** | 数据/综述      | EVE 数据库、数据共享和系统整理                                               |

---

## 二、其中最值得你重点看的 10 篇

如果你的真正目的是研究：

> **“利用 WGS/长读长基因组，在人类/动物/植物基因组中系统寻找病毒或者微生物来源的外源序列，并研究其插入、结构变异和群体频率。”**

那么我建议优先读下面这些。

### 1. Science 2026：BC200 → poxvirus

**Escape of the BC200 gene to a human poxvirus reveals its persistent transposition in primates**

这是你刚才让我翻译的文章。

它的特殊之处是：

```text
Human genome
     │
     │ BC200
     ↓
LINE-1 mediated retrotransposition
     │
     ↓
Molluscum contagiosum virus
```

也就是说不是传统的：

```text
virus → host
```

而是出现：

```text
host → virus
```

这是研究“**真核基因组与病毒基因组之间双向 DNA 流动**”非常漂亮的案例。论文发表于 **Science, 24 September 2026**。([PubMed][1])

---

### 2. Nature 2026：微生物参与真核生物起源

**Gene ancestries reveal diverse microbial associations during eukaryogenesis**

这是目前非常值得关注的一篇。

作者重新构建 LECA 的蛋白组，通过：

```text
65,000+ prokaryotic genomes
+
1.3 million viral sequence clusters
+
eukaryotic proteomes
        ↓
orthologous groups
        ↓
phylogenetic reconstruction
        ↓
gene ancestry
```

发现除了经典的：

```text
Alphaproteobacteria
Asgard archaea
```

之外，还有 **Myxococcota、Planctomycetota** 等细菌来源，并提出病毒，特别是 **Nucleocytoviricota**，可能作为部分基因转移的中介。([Nature][2])

这篇对你如果想把“**病毒/微生物来源序列 → 真核基因组演化**”做成一个大的研究方向，非常重要。

---

### 3. Nature Microbiology 2024：2,040 个 EVE

**Deep mining reveals the diversity of endogenous viral elements in vertebrate genomes**

这是近几年 EVE 挖掘非常重要的一篇。

作者：

* 295 个脊椎动物基因组
* 24,478 个病毒蛋白查询
* 2,040 个 EVE
* Chuviridae
* Paramyxoviridae
* Benyviridae
* Nairoviridae
* Hepacivirus

其中很多是过去数据库没有记录的病毒“化石”。([Nature][3])

它非常接近你可能想做的：

> **跨物种 genome mining → EVE discovery → phylogeny → integration age → host-virus coevolution**

---

### 4. PNAS 2023：单细胞真核生物中的病毒整合

**Large-scale invasion of unicellular eukaryotic genomes by integrating DNA viruses**

这篇特别值得注意，因为它不是传统动物/植物，而是 **protists**。

发现：

```text
Protist genomes
      ↓
14–40 kb dsDNA virus fragments
      ↓
Virophage
Polinton-like virus
related entities
```

而且很多 EVE 保持相对完整，并具有 integrase。作者特别指出 **long-read sequencing 对解决这些病毒插入非常重要**。([PubMed][4])

这与你目前做的 **PacBio HiFi / ONT / long-read genome** 非常契合。

---

### 5. Current Biology 2022：完整 Wolbachia 基因组进入动物染色体

**Accumulation of endosymbiont genomes in an insect autosome followed by endosymbiont replacement**

这是“**病毒之外，真正微生物基因组进入真核基因组**”的经典案例。

在 *Drosophila ananassae* 中：

> **4.9 Mb Wolbachia DNA**

进入了宿主 chr4。

而且不是简单的几个 bacterial genes，而是：

```text
Wolbachia genome
       ↓
whole-genome integration
       ↓
duplication
       ↓
4.9 Mb nuclear Wolbachia transfers
       ↓
multiple copies
```

并且作者使用 **long-read sequencing + Nanopore direct RNA sequencing** 来确认结构和转录。([科学直通车][5])

这篇对你尤其重要。

---

## 三、病毒和细菌来源可以分成几个研究方向

把这 30 篇放在一起后，其实可以看到一个非常清晰的研究框架。

### A. Virus → eukaryotic genome

即：

```text
Virus
  ↓
integration
  ↓
EVE
  ↓
host genome
```

主要包括：

* ERV
* non-retroviral EVE
* NIRVS
* giant virus EVE
* dsRNA virus EVE
* ssDNA virus EVE
* DNA virus EVE

例如 Nature Microbiology 2024 的大规模 vertebrate EVE mining 就属于这一类。([Nature][3])

---

### B. Bacteria → eukaryotic genome

即：

```text
Bacterium
   ↓
HGT/LGT
   ↓
Eukaryotic chromosome
```

典型就是：

**Wolbachia → Drosophila**

而且可以发生：

```text
whole bacterial genome
        ↓
integration
        ↓
duplication
        ↓
rearrangement
        ↓
gene degeneration
        ↓
gene domestication
```

Current Biology 2022 的 *D. ananassae* 案例就是非常典型的全过程。([科学直通车][5])

---

### C. Virus → eukaryotic gene

这个比完整病毒整合更广。

例如：

```text
viral gene
    ↓
HGT
    ↓
eukaryotic genome
    ↓
functionalization
```

Nature Microbiology 2022 那篇系统研究尤其重要。作者在 201 个真核类群和 108,842 个病毒类群中系统分析了病毒-真核 HGT，发现大量 virus-to-eukaryote transfers。([Nature][6])

---

### D. Eukaryote → virus

你刚才那篇 Science 2026 把这个方向进一步推进：

```text
Human
  ↓
BC200
  ↓
LINE-1
  ↓
retrotransposition
  ↓
poxvirus
```

也就是说：

> **病毒不仅可以把 DNA 带入宿主，宿主 DNA 也可以进入病毒基因组。**

这实际上是一个非常值得做的方向。

---

### E. Virus-mediated HGT

这是现在非常有意思的一个方向：

```text
Bacterium
    ↓
virus
    ↓
eukaryote
```

而不是简单的：

```text
bacterium → eukaryote
```

2026 Nature 的 LECA 工作就提出了这种可能性，认为部分 LECA 基因可能经历：

```text
Prokaryote
    ↓
Nucleocytoviricota virus
    ↓
early eukaryote
```

并估计严格筛选后至少 **4.5% 的 acquisition** 具有病毒介导的信号，其中约 74% 与 Nucleocytoviricota 有关。([Nature][2])

---

# 四、如果你的目标是“自己做一个类似的项目”

我认为你目前的 **PacBio HiFi + ONT + WGS + pan-genome** 背景非常适合把这个方向进一步做成：

## “Eukaryotic genome microbiome / viral fossil atlas”

例如：

```text
                 Eukaryotic WGS
                       │
          ┌────────────┴────────────┐
          │                         │
      Long-read                  Short-read
     HiFi / ONT                    WGS
          │                         │
          └────────────┬────────────┘
                       ↓
               Genome assembly
                       │
          ┌────────────┴────────────┐
          │                         │
      Virus database            Microbe database
          │                         │
      NCBI Virus                 GTDB
      RVDB                       RefSeq
      IMG/VR                     MAGs
          │                         │
          └────────────┬────────────┘
                       ↓
              Genome-wide alignment
                       ↓
              Candidate foreign DNA
                       ↓
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        EVE          EBE           HGT
          │            │            │
          └────────────┼────────────┘
                       ↓
             Long-read validation
                       ↓
              Host–foreign junction
                       ↓
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       SNV/Indel       SV          CNV
          │            │            │
          └────────────┼────────────┘
                       ↓
               Population PAV
                       ↓
             Frequency / haplotype
                       ↓
             Phylogenetic dating
                       ↓
           Host–microbe coevolution
```

尤其可以把你之前关注的 **长读长、SV、群体基因组、pangenome** 全部接进来。

### 我会特别推荐关注 5 个指标

| 分析            | 核心问题               |
| ------------- | ------------------ |
| **PAV**       | 这个病毒/细菌插入是不是群体特异？  |
| **SV**        | 插入长度、倒位、复制、缺失如何变化？ |
| **Haplotype** | 插入与宿主单倍型是否共分离？     |
| **Phylogeny** | 外源序列与现存病毒/细菌的亲缘关系？ |
| **Age**       | 这个整合事件什么时候发生？      |

这样就不再只是“发现几个 EVE”，而可以变成：

> **从发现 → 结构 → 群体 → 演化 → 功能**

的一整套研究。

---

## 五、特别值得你关注的几个数据库/工具

如果你准备实际开展这类 genome mining，我建议重点关注：

* **EEfinder**：同时针对 bacterial + viral endogenized elements，2024 年发表。([科学直通车][7])
* **detectEVE**：专门做 EVE detection，2025 年发表于 *Molecular Ecology Resources*。([PubMed Central (PMC)][8])
* **ContScout**：非常适合解决一个关键问题——**到底是真实 HGT，还是 assembly contamination？** 该研究筛查了 844 个真核基因组。([Nature][9])
* **ViralRecall / GVClass**：特别适合 giant virus / GEVE。
* **BLAST/DIAMOND + HMMER + phylogeny**：适合建立更严格的病毒/微生物来源证据链。

这里其实有一个非常关键的问题：**“在真核基因组中比对到细菌/病毒序列”并不等于“发生了真实整合”。** Contamination、共测序、haplotig、未去除的 endosymbiont、assembly artifact 都可能造成假阳性。ContScout 这类工作正是针对这个问题。([Nature][9])

---

### 如果按你的研究背景，我最建议优先读这 8 篇

```text
1. Science 2026
   BC200 → poxvirus

2. Nature 2026
   microbial associations during eukaryogenesis

3. Nature Microbiology 2026
   giant virus genomic catalogue / GEVEs

4. Nature Microbiology 2024
   2,040 EVEs / 295 vertebrate genomes

5. PNAS 2023
   large-scale invasion of protist genomes

6. Current Biology 2022
   4.9 Mb Wolbachia → Drosophila

7. Nature Microbiology 2022
   systematic virus ↔ eukaryote HGT

8. Nature Communications 2024
   ContScout / contamination vs HGT
```

其中 **#1、#2、#4、#5、#6** 与你现在的 **长读长 + 真核基因组 + SV + 群体基因组** 的结合尤其紧密。

[1]: https://pubmed.ncbi.nlm.nih.gov/42784687/?utm_source=chatgpt.com "Escape of the BC200 gene to a human poxvirus reveals its persistent transposition in primates."
[2]: https://www.nature.com/articles/s41586-026-10639-9 "Gene ancestries reveal diverse microbial associations during eukaryogenesis | Nature"
[3]: https://www.nature.com/articles/s41564-024-01825-4?utm_source=chatgpt.com "Deep mining reveals the diversity of endogenous viral elements in vertebrate genomes | Nature Microbiology"
[4]: https://pubmed.ncbi.nlm.nih.gov/37036967/?utm_source=chatgpt.com "Large-scale invasion of unicellular eukaryotic genomes by integrating DNA viruses - PubMed"
[5]: https://www.sciencedirect.com/science/article/pii/S0960982222007837?utm_source=chatgpt.com "Accumulation of endosymbiont genomes in an insect autosome followed by endosymbiont replacement - ScienceDirect"
[6]: https://www.nature.com/articles/s41564-021-01026-3?utm_source=chatgpt.com "Systematic evaluation of horizontal gene transfer between eukaryotes and viruses | Nature Microbiology"
[7]: https://www.sciencedirect.com/science/article/pii/S2001037024003325?utm_source=chatgpt.com "EEfinder, a general purpose tool for identification of bacterial and viral endogenized elements in eukaryotic genomes - ScienceDirect"
[8]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11969637/?utm_source=chatgpt.com "detectEVE : Fast, Sensitive and Precise Detection of Endogenous Viral Elements in Genomic Data - PMC"
[9]: https://www.nature.com/articles/s41467-024-45024-5?utm_source=chatgpt.com "ContScout: sensitive detection and removal of contamination from annotated genomes | Nature Communications"

---

# 总结在基因组中鉴定病毒或者其他微生物基因组的方法

可以把“**在真核生物基因组中鉴定病毒或其他微生物来源 DNA**”归纳成一个比较完整的 **Genome Mining → 外源序列鉴定 → 整合验证 → 演化与功能分析** 流程。

最关键的一点是：**不要把“BLAST 到病毒/细菌序列”直接当作整合事件。** 一个可靠的研究通常需要同时证明 **来源、基因组位置、宿主-微生物边界、结构完整性、群体存在以及排除污染**。

## 一、总体技术路线

```text
真核生物基因组
WGS / PacBio HiFi / ONT
        │
        ↓
高质量 genome assembly
        │
        ├───────────────┐
        ↓               ↓
蛋白/核酸数据库搜索   assembly QC
        │               │
        ↓               ↓
候选病毒/微生物序列   contamination检查
        │
        ↓
Candidate foreign sequences
        │
        ├──────────────┬──────────────┐
        ↓              ↓              ↓
Virus EVE          Bacterial DNA    Other HGT
        │              │              │
        └──────────────┼──────────────┘
                       ↓
             Host–microbe junction
                       ↓
             Integration validation
                       ↓
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      长度/结构       SV/PAV        Repeat/context
        │              │              │
        └──────────────┼──────────────┘
                       ↓
                  群体基因组
                       ↓
                 Phylogeny
                       ↓
             Integration age
                       ↓
             Functional analysis
```

---

# 二、第一步：准备高质量真核基因组

这是整个分析最容易被忽略、但非常重要的一步。

### 1. Short-read WGS

例如：

```text
Illumina WGS
   ↓
BWA
   ↓
variant calling
   ↓
foreign sequence search
```

优点是：

* 样本多
* 群体频率容易计算
* 成本低

缺点是：

* 很难解决大型病毒插入
* 重复区域容易错配
* 很难解析 host–virus junction
* 很难确定完整插入结构

---

### 2. PacBio HiFi

如果目标是：

> **发现真核基因组中的病毒/细菌整合片段**

PacBio HiFi 非常适合。

```text
HiFi reads
    ↓
hifiasm
    ↓
haplotype-resolved assembly
    ↓
foreign DNA discovery
```

尤其适合：

* EVE
* Wolbachia insertion
* giant virus insertion
* 大型 HGT
* SV
* haplotype-specific insertion

---

### 3. ONT ultra-long

如果插入很大，例如：

```text
virus → host
    500 kb
    1 Mb
    5 Mb
```

ONT ultra-long 可以直接跨越：

```text
host ───── foreign DNA ───── host
```

因此对于确定整合位置非常有价值。

---

# 三、第二步：寻找“外源序列”

这是核心。

基本上有 **5 类方法**。

---

## 方法 1：BLAST / DIAMOND

最经典。

### DNA → DNA

```bash
blastn \
    -query genome.fa \
    -db virus_nt \
    -out virus_hits.tsv \
    -evalue 1e-10 \
    -outfmt 6
```

或者：

```bash
blastn \
    -query genome.fa \
    -db nt \
    -outfmt 6 \
    -evalue 1e-10
```

但是对于大型基因组：

> **不推荐直接用整个 genome 对 NT 做 BLASTN 作为唯一方法。**

计算量太大。

---

### Protein-level search

通常更敏感：

```text
genome
 ↓
gene prediction
 ↓
protein
 ↓
DIAMOND
 ↓
viral/bacterial protein database
```

例如：

```bash
diamond blastp \
    --query proteins.fa \
    --db virus_proteins.dmnd \
    --evalue 1e-5 \
    --outfmt 6 \
    --out viral_hits.tsv
```

对于远缘病毒，protein-level search 往往比 nucleotide BLAST 更有效。

---

# 四、方法 2：HMM / profile-based detection

这是比 BLAST 更重要的一层。

例如很多病毒蛋白非常保守的是：

```text
RNA-dependent RNA polymerase
reverse transcriptase
integrase
capsid protein
helicase
protease
```

可以建立：

```text
HMM database
       ↓
HMMER
       ↓
candidate viral proteins
```

例如：

```bash
hmmsearch \
    --cpu 32 \
    --domtblout viral.domtblout \
    virus_profiles.hmm \
    proteins.fa
```

优点：

> 可以发现已经高度发散、BLAST 相似性很低的病毒来源序列。

所以比较理想的是：

```text
BLAST/DIAMOND
       +
HMMER
       ↓
Candidate EVE
```

---

# 五、方法 3：基于 k-mer / composition 的病毒序列识别

这一类方法不要求非常强的序列同源性。

例如：

```text
k-mer
GC content
codon usage
dinucleotide frequency
sequence composition
```

然后使用机器学习：

```text
sequence
   ↓
k-mer features
   ↓
classifier
   ↓
viral / bacterial / host
```

典型用途：

> 从未知序列中寻找可能的病毒来源区域。

但是：

**它适合 discovery，不适合单独作为最终证据。**

---

# 六、方法 4：Assembly graph / read-level evidence

这是长读长时代非常重要的方法。

假设：

```text
Host genome

chr1
──────────────────────────────
              │
              │
              ▼
          virus DNA
          ─────────
```

如果 assembly graph 中出现：

```text
Host ─────┐
          ├──── Viral
Host ─────┘
```

就可以直接寻找：

> **host–virus junction**

---

对于 PacBio/ONT：

```text
long read
────────────────────────────────────
host──────virus──────host
```

一条 read 如果跨越两侧：

```text
host sequence
       │
       ↓
───────┼──────── virus
       │
       ↓
host sequence
```

这是非常强的整合证据。

---

# 七、方法 5：直接比对病毒/微生物数据库

可以建立：

### Virus database

```text
NCBI Virus
RVDB
RefSeq Viral
IMG/VR
GVD
GVD-derived giant virus database
```

### Bacteria database

```text
GTDB
RefSeq
NCBI nt
MAG databases
```

然后：

```text
host genome
      ↓
minimap2 / DIAMOND / BLAST
      ↓
microbial hits
```

---

# 八、最重要：如何判断是真正的“基因组整合”

找到：

```text
chr5:1000000-1020000
```

有病毒相似序列还远远不够。

建议至少满足下面几个证据。

## Evidence 1：定位在宿主染色体

```text
chr5
────────────────────────────────
host────viral────host
```

而不是：

```text
separate viral contig
```

---

## Evidence 2：host–foreign junction

例如：

```text
Read1:
host | virus

Read2:
host | virus

Read3:
virus | host
```

最好有多个独立 reads 支持。

---

## Evidence 3：双端/长读长证据

Short-read：

```text
R1 → host
R2 → virus
```

Long-read：

```text
host ───── viral ───── host
```

后者更强。

---

## Evidence 4：多个样本中可以重复观察

例如：

```text
Sample1  +
Sample2  +
Sample3  -
Sample4  +
Sample5  -
```

可以进一步研究：

> insertion polymorphism

即：

```text
PAV
Presence / Absence Variation
```

---

# 九、一定要排除 contamination

这是整个领域非常关键的问题。

例如你的 genome：

```text
Host assembly
       │
       ├── bacterial contamination
       ├── fungal contamination
       ├── virus contamination
       └── true HGT
```

仅仅发现：

```text
chr1:50000000
```

有 bacterial sequence 不够。

---

## 常见假阳性来源

### 1. 实验污染

```text
DNA extraction
    ↓
bacteria
fungi
virus
```

---

### 2. Assembly contamination

例如：

```text
contig_12345
```

实际上是：

```text
bacterial chromosome
```

而不是 host chromosome。

---

### 3. 共生菌

例如：

```text
Wolbachia
```

如果 DNA extraction 是 whole insect：

```text
insect
+
Wolbachia
```

那么 assembly 中出现 Wolbachia 序列并不意味着它已经整合。

---

### 4. Haplotig / duplicated contig

两个 haplotype：

```text
hap1
hap2
```

可能被错误当作两个独立基因组区域。

---

# 十、因此推荐使用 contamination filtering

可以采用：

```text
Assembly
   ↓
BUSCO
   ↓
BlobToolKit / BlobTools
   ↓
GC + coverage + taxonomy
   ↓
foreign contig detection
```

尤其推荐：

**BlobToolKit / BlobTools**

因为它可以同时观察：

```text
GC
coverage
taxonomy
```

例如：

```text
                coverage
                    ↑
                    │
 bacteria ●         │
                    │
                    │       ● host
                    │
 virus ●            │
                    └────────────→ GC
```

可以快速发现异常 contig。

---

# 十一、病毒 EVE 的标准分析流程

如果目标专门是：

> **Endogenous Viral Elements**

可以采用：

```text
Genome
   ↓
Repeat masking
   ↓
Gene prediction
   ↓
BLAST/DIAMOND
   ↓
HMMER
   ↓
viral protein hits
   ↓
candidate EVE
   ↓
genomic context
   ↓
host-virus junction
   ↓
read support
   ↓
phylogenetic analysis
   ↓
EVE classification
```

最终输出：

```text
EVE_ID
Chromosome
Start
End
Length
Virus_family
Virus_species
Identity
Coverage
E_value
Supporting_reads
Integration_status
```

---

# 十二、细菌/微生物基因组整合的流程

对于：

```text
Wolbachia
mitochondria
fungi
protists
bacteria
archaea
```

可以：

```text
Eukaryotic genome
        ↓
DIAMOND / BLAST
        ↓
GTDB / RefSeq
        ↓
candidate microbial DNA
        ↓
taxonomy assignment
        ↓
contamination filtering
        ↓
host junction detection
        ↓
long-read validation
        ↓
HGT candidate
```

然后再判断：

```text
真正 HGT
vs
endosymbiont DNA
vs
contamination
```

---

# 十三、如果是你的长读长数据，我最推荐的方案

结合你目前主要做的 **PacBio HiFi / ONT / WGS / SV / pangenome**，我会优先采用：

```text
                 PacBio HiFi / ONT
                         │
                         ↓
                 haplotype-resolved
                    genome assembly
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
       Virus database          Microbe database
       RVDB/RefSeq              GTDB/RefSeq
             │                       │
             └───────────┬───────────┘
                         ↓
                  DIAMOND / BLAST
                         +
                       HMMER
                         ↓
                Foreign DNA candidates
                         ↓
              ┌──────────┴──────────┐
              ↓                     ↓
          Virus EVE              Microbial HGT
              │                     │
              └──────────┬──────────┘
                         ↓
                  minimap2 / BLAST
                         ↓
               Host–foreign junction
                         ↓
                 PacBio/ONT reads
                         ↓
                    SV validation
                         ↓
                   contamination
                     filtering
                         ↓
                High-confidence set
                         ↓
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
      PAV              SV              Haplotype
        │                │                │
        └────────────────┼────────────────┘
                         ↓
                   Population
                    genomics
                         ↓
                    Phylogeny
                         ↓
                  Integration age
                         ↓
              Functional annotation
```

---

# 十四、如果进一步做成一个“系统性研究”，可以分成 4 个层次

| 层次             | 核心问题           | 主要方法                              |
| -------------- | -------------- | --------------------------------- |
| **Discovery**  | 有没有病毒/微生物 DNA？ | BLAST、DIAMOND、HMMER、k-mer         |
| **Validation** | 是不是真整合？        | Long-read、junction、assembly graph |
| **Evolution**  | 什么时候、从哪里来的？    | phylogeny、molecular clock、synteny |
| **Population** | 是否存在群体多态性？     | PAV、SV、haplotype、AF、FST           |

最终可以得到一个非常有价值的：

> **Eukaryotic Foreign DNA Atlas**

例如：

```text
Foreign element
      │
      ├── Virus
      │    ├── Retrovirus
      │    ├── DNA virus
      │    ├── RNA virus
      │    └── Giant virus
      │
      └── Microbe
           ├── Bacteria
           ├── Archaea
           ├── Fungi
           └── Protists
```

再结合：

```text
Species
Population
Chromosome
Insertion age
Insertion frequency
Structure
Gene content
Expression
Selection
```

就可以从单纯的 **“发现病毒序列”** 上升到 **“研究病毒/微生物如何塑造真核生物基因组演化”**。

对于你目前的研究基础，尤其值得做的是 **PacBio HiFi/ONT + pangenome + SV + population genetics** 这一组合，因为短读长研究通常很难可靠解析大型、重复丰富或复杂结构的外源 DNA 插入。



