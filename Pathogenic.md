# Annovar 注释过滤
要从您提供的变异注释文件中筛选出**潜在的致病变异**，通常需要遵循一套系统性的过滤策略，主要关注以下三个方面：**突变质量、人群频率** 和 **功能影响/预测**。

以下是根据您的列名提供的筛选步骤和建议标准：



## 1. 质量控制 (QC) 过滤

首先排除低质量的变异，确保变异数据的可靠性。

| 列名 | 建议筛选标准 | 说明 |
| :--- | :--- | :--- |
| **FILTER** | **PASS** | 仅保留通过所有质量过滤的变异。排除 $\text{LowQual}$、$\text{GATK}$ $\text{filter}$ 等标记的变异。 |
| **QUAL** | $\ge$ **20 或 $\ge$ 30** | 保留测序质量得分较高的变异（具体阈值取决于实验质量）。 |
| **KD636\_Depth** | $\ge$ **10** 或 $\ge$ **20** | 样本的测序深度应足够高，以确保变异的可靠性。 |
| **KD636\_AltFreq** | $\ge$ **0.2**（对于杂合）或 $\ge$ **0.8**（对于纯合） | 根据预期的遗传模式（如常染色体显性/隐性）来确定最小的等位基因频率，排除可能是测序错误的低频变异。 |



## 2. 人群频率过滤：排除常见多态性

致病变异通常在普通人群中**非常罕见**。利用大型人群数据库的等位基因频率 ($\text{AF}$) 数据，排除常见的良性变异。

| 列名 | 建议筛选标准 | 说明 |
| :--- | :--- | :--- |
| **gnomad\_exome\_AF** | **$\le$ 0.01**（或更严格，如 **$\le$ 0.005**） | 这是所有人群外显子组数据的总等位基因频率。对于明确的孟德尔遗传疾病，应使用更严格的阈值。 |
| **gnomad\_genome\_ALL** | **$\le$ 0.01**（或更严格） | $\text{gnomAD}$ 基因组数据的总等位基因频率。 |
| **ExAC\_ALL** | **$\le$ 0.01**（或更严格） | $\text{ExAC}$ 数据库的总等位基因频率。 |
| **1kg\_AF** | **$\le$ 0.01**（或更严格） | $\text{1000}$ 基因组计划的总等位基因频率。 |
| **所有人群特异性 AF 列** | **$\le$ 0.01**（或更严格） | 包括 $\text{gnomad\_exome\_AF\_eas}$、$\text{gnomad\_exome\_AF\_popmax}$、$\text{1kg\_EAS\_AF}$ 等。确保变异在所有主要人群（尤其是您研究的东亚人群，$\text{EAS}$）中都罕见。 |

**$\Rightarrow$ 筛选原则：** 仅保留在所有人群数据库中**最大等位基因频率（$\text{AF}$）低于某个阈值**（如 $1\%$ 或 $0.5\%$）的变异。



## 3. 功能影响过滤：关注有害突变

致病变异往往会对蛋白质功能产生显著影响。

### 3.1 基于基因区域和突变类型

| 列名 | 建议保留的值 | 说明 |
| :--- | :--- | :--- |
| **Func\_refGene** | **exonic** ($\text{外显子}$)、**splicing** ($\text{剪接}$ $\text{位点}$) | 这两类变异最有可能影响蛋白质结构或表达。也可以保留 $\text{ncRNA}$ 或 $\text{UTR}$ 变异，但需后续更详细分析。 |
| **ExonicFunc\_refGene** | **frameshift $\text{deletion/insertion}$** ($\text{移码}$ $\text{缺失/插入}$)、**stopgain** ($\text{终止}$ $\text{密码子}$ $\text{获得}$)、**nonframeshift $\text{deletion/insertion}$** ($\text{非移码}$ $\text{缺失/插入}$)、**nonsynonymous $\text{SNV}$** ($\text{非同义}$ $\text{单核苷酸}$ $\text{变异}$) | 这些类型会对蛋白质功能产生实质性改变。通常**同义 ($\text{synonymous}$ $\text{SNV}$) 变异**可被排除，除非其位于剪接位点附近或已知影响 $\text{mRNA}$ 稳定性。 |
| **AAChange\_refGene** | $\text{检查}$ $\text{AA}$ $\text{改变}$ $\text{是否}$ $\text{影响}$ $\text{关键}$ $\text{功能}$ $\text{域}$ | 用于确认氨基酸变化，结合 $\text{Interpro\_domain}$ 评估变异是否位于重要蛋白功能域。 |

### 3.2 基于预测软件评分

使用多种计算工具预测变异的有害性，并要求多个工具都预测为有害。

| 列名 | 建议保留的值/阈值 | 说明 |
| :--- | :--- | :--- |
| **SIFT\_pred** | **D** ($\text{Damaging}$，有害) | 预测氨基酸替换是否耐受。 |
| **Polyphen2\_HDIV\_pred** | **D** ($\text{Probably}$ $\text{Damaging}$ 或 $\text{Possibly}$ $\text{Damaging}$) | 预测氨基酸替换是否影响蛋白质结构和功能。 |
| **MetaSVM\_pred** | **D** ($\text{Damaging}$，有害) | 基于多种预测工具的集成预测结果。 |
| **CADD\_phred** | **$\ge$ 20** 或 **$\ge$ 25** | 综合预测变异有害性的分数。$\ge 20$ 表示变异在人群中最有害的 $1\%$ 之内；$\ge 25$ 表示 $0.5\%$。 |
| **REVEL\_score** | **$\ge$ 0.75** | 用于预测错义变异致病性的集成工具，分数越高越可能致病。 |
| **GERP++\_RS** | **$\ge$ 2** 或 **$\ge$ 3** | 进化保守性得分。得分越高，保守性越强，变异越可能有害。 |

**$\Rightarrow$ 筛选原则：** 至少**三个或更多**可靠的预测软件（如 $\text{SIFT}$、$\text{Polyphen}2$、$\text{CADD}$、$\text{REVEL}$）将变异预测为**有害 ($\text{Damaging}$ / $\text{Deleterious}$)**。



## 4. 临床和文献关联过滤：确认致病性

最后，利用临床数据库和文献信息，寻找已被报道的致病变异。

| 列名 | 建议保留的值/策略 | 说明 |
| :--- | :--- | :--- |
| **CLNSIG** | 关注 **Pathogenic** ($\text{致病性}$)、**Likely $\text{Pathogenic}$** ($\text{可能致病性}$)、**$\text{Conflicting}$ $\text{interpretations}$ $\text{of}$ $\text{pathogenicity}$** | **CLNSIG** 是 $\text{ClinVar}$ 数据库的临床意义。应优先保留被判为致病性的变异。对于 $\text{VUS}$ ($\text{意义}$ $\text{不}$ $\text{明}$ $\text{变异}$)，需要更深入分析。 |
| **CLNDN** | 检查是否有与研究疾病相关的疾病名称 | $\text{ClinVar}$ 数据库中的疾病名称。 |
| **CosmicID** | **存在 $\text{ID}$** | 如果是肿瘤相关的研究，$\text{COSMIC}$ ($\text{癌症}$ $\text{体}$ $\text{细胞}$ $\text{突}$ $\text{变}$ $\text{图}$ $\text{谱}$ $\text{数据库}$) 编号的存在表明其可能与癌症相关。 |



## 总结的推荐筛选流程

一个实用的、由粗到精的筛选流程如下：

1.  **质量过滤：** $\text{FILTER} = \text{PASS}$ $\text{AND}$ 样本 $\text{Depth}$ $\ge 10$。
2.  **功能过滤 (I)：** $\text{Func\_refGene}$ **是外显子或剪接位点**。
3.  **频率过滤：** $\text{gnomad\_exome\_AF\_popmax} \le 0.005$ ($\text{0.5\%}$) $\text{AND}$ $\text{ExAC\_ALL} \le 0.005$。
4.  **功能过滤 (II) & $\text{ClinVar}$ 优先：**
    * **优先级 1：** $\text{CLNSIG}$ $\text{包含}$ $\text{Pathogenic}$。
    * **优先级 2：** $\text{ExonicFunc\_refGene}$ **是 $\text{stopgain}$、$\text{frameshift}$ $\text{insertion/deletion}$** $\text{（}$通常影响最大）。
    * **优先级 3：** $\text{ExonicFunc\_refGene}$ **是 $\text{nonsynonymous}$ $\text{SNV}$** $\text{AND}$ 至少 $\text{3}$ 个预测工具（如 $\text{SIFT}$ $\text{pred}$、$\text{Polyphen}2$ $\text{HDIV}$ $\text{pred}$、$\text{MetaSVM}$ $\text{pred}$）预测为有害 $\text{D}$ $\text{（}$$\text{Damaging}$ $\text{）}$ $\text{AND}$ $\text{CADD\_phred} \ge 20$。

筛选后得到的变异列表将是您需要深入分析的**潜在致病变异**。对于最终筛选出的变异，需要结合**疾病遗传模式、家族验证**和**相关文献**来确认其致病性。
