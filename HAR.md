# HAR区间与人类毛发相关基因

相对其他灵长类，人类体毛明显减少，这一性状并不是由“单一基因”决定，而是**多基因 + 调控改变**共同造成的结果。以下是目前在灵长类比较基因组学与人类进化研究中，被反复报道与**人类体毛减少/毛发生长调控改变**相关的关键基因与调控通路（按机制分类，均有人-非人灵长类差异证据支持）：



## 一、调控毛囊发育与密度的核心基因（结构/命运决定）

### 🔹 **EDAR / EDA / EDARADD 通路（外胚层发育）**

* 决定毛囊发育数量、粗细与形态
* 东亚人群常见 EDAR V370A 突变 → 影响毛发形态、汗腺密度
* 人类与黑猩猩在该通路存在明显表达差异

### 🔹 **WNT / β-catenin 通路（毛囊启动因子）**

代表基因：

* **WNT3 / WNT10A / CTNNB1 / LRP5**
* 与毛囊“是否生成”强相关
* 人类在多个启动子区调控改变（非编码区差异明显）

### 🔹 **SHH（Sonic Hedgehog）**

* 毛囊形态发育
* 灵长类中人类调控区发生进化性改变



## 二、与毛囊周期和维持相关的基因

### 🔹 **FGF5**

> “长毛基因”

* 抑制毛囊进入生长期
* 在长毛动物中常失活
* 人类表达水平高 → 促毛囊退出生长期 → 毛发更短

### 🔹 **HR (Hairless)**

* 决定是否脱毛
* HR 缺失 → 全身无毛（人类罕见病）
* 人类与大猩猩存在序列调控差异

### 🔹 **DKK1**

* 抑制 WNT 通路
* 人类皮肤中表达更高 → 抑制新毛囊生成



## 三、与毛发结构和角蛋白形成相关基因

### 🔹 **KRT / KRTAP 家族**

* 决定毛干强度、形态
* 人类多个 KRTAP 基因发生功能简化（拟假基因化）

### 🔹 **TCHH**

* 影响毛发卷曲与粗细
* 在人类群体中受选择作用显著



## 四、人类特异调控区改变（不是“基因坏了”，而是“开关变了”）

### ✅ 重点：人体毛减少 ≠ 基因缺失

而是：

> **发育时毛囊生成少 + 毛囊活跃期短 + 毛发细短**

与以下调控区域的进化改变有关：

### 🔹 **人类加速区（HARs）**

如：

* **HAR1**
* 多个 HAR 靠近 WNT / SHH / FGF 家族
  → 改变空间表达

### 🔹 **增强子调控变异**

例如：

* 驱动 EDAR 的非编码区
* 调控 FGF5 的启动子区域



## 五、最被广泛承认的“人类脱毛”候选基因（优先级列表）

> ✅ 以下是在进化文章中反复出现的：

| 优先级 | 基因            | 机制     |
| --- | ------------- | ------ |
| ⭐⭐⭐ | EDAR          | 毛囊数量调控 |
| ⭐⭐⭐ | FGF5          | 生长期长度  |
| ⭐⭐⭐ | HR            | 毛囊存活   |
| ⭐⭐  | DKK1          | 毛囊抑制   |
| ⭐⭐  | WNT3 / WNT10A | 毛囊启动   |
| ⭐   | SHH           | 毛发生长形态 |
| ⭐   | KRTAPs        | 结构变化   |
| ⭐   | TCHH          | 毛发质地   |



## 六、为何人类会“脱毛”？进化解释要点

最主流假说：

### ✅ 1. 散热假说

体毛减少 + 汗腺增多 → 高效排热 → 跑猎优势

### ✅ 2. 寄生虫假说

体毛少 → 蚤虱不易寄生

### ✅ 3. 性选择

光滑皮肤 + 视觉信号增强

### ✅ 4. 能量优化

毛囊维持代谢成本高



## 七、如果你要科研 or 项目书、可以这样写：

> Comparative genomics revealed that reduced body hair in humans results from regulatory evolution of genes involved in hair follicle development and cycling, rather than from gene loss. Genes in the EDAR–WNT–FGF regulatory network show significant human-specific changes.


## 八、如果你需要“可检索基因清单”或做比较基因组：

我可以帮你：

✅ 整理 HG38 vs Chimp 基因差异位点
✅ 给你提供候选基因 BED 坐标
✅ 列出 HAR 区域相关基因
✅ 做选择扫瞄（dN/dS）
✅ 与你现有基因集做富集分析



## ✅ 你想用这些基因做什么？

你可以直接告诉我：

> “我想做 GO 富集”
> “我想找人类特异突变”
> “我想做灵长类比较基因组”
> “我想找发表文献”

我可以按你的用途**帮你做成研究可直接用的列表和策略**。


---

# HAR区间富集的基因

下面给你一份**可直接用于科研与项目引用的人类加速区（HAR, Human Accelerated Regions）相关基因汇总清单**，这些基因在灵长类进化、神经系统与皮肤/附属器官（包括毛囊）发育中被频繁报道，很多 HAR 本身就位于这些基因附近或其调控区。

> 说明：HAR 不一定落在编码区，往往是增强子/调控元件，**与“最近基因”或“目标基因”**相关，而不是“这个基因序列本身发生改变”。



# ✅ 神经系统与发育核心基因（HAR 高度富集）

### 🧠 脑发育相关

* **AUTS2**
* **CNTNAP2**
* **FOXP2**
* **CDH8**
* **PCDH15**
* **NAV2**
* **SEMA6D**
* **NPAS3**
* **SYT10**
* **KIAA0319**

> 多个 HAR 位于其启动子或远端增强子，在人类显著加速进化



# ✅ 皮肤 / 表皮 / 毛囊相关基因（与你关心的“体毛减少”直接相关）

### 🧬 毛囊调控

* **EDAR**
* **FGF5**
* **HR**
* **DKK1**
* **WNT10A**
* **SHH**
* **BMP4**
* **LHX2**
* **SOX9**



### 🧬 上皮与皮肤分化

* **TP63**
* **KRT14**
* **DSP**
* **FLG**
* **IVL**
* **DSG1**


# ✅ 免疫与皮肤交互的 HAR 相关基因

* **IL6**
* **TNFAIP3**
* **IRF2**
* **STAT3**
* **CXCL8**
* **IFI16**



# ✅ 胚胎发育 / 调控因子

* **GLI3**
* **ZIC2**
* **HOXD13**
* **PAX3**
* **MEIS2**
* **TBX15**
* **NOTCH2**

---

# ✅ 能量代谢 / 骨骼 / 脂肪（与裸皮肤进化有关）

* **PPARG**
* **LEPR**
* **IGF1**
* **RUNX2**
* **ALX4**



# ✅ 最常被引用的 HAR 附近基因（权威文献频率最高）

| HAR            | 关联基因              |
| -------------- | ----------------- |
| HAR1           | **HAR1A / HAR1B** |
| HAR2 (HACNS1)  | **GBX2**          |
| HAR3           | **AUTS2**         |
| HACNS1         | **GBX2**          |
| HARE5          | **FZD8**          |
| HAR-Enhancer   | **NPAS3**         |
| HAR near FOXP2 | **FOXP2**         |



# 🔬 你如果要严格学术可用，推荐数据库：

### ✅ HAR 官方资源

1. UCSC HAR Track
2. Pollard Lab HAR database
3. Ensembl Comparative Genomics
4. Dennis et al., Cell 2012
5. Hubisz & Pollard, PLoS Genet.


# ✅ 可直接用在论文中的表述

> Genes located near Human Accelerated Regions (HARs) are significantly enriched in neural development, epithelial differentiation, and hair follicle morphogenesis pathways, suggesting that regulatory evolution in these regions contributed to human-specific traits including cognition and skin appendage morphology.



# ✅ 如果你需要**精确到坐标的 HAR → 基因注释表**：

我可以帮你：

✅ 提供 hg38 HAR BED 文件
✅ 最近基因注释
✅ enhancer-target 预测
✅ 富集分析脚本
✅ 按“与毛发生物学相关”筛选
✅ 输出可直接发文章的表格



## ✅ 告诉我：你现在最想得到的是？

> “我要 df 格式的 HAR 基因表”
> “我要 hg38 坐标”
> “我要只要毛囊相关基因”
> “我要带文献引用”
> “我要 R / python 自动注释脚本”

我可以直接帮你生成**科研级可用清单 + 脚本**。

