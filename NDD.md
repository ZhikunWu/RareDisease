## NDD 项目申请

围绕粤港澳认知与情感神经生物学的生命科学领域，聚焦认知与情感神经生物学基础，脑疾病诊疗新策略，神经生物学与遗传学、光学成像、生物信息学、人工智能等多学科的交叉融合等。 上述项目申请中，做NDD （神经发育障碍）罕见病队列的诊断，是否合适，如何对研究方向和科学问题进行凝练


**合适，而且我认为是比较有潜力的切入点。**但关键不在于把项目表述成“建立一个 NDD 罕见病队列并提高诊断率”，因为这样容易显得偏**临床遗传学/疾病诊断平台建设**，与“认知与情感神经生物学”的主线连接不够紧。

更好的做法是把它凝练成：

> **以神经发育障碍（NDD）为疾病模型，以认知与情感表型为核心表型，以遗传变异为分子基础，建立粤港澳跨区域 NDD 队列，利用人工智能和生物信息学整合“基因型—表型—脑功能表型”，解析认知/情感异常的遗传基础，并建立精准诊断与分型新策略。**

这样，“罕见病诊断”只是**应用出口**，而不是整个项目的科学问题。

---

# 一、为什么 NDD 特别适合这个指南方向？

你给出的指南实际上有四个关键词：

> **认知与情感神经生物学基础**
> **脑疾病诊疗新策略**
> **遗传学**
> **生物信息学 + AI + 光学成像等交叉**

NDD 恰好可以把这几条串起来。

尤其是 NDD 的一个核心特点是：

**相同基因 → 表型高度异质**

以及：

**相似认知/情感表型 → 可以由完全不同的基因导致。**

这就天然形成一个很好的科学问题：

$$
\text{Genotype}
\rightarrow
\text{Neural developmental mechanism}
\rightarrow
\text{Cognitive/affective phenotype}
$$

但实际情况并不是简单的一条线，而是：

$$
\text{Genotype}
+
\text{genetic modifiers}
+
\text{developmental stage}
+
\text{brain function}
\rightarrow
\text{phenotype}
$$

这比单纯的“发现致病基因”更符合**神经生物学**。

事实上，近年的大型 NDD 研究已经明显从单纯寻找致病变异转向 **genotype–phenotype architecture**。例如，2024 年 *Nature* 对 11,573 名 NDD 患者、9,128 名父母和 26,869 名对照研究发现，共同遗传变异约解释 NDD 风险 10% 的方差，而且 rare variant 与 polygenic background 可以共同影响 NDD 表型。([Nature][1])

---

# 二、我建议不要把项目题目定成“粤港澳 NDD 罕见病诊断队列”

这个题目会有一个明显问题：

### 它回答的是：

> **“怎么提高诊断率？”**

而指南更希望看到：

> **“认知和情感异常为什么发生？”**

所以建议把“诊断”放在**第二层**。

---

# 三、最推荐的总体科学框架

我建议你把整个项目设计成：

```text
                  粤港澳 NDD 人群队列
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
     基因组学            深度表型            脑功能表型
        │                  │                  │
     WES/WGS              HPO              MRI/fMRI
     SNV/Indel             │              EEG
     CNV/SV                │              光学成像*
     STR/TE                │                  │
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ↓
                 AI / 生物信息学整合
                           │
             ┌─────────────┴─────────────┐
             ↓                           ↓
       genotype–phenotype            patient
          association               subtyping
             ↓                           ↓
       NDD分子机制                  精准诊断
             │                           │
             └──────────────┬────────────┘
                            ↓
                  认知/情感异常机制
                            ↓
                     干预靶点/策略
```

* 如果你们团队确实有动物/细胞光学成像平台，再把光学成像放进去；**不要为了迎合指南硬塞一个光学成像模块**。

---

# 四、项目最核心的科学问题，我建议凝练成 3 个

这是最关键的地方。

## 科学问题 1：NDD 遗传异质性如何映射到认知与情感表型异质性？

这是整个项目最“神经生物学”的问题。

不要问：

> 哪个基因导致 NDD？

而是问：

> **不同遗传变异如何通过共同或不同的神经发育通路，形成特异性的认知与情感表型谱？**

例如把 phenotype 从：

```text
NDD
├── ID
├── ASD
├── ADHD
├── speech delay
└── behavioral abnormality
```

进一步拆成：

```text
认知
├── general intelligence
├── executive function
├── working memory
├── processing speed
└── language

情感
├── anxiety
├── emotional regulation
├── social cognition
├── reward processing
└── behavioral dysregulation
```

然后建立：

$$
Genotype \leftrightarrow Cognitive/Affective phenotype
$$

这就从“罕见病诊断”升级成了**认知与情感神经生物学研究**。

---

# 五、科学问题 2：为什么相同致病基因会产生不同的认知/情感表型？

这个问题其实非常有深度。

例如：

```text
Gene A pathogenic variant
          │
          ├── Patient 1 → ID
          ├── Patient 2 → ASD
          ├── Patient 3 → anxiety
          └── Patient 4 → severe behavioral phenotype
```

为什么？

可以提出：

$$
Phenotype =
f(G_{rare},G_{common},age,sex,brain\ network,environment)
$$

也就是：

### rare variant

*

### common genetic background

*

### developmental stage

*

### brain functional state

↓

**phenotypic expressivity**

这是目前 NDD 研究非常重要的方向。

2024 年 *Nature Genetics* 的研究显示，携带 NDD 相关 rare damaging variants 的个体，其认知/社会经济表型还受到 common variant polygenic background 的调节，提示 rare variant burden 和 PGS 可以共同影响临床表现。([Nature][2])

这就给你的项目提供了非常好的理论支撑。

---

# 六、科学问题 3：能否利用“基因型 + 深度表型”建立 NDD 精准诊断和分型新方法？

这才是**诊疗新策略**。

也就是说：

> 前两个科学问题负责“发现规律”，第三个负责“转化”。

可以建立：

$$
P(Disease/Gene|Genotype,Phenotype)
$$

也就是：

> **给定一个患者的基因型和认知/情感表型，预测最可能的疾病机制/致病基因。**

这就可以引入：

* HPO
* Exomiser
* LIRICAL
* Phen2Gene
* AI
* multimodal learning
* genotype embedding
* phenotype embedding

最终形成：

```text
患者
 │
 ├── WES/WGS
 │
 ├── HPO
 │
 ├── cognitive phenotype
 │
 ├── affective phenotype
 │
 └── brain phenotype
          ↓
       AI model
          ↓
 ┌────────┼────────┐
 ↓        ↓        ↓
gene     disease  phenotype
ranking  subtype   subtype
          ↓
      精准诊断
```

这个方向已经有非常强的文献依据。

例如 2024 年 *Nature Genetics* 的 TRANSLATE NAMSE 研究纳入 1,577 名接受外显子测序的患者，整体分子诊断率为 32%，而 NDD 患者达到 **42%**；同时通过 HPO + UMAP + 机器学习分析表型空间，并发现了新的 genotype–phenotype associations。([Nature][3])

这篇文章**与你申请方向的契合度非常高**。

---

# 七、其实可以进一步提出一个非常漂亮的核心假说

如果让我帮你凝练，我会写成：

> **NDD 的认知与情感异常并非由单一致病变异决定，而是由罕见致病变异、遗传修饰因素及神经发育表型共同塑造。通过建立粤港澳多中心 NDD 队列，整合基因组、深度认知与情感表型及脑功能表型，构建 genotype–phenotype 多模态关联模型，有望揭示 NDD 临床异质性的遗传与神经生物学基础，并建立面向个体的精准分型与诊断策略。**

这个作为**核心科学假说**会比：

> “通过 AI 提高罕见病诊断率”

高级很多。

---

# 八、项目名称也建议换一种写法

我不太建议：

> **粤港澳神经发育障碍罕见病队列建立及精准诊断研究**

因为“队列建立”味道太重，像平台项目。

### 方案 A：偏基础神经生物学

> **粤港澳神经发育障碍认知与情感表型的遗传基础及多模态解析**

### 方案 B：偏遗传学 + AI

> **基于多模态基因型—表型整合的神经发育障碍认知与情感异常机制及精准诊断研究**

### 方案 C：我最推荐

> **粤港澳神经发育障碍认知与情感异质性的遗传基础及多模态精准解析**

这个题目有三个优点：

1. **NDD** 是疾病模型；
2. **认知与情感异质性** 对应指南；
3. **遗传基础 + 多模态解析** 对应遗传学、生物信息学、AI、脑成像。

“诊断”放到研究目标里，而不是标题里。

---

# 九、研究内容建议拆成 4 个任务

## 任务 1：建立粤港澳 NDD 深度表型—基因组队列

不要只是收：

```text
WES + diagnosis
```

而要收：

### Genotype

* WES/WGS
* SNV/Indel
* CNV
* SV
* repeat
* mtDNA
* mosaic variants
* noncoding variants

### Phenotype

* HPO
* IQ
* language
* executive function
* social cognition
* emotional regulation
* ASD/ADHD-related scales
* behavioral phenotype
* developmental trajectory

### Brain phenotype

根据现有平台：

* MRI
* fMRI
* DTI
* EEG
* eye tracking
* facial/behavioral analysis

这样队列本身就具有“神经生物学”属性。

---

# 十、任务 2：建立“基因型—认知/情感表型”关联图谱

这一部分可以非常漂亮。

建立：

$$
Gene/Variant
\rightarrow
Pathway
\rightarrow
Brain function
\rightarrow
Cognitive/Affective phenotype
$$

例如：

```text
Synaptic genes
      ↓
synaptic function
      ↓
neural circuit
      ↓
social cognition
      ↓
ASD phenotype
```

或者：

```text
Chromatin regulation genes
      ↓
neuronal development
      ↓
cortical maturation
      ↓
executive function
      ↓
ID/behavioral phenotype
```

最终形成：

> **NDD genotype–phenotype map**

---

# 十一、任务 3：利用 AI 进行 NDD 表型分型

这个部分很适合这个指南。

比如：

### 输入

```text
Genome
+
HPO
+
cognitive scores
+
affective scores
+
MRI/EEG
```

### AI

```text
multimodal transformer
/
graph neural network
/
variational autoencoder
/
contrastive learning
```

### 输出

```text
NDD molecular subtype
+
cognitive subtype
+
affective subtype
+
candidate gene
```

尤其可以做：

> **phenotype-first clustering**

例如：

```text
5000 NDD patients

       ↓

HPO + cognitive + affective

       ↓

AI clustering

       ↓

Subtype A
Subtype B
Subtype C
Subtype D

       ↓

GWAS / rare variant burden

       ↓

不同遗传机制
```

这会非常符合“神经生物学 + AI + 遗传学”的交叉定位。

2025 年 *Nature Genetics* 的研究已经采用 generative mixture modeling，在大规模 autism cohort 中从广泛表型数据识别稳定的临床亚型，并进一步发现不同亚型与 common、de novo、inherited genetic variation 以及不同分子通路相关。([Nature][4])

---

# 十二、任务 4：从“诊断”进一步走向“机制和干预”

这是申请书能不能从“临床队列项目”升级为“生命科学项目”的关键。

对于：

```text
高置信度 genotype
+
特异性 phenotype
```

选择代表性病例/基因：

```text
Patient
   ↓
candidate gene
   ↓
neuronal model
   ↓
functional validation
   ↓
synaptic phenotype
   ↓
neural circuit
   ↓
cognitive/affective phenotype
```

如果团队有光学成像能力，可以在这里使用：

* calcium imaging
* optogenetics
* two-photon imaging
* neuronal morphology
* synaptic imaging

这样“光学成像”就不是为了凑指南，而是真正解决科学问题。

---

# 十三、你这个项目特别适合形成一个“闭环”

我建议申请书最终画成：

```text
                    粤港澳 NDD 队列
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
       基因组           认知/情感          脑功能
          │                │                │
       WGS/WES           HPO              MRI/EEG
       SV/CNV         cognitive           fMRI
       SNV/Indel       affective           DTI
          │                │                │
          └────────────────┼────────────────┘
                           ↓
                    AI多模态融合
                           ↓
              genotype–phenotype map
                           ↓
             ┌─────────────┴─────────────┐
             ↓                           ↓
       NDD分子分型                 认知/情感分型
             │                           │
             └─────────────┬─────────────┘
                           ↓
                     精准诊断
                           ↓
                   候选机制/靶点
                           ↓
                神经元/脑回路验证
                           ↓
                     干预策略
```

这就把指南里面的：

**认知与情感神经生物学 → 遗传学 → AI → 生物信息学 → 光学成像 → 脑疾病诊疗**

全部串起来了。

---

# 十四、还有一个非常重要的创新点：粤港澳本身不要只是“地域标签”

这是申请书里很容易被评审问的问题：

> **为什么一定要粤港澳？**

不能回答：

> 因为我们在粤港澳，所以收粤港澳患者。

最好把它变成科学问题：

### ① 多中心异质性

不同医院、不同人群、不同临床亚型。

### ② 多祖源/人群遗传背景

可以研究：

$$
Rare\ variant + ancestry + common\ variant
$$

对 phenotype 的影响。

### ③ 不同临床资源互补

例如：

```text
广州
↓
儿童神经发育/遗传病

香港
↓
genomics / neuroimaging / AI

澳门
↓
clinical cohort / population phenotype
```

如果你们实际合作单位确实具有这样的资源，就可以写。

### ④ 建立粤港澳 NDD reference cohort

最终形成：

> **粤港澳 NDD genotype–phenotype reference atlas**

这本身就具有区域特色和长期价值。

---

# 十五、诊断率不要作为唯一终点

建议把 outcome 设计成三个层级：

### Level 1：诊断

```text
diagnostic yield
```

例如：

> WES → WGS → SV → RNA → methylation

逐级提高。

---

### Level 2：分型

```text
NDD molecular subtype
+
cognitive subtype
+
affective subtype
```

---

### Level 3：机制

```text
gene
 ↓
molecular pathway
 ↓
neural circuit
 ↓
cognition/emotion
```

**这第三层才是生命科学项目最重要的价值。**

---

# 十六、一个我认为很适合申报书的“科学问题”表述

可以直接考虑这样写：

> **科学问题一：神经发育障碍高度遗传异质性如何塑造认知与情感表型的异质性？**

> **科学问题二：罕见致病变异与遗传修饰因素如何共同影响神经发育及认知—情感表型的外显率和临床表现？**

> **科学问题三：如何融合基因组、深度表型及脑功能信息，建立能够解释 NDD 分子机制并提高罕见病诊断效率的多模态模型？**

然后三个问题正好对应：

```text
问题1 → genotype–phenotype map
问题2 → genetic modifiers / penetrance
问题3 → AI + multimodal diagnosis
```

---

# 十七、如果希望再“拔高”一级

我甚至建议把核心概念从：

> **NDD 罕见病诊断**

升级成：

> ### **NDD 的“基因型—脑功能—认知/情感表型”多尺度关联**

即：

$$
\boxed{
Genotype
\rightarrow
Molecular/Cellular
\rightarrow
Neural\ circuit
\rightarrow
Cognition/Emotion
\rightarrow
Clinical\ phenotype
}
$$

这条链非常符合“生命科学领域”。

而**精准诊断**只是利用这条规律的一个应用：

$$
Genotype + Phenotype + Brain\ phenotype
\rightarrow
Precision\ diagnosis
$$

---

## 我的总体判断

**这个方向是合适的，而且 NDD 是非常好的疾病切入点。**

但我会明确建议你避免把项目写成：

> ❌ “建立粤港澳 NDD 罕见病队列，提高诊断率”

而改成：

> **“以 NDD 为疾病模型，解析认知与情感表型异质性的遗传与神经生物学基础，并利用多模态 AI 建立精准分型与诊断新策略。”**

这样项目的逻辑层次就变成：

**队列是基础 → 基因型/表型整合是方法 → 认知/情感神经生物学是核心科学问题 → AI/生物信息学是技术手段 → 精准诊断是转化产出 → 功能/光学成像是机制验证。**

这比单纯做“罕见病队列诊断”与题目所给的**“认知与情感神经生物学 + 脑疾病诊疗 + 遗传学 + AI交叉”**契合度高得多。

另外，2024 年 *Nature Medicine* 建立的 **PhenomAD-NDD** 已经汇总了 **51,227 名 NDD 儿童的表型数据**并标准化为 HPO，说明“**NDD 大规模表型图谱 → genotype–phenotype association → 精准诊断**”本身已经是国际前沿方向；它发现 33 种遗传性 NDD 中约 32% 的富集表型此前并未出现在 OMIM 的临床概要中。([Nature][5]) 这恰好支持你把**“深度表型”而不是单纯诊断标签**作为这个项目的重要创新点。

如果你准备正式写这个申请，我下一步最建议直接把它进一步凝练成 **“项目名称 + 总体假说 + 3个科学问题 + 4个研究内容 + 3个创新点 + 技术路线图”**的一套申请书框架；尤其可以把你前面关注的 **Singleton-WES/rare variant burden + HPO/Exomiser + AI + NDD** 直接嵌进去。

[1]: https://www.nature.com/articles/s41586-024-08217-y?utm_source=chatgpt.com "Examining the role of common variants in rare neurodevelopmental conditions | Nature"
[2]: https://www.nature.com/articles/s41588-024-01710-0?utm_source=chatgpt.com "Genetic modifiers of rare variants in monogenic developmental disorder loci | Nature Genetics"
[3]: https://www.nature.com/articles/s41588-024-01836-1?utm_source=chatgpt.com "Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings | Nature Genetics"
[4]: https://www.nature.com/articles/s41588-025-02224-z?utm_source=chatgpt.com "Decomposition of phenotypic heterogeneity in autism reveals underlying genetic programs | Nature Genetics"
[5]: https://www.nature.com/articles/s41591-024-03005-7?utm_source=chatgpt.com "Prevalence of comorbidities in individuals with neurodevelopmental disorders from the aggregated phenomics data of 51,227 pediatric individuals | Nature Medicine"



