# Firth’s logistic regression  

在生物信息学和医学统计学（特别是你经常涉及的临床风险预测和变异分析）中，**Firth's Logistic Regression** 是一种非常关键的统计方法。

简单来说，它是对传统 Logistic 回归的一种**改良版**，专门用来处理“小样本”或“数据分布极度不平衡”导致的问题。



### 1. 为什么要用它？（解决“分离”问题）

在处理临床数据或稀有变异（Rare Variants）时，经常会遇到准则分离（Separation）的问题。

* **现象：** 比如你想研究某个基因突变与某种疾病的关系。结果发现，所有携带该突变的人都患病了（或者都没患病）。
* **后果：** 在这种情况下，传统的极大似然估计（MLE）会崩溃，模型无法收敛，算出来的优势比（OR值）会趋向于无穷大，P 值也变得不可靠。
* **Firth 的作用：** 它通过引入一个“惩罚项”（基于 Jeffreys 先验），给似然函数加了一个约束。即使数据点完全分离，它也能给出一个稳定、可靠的估计值。

### 2. Firth 回归的核心特点

* **偏差缩减（Bias Reduction）：** 传统的 Logistic 回归在样本量较小时，往往会高估回归系数。Firth 方法本质上是一种“惩罚似然估计”，能有效消除这种偏倚。
* **解决零单元问题：** 在 $2 \times 2$ 表格中，如果某个格子是 0（比如：突变且未患病的人数为 0），Firth 回归依然可以运行，而不会像普通回归那样报错。
* **计算稳定性：** 它产生的置信区间（Profile Likelihood CI）比普通回归产生的 Wald 置信区间在小样本下更准确。

### 3. 你的科研场景中如何应用？

考虑到你从事的**基因组学（Genomics）**和**生物信息学**研究，这个方法通常出现在以下场景：

* **稀有变异关联分析（Rare Variant Association）：** 当某个突变的频率极低，但在病例组中偶尔出现时，普通回归失效，必须用 Firth。
* **小样本临床队列：** 当你分析 Kawasaki 疾病（川崎病）等特定病例时，如果样本量不足以支撑复杂的模型，Firth 是默认的稳健选择。
* **GWAS 分析：** 很多主流工具（如 **PLINK 2.0**，你经常使用的工具）都内置了 `--firth` 参数。

### 4. 如何在工具中调用？

* **PLINK 2.0:**
```bash
plink2 --vcf input.vcf --pheno pheno.txt --logistic firth-fallback

```


*这里 `firth-fallback` 的意思是：先尝试普通回归，如果遇到“分离”导致无法收敛，自动切换到 Firth 方法。*
* **R 语言:**
使用 `logistf` 包：
```R
library(logistf)
model <- logistf(y ~ x1 + x2, data = mydata)
summary(model)

```



---

### 总结

**Firth's Logistic Regression** 就是 Logistic 回归的“稳健加强版”。它是你处理**小样本、稀有事件或数据分布不均**时的标准统计武器，能防止模型跑出“无穷大”的离谱结果。

你在做 Kawasaki 疾病的变异筛选或者构建预测模型时，如果遇到某些特征的回归结果非常奇怪（比如标准误大得惊人），换成 Firth 方法通常就能解决问题。

你目前是在处理 PLINK 的关联分析输出，还是在用 R 跑临床预测模型的评估？

```

```

---

# 常见的关联分析的工具有哪些

在全基因组关联研究（GWAS）中，有许多常用的工具用于进行关联分析。这些工具各有特点，适用于不同的分析需求，包括大规模的遗传数据处理、亲缘关系调整、群体结构校正、以及与表型的关联测试。以下是一些常见的关联分析工具及其特点：

### 1. **PLINK**

* **简介**：PLINK 是最常用的 GWAS 工具之一，广泛应用于基因型数据的质量控制、表型数据的关联分析、群体结构控制、关联分析结果的后处理等。
* **特点**：

  * 支持广泛的输入输出格式，包括 `.ped`, `.map`, `.bed`, `.fam`, `.bim`, `.pgen` 等。
  * 可以进行多种类型的关联分析，包括单标记分析、亚组分析、连锁不平衡（LD）分析等。
  * 提供了 PCA（主成分分析）、群体结构校正等功能。
* **常用命令**：

  ```bash
  plink2 --bfile my_data --assoc --out gwas_results
  plink2 --bfile my_data --pca 10 --out pca_results
  ```

### 2. **PLINK2**

* **简介**：PLINK2 是 PLINK 的升级版本，提供了更多功能和更高效的处理能力，尤其是在大规模数据的处理和复杂分析方面。
* **特点**：

  * 支持更高效的基因型数据存储（如 `pgen` 格式）和计算。
  * 增强了混合线性模型（LMM）的支持，适用于大规模的 GWAS 分析。
  * 支持对多重测试的校正和全基因组范围的高效计算。
* **常用命令**：

  ```bash
  plink2 --pgen dataset.pgen --pvar dataset.pvar --psam dataset.psam --assoc --out gwas_results
  ```

### 3. **GCTA (Genome-wide Complex Trait Analysis)**

* **简介**：GCTA 主要用于混合线性模型（LMM）分析，它可以有效控制亲缘关系和群体结构的影响，适用于对表型进行大规模的基因组关联分析。
* **特点**：

  * 支持使用亲缘关系矩阵（GRM）进行基因型-表型的关联分析。
  * 提供了基于遗传相关性的复杂性状分析，适用于表型关联和遗传相关分析。
  * 支持计算遗传评分（genetic risk scores）和群体结构校正。
* **常用命令**：

  ```bash
  gcta64 --bfile my_data --pheno phenotype.txt --grm kinship_matrix --reml --out gwas_results
  ```

### 4. **BOLT-LMM**

* **简介**：BOLT-LMM 是基于混合线性模型（LMM）的高效 GWAS 工具，能够有效处理大规模的遗传数据，并且支持亲缘关系和群体结构的校正。
* **特点**：

  * 适用于超大规模的 GWAS 数据（上百万个样本）。
  * 在控制亲缘关系和群体结构的同时，具有较高的计算效率。
  * 支持多种分析模式，包括单标记分析、加性模型和回归模型。
* **常用命令**：

  ```bash
  bolt --bfile my_data --pheno phenotype.txt --covar covariate_file.txt --lmm --out gwas_results
  ```

### 5. **fastGWA**

* **简介**：fastGWA 是一个高效的 GWAS 工具，设计用于处理超大规模的数据集（例如，百万级样本）。它使用混合线性模型（LMM）来控制亲缘关系。
* **特点**：

  * 高效处理大规模基因型数据，适用于大规模人群数据（如 10 万到 100 万个样本）。
  * 支持精确的控制群体结构、亲缘关系和样本间的遗传相关性。
* **常用命令**：

  ```bash
  fastGWA --bfile my_data --pheno phenotype.txt --covar covar.txt --out gwas_results
  ```

### 6. **SAIGE (Scalable and Accurate Implementation of GEneralized mixed model)**

* **简介**：SAIGE 是一种专为大规模 GWAS 数据设计的混合模型工具，尤其适用于稀有变异的分析。
* **特点**：

  * 能处理稀有变异，适用于大规模遗传数据集。
  * 采用混合效应模型（Mixed-Effects Model）来进行关联分析，能够提高稀有变异的统计效能。
  * 支持在计算时控制群体结构和亲缘关系。
* **常用命令**：

  ```bash
  saige --bfile my_data --pheno phenotype.txt --covar covar.txt --out gwas_results
  ```

### 7. **EIGENSTRAT**

* **简介**：EIGENSTRAT 是一种基于主成分分析（PCA）来校正群体结构的方法，常用于控制遗传结构的群体（例如，亚群体）对GWAS结果的影响。
* **特点**：

  * 通过 PCA 来提取主要的群体结构信息，适用于大规模的数据集。
  * 通过将这些主成分作为协变量纳入回归模型，调整群体结构。
* **常用命令**：

  ```bash
  eigenstrat --bfile my_data --pca 10 --out pca_results
  ```

### 8. **HaploReg**

* **简介**：HaploReg 是一个用于基因组注释和变异功能分析的工具，主要用于 GWAS 后的功能注释。
* **特点**：

  * 提供了对 SNP 变异的详细功能注释，尤其是对调控区域和基因表达的影响。
  * 支持不同人群的分析，可以帮助解释 GWAS 结果中的标记。

### 9. **Meta-Analysis Tools (如 METAL)**

* **简介**：METAL 是用于 GWAS 元分析的工具，可以将多个研究的 GWAS 结果进行整合，以获得更高的统计效能。
* **特点**：

  * 适用于跨研究的 GWAS 结果汇总。
  * 支持多种分析方法，包括固定效应模型和随机效应模型。
* **常用命令**：

  ```bash
  metal gwas_results1.txt gwas_results2.txt --out meta_analysis_results
  ```

### 10. **REGENIE**

* **简介**：REGENIE 是一个适用于大规模 GWAS 的高效工具，采用分步回归方法进行基因型-表型关联分析，并通过线性混合模型进行调整。
* **特点**：

  * 高效处理大规模数据，特别适用于复杂的表型和稀有变异的分析。
  * 使用分步回归方法来控制基因型和表型之间的复杂关系。
* **常用命令**：

  ```bash
  regenie --bfile my_data --pheno phenotype.txt --covar covar.txt --out gwas_results
  ```

### 总结

选择合适的 GWAS 工具取决于数据的规模、分析的复杂性、以及你对群体结构或亲缘关系控制的需求。常用的工具如 **PLINK**、**GCTA**、**BOLT-LMM**、**fastGWA** 和 **SAIGE** 提供了灵活且高效的方案，适用于大规模数据集、亲缘关系校正、群体结构控制以及混合模型分析。




---

# BOLT-LMM的使用

**BOLT-LMM**（**BOLT-LMM**: **B**ayesian **L**inear **M**ixed **M**odel）是一个用于进行大规模全基因组关联研究（GWAS）分析的工具，特别适合处理大规模数据集（如百万级别的样本）并能有效地控制亲缘关系和群体结构。它通过使用混合线性模型（LMM）来处理遗传数据，控制了样本间的遗传相关性，从而减少假阳性结果。

以下是使用 **BOLT-LMM** 进行 GWAS 分析的详细步骤。

### 安装 BOLT-LMM

首先，你需要下载并安装 **BOLT-LMM**。可以从 [BOLT-LMM 官方网站](http://alkesgroup.broadinstitute.org/BOLT-LMM/) 下载预编译的二进制文件，或根据网站上的说明进行安装。

### 1. **准备输入文件**

BOLT-LMM 需要以下几个文件作为输入：

* **基因型文件**（`.bgen` 格式）：这个文件包含了基因型数据，适用于大规模的遗传数据。
* **表型文件**（`.phen` 格式）：这个文件包含了与样本相关的表型数据。
* **样本的协变量文件**（`.covar` 格式）：用于提供样本的协变量（如年龄、性别等）信息，通常是控制群体结构和其他潜在混杂因素。
* **亲缘关系矩阵文件（GRM）**：用于控制亲缘关系。GRM 通常是通过 PLINK 生成的。

### 2. **基因型数据准备**

如果你的基因型数据已经是 `.bgen` 格式，BOLT-LMM 可以直接使用。如果数据是 `.ped` 或 `.vcf` 格式，可以使用 **BGEN** 工具将其转换为 `.bgen` 格式。

如果数据是 **PLINK** 格式（`.ped`, `.map`, `.bim`, `.fam`），你可以用以下命令将其转换为 `.bgen` 格式：

```bash
plink2 --bfile my_data --make-bgen --out my_data_bgen
```

### 3. **计算亲缘关系矩阵（GRM）**

BOLT-LMM 需要一个亲缘关系矩阵（GRM）文件，用于控制亲缘关系。你可以通过 **PLINK** 来计算 GRM 文件。假设你已经有了 PLINK 格式的数据（`.ped`, `.map`），你可以运行：

```bash
plink2 --bfile my_data --make-grm --out grm_matrix
```

这将生成亲缘关系矩阵 `grm_matrix.grm.bin` 和 `grm_matrix.grm.id` 文件。

### 4. **运行 BOLT-LMM 分析**

**BOLT-LMM** 使用混合线性模型进行分析，通常的命令格式如下：

```bash
bolt --bgen my_data.bgen --pheno phenotype.txt --covar covariate_file.txt --grm grm_matrix.grm.bin --lmm --out gwas_results
```

**参数说明**：

* `--bgen my_data.bgen`：指定基因型数据文件（`.bgen` 格式）。
* `--pheno phenotype.txt`：指定表型文件，通常是包含样本ID和表型值的文件。
* `--covar covariate_file.txt`：指定包含协变量（如性别、年龄、群体结构主成分等）的文件。
* `--grm grm_matrix.grm.bin`：指定亲缘关系矩阵文件。
* `--lmm`：指定使用线性混合模型进行分析。
* `--out gwas_results`：指定输出文件前缀，结果将保存在 `gwas_results` 文件夹中。

### 5. **分析结果**

BOLT-LMM 运行结束后，将输出以下结果文件：

* **gwas_results.profile**：包含每个样本的遗传评分（genetic risk score）信息。
* **gwas_results.stats**：包含每个 SNP 的关联统计信息（如 P 值、效应值、标准误差等）。
* **gwas_results.hsq**：包含每个表型的遗传方差（heritability）估计。
* **gwas_results.log**：包含运行的日志信息。

### 6. **解释和后续分析**

分析结果文件中，主要关注的是 **gwas_results.stats** 文件，这个文件包含了与每个 SNP 的 P 值、效应大小等信息。你可以用这些结果进行后续的 GWAS 结果可视化、路径富集分析等。

例如，你可以使用 **R** 来加载和可视化分析结果：

```r
# 加载结果文件
gwas_results <- read.table("gwas_results.stats", header = TRUE)

# 绘制 Manhattan plot
library(qqman)
manhattan(gwas_results)
```

### 7. **混合线性模型参数调节**

BOLT-LMM 提供了多个参数来优化模型，包括：

* `--lmm`：指定混合线性模型。
* `--linear`：指定线性回归模型。
* `--qcovar`：用于指定量化协变量（例如年龄、体重等）。
* `--covar`：用于指定类别协变量（例如性别、种族等）。
* `--numThreads`：设置使用的 CPU 线程数，通常在大规模数据分析中，增加线程数能显著提高计算速度。

### 8. **其他高级选项**

* **`--exclude`**：排除某些标记或样本进行分析。
* **`--minMAF`**：设置最小等位基因频率，排除低频变异。
* **`--noPhenoScan`**：禁用表型扫描，只执行基因型相关分析。

### 总结

**BOLT-LMM** 是一个高效且灵活的 GWAS 工具，特别适用于大规模数据集。通过混合线性模型，它能够有效地控制亲缘关系和群体结构的影响，从而降低假阳性并提高统计效能。使用时，确保准备好基因型数据、表型数据、协变量文件和亲缘关系矩阵，并合理选择模型参数以适应数据特征。
---






