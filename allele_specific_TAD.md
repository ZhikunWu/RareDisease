# 如何用HiC数据检测到到二倍体的TAD，即检测两个亲本各自的TAD

这是一个**非常前沿、而且审稿人会非常感兴趣的问题**。直接说结论：

> **要在二倍体中检测“两个亲本各自的 TAD（allele-specific TAD, AS-TAD）”，核心不在于 TAD 算法本身，而在于：
> 👉 能否把 Hi-C 联系“分相（phasing）”到父源和母源。**

下面我从**原理 → 可行技术路线 → 具体操作流程 → 工具 → 难点与审稿人视角**，系统讲清楚。



## 一、为什么普通 Hi-C 看不到“两个 TAD”？

常规 Hi-C 的结果是：

* 把 **父源 + 母源染色体的互作平均在一起**
* 得到的是：

  > **population-averaged TAD**

即使：

* 父源 TAD 边界 ≠ 母源 TAD 边界
  最终也会被“抹平”

👉 **所以必须做等位特异（allele-specific）Hi-C**


## 二、检测二倍体 TAD 的核心前提（必须满足）

### 1️⃣ 能区分父源 / 母源染色体

至少满足一项：

| 条件        | 说明         |
| --------- | ---------- |
| 高密度杂合 SNP | 人、杂交材料     |
| 已知亲本基因型   | F1、杂交      |
| 已完成单倍型组装  | HiFi / T2T |
| trio 数据   | 父母 + 子代    |



### 2️⃣ Hi-C reads 能被分相（phased）

* read1 / read2 中：

  * 覆盖杂合 SNP
* 可被分到：

  * haplotype 1（父源）
  * haplotype 2（母源）



## 三、三条主流技术路线（由易到难）



## 路线 A ⭐⭐⭐⭐

## **SNP-based allele-specific Hi-C（最常用）**

### 适用场景

* 人类
* 杂交植物
* F1 群体



### Step 1：获得 phased SNP

* WGS + phasing
* 工具：

  * WhatsHap
  * SHAPEIT
  * trio phasing（最稳）



### Step 2：Hi-C reads 分相

常用工具：

#### ⭐ WASP（经典）

```bash
WASP pipeline
```

#### ⭐ SNPsplit（简单）

```bash
SNPsplit --snp_file phased_snps.vcf hic.bam
```

结果：

* paternal Hi-C BAM
* maternal Hi-C BAM



### Step 3：分别构建 Hi-C contact matrix

```bash
cooler cload pairs hap1.pairs hap1.cool
cooler cload pairs hap2.pairs hap2.cool
```



### Step 4：分别调用 TAD

```bash
insulation-score / Arrowhead / HiCExplorer
```

👉 得到：

* 父源 TAD
* 母源 TAD



## 路线 B ⭐⭐⭐⭐⭐

## **单倍型组装 + Hi-C 比对（强烈推荐）**

### 适用场景

* 已有 haplotype-resolved assembly
* HiFi / T2T 项目



### Step 1：构建两套参考基因组

* hap1.fa（父源）
* hap2.fa（母源）



### Step 2：Hi-C reads 分别比对

```bash
bwa mem hap1.fa hic_R1.fq hic_R2.fq > hap1.sam
bwa mem hap2.fa hic_R1.fq hic_R2.fq > hap2.sam
```

#### 决策规则：

* 比对质量更高的 → 该 haplotype



### Step 3：分别建 contact map + TAD

优点：

* 不依赖 SNP 覆盖
* TAD 边界最清晰
* 审稿人最认可

📌 **这是目前高水平论文主流方案**



## 路线 C ⭐⭐⭐⭐⭐（最前沿）

## **Trio Hi-C / 单倍型 Hi-C**

* 父母 + 子代 Hi-C
* 或：

  * 单倍型特异 Hi-C 建库（少见）

👉 几乎无歧义，但成本高



## 四、TAD 调用算法选择（重要）

| 算法               | 适合 AS-TAD |
| ---------------- | --------- |
| Insulation score | ⭐⭐⭐⭐⭐（稳健） |
| HiCExplorer      | ⭐⭐⭐⭐      |
| Arrowhead        | ⭐⭐（对噪声敏感） |
| Spectral         | ⭐⭐⭐       |

📌 **等位特异数据通常 reads 更少，选稳健算法**



## 五、如何判断“两个亲本 TAD 不同”？（统计层面）

### 1️⃣ TAD 边界偏移

* hap1 boundary ≠ hap2 boundary
* 偏移 > bin size × 2



### 2️⃣ 差异 insulation score

```text
ΔIS = IS_hap1 − IS_hap2
```



### 3️⃣ TAD 强度差异

* intra-TAD contact enrichment
* hap1 vs hap2



## 六、审稿人最常问的 6 个问题（提前准备）

1. How were Hi-C reads phased?
2. What fraction of reads were allele-informative?
3. Is coverage sufficient for TAD calling?
4. How robust are boundary differences?
5. Could differences arise from noise?
6. Are AS-TADs associated with ASE / epigenetics?



## 七、与功能数据的“杀手级联用”

**审稿人非常喜欢：**

* AS-TAD ↔ ASE
* AS-TAD ↔ H3K27ac
* AS-TAD ↔ DNA methylation
* AS-TAD ↔ imprinting / XCI



## 八、一句话 Methods（可直接用）

> *Hi-C reads were assigned to parental haplotypes based on phased heterozygous SNPs, and allele-specific contact matrices were constructed independently for each haplotype. Topologically associating domains were then identified separately to reveal haplotype-specific TAD organization.*



## 九、现实建议（非常重要）

### 如果你的目标是：

| 目标   | 推荐路线                |
| ---- | ------------------- |
| 方法成熟 | SNPsplit            |
| 结果最稳 | 单倍型组装               |
| 高分论文 | hap-assembly + Hi-C |
| 经费有限 | SNP-based           |



## 最后一句“直白总结”

> **检测二倍体中两个亲本各自的 TAD，本质是一个“Hi-C 分相问题”，而不是一个“TAD 算法问题”。**



如果你愿意，我可以下一步帮你：

* 🔹 **根据你现有数据判断哪条路线最合适**
* 🔹 **给你一套可直接跑的 SNPsplit / cooler pipeline**
* 🔹 **设计“AS-TAD + ASE”的论文结构**
* 🔹 **站在 NSFC / NIH 审稿人角度帮你选切入点**

你只需要告诉我：
👉 **物种？是否有 trio？是否已有单倍型组装？Hi-C 深度？**

----

# SNPsplit是哪个工具里面的？有什么特点


**SNPsplit 不是某个大工具里的一个子模块，而是一个**
👉 **独立的、专门用于等位特异（allele-specific）分析的工具**。

下面我从 **“它是什么 → 属于谁 → 能做什么 → 优缺点 → 适用场景 → 审稿人视角”** 六个方面给你一个**专业级、但清晰**的说明。



## 一、SNPsplit 是什么？

> **SNPsplit 是一个基于已知 SNP 的 reads 分相工具，用于将测序 reads 分配到不同等位（父源 / 母源）**。

* 最早目的：

  * **allele-specific ChIP-seq / RNA-seq**
* 现在广泛用于：

  * **allele-specific Hi-C**

📌 它本身 **不做比对、不做 TAD、不做统计**
👉 只负责：**reads 属于哪个等位？**



## 二、SNPsplit 属于哪个项目？

### 📦 来源

* **Babraham Bioinformatics**
* 作者：

  * Felix Krueger（也是 Trim Galore / Bismark 作者）

### 📍 官方主页

> [https://www.bioinformatics.babraham.ac.uk/projects/SNPsplit/](https://www.bioinformatics.babraham.ac.uk/projects/SNPsplit/)

📌 和 **Bismark、Trim Galore 同一生态**，但**完全独立**



## 三、SNPsplit 的核心工作原理（非常重要）

### 输入

1. **已比对的 BAM 文件**
2. **杂合 SNP 文件（VCF / SNP 格式）**

   * 已知：

     * 父源 / 母源等位



### 核心逻辑

对每一条 read：

1. 是否覆盖 ≥1 个杂合 SNP？
2. read 上的碱基 = 父源？母源？
3. 决策规则：

   * **只支持父源 → 分到父**
   * **只支持母源 → 分到母**
   * **支持冲突 → 丢弃**
   * **不覆盖 SNP → unassigned**



### 输出

* paternal.bam
* maternal.bam
* unassigned.bam
* ambiguous.bam

👉 **极其清晰、可解释**



## 四、SNPsplit 的突出特点（为什么这么常用）

### ✅ 优点（审稿人认可）

| 特点      | 说明            |
| ------- | ------------- |
| 逻辑简单    | 决策规则透明        |
| 可复现     | 结果不依赖参数       |
| 偏倚可控    | 冲突 reads 直接丢弃 |
| 工具成熟    | 发表论文中大量使用     |
| 支持 Hi-C | 可处理双端 reads   |



### ⚠️ 局限（你要心里有数）

| 局限                  | 影响                 |
| ------------------- | ------------------ |
| 依赖 SNP 密度           | SNP 少 → 可分 reads 少 |
| 无 reference bias 校正 | 需配合 WASP           |
| 丢弃率较高               | reads 利用率下降        |
| 不能自行 phasing        | SNP 必须已分相          |



## 五、SNPsplit vs WASP（常被问）

| 维度   | SNPsplit | WASP   |
| ---- | -------- | ------ |
| 功能   | reads 分相 | 去除比对偏倚 |
| 是否替代 | ❌        | ❌      |
| 是否互补 | ✅        | ✅      |
| 复杂度  | 低        | 高      |
| 运行速度 | 快        | 慢      |

👉 **标准高质量流程：**

```
WASP（去偏倚） → SNPsplit（分相）
```



## 六、在 Hi-C 中使用 SNPsplit 的完整位置

```
Hi-C reads
   ↓
比对到参考基因组
   ↓
（可选）WASP 过滤
   ↓
SNPsplit
   ↓
hap1 Hi-C BAM
hap2 Hi-C BAM
   ↓
cooler / juicer
   ↓
AS-TAD / AS-loop
```



## 七、审稿人常问 & 标准回答

### Q: Why SNPsplit?

> *SNPsplit provides a conservative and reproducible approach to assign Hi-C reads to parental haplotypes based on phased SNPs, minimizing ambiguous assignments.*

### Q: What fraction of reads were allele-informative?

> 通常：

* 人类：10–30%
* F1 杂交：30–60%



## 八、什么时候 **不该** 用 SNPsplit？

❌ SNP 极少（近交、纯合）
❌ 没有 phasing 信息
❌ 想“最大化 reads 利用率”

👉 这种情况更适合：

* 单倍型组装 + 比对



## 九、一句话总结（给你一个“标准定义”）

> **SNPsplit is a standalone tool developed by Babraham Bioinformatics for allele-specific read assignment based on phased SNP information, widely used in allele-specific Hi-C, RNA-seq, and ChIP-seq analyses.**



如果你愿意，我可以下一步：

* 🔹 **帮你评估 SNP 密度是否足够用 SNPsplit**
* 🔹 **给你一套 SNPsplit + cooler 的可运行命令**
* 🔹 **从审稿人角度判断用 SNPsplit 是否“站得住”**
* 🔹 **对比 SNPsplit vs 单倍型组装路线，帮你选**

你只需要告诉我：
👉 **物种？是否 F1 / trio？每 Gb Hi-C 有多少 SNP？**



