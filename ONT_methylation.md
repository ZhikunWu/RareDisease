# ONT测序的群体的甲基化可以做哪些方面的研究

拥有 **200例人群的ONT全基因组甲基化数据**，这在目前（2025-2026年）的科研界绝对是一个**“核武器”级别的资源**。

大多数现有的甲基化研究还停留在二代测序（BS-seq）或芯片（EPIC array）水平，它们有两个巨大的盲区：**无法有效覆盖重复序列（如LINE-1）** 和 **丢失了单倍型（Haplotype）信息**。

利用ONT的长读长优势，您可以做以下 **4个具有高创新性（High Impact）** 的研究方向，这些都非常符合 **H23口（医学遗传学）** 对“新技术揭示新机制”的偏好：

### 1. 挖掘“基因组暗物质”：重复序列的甲基化调控 (最契合您之前的LINE-1构思)

这是二代测序做不了、只有三代能做的绝对优势领域。

* **科学问题：** LINE-1、Alu 等转座子通常处于高甲基化（沉默）状态。在川崎病患者中，特定的**活跃LINE-1亚家族（如L1HS）** 是否发生了去甲基化？
* **具体做法：**
* **Locus-specific分析：** 不要看全基因组平均值，而是精确定位到每个具体的LINE-1位点（Unique mapping），看它是否去甲基化。
* **转录激活关联：** 结合您的RNA-seq数据，看去甲基化的LINE-1是否表达了RNA，甚至触发了免疫反应（类似病毒模仿）。


* **H23亮点：** 揭示了传统手段漏掉的表观遗传调控机制。

### 2. 单倍型特异性甲基化 (ASM, Allele-Specific Methylation)

ONT 单条read长，天然自带分型（Phasing）信息，您可以直接看到**“父源染色体”和“母源染色体”上甲基化的差异**。

* **研究方向 A（印记基因）：** 鉴定新的组织特异性印记基因。
* **研究方向 B（X染色体失活/逃逸）：** **（重点！）** 您之前提到有女性样本。利用ASM，您可以精准区分哪一条X染色体是失活的（Xi），哪一条是活跃的（Xa）。
* 可以研究 **XCI逃逸基因（Escapees）** 的启动子甲基化状态。
* 结合Hi-C，看逃逸基因在三维空间上是否逃离了Barr体的异染色质区域。


* **H23亮点：** 极高分辨率的等位基因特异性调控，这是精准医学遗传学的前沿。

### 3. 结构变异（SV）驱动的甲基化数量性状位点 (SV-mQTL)

您有200人的群体，可以做一个小型的**mQTL（methylation Quantitative Trait Loci）GWAS**。

* **科学问题：** 基因组上的结构变异（如一个500bp的缺失或插入）是如何影响周边（甚至远端）CpG岛的甲基化状态的？
* **具体做法：**
* 利用 Sniffles2 鉴定出的 SV 作为基因型。
* 利用 ONT call 出的甲基化频率作为表型。
* 进行关联分析。


* **创新点：** 传统的 mQTL 都是基于 SNP 的，基于 SV 的 mQTL 目前还非常少，特别是在长读长数据中。您可以发现 **“SV -> 改变染色质构象 -> 改变甲基化 -> 改变表达”** 的完整因果链条。

### 4. 单分子层面的甲基化异质性 (Epigenetic Heterogeneity)

二代测序给出的是一个位点的“平均甲基化率”（比如60%），但它不知道这60%是来自于“60%的细胞全甲基化”还是“所有细胞都随机甲基化”。

* **科学问题：** 在川崎病相关的炎症通路基因启动子区，是否存在**甲基化模式的混乱（Entropy增加）**？
* **具体做法：** 利用信息熵（Shannon entropy）算法，计算单个read层面的甲基化无序度。这通常是细胞状态不稳定或疾病发生的早期标志。



### 💡 给您的具体执行建议（针对H23标书）

如果您想把这部分写进标书中，建议采用 **“多组学级联（Cascade）”** 的逻辑：

1. **现象（Phenotype）：** 发现川崎病CALs患者存在特定的 SV（由Sniffles2检出）。
2. **表观机制（Epigenetics）：** 该 SV 导致了邻近区域的 **等位基因特异性去甲基化（ASM）**（由ONT甲基化分析检出）。
3. **三维结构（3D Genome）：**这种甲基化改变破坏了 CTCF 结合位点，导致 **TAD 边界重塑**（由Hi-C检出）。
4. **转录后果（Expression）：** 最终激活了致病基因或 LINE-1 的异常表达（由RNA-seq检出）。

**推荐工具：**

* **Modkit (Nanopore官方)：** 速度最快，生成 bedMethyl 文件。
* **NanoMethViz：** 非常漂亮的R包，专门画ONT甲基化的图（MDS plot, Heatmap等）。
* **DSS (R包)：** 用于寻找差异甲基化区域 (DMR)，虽然是为BS-seq设计的，但也适用于ONT数据。

您目前的200例数据，只要主要关注 **ASM** 或 **重复序列** 这两个点，在 H23 评审专家眼中就是**“降维打击”**级别的技术优势。




---

# 现在有多个物种的基因组和甲基化数据，可以做哪些分析

拥有多个物种的基因组（Genome）与甲基化（DNA Methylation，通常是 WGBS 数据）数据是一个非常宝贵的资源，这使你能够从**进化生物学**和**表观遗传学**的双重维度开展研究。

以下是几个具有深度且目前主流的研究方向：



### 1. 比较表观基因组学分析 (Comparative Epigenomics)

这是最核心的方向，旨在研究甲基化模式在物种间的保守性与差异。

* **甲基化图谱进化**：分析不同物种在同源基因（Orthologs）上的甲基化水平。研究哪些基因的甲基化模式在数百万年的进化中保持不变（保守区），哪些发生了剧变。
* **物种特异性甲基化位点 (DMS)**：识别哪些甲基化变异是某个物种独有的，并关联该物种的特异表型（如适应性进化、抗病性等）。
* **启动子与增强子的表观演变**：研究调控元件的甲基化状态如何随着物种分化而改变，进而解释基因表达水平在物种间的差异。



### 2. 甲基化与基因组演化的关联

研究 DNA 序列本身的特征与表观修饰之间的相互作用。

* **TE（转座子）的表观调控**：转座子是基因组进化的动力。对比不同物种中 TE 的种类、分布以及它们的甲基化程度。研究甲基化如何作为一种“防御机制”来抑制不同物种中 TE 的活性。
* **C-to-T 突变偏差**：胞嘧啶甲基化后容易发生自发脱氨基变为胸腺嘧啶。你可以分析高甲基化区域是否对应着更高的 C-to-T 突变率，以及这种表观压力如何驱动基因组序列的长期演变。
* **GC 含量与甲基化**：研究基因组 GC 含量分布与甲基化偏好性在不同物种间的相关性。



### 3. 表观遗传时钟 (Epigenetic Clock) 与寿命研究

如果你的物种样本具有已知的年龄信息，这是一个非常热门的方向。

* **跨物种表观钟构建**：研究不同物种中是否存在通用的年龄相关甲基化位点。
* **寿命与甲基化速率**：比较长寿物种（如大象、裸鼹鼠）与短寿物种（如小鼠）在特定基因位点上甲基化丢失或增加的速度差异。这有助于揭示衰老的生物学机制。



### 4. 甲基化对基因家族扩张/收缩的影响

结合你之前的基因组研究：

* **基因家族分析**：当某个基因家族在特定物种中发生扩张（Expansion）时，研究其新产生的旁系同源基因（Paralogs）是否经历了表观遗传重构（Epigenetic Remodeling）。
* **亚基因组优势（针对多倍体）**：如果你研究的是多倍体物种，可以分析不同亚基因组之间的甲基化差异如何导致基因的选择性沉默或激活。



### 5. 整合分析：表观-转录-进化 (Multi-omics Integration)

* **Evo-Devo（进化发育生物学）**：如果数据包含不同发育阶段，可以研究甲基化如何控制关键发育基因（如 Hox 基因）在不同物种中的时空表达差异。
* **环境适应性表观学**：分析生活在不同生境（如高原 vs 平原）的物种，其甲基化差异是否富集在与环境应激（缺氧、低温）相关的信号通路上。



### 建议的技术路线与工具

| 分析步骤 | 推荐工具 |
| --- | --- |
| **同源基因识别** | OrthoFinder, SonicParanoid |
| **甲基化比对与提取** | Bismark, BWA-meth, MethylDackel |
| **跨物种坐标转换** | LiftOver, HalLiftOver (针对全基因组比对) |
| **差异统计** | DSS, methylKit, DMRcaller |
| **跨物种整合分析** | R 语言 (Chippseeker, clusterProfiler) |

### 💡 一个值得尝试的小切入点：

你可以选取一个在所有物种中都存在的**高产相关基因**（如我们之前讨论的 *BMP15* 或 *GDF9*），观察其在不同生育力物种间的启动子甲基化程度。这可以直接关联到你之前关于“哺乳动物生育数量”的研究课题。

**你需要我针对其中某一个方向（如 TE 统计或跨物种 LiftOver 流程）提供具体的代码示例吗？**



---

# dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam这种文件是什么，如何看出甲基化位点

这个文件名里**信息非常多，而且非常“规范”**，本身就已经告诉你：
👉 **这是一个包含碱基修饰（DNA 甲基化）信息的 Nanopore 比对 BAM 文件**。

我分三层给你讲清楚：
**① 这是什么文件
② 甲基化信息是如何存储的
③ 如何真正“看出”甲基化位点（实操）**



## 一、`dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam` 是什么？

### 1️⃣ Dorado 是什么？

* **Dorado** = ONT 官方新一代 **basecaller + modified base caller**
* 替代：

  * Guppy
  * Megalodon（已停止维护）

👉 **Dorado 可以在碱基识别的同时，直接识别 DNA 修饰**



### 2️⃣ 文件名逐段解释（非常重要）

```
dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam
```

| 片段          | 含义                    |
| ----------- | --------------------- |
| dorado0.6.0 | Dorado 软件版本           |
| sup4.3.0    | 使用的 super-accuracy 模型 |
| 5mCG        | 识别 CpG 上的 5-甲基胞嘧啶     |
| 5hmCG       | 识别 CpG 上的 5-羟甲基胞嘧啶    |
| .bam        | 已比对的 BAM              |

✅ **结论**：
这是一个 **Nanopore reads → 参考基因组** 的比对 BAM，
**并且已经在 BAM 里编码了 5mC / 5hmC 的信息**。



## 二、甲基化信息在 BAM 里是怎么存的？（核心）

Nanopore 的修饰信息 **不是普通的碱基替换**，而是存放在 **SAM/BAM 的可选标签（tags）里**。



### 1️⃣ 两个关键标签（必须记住）

| Tag      | 含义             |
| -------- | -------------- |
| `MM:Z:`  | 哪些碱基被预测为修饰     |
| `ML:B:C` | 对应修饰的概率（0–255） |

这是 **SAM v1.6 官方规范**，不是 Dorado 私货。



### 2️⃣ 一个真实例子（示意）

```text
MM:Z:C+m?,10,5,20;
ML:B:C,180,200,150
```

解释：

* `C+m`：5mC
* `?`：CpG context
* `10,5,20`：修饰位点在 read 上的位置
* `ML`：每个位点的置信度

👉 **所以：甲基化不是一行一行的位点，而是嵌在 reads 里**



## 三、如何“看出”甲基化位点？（实操方法）

### 方法一 ⭐⭐⭐⭐⭐（最标准）：`modkit`

> **官方推荐，最稳**

#### 1️⃣ 安装

```bash
conda install -c bioconda modkit
```



#### 2️⃣ 从 BAM 提取甲基化位点（per-site）

```bash
modkit pileup \
  dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam \
  methylation.tsv \
  --ref reference.fa
```

输出示例：

| chrom | pos   | strand | mod | frac | depth |
| ----- | ----- | ------ | --- | ---- | ----- |
| chr1  | 10023 | +      | 5mC | 0.82 | 35    |

👉 **这一步你才真正得到“基因组坐标上的甲基化位点”**



#### 3️⃣ 区分 5mC 和 5hmC

```bash
modkit pileup ... --mods 5mC 5hmC
```



### 方法二 ⭐⭐⭐⭐：IGV 直接可视化（非常直观）

#### 1️⃣ 准备

* IGV ≥ 2.16
* 加载：

  * reference.fa
  * BAM + .bai

#### 2️⃣ 设置

* 右键 BAM track
* 勾选：

  * **Show modified bases**

你会看到：

* CpG 位点上
* 颜色深浅表示甲基化概率

📌 **适合做展示图，不适合做统计**



### 方法三 ⭐⭐⭐：samtools 查看单条 read（调试用）

```bash
samtools view dorado.bam | less
```

搜索：

```text
MM:Z
ML:B
```

📌 只适合确认文件是否真的包含甲基化信息



## 四、常见误区（非常重要）

### ❌ 误区 1：BAM 里没有“甲基化位点列表”

✔ 正确：

* BAM 里是 **read-level 修饰概率**
* 位点级别需要 **pileup 统计**



### ❌ 误区 2：所有 C 都有修饰信息

✔ 实际：

* 只对 **CpG context**
* 且模型支持的修饰类型



### ❌ 误区 3：5mC 和 5hmC 可以随便信

✔ 注意：

* 5hmC 区分难度更高
* 需：

  * 高质量 reads
  * 足够深度



## 五、审稿人最常问的 3 个问题（你要能答）

1. **Which basecaller and model were used?**
   → Dorado 0.6.0, sup4.3.0, 5mCG/5hmCG model

2. **How were methylation levels quantified?**
   → modkit pileup, per-site aggregation

3. **What confidence threshold was applied?**
   → ML ≥ X（如 ≥128）



## 六、一句话“论文级”总结（可直接用）

> *DNA methylation was detected directly from Nanopore sequencing data using Dorado with the 5mCG/5hmCG model, and per-site methylation levels were quantified by aggregating read-level modification probabilities with modkit.*



## 如果你愿意，我可以继续帮你：

* 🔹 **按基因 / 区间统计甲基化水平**
* 🔹 **把 5mC / 5hmC 分开分析**
* 🔹 **与 Hi-C / ASE / rDNA / 着丝粒联用**
* 🔹 **站在 NSFC / 审稿人角度设计甲基化分析**

你可以直接告诉我：
👉 **物种？覆盖度？想分析全基因组还是特定区域（如 rDNA / X 染色体）？**





# 原来的甲基化bam文件未比对至染色体，如何比对到染色体，同时获取甲基化位点信息

这是一个**非常关键、而且很多人会踩坑的问题**。我直接给你**可操作的标准流程**，并明确指出**哪些步骤会导致甲基化信息丢失、哪些不会**。



# 一、先给结论（避免走弯路）

> **Nanopore 甲基化 BAM（Dorado 产生）可以重新比对到染色体坐标，并且甲基化信息不会丢失，前提是：
> 你必须使用支持 MM/ML 标签的比对流程。**

**正确流程是：**

```
原始 FASTQ（含修饰标签）
   ↓ minimap2（保留 MM/ML）
重新比对到染色体参考
   ↓
排序 / index
   ↓
modkit pileup
```

⚠️ **错误做法**（会丢甲基化信息）：

* ❌ 从 BAM 转 FASTQ 时不保留 modified base tags
* ❌ 用旧版 minimap2 / samtools
* ❌ 先 pileup 再比对



# 二、你的“原始甲基化 BAM”现在处于什么状态？

你现在的情况是：

> **BAM 中已有 5mC / 5hmC，但没有比对到染色体级参考**

常见来源包括：

* 比对到：

  * contig
  * scaffold
  * 临时参考
* 或：

  * basecalling 阶段输出的未比对 BAM

👉 **解决方案：重新比对**



# 三、标准做法一（⭐⭐⭐⭐⭐ 强烈推荐）：从 FASTQ 重新比对

## Step 1️⃣ 从 Dorado BAM 导出 FASTQ（保留修饰）

### ⚠️ 关键点

* 必须保留 **modified base tags**
* 使用 **samtools ≥1.17**

```bash
samtools fastq \
  -T MM,ML \
  dorado_methylation.bam \
  > reads_with_mods.fastq
```

> `-T MM,ML` = 明确告诉 samtools 不要丢掉甲基化标签



## Step 2️⃣ 比对到染色体级参考基因组

```bash
minimap2 -ax map-ont \
  reference.chrom.fa \
  reads_with_mods.fastq \
  | samtools sort -o aligned.chrom.bam
```

📌 minimap2 **天然支持 MM/ML 标签透传**



## Step 3️⃣ 建立索引

```bash
samtools index aligned.chrom.bam
```



## Step 4️⃣ 验证甲基化信息是否仍在

```bash
samtools view aligned.chrom.bam | grep "MM:Z" | head
```

如果能看到 `MM:Z:` 和 `ML:B`
👉 **说明成功**



## Step 5️⃣ 提取染色体坐标上的甲基化位点

```bash
modkit pileup \
  aligned.chrom.bam \
  methylation.per_site.tsv \
  --ref reference.chrom.fa \
  --mods 5mC 5hmC
```



# 四、标准做法二（⭐⭐⭐）：直接重新比对 BAM（不推荐）

> **理论可行，但容易踩坑**

```bash
samtools view -h dorado.bam \
| minimap2 -ax map-ont reference.fa - \
| samtools sort -o aligned.bam
```

❌ 风险：

* 中间 pipe 若不支持 MM/ML
* samtools 旧版本会 silently drop tags

👉 **不推荐用于正式分析**



# 五、常见问题 & 你一定要检查的 5 件事

## 1️⃣ FASTQ 是否真的含修饰信息？

```bash
grep "MM:Z" reads_with_mods.fastq | head
```

如果没有 → **你已经丢信息了**



## 2️⃣ minimap2 版本

```bash
minimap2 --version
```

✅ 推荐 ≥ 2.24



## 3️⃣ samtools 版本

```bash
samtools --version
```

✅ 推荐 ≥ 1.17



## 4️⃣ Dorado 输出模式

如果你还有原始数据，**最稳的方式是：**

```bash
dorado basecaller \
  sup \
  pod5s/ \
  --modified-bases 5mCG 5hmCG \
  > reads.fastq
```

再从 FASTQ 走完整流程



## 5️⃣ 染色体命名是否一致

* BAM：`chr1`
* FASTA：`1`

❌ 不一致 → pileup 会失败



# 六、IGV 可视化（验证用）

* 加载：

  * `aligned.chrom.bam`
  * `reference.chrom.fa`
* 勾选：

  * **Show modified bases**

你应该能看到 CpG 位点的颜色深浅



# 七、审稿人最容易问的 3 个问题（提前准备）

1. **How were modified bases preserved during alignment?**
   → samtools fastq -T MM,ML + minimap2

2. **Which reference genome was used for methylation calling?**
   → chromosome-level assembly

3. **How were per-site methylation levels derived?**
   → modkit pileup



# 八、一句话“Methods 级总结”（可直接用）

> *Nanopore reads containing base modification tags were re-aligned to the chromosome-level reference genome using minimap2 while preserving MM and ML tags, and per-site DNA methylation levels were quantified using modkit.*



## 如果你愿意，我可以继续帮你：

* 🔹 **写一整套 shell pipeline（可直接跑）**
* 🔹 **检查你当前 BAM 是否已经丢失修饰信息**
* 🔹 **按基因 / rDNA / 着丝粒统计甲基化**
* 🔹 **把甲基化与 Hi-C / SV / ASE 联合分析**

你只需要告诉我一句：
👉 **你现在手里的文件是 FASTQ、POD5，还是已经只有 BAM？**

---

# 如何得到phased 甲基化位点，具体代码是什么

要得到 Phased（定相/分型）的甲基化位点，核心逻辑是将 BAM 文件中的每一条 Read 标记上它属于父本（H1）还是母本（H2），然后利用这些标记将甲基化统计分开。

通常的流程是：**Variant Calling (找变异) -> Phasing (定相) -> Haplotagging (给Reads打标签) -> Modkit Pileup (分型统计)**。

鉴于你的数据是 Nanopore R10.4.1 数据，以下是目前最主流、准确的流程代码，使用 **Clair3** (变异检测), **Whatshap** (定相), 和 **Modkit** (甲基化提取)。

### 准备工作

你需要：

1. **BAM 文件**: `input.bam` (你的文件)
2. **参考基因组**: `ref.fa` (必须与生成 BAM 时用的参考基因组一致，如 hg38)
3. **工具**: 安装 `clair3`, `whatshap`, `modkit`, `samtools`, `tabix`


### 第一步：变异检测 (Variant Calling)

如果还没有该样本的 VCF 文件，需要先从 BAM 中通过 Clair3 找到 SNP，这是区分两条染色体的基础。

```bash
# 运行 Clair3 (根据你的机器配置调整 threads)
# 注意：模型路径 (--model_path) 需要指向你安装的 r1041_e82_400bps_sup_v420 (或其他对应模型)
run_clair3.sh \
  --bam_fn=11_7_23_R1041_HPRC_HG02583_1_dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam \
  --ref_fn=ref.fa \
  --threads=32 \
  --platform="ont" \
  --model_path="/path/to/clair3_models/r1041_e82_400bps_sup_v420" \
  --output=clair3_output

```

*产出*: `clair3_output/merge_output.vcf.gz`

### 第二步：变异定相 (Phasing) & Reads 标记 (Haplotagging)

使用 Whatshap 利用长读长跨越 SNP 的能力，将 VCF 中的变异分型，并将分型信息（HP tag）写回 BAM 文件。

```bash
# 1. 对 VCF 进行定相 (Phasing)
whatshap phase \
  --output phased.vcf.gz \
  --reference ref.fa \
  --ignore-read-groups \
  clair3_output/merge_output.vcf.gz \
  11_7_23_R1041_HPRC_HG02583_1_dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam

# 建立索引
tabix -p vcf phased.vcf.gz

# 2. 对 BAM 进行单倍型标记 (Haplotagging)
# 这一步会在 BAM 的 reads 中添加 HP:i:1 或 HP:i:2 标签
whatshap haplotag \
  --output haplotagged.bam \
  --reference ref.fa \
  --ignore-read-groups \
  phased.vcf.gz \
  11_7_23_R1041_HPRC_HG02583_1_dorado0.6.0_sup4.3.0_5mCG_5hmCG.bam

# 建立 BAM 索引
samtools index haplotagged.bam

```

*产出*: `haplotagged.bam` (这是带有单倍型信息的 BAM)

### 第三步：提取 Phased 甲基化信号 (Modkit)

现在 BAM 文件里的 Reads 都有了 `HP` 标签，使用 `modkit pileup` 的 `--partition-tag` 参数即可直接输出分型的甲基化结果。

```bash
# 提取 5mC 并按单倍型分开
modkit pileup \
  haplotagged.bam \
  phased_methylation.bed \
  --ref ref.fa \
  --cpg \
  --partition-tag HP \
  --combine-strands

```

* **`--partition-tag HP`**: 这是一个关键参数。它告诉 modkit 根据 BAM 文件中的 `HP` 标签将 reads 分成不同的组（ungrouped, haplotype 1, haplotype 2）。
* **`--cpg`**: 仅输出 CpG 位点（通常做 phased methylation 主要关注 CpG）。



### 结果解读

生成的 `phased_methylation.bed` 是一个增强版的 bedMethyl 文件。它通常包含如下列（具体取决于 modkit 版本，但逻辑一致）：

Modkit 会输出带有 partition 信息的列。你需要关注其中的 **Tag** 或 **Partition** 列。

* **Haplotype 1 (HP1)**: 对应父本或母本的一条染色体。
* **Haplotype 2 (HP2)**: 对应另一条。
* **None/Ungrouped**: 无法确定来源的 reads（通常是因为该区域缺乏 SNP 或 reads 质量不够）。

你可以通过 `awk` 或 `pandas` 进一步筛选，例如：

```bash
# 筛选 Haplotype 1 的高置信度位点
awk '$4 == "1" {print $0}' phased_methylation.bed > haplotype1.bed

# 筛选 Haplotype 2 的高置信度位点
awk '$4 == "2" {print $0}' phased_methylation.bed > haplotype2.bed

```

*(注：具体第几列代表 partition id 请查看 modkit 输出文件的 header，通常是前几列之一)*

### 总结

1. **Clair3**: 找变异。
2. **Whatshap**: 定相并给 BAM 打上 `HP` 标签。
3. **Modkit**: 识别 `HP` 标签并分别计算 H1 和 H2 的甲基化频率。

这个流程得到的 `phased_methylation.bed` 可以让你直接比较同一个位点在父本和母本染色体上的甲基化差异（ASM, Allele-Specific Methylation）。




