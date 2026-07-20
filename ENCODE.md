# NCODE各类数据的清单
ENCODE（**Encyclopedia of DNA Elements**）项目的数据主要用于系统解析基因组功能元件，包括转录调控、染色质状态、DNA/RNA-蛋白互作等。ENCODE Portal 提供实验数据、处理后的数据以及元数据。([ENCODE][1])

下面按**数据类型（Data Category）/实验技术（Assay）**列出 ENCODE 的主要数据清单。([ENCODE][2])

---

# 1. 转录组（Transcriptomics）数据

| 数据类型              | 实验方法                                                                    | 主要内容               |
| ----------------- | ----------------------------------------------------------------------- | ------------------ |
| RNA-seq           | RNA sequencing                                                          | 基因表达量、转录本丰度        |
| Total RNA-seq     | 全RNA测序                                                                  | mRNA、lncRNA、非编码RNA |
| polyA RNA-seq     | poly(A)+ RNA测序                                                          | 成熟mRNA表达           |
| CAGE              | Cap Analysis of Gene Expression                                         | 转录起始位点（TSS）        |
| RAMPAGE           | RNA Annotation and Mapping of Promoters for Analysis of Gene Expression | 启动子活性              |
| GRO-seq           | Global Run-On Sequencing                                                | 新生RNA转录            |
| PRO-seq           | Precision Run-On Sequencing                                             | 高分辨率转录活动           |
| TT-seq            | Time-resolved Transcriptome sequencing                                  | 新生成RNA             |
| miRNA-seq         | microRNA sequencing                                                     | miRNA表达            |
| long-read RNA-seq | 长读长RNA测序                                                                | 全长转录本结构            |
| scRNA-seq         | 单细胞RNA测序                                                                | 单细胞基因表达谱           |

---

# 2. 转录因子结合（TF Binding）数据

主要用于定位转录因子（Transcription Factor, TF）在基因组上的结合区域。

| 数据类型          | 实验方法                                            | 结果        |
| ------------- | ----------------------------------------------- | --------- |
| TF ChIP-seq   | 转录因子ChIP测序                                      | TF结合峰     |
| ChIP-exo      | ChIP + exonuclease                              | 超高分辨率结合位点 |
| CUT&RUN       | Cleavage Under Targets & Release Using Nuclease | TF/蛋白结合   |
| CUT&Tag       | Cleavage Under Targets and Tagmentation         | TF和染色质定位  |
| ChIP-chip（早期） | 芯片杂交                                            | TF结合区域    |

典型靶标：

* CTCF
* REST
* MYC
* JUN
* FOXA1
* GATA1
* TP53
* POLR2A

---

# 3. 组蛋白修饰（Histone Modification）数据

用于研究染色质开放和调控状态。

| 数据类型             | 实验方法        | 生物意义       |
| ---------------- | ----------- | ---------- |
| Histone ChIP-seq | 组蛋白ChIP-seq | 修饰分布       |
| H3K4me1          | ChIP-seq    | 增强子标记      |
| H3K4me3          | ChIP-seq    | 活跃启动子      |
| H3K27ac          | ChIP-seq    | 活跃增强子      |
| H3K27me3         | ChIP-seq    | Polycomb抑制 |
| H3K9me3          | ChIP-seq    | 异染色质       |
| H3K36me3         | ChIP-seq    | 转录延伸       |
| H4K20me1         | ChIP-seq    | 染色质状态      |

---

# 4. 染色质开放性（Chromatin Accessibility）

| 数据类型      | 方法                                                     | 用途      |
| --------- | ------------------------------------------------------ | ------- |
| DNase-seq | DNase I hypersensitivity                               | 开放染色质区域 |
| ATAC-seq  | Assay for Transposase Accessible Chromatin             | 开放区域    |
| FAIRE-seq | Formaldehyde-Assisted Isolation of Regulatory Elements | 调控区域    |
| MNase-seq | Micrococcal nuclease sequencing                        | 核小体定位   |

输出：

* DHS peaks（DNase hypersensitive sites）
* ATAC peaks
* nucleosome occupancy

---

# 5. DNA甲基化（DNA Methylation）

| 数据类型    | 方法                                          | 内容         |
| ------- | ------------------------------------------- | ---------- |
| WGBS    | Whole Genome Bisulfite Sequencing           | 全基因组CpG甲基化 |
| RRBS    | Reduced Representation Bisulfite Sequencing | CpG富集区域甲基化 |
| TAB-seq | TET-assisted bisulfite sequencing           | 5hmC检测     |

主要分析：

* CpG methylation
* Differential methylation regions (DMR)

---

# 6. 三维基因组（3D Genome）

研究染色体空间结构。

| 数据类型     | 方法                                                  | 数据                  |
| -------- | --------------------------------------------------- | ------------------- |
| Hi-C     | Genome-wide chromosome conformation capture         | 全基因组互作              |
| ChIA-PET | Chromatin Interaction Analysis with Paired-End Tags | TF介导互作              |
| HiChIP   | Hi-C + ChIP                                         | 蛋白相关染色质互作           |
| PLAC-seq | Protein-Ligand Associated Chromatin                 | enhancer-promoter互作 |

分析：

* TAD（Topologically Associating Domain）
* Loop
* A/B compartment
* enhancer-promoter interaction

---

# 7. DNA结合蛋白与RNA互作

| 数据类型     | 方法                       | 内容                    |
| -------- | ------------------------ | --------------------- |
| eCLIP    | enhanced CLIP-seq        | RNA-binding protein结合 |
| CLIP-seq | RNA-protein interaction  | RBP结合                 |
| RIP-seq  | RNA immunoprecipitation  | RNA结合蛋白               |
| ChAR-seq | Chromatin-associated RNA | RNA-染色质关系             |

典型蛋白：

* RBFOX
* HNRNPC
* ELAVL1
* FUS

---

# 8. 蛋白质组 / 蛋白-DNA相关数据

| 数据类型                      | 方法     | 内容       |
| ------------------------- | ------ | -------- |
| Antibody characterization | 抗体验证   | ChIP抗体质量 |
| Protein localization      | IF/IHC | 蛋白空间定位   |
| Mass spectrometry         | 质谱     | 蛋白表达     |

---

# 9. 单细胞（Single-cell）数据

ENCODE 第四阶段增加大量单细胞数据。([arXiv][3])

| 数据类型       | 技术         |
| ---------- | ---------- |
| scRNA-seq  | 单细胞转录组     |
| scATAC-seq | 单细胞染色质开放   |
| Multiome   | RNA + ATAC |
| scCUT&Tag  | 单细胞组蛋白修饰   |
| scHi-C     | 单细胞三维基因组   |

---

# 10. CRISPR功能筛选数据

用于验证功能元件。

| 数据类型                   | 方法             |
| ---------------------- | -------------- |
| CRISPR knockout screen | 基因敲除筛选         |
| CRISPRi screen         | 转录抑制           |
| CRISPRa screen         | 转录激活           |
| Perturb-seq            | CRISPR + scRNA |

研究：

* enhancer功能
* gene regulation
* essential genes

---

# 11. 增强子/调控元件功能验证

| 数据类型                      | 方法                                                    |
| ------------------------- | ----------------------------------------------------- |
| MPRA                      | Massively Parallel Reporter Assay                     |
| STARR-seq                 | Self-transcribing Active Regulatory Region sequencing |
| Enhancer reporter assay   | 增强子验证                                                 |
| Transgenic enhancer assay | 动物模型增强子实验                                             |

---

# 12. 基因组序列与参考数据

| 数据类型               | 内容         |
| ------------------ | ---------- |
| Reference genome   | hg38、mm10等 |
| Genome annotation  | 基因模型       |
| Blacklist regions  | 高噪声区域      |
| mappability tracks | 可比对性       |
| Repeat annotation  | 重复序列       |

---

# 13. ENCODE标准化分析输出文件

ENCODE通常提供：

| 文件类型                   | 格式                           |
| ---------------------- | ---------------------------- |
| Raw reads              | FASTQ                        |
| Alignment              | BAM                          |
| Peaks                  | BED / narrowPeak / broadPeak |
| Signal tracks          | bigWig                       |
| Counts                 | TSV                          |
| Gene expression matrix | TSV                          |
| Methylation calls      | bedGraph                     |
| Hi-C matrices          | hic                          |
| Metadata               | JSON                         |

([ENCODE][4])

---

# 14. ENCODE核心数据类别总结表

| 大类                      | 代表实验                     |
| ----------------------- | ------------------------ |
| Gene expression         | RNA-seq                  |
| TF binding              | TF ChIP-seq              |
| Histone marks           | H3K27ac/H3K4me3 ChIP-seq |
| Chromatin accessibility | DNase-seq, ATAC-seq      |
| DNA methylation         | WGBS                     |
| Genome architecture     | Hi-C                     |
| RNA binding             | eCLIP                    |
| Single-cell             | scRNA/scATAC             |
| Functional screening    | CRISPR                   |
| Regulatory elements     | MPRA/STARR-seq           |

([ENCODE][2])

---

如果用于**机器学习/基因组大模型（Genome Foundation Model）训练**，ENCODE 数据通常会进一步整理成：

1. **序列数据**

   * FASTA
   * k-mer
   * one-hot DNA

2. **功能轨迹（tracks）**

   * ATAC-seq
   * DNase-seq
   * ChIP-seq
   * RNA-seq
   * methylation

3. **三维结构**

   * Hi-C contact matrix

4. **多组学标签**

   * enhancer
   * promoter
   * TF binding
   * chromatin state

这些是目前训练 DNABERT、Enformer、Nucleotide Transformer 等模型时常用的 ENCODE 数据组成。

[1]: https://www.encodeproject.org/about/data-access/?utm_source=chatgpt.com "Access to ENCODE data – ENCODE"
[2]: https://www.encodeproject.org/data-analysis/?utm_source=chatgpt.com "Data Analysis – ENCODE"
[3]: https://arxiv.org/abs/2305.00006?utm_source=chatgpt.com "Data navigation on the ENCODE portal"
[4]: https://www.encodeproject.org/help/data-organization/?utm_source=chatgpt.com "Data Organization – ENCODE"


