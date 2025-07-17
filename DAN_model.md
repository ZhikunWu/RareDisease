## Annotating the genome at single-nucleotide resolution with DNA foundation models

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


