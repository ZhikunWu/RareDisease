# Annotating the genome at single-nucleotide resolution with DNA foundation models

这篇论文《**Annotating the genome at single-nucleotide resolution with DNA foundation models**》（2025年）提出了一种名为 **SegmentNT** 的深度学习模型，用于对人类及多物种基因组进行**单碱基分辨率注释**。下面是该模型使用的数据集与训练过程的详细总结：



### 📊 使用的数据集

SegmentNT 的训练数据来自两个主要来源：

1. **[GENCODE](https://www.gencodegenes.org/)**：

   * 提供人类基因元素注释，如：

     * 蛋白编码基因（protein-coding genes）
     * lncRNA
     * 5’UTR、3’UTR
     * 外显子（exon）、内含子（intron）
     * 剪接位点（splice acceptor/donor）

2. **[ENCODE](https://www.encodeproject.org/)**：

   * 提供调控元件注释：

     * polyA信号
     * 各类启动子（promoters）：组织不变型和组织特异型
     * 增强子（enhancers）
     * CTCF结合位点（CTCF-bound sites）

这些元素的注释被处理为**每个碱基位置的标签**，构建成一个多标签的分段（segmentation）任务。



### 🧠 模型架构和训练方式

#### 1. **基础模型：Nucleotide Transformer (NT)**

* 来源：[Nucleotide Transformer 项目](https://github.com/instadeepai/nucleotide-transformer)
* 是一个用**自监督方法**预训练的大规模 DNA 基础模型。
* 使用方式：SegmentNT 使用 NT 提供的 DNA 表示作为编码器输出。

#### 2. **SegmentNT 架构：**

* 编码器：Nucleotide Transformer（预训练模型）
* 解码器：1D U-Net（用于对 DNA 表示进行下采样-上采样，进行多尺度分段预测）
* 每个元素类型有一个二分类输出，最后预测每个碱基属于某种元素的概率。

#### 3. **训练任务设置：**

* 任务类型：多标签二分类 + 分段任务（类似图像语义分割）
* 输入长度：

  * 先训练 3kb → 微调到 10kb → 进一步扩展到 20kb 和 30kb（SegmentNT-30kb）
* 损失函数：**Focal Loss**，用于处理类别不平衡问题（很多碱基并不属于注释区域）

#### 4. **跨物种泛化**

* 多物种模型训练使用：

  * 人类 + 小鼠、鸡、果蝇、斑马鱼、线虫等
  * 测试集包含其他10种动物和5种植物（如玉米、水稻、小麦、拟南芥）
* 所使用的物种注释来自 **Ensembl** 数据库



### 🧪 性能评估与对比

* 评估指标包括：MCC（Matthews correlation coefficient）、auPRC、F1、Jaccard等。
* 与其他工具比较：

  * **AUGUSTUS**：基于HMM的传统注释工具
  * **SpliceAI、Pangolin**：剪接位点预测模型
  * **DeePromoter、NTv2**：启动子预测
* SegmentNT 表现出更高的精度和更强的泛化能力，尤其在全基因组尺度预测时优势明显。



### 📦 模型可用性

* 提供了以下预训练模型：

  * SegmentNT-30kb（人类和多物种）
  * SegmentEnformer（基于 Enformer 编码器）
  * SegmentBorzoi（基于 Borzoi 编码器）
* GitHub：[https://github.com/instadeepai/nucleotide-transformer](https://github.com/instadeepai/nucleotide-transformer)
* HuggingFace：[https://huggingface.co/collections/InstaDeepAI/segmentnt-65eb4941c57808b4a3fe1319](https://huggingface.co/collections/InstaDeepAI/segmentnt-65eb4941c57808b4a3fe1319)


### ✅ 总结

| 模型/数据 | 说明                                   |
| ----- | ------------------------------------ |
| 预训练模型 | Nucleotide Transformer（NT），使用自监督方法训练 |
| 注释数据  | GENCODE（基因结构），ENCODE（调控元件）           |
| 目标元素  | 14类（如exon、enhancer、splice sites等）    |
| 训练任务  | 分段（segmentation），多标签二分类              |
| 架构    | NT 编码器 + U-Net 解码器                   |
| 结果    | 在单碱基分辨率上优于现有方法，能泛化到多个物种              |



如果你需要复现实验、下载模型或使用其训练数据，我可以提供对应的代码片段或下载链接。是否需要我为你准备这些？

---

# DNA language model GROVER learns sequence context in the human genome




## 🧠 一、主要工作概述

这篇文章的核心贡献是：

> 构建并训练了一个名为 **GROVER** 的 DNA语言模型（类似GPT），它能够捕捉人类基因组中核苷酸序列的上下文特征，并在多个下游生物学任务中取得领先表现。

### ✨ 简单地说，它的主要工作是：

* 借鉴 NLP 语言模型的思想，**在全人类基因组上训练 transformer 模型**；
* 让模型学习 DNA 序列的“语言结构”，即核苷酸的上下文依赖关系；
* 将其用于下游任务如基因结构预测、调控元件定位、突变效应预测等；
* 模型无需显式注释或标签，依赖自监督训练，仅用原始 DNA 序列。



## 🧪 二、具体方法与技术路线

### 1. **模型结构**

* 模型基于 **Transformer decoder 架构**，结构上类似 GPT：

  * 输入：DNA序列（ATCG）
  * 编码：将每个核苷酸转为embedding（4个基本字母 + 1个mask token）
  * 模型目标：预测下一个碱基（语言建模任务）

### 2. **训练数据**

* 使用 **人类参考基因组 GRCh38**（不含重复区域、未组装染色体）
* 序列切分为 1kb 长度，每个token是一个碱基
* 共训练了多个不同大小的模型（如GROVER-mini到GROVER-large）

### 3. **训练目标**

* 自监督的 **masked language modeling (MLM)**：

  * 随机mask掉一部分碱基
  * 训练模型去恢复被遮盖的碱基

### 4. **评估任务**

GROVER被评估在多个基因组下游任务上，包括：

* **转录起始位点（TSS）预测**
* **剪接位点预测**
* **调控元件识别（如enhancer、promoter）**
* **疾病突变位点预测（如ClinVar）**

并与其他模型如:

* DeepSEA
* Basenji
* Enformer

进行了对比。



## 🎯 三、研究意义

### 1. **无需人工注释**，全自监督学习：

* 与传统模型不同，GROVER只用DNA序列本身即可训练，无需预先的标注数据。

### 2. **捕捉上下文依赖性**：

* 能有效学习序列中的语法（如TATA box、splice site等motif之间的依赖关系），适合建模**长程调控信息**。

### 3. **泛化能力强**：

* 模型对多个下游任务都表现出良好泛化能力，尤其是在缺乏标签或稀疏任务上。

### 4. **生物医学应用潜力大**：

* 对突变注释、罕见变异功能分析、遗传病研究、精准医学具有重要意义。



## 🔮 四、还能做哪些类似或下游任务？

GROVER作为DNA语言模型的基础，可以扩展到许多任务，示例如下：

| 下游任务             | 描述                                    | 可行性                             |
| ---------------- | ------------------------------------- | ------------------------------- |
| 🧬 **变异功能预测**    | 输入SNP突变前后序列，预测影响                      | ✅ 可用GROVER提取上下文embedding进行分类或回归 |
| 🧬 **非编码区注释**    | 判断非coding区是否为调控元件（enhancer/silencer等） | ✅ 高上下文依赖，GROVER适用               |
| 🧬 **罕见突变致病性评分** | 对临床变异（如ClinVar未注释突变）进行功能打分            | ✅ 可替代/增强CADD、REVEL等评分方法         |
| 🧬 **剪接位点预测**    | 预测新的splice site或预测突变是否破坏剪接            | ✅ 已在文章中初步探索过                    |
| 🧬 **跨物种基因组建模**  | 迁移学习到其他物种（如小鼠、斑马鱼）                    | ✅ 使用人类预训练模型微调即可                 |
| 🧬 **表观遗传预测**    | 如预测DNA甲基化、ATAC-seq开放区间等               | ✅ 需要与实验数据联合训练/微调                |
| 🧬 **泛癌变异注释**    | 与癌症突变数据库整合分析 driver/passenger         | ✅ 可以做功能打分模型或预测热区                |



## ✅ 总结一句话

> GROVER 是一个在全基因组上自监督预训练的 DNA Transformer 模型，能够学习 DNA 序列的“语言规则”，并在多种基因组任务中展现出强大的泛化能力，为基因组注释与变异功能分析提供了新范式。


如你有兴趣进一步使用 GROVER 模型，我可以：

* 帮你找到模型代码和预训练权重；
* 帮你将其应用到你的数据中（如变异注释、区域预测）；
* 或者设计下游任务pipeline。是否需要？


# alphamissense和alphagenome关系、区别

AlphaMissense 和 AlphaGenome 是 Google DeepMind 推出的两款互补性人工智能工具，均聚焦于基因组变异分析，但在**功能定位、覆盖范围和技术目标**上存在显著差异。以下是两者的核心关系及对比分析：



### 🔬 **1. 功能定位：分工明确，覆盖基因组不同区域**
- **AlphaMissense**  
  - **专注领域**：仅分析占基因组 **2% 的蛋白质编码区**，特别是**错义突变**（即单核苷酸变异导致的氨基酸改变）。  
  - **核心任务**：预测错义突变的致病性（如是否导致蛋白质功能异常），将突变分类为“致病”“良性”或“意义不明”，准确率高达 90%。  
  - **应用场景**：疾病遗传机制研究（如囊性纤维化、癌症驱动突变）。

- **AlphaGenome**  
  - **专注领域**：解读占基因组 **98% 的非编码区**，这些区域调控基因表达，包含大量与疾病相关的变异。  
  - **核心任务**：  
    - 预测 DNA 序列的调控活性（如基因起始/终止位点、RNA 剪接模式、染色质开放性等）；  
    - 评估非编码区变异对基因调控的影响。  
  - **应用场景**：罕见病病因挖掘（如脊髓性肌萎缩症）、合成生物学设计、癌症非编码突变机制研究。

> ✅ **关系总结**：两者形成“编码区 vs. 非编码区”的互补组合，覆盖基因组全部区域的分析需求。



### ⚙️ **2. 技术关联：共享基础架构，但目标不同**
- **共同基础**：  
  二者均基于 DeepMind 的底层 AI 技术（如 Transformer 架构），且延续了早期基因组模型 **Enformer** 的部分设计理念。  
- **关键差异**：  
  - **AlphaMissense**：在 **AlphaFold**（蛋白质结构预测模型）上微调，依赖蛋白质序列和结构数据。  
  - **AlphaGenome**：独立训练，输入长达 **100 万个碱基**的 DNA 序列，直接预测多模态调控特性（如染色质空间构象、剪接连接点）。

> ✅ **关系总结**：技术同源但独立优化，分别针对蛋白质功能变异和基因调控网络建模。



### 🧬 **3. 应用协同：联合解决复杂生物学问题**
二者可结合使用，提供**从编码到非编码的完整变异解析**：  
1. **致病机制研究**：  
   - 用 **AlphaGenome** 定位非编码区调控突变（如增强子变异）；  
   - 用 **AlphaMissense** 分析同一基因编码区的错义突变影响。  
   *示例*：在白血病研究中，AlphaGenome 发现非编码突变激活癌基因 *TAL1*，而 AlphaMissense 可验证该基因编码区突变的协同致病性。  

2. **罕见病诊断**：  
   - AlphaMissense 识别蛋白质功能破坏性突变；  
   - AlphaGenome 补充剪接异常或调控失衡导致的疾病（如剪接错误引发的囊性纤维化）。  

3. **药物靶点发现**：  
   - AlphaGenome 筛选调控关键基因的非编码靶点；  
   - AlphaMissense 评估靶点蛋白区域的成药性。



### 📊 **核心差异对比表**
| **维度**         | **AlphaMissense**                     | **AlphaGenome**                          |  
|------------------|--------------------------------------|------------------------------------------|  
| **覆盖区域**     | 基因组 2% 的蛋白质编码区              | 基因组 98% 的非编码区                    |  
| **核心功能**     | 错义突变致病性分类                   | 多模态基因调控预测 & 变异效应评分         |  
| **分辨率**       | 单氨基酸水平                         | 单碱基分辨率 + 百万碱基长上下文          |  
| **关键技术**     | 基于 AlphaFold 微调                  | 类 U-Net 架构 + Transformer              |  
| **典型应用**     | 遗传病蛋白功能异常诊断               | 非编码区致病机制、合成生物学设计         |  



### 💎 **总结**
- **互补性**：AlphaMissense 和 AlphaGenome 是 DeepMind 在基因组分析领域的“双引擎”，前者破解蛋白质功能变异，后者解读基因调控密码，共同覆盖从编码到非编码的完整基因组分析链条。  
- **协同价值**：在复杂疾病研究中，二者联合可揭示“调控变异→蛋白质功能失调”的全链条机制，推动精准医疗和合成生物学发展。  
- **局限提示**：二者均**不适用于临床诊断**，且对复杂性状（如环境互作）的解析能力有限，需结合实验验证。


