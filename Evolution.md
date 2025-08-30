
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



👉 我可以帮你写一个自动化 **Python 脚本 / Snakemake 流程** 来从 BUSCO 输出直接跑完 MSA → 拼接 → 树推断。
你是希望走 **超级矩阵拼接法 (concatenation)** 还是 **基因树整合法 (coalescent)**？
