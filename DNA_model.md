# GENA-LM 
这篇文章（[GENA-LM: a family of open-source foundational models for long DNA sequences](https://academic.oup.com/nar/article/53/2/gkae1310/7954523?login=false)）中发布的 **GENA-LM** 是一系列基于 Transformer 的 DNA 语言模型。

以下是该模型的核心参数和架构细节：

### 1. 模型架构 (Architecture)

GENA-LM 属于 **Encoder-only (仅编码器)** 的 Transformer 模型，类似于 NLP 领域的 **BERT**。为了处理不同长度的 DNA 序列，它采用了两种变体：

* **标准架构 (BERT-based)**：用于处理较短序列（约 4.5kb）。采用标准的 Transformer Encoder 架构，拥有  的注意力复杂度。
* **长序列架构 (BigBird-based)**：用于处理长序列（约 36kb）。采用了 **BigBird** 架构的 **稀疏注意力机制 (Sparse Attention)**，将计算复杂度降低到线性 ，从而能够处理极长的上下文（36,000 bp）。
* **扩展机制**：论文还提到结合 **RMT (Recurrent Memory Transformer)** 机制，可以将上下文进一步扩展到 **100kb+** 级别。

### 2. Tokenization (分词策略)

与之前的 DNABERT 等模型使用 k-mer 不同，GENA-LM 使用了 **BPE (Byte Pair Encoding)** 算法。

* **策略**：它通过统计学习 DNA 序列中出现频率最高的片段（subsequences）作为 token。
* **词表大小**：约 **32,000 (32k)** 个 token。
* **平均 Token 长度**：约 9 bp。这意味着一个 token 可以代表约 9 个碱基，相比 k-mer 策略大大增加了模型能“看到”的实际序列长度（Context Window）。

### 3. 参数量 (Parameters)

GENA-LM 发布了不同规模的模型，主要参数量如下：

* **Base 模型**：约 **1.1 亿 (110M)** 参数（类似于 BERT-base）。
* 例如：`gena-lm-bert-base` (110M), `gena-lm-bigbird-base-t2t` (110M)。


* **Large 模型**：约 **3.5 亿 (336M - 350M)** 参数（类似于 BERT-large）。
* 例如：`gena-lm-bert-large-t2t` (336M)。



### 4. 训练物种 (Species)

GENA-LM 的训练策略涵盖了单物种和多物种：

* **核心训练数据**：主要是 **人类 (Human)** 基因组，特别是使用了最新的 **T2T-CHM13 (Telomere-to-Telomere)** 高质量组装版本。这使其在人类基因组任务上表现尤为出色。
* **多物种扩展**：除了人类模型，作者还发布了多物种版本（Multispecies models），训练数据涵盖了脊椎动物、植物等多种生物。
* **特定物种模型**：论文中还特别提到了针对果蝇 (*Drosophila*)、酵母 (*Yeast*) 和拟南芥 (*Arabidopsis*) 的特定模型。

### 总结表

| 特性 | GENA-LM 详情 |
| --- | --- |
| **基础架构** | **Transformer Encoder** (BERT & BigBird) |
| **注意力机制** | **Sparse Attention (稀疏注意力)** (用于长序列) |
| **Tokenization** | **BPE** (Byte Pair Encoding), Vocab ~32k |
| **参数量** | **110M (Base)** / **336M (Large)** |
| **输入长度** | **4.5kb** (BERT) ~ **36kb** (BigBird) |
| **核心物种** | **Human (T2T-CHM13)** + Multispecies |

---

# Mistral-DNA

基于 Raphael Mourad 开发的 **Mistral-DNA** 项目（GitHub: [raphaelmourad/Mistral-DNA](https://github.com/raphaelmourad/Mistral-DNA)），以下是该模型的详细架构与参数配置：

### 1. 模型架构 (Architecture)

Mistral-DNA 是基于 **Mistral AI** 的 **Mixtral-8x7B** 架构进行修改和精简而来的。它属于 **Transformer Decoder-only**（仅解码器）架构，并保留了 Mistral 系列的核心高效技术：

* **稀疏混合专家 (Sparse Mixture-of-Experts, SMoE)**：这是其最核心的特征（尤其是 138M 版本）。模型包含多个“专家”网络，推理时只激活部分参数，从而在增加模型容量的同时保持高效的推理速度。
* **分组查询注意力 (Grouped-Query Attention, GQA)**：用于加速推理并降低显存占用。
* **滑动窗口注意力 (Sliding-Window Attention)**：用于更高效地处理长序列。
* **旋转位置编码 (RoPE)**：用于捕捉序列位置信息。

### 2. Tokenization (分词策略)

Mistral-DNA 使用的是 **Byte-fallback BPE (Byte-Pair Encoding)** 分词器。

* 虽然有些早期描述或教程提到 k-mer（如 6-mer），但官方 Hugging Face 模型卡明确指出它使用 BPE。
* BPE 能够根据数据自动学习高频出现的 DNA 序列片段（子词），比固定的 k-mer 更加灵活。
* **词表大小**：通常配置为约 **4,096**（这与 6-mer 的  数量级一致，可能是为了兼容或初始化便利）。

### 3. 参数量 (Parameters)

该项目提供了多个不同大小的版本，以适应不同的计算资源（如单张 RTX 3090）：

* **Mistral-DNA-v1-17M-hg38**：约 **1700 万 (16.8M)** 参数。这是一个高度精简的稠密版本（Dense），减少了层数和隐藏层维度。
* **Mistral-DNA-v1-138M**系列：约 **1.38 亿 (138.5M)** 参数。
* 这是一个 **SMoE** 模型。
* 具体计算：基础参数约 **17.31M** × **8 个专家** ≈ 138M。


* **Mistral-DNA-v1-1.6B**：约 **16 亿 (1.6B)** 参数。这是较大的版本，通常在一些基准测试论文（如 DART-Eval）中会出现。

### 4. 训练物种 (Species)

模型根据不同的版本使用了不同的基因组数据进行预训练：

* **人类 (Human)**：
* 用于 `Mistral-DNA-v1-17M-hg38` 和 `Mistral-DNA-v1-138M-hg38`。
* 使用人类参考基因组 **hg38** (GRCh38)。


* **病毒 (Virus)**：
* 用于 `Mistral-DNA-v1-138M-virus`。
* 使用 NCBI RefSeq 的病毒基因组数据库。


* **细菌 (Bacteria)**：
* 用于 `Mistral-DNA-v1-138M-bacteria`。
* 使用了约 700 个细菌基因组。


* **长非编码 RNA (lncRNA)**：
* 用于 `Mistral-DNA-v1-138M-lncRNA`。



**总结：**
Mistral-DNA 是一个将先进的 **SMoE (Mixtral)** 架构应用于基因组学的尝试，主要特点是**轻量级**（参数量主要在 17M-138M 级别），使用 **BPE** 分词，并针对**人类、病毒、细菌**等特定物种分别发布了预训练模型。

---

# TrinityDNA

**TrinityDNA** (arXiv:2507.19229) 是一个最新的“生物启发式”DNA 基础模型。

根据该论文及相关引用的详细信息，以下是该模型的核心配置解答：

### 1. 模型架构 (Architecture)

TrinityDNA 采用了一种**生物启发式（Bio-Inspired）的混合架构**，旨在克服传统 Transformer 在处理超长 DNA 序列时的局限性。其架构主要包含以下三个核心组件：

* **Groove Fusion (大沟/小沟融合)**：这是该模型最独特的创新点。不同于以往模型只看 ATCG 序列，TrinityDNA 引入了 DNA 双螺旋结构的物理特征（如大沟和小沟的几何信息），将这些**结构信息**与序列信息融合。
* **Gated Reverse Complement (GRC, 门控反向互补)**：为了处理 DNA 的双链对称性（正义链和反义链代表同一生物学实体），模型内置了 GRC 模块，使其具有**反向互补等变性**。
* **Multi-scale Attention (多尺度注意力)**：用于捕捉基因组中不同长度范围（从短基序到长距离染色质互作）的依赖关系。

### 2. Tokenization (词元策略)

TrinityDNA 摒弃了传统的简单的 k-mer 切分，采用了一种**结构感知的 Tokenization** 策略。

* 它不只是将碱基字符（A, T, C, G）作为输入，而是结合了**Groove Fusion** 模块提取的结构特征。
* 这种策略使其能比单纯的单核苷酸（Single Nucleotide）或 k-mer 模型更敏感地捕捉到转录因子结合位点（通常与 DNA 形状有关）和变异对结构的影响。

### 3. 参数量 (Parameters)

该模型（主要版本）的参数量约为 **10 亿 (1 Billion, 1B)**。

* 在与其他模型的对比中（如 Evo-2），TrinityDNA 及其衍生版本（如 TrinityDNA-LabFauna）通常与 **1B** 参数级别的模型进行直接对标，并在此量级下实现了优于同类模型的性能。

### 4. 训练物种 (Species)

TrinityDNA 采用了**进化训练策略 (Evolutionary Training Strategy)**，训练数据涵盖了从原核生物到真核生物的跨度。

* **具体数量**：核心衍生版本（TrinityDNA-LabFauna）明确提到使用了 **27 个物种** 的基因组进行训练。
* **物种选择**：这 27 个物种主要是遗传学和疾病研究中广泛使用的模式生物（覆盖了哺乳动物、真菌等），而非盲目追求物种数量。训练过程采用了从简单的原核基因组逐步过渡到复杂的真核基因组的“课程学习”方式。



**总结表格**

| 特性 | TrinityDNA 配置 |
| --- | --- |
| **核心架构** | **Bio-Inspired Hybrid** (Groove Fusion + GRC + Multi-scale Attention) |
| **Token 策略** | **结构感知 (Structure-aware)** 融合特征 (非传统 k-mer) |
| **参数量** | **~1 Billion (1B)** |
| **训练物种** | **27 个模式物种** (采用 Pro  Eu 进化策略) |

它引入的“Groove Fusion”通过模拟DNA物理结构来提升模型理解力，这在基因组AI领域是一个非常新颖的方向。

[TrinityDNA: A Bio-Inspired Foundational Model for Efficient Long-Sequence DNA Modeling](https://www.google.com/search?q=https://www.youtube.com/watch%3Fv%3DFj2t4k4eJ-w)

This video provides a deep dive into the TrinityDNA paper, explaining its unique Groove Fusion and Gated Reverse Complement mechanisms in detail.


