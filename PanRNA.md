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

