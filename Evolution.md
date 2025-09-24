
# BUSCO结果构建进化树


## 🧩 思路概览

BUSCO 的核心是利用一组单拷贝直系同源基因 (single-copy orthologs, SCOs)，这些基因是跨物种保守的，非常适合用来构建系统发育树。
主要步骤如下：

1. **提取单拷贝基因序列**

   * BUSCO 在 `run_xxx/busco_sequences/single_copy_busco_sequences/` 目录下会输出每个物种的单拷贝 BUSCO 蛋白/核苷酸序列（FASTA 格式）。
   * 你需要把所有物种对应的同一个 BUSCO ID 的序列整理在一起。

2. **对每个 BUSCO 基因进行多序列比对 (MSA)**

   * 工具：`MAFFT` / `MUSCLE` / `Clustal Omega`
   * 例如：

     ```bash
     mafft --auto input.fasta > aligned.fasta
     ```

3. **比对结果修剪 (可选，但推荐)**

   * 去除低质量比对区域，比如用 `trimAl` 或 `BMGE`。
   * 例：

     ```bash
     trimal -in aligned.fasta -out aligned_trimmed.fasta -automated1
     ```

4. **拼接所有 BUSCO 基因的比对结果 (Concatenation approach)**

   * 将所有单拷贝基因的比对结果按物种顺序拼接成一个超级矩阵 (supermatrix)。
   * 工具：`AMAS`, `FASconCAT-G`。
   * 例如用 AMAS：

     ```bash
     python AMAS.py concat -i *.fasta -f fasta -d aa -p partitions.txt -t supermatrix.phy
     ```

5. **构建系统发育树**

   * 常用方法：

     * **最大似然法 (ML)** → `IQ-TREE`, `RAxML`, `FastTree`
     * **贝叶斯推断 (BI)** → `MrBayes`
   * 例：用 IQ-TREE：

     ```bash
     iqtree2 -s supermatrix.phy -m MFP -bb 1000 -alrt 1000
     ```

     这里 `-m MFP` 自动选择最佳替代模型，`-bb` 自助法 bootstrap。

6. **可视化进化树**

   * 工具：`iTOL` (网页版), `FigTree`, `ETE3` (Python)。



## 🧪 举个简单流程示例

假设你有三个物种：A, B, C，各自跑完 BUSCO：

```
A/run_busco/busco_sequences/single_copy_busco_sequences/
B/run_busco/busco_sequences/single_copy_busco_sequences/
C/run_busco/busco_sequences/single_copy_busco_sequences/
```

操作流程：

```bash
# 1. 整理同源序列
mkdir all_busco
for id in $(ls A/run_busco/.../*.faa | sed 's/.*\///'); do
   cat A/.../$id B/.../$id C/.../$id > all_busco/$id.faa
done

# 2. 对每个基因比对
for f in all_busco/*.faa; do
   mafft --auto $f > ${f%.faa}.aln
done

# 3. 修剪
for aln in all_busco/*.aln; do
   trimal -in $aln -out ${aln%.aln}.trim -automated1
done

# 4. 拼接成超级矩阵
AMAS.py concat -i all_busco/*.trim -f fasta -d aa -t supermatrix.phy -p partitions.txt

# 5. 构建进化树
iqtree2 -s supermatrix.phy -m MFP -bb 1000 -alrt 1000
```



## ⚖️ 另一种方法：基于单基因树的 **共识树 (coalescent-based species tree)**

如果你不想用超级矩阵方法，也可以：

* 对每个 BUSCO 基因分别构建 ML 树（比如用 IQ-TREE）。
* 然后用 `ASTRAL` 或 `MP-EST` 等工具推断物种树。
  这种方法更符合多基因树整合的统计框架。


---
## 选择信号工具

下面我把你列出的 **每个选择信号/方法** 的**核心含义、一般输入要求（是否需要相位/祖先等）**，以及**常见可调参数和判定/归一化方式** 摘要成一张便于参考的表格与要点。这样你在用不同工具时就知道要调哪些参数、输出怎么看、以及对比不同方法时要注意的前置处理。

> 说明：不同软件实现细节会有差异（例如 F\_ST 的计算方法就有多种实现），但下面列出的是每种统计量/方法的一般通用要求与常用参数范式 —— 可直接用于选择工具参数设置的快速参考。


# 快速参考表（每行：方法 — 含义 — 输入/是否需相位/需极化 — 常见参数/阈值/注意事项）

1. **DDAF (ΔDAF / D DAF)**

   * 含义：两群体间**派生等位基因频率差**（Derived Allele Frequency difference）。
   * 输入：需要知道祖先等位基因以判定“派生/祖先”；不需要相位。
   * 参数：群体A/B的等位基因频率；常用阈值是取极端差值 top X% 或设定绝对差（如 >0.3）。注意样本量效应与等位基因频率偏差过滤（MAF 最低阈值）。

2. **HET（Heterozygosity）**

   * 含义：杂合度/多样性（1 - Σp²）。
   * 输入：频率即可，不需相位或极化。
   * 参数：窗口大小（bp或SNP数）、最小SNP数；低杂合度区域可能是选择（或瓶颈）。通常比较基因组背景作 Z-score。

3. **FST1 / FST2（F\_ST）**

   * 含义：群体分化（多种实现：Weir & Cockerham、Hudson 等）。
   * 输入：群体等位基因频率（可用VCF）；不需相位或极化。
   * 参数：按-SNP或窗口计算；窗口大小、最小等位基因计数、方法（Weir-Cockerham vs Hudson）；阈值通常用 top 1%/0.1% 或通过模拟得到显著性。注意样本大小偏差校正。

4. **CL / CLR（Composite Likelihood Ratio）**

   * 含义：基于SFS的**选择扫荡似然比**（如 SweepFinder、SweeD）。
   * 输入：未/已极化的SNP频率（通常需要极化/祖先态更好）。不需要相位。
   * 参数：网格分辨率（步长）、背景SFS估计区域、最小SNP数；输出是局部 CLR 峰，显著性通常用基于模拟的阈值或经验分位数。

5. **Tajima's D**

   * 含义：比较多样性指标（π与θ），负值提示向低频变异富集（可能选择/扩张）。
   * 输入：SNP频率即可，不需相位或极化。
   * 参数：窗口大小、最小SNP数；负值强烈（例如 < -2）常作为异常信号，但需结合 demography。

6. **Rozas R\_2**

   * 含义：基于单个序列样本数与单倍态数的统计，敏感于最近扩张/选择。
   * 输入：SNP频率/序列数据（不需相位）。
   * 参数：样本量影响较大；可用模拟判定显著性。

7. **Fu & Li F / Fu & Li D**

   * 含义：又一类基于SFS的中性检验（考虑单态变异 vs 总变异）。
   * 输入：需要极化（需要祖先态）以区分单态。
   * 参数：窗口/基因区间、最小SNP数；显著性用基于模拟的 p 值。

8. **iHH / iHH1 / iHH0**

   * 含义：integrated Haplotype Homozygosity —— 沿 haplotype homozygosity 曲线对核心等位基因积分。`iHH1`（或 iHH for derived），`iHH0`（或 ancestral）。
   * 输入：**需要相位**与核心等位基因的极化（知道哪个是派生）。
   * 参数：积分距离（cM 或 bp 上限）、核心 SNP 选择、至少 SNP 密度与最小样本数。通常与 iHS 配合。

9. **iHS / normiHS**

   * 含义：标准化的 log(iHH1/iHH0)，用于检测部分清扫（incomplete sweep）。`normiHS` 表示在等位基因频率 bins 内**标准化**后的 iHS。
   * 输入：**需要相位与祖先/派生极化**。
   * 参数：频率分箱（如何分箱非常关键，常见 20 bins）、积分截断、最小MAF（例如 MAF>0.05）、是否在全基因组内作 Z-score 标准化。显著性通常看 |iHS| 大（如 >2）或 top X%。

10. **SL1 / SL0（SweeD / SweepLog?）或 S\_L?**

    * 说明：你列出的 SL1/SL0 可能是指基于长 homozygosity 的某种方向性统计（不同软件命名各异）。若是 S/L 系列，通常与 haplotype length 在祖先/派生分支比较有关。
    * 输入/参数：通常需要相位、选择核心 SNP、设置距离阈值。建议查看具体工具文档（命名不统一）。

11. **nSL / normnSL**

    * 含义：类似 iHS，但直接基于 haplotype length（number of segregating sites 而非 cM），对部分扫荡敏感。`normnSL` 为在频率 bin 标准化后结果。
    * 输入：**需要相位**（但不必极化）。
    * 参数：窗口/积分长度、min MAF、分箱标准化；显著性通常用 |nSL| 大或经验 top%.

12. **iHH12 / normiHH12**

    * 含义：将前两高 haplotype 的整合 homozygosity 合并积分（对软选择/多个高频 haplotype敏感）。
    * 输入：**需要相位**。
    * 参数：积分区间、标准化方法（全基因组分布标准化）；阈值用经验分位数或 Z-score。

13. **iSAFE**

    * 含义：定位选择位点的工具，通过 haplotype 和频率信息优先级排序候选致变位点。
    * 输入：**需相位**，最好有高 SNP 密度与重组速率/基因组位置。
    * 参数：窗口大小、最小 SNP 数、输出 top candidate score；通常把 iSAFE score 最大位点作为候选致变位点。可结合模拟/基因注释判断显著性。

14. **HWE（Hardy-Weinberg Equilibrium）**

    * 含义：等位基因基因型频率与 HWE 偏离（p 值）可提示选择或技术问题（比如非随机交配、污染、基因型错误）。
    * 输入：基因型表（不需相位或极化）。
    * 参数：显著性阈值（如 p < 1e-6 或 Bonferroni 校正后），注意样本结构会导致大量偏离。

15. **PI (π，Nucleotide diversity)**

    * 含义：核苷酸多样性（平均配对差异）。
    * 输入：序列或频率数据（不需相位或极化）。
    * 参数：窗口大小、最小有效碱基/SNP；低 π 可能提示选择或瓶颈，高 π 与平衡选择相关。

16. **FST2**

    * 含义：另一个 F\_ST 形式或在另一软件中的实现（看起来是你用了两个 FST 实现）。
    * 输入/参数：同上 FST，区别在计算公式／无偏估计方法与窗口化策略。要看软件说明选择 Weir-Cockerham / Hudson / Nei 等。

17. **其他（CLR/CL/SL 命名变体、normiHS/normnSL 等）**

    * 含义：很多工具都提供“原始值 + 在频率bin或基因组上标准化后的值（norm...）”。
    * 参数：**标准化策略**（按频率bin或按染色体/全基因组），以及如何处理缺失/低MAF 会显著影响结果。



# 通用的“必须关注”的参数（适用于绝大多数工具）

1. **数据预处理**

   * 过滤低质量SNP与低深度样本。
   * 统一参考/变异等位基因标注（注意 REF/ALT）。
   * 最小 MAF（常用 0.01–0.05）和最小基因型调用率。

2. **相位（phasing）**

   * haplotype-based 方法（iHS、nSL、iHH、iHH12、iSAFE 等）**必须相位**（或误差会大）。常用 Shapeit / Eagle / Beagle 相位。

3. **极化（polarization）**

   * 若统计需要区分祖先/派生（iHS、iHH1/iHH0、DDAF、Fu\&Li 等），必须提供祖先等位基因（outgroup 比对）或用频率推断，但推断有风险。

4. **窗口与步长**

   * 常见单位：固定bp（如 10 kb）、固定SNP数（如 50 SNP）、或cM。选择取决于SNP密度与目标选择规模。

5. **归一化/分箱**

   * 很多基于 haplotype 的统计需要在等位基因频率 bins 内归一化（避免频率依赖性）。例如 iHS 常分 20 个频率 bin。

6. **显著性判定**

   * 常用方法：全基因组经验分布（top 1%/0.1%）、基于 coalescent 模拟或置换得到 p 值、或与附近基因注释结合。不要直接用单点阈值而不考虑 demography。

7. **重组/基因组注释**

   * haplotype 长度基于遗传距离（cM）更合理；有重组图可提升精度。注释（基因、调控区）用于解释候选位点。

8. **样本量影响**

   * 许多统计量在小样本下方差大；尽量保证每组至少几十个样本（越多越好）。



# 常见分析流程建议（实践清单）

1. 质量过滤（MAF、缺失、深度、HWE 过滤视情况）。
2. 相位（对 haplotype 方法）并保留相位质量。
3. 极化（如果需要祖先态），并记录未极化位点的处理方式。
4. 逐方法计算：SFS-based（Tajima, Fu\&Li, CLR）、freq-diff（DDAF, FST）、haplotype-based（iHS, nSL, iHH12, iSAFE）。
5. 在相应频率 bins / window 中做标准化或归一化。
6. 用经验分布或模拟获得阈值，合并不同统计的交集以提高置信（例如 iHS高且π低）。
7. 注释候选区域，考虑基因组特征（重复、低覆盖、重组低区）排除假阳性。


如果你愿意，我可以接下来做其中一件事（你任选其一）并给出具体命令 / 示例参数：

* A. 针对你的数据（告诉我是 VCF、样本数、是否相位/有祖先态），给出一套 **iHS / nSL / iSAFE** 的具体命令与参数（包括如何标准化）。
* B. 给出 **Windows/命令行下 SweeD/SweepFinder** 运行示例和如何解析 CLR 峰。
* C. 给出计算 **FST（Weir–Cockerham）与 ΔDAF** 的 awk/VCFtools/PLINK 命令示例并解释阈值。

你选一个，我就直接给出可复制粘贴的命令和参数。



👉 我可以帮你写一个自动化 **Python 脚本 / Snakemake 流程** 来从 BUSCO 输出直接跑完 MSA → 拼接 → 树推断。
你是希望走 **超级矩阵拼接法 (concatenation)** 还是 **基因树整合法 (coalescent)**？
