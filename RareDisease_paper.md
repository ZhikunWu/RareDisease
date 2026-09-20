# 近四年在四大医学期刊的子刊上发表的关于罕见病的论文有哪些（尽可能多列出）

可以。这里我把“**四大医学期刊的子刊**”按通常科研语境理解为四大医学出版体系：

* **NEJM Group**：NEJM Evidence、NEJM AI 等
* **The Lancet 系列**：Lancet Neurology、Lancet Child & Adolescent Health、Lancet Haematology、Lancet Gastroenterology & Hepatology、Lancet Respiratory Medicine、Lancet Diabetes & Endocrinology 等
* **JAMA Network**：JAMA Neurology、JAMA Pediatrics、JAMA Psychiatry、JAMA Dermatology、JAMA Network Open 等
* **BMJ Group**：Journal of Medical Genetics、BMJ Neurology Open、BMJ Paediatrics Open 等

时间范围按 **2022–2026年9月**。由于“罕见病”可以包括单基因病、NDD、罕见神经病、遗传性眼病、罕见血液病、罕见代谢病、罕见肺病以及基因治疗等，所以我下面重点放在**与你的 WES/WGS、NDD、long-read、SV、AI诊断和罕见病队列最相关**的文章，同时补充一些重要疾病治疗论文。

---

# 一、首先推荐：与你的 NDD/WES/WGS/Long-read 最相关

这一部分是我认为你最应该重点阅读的。

| 年份       | 期刊                              | 论文                                                                                                                                         | 疾病/方向        | 数据/方法                     |
| -------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | ------------------------- |
| **2025** | **JAMA Pediatrics**             | **Clinical Long-Read Sequencing Test for Genetic Disease Diagnosis**                                                                       | 罕见遗传病        | 235例儿童 HiFi long-read     |
| **2024** | **JAMA Neurology**              | **Genome Sequencing After Exome Sequencing in Pediatric Epilepsy**                                                                         | 儿童癫痫/NDD     | 125例，ES后GS                |
| **2023** | **JAMA Neurology**              | **Exome Sequencing and the Identification of New Genes and Shared Mechanisms in Polymicrogyria**                                           | PMG/NDD      | 275个家系                    |
| **2022** | **JAMA Neurology**              | **Molecular Diagnostic Yield of Exome Sequencing and Chromosomal Microarray in Cerebral Palsy**                                            | CP/NDD       | ES/CMA meta-analysis      |
| **2024** | **JAMA Pediatrics**             | **Rare De Novo and Inherited Genes in Familial and Nonfamilial Pediatric ADHD**                                                            | ADHD/NDD     | WES + rare variants       |
| **2023** | **JAMA Pediatrics**             | **Molecular Diagnostic Yield of Exome Sequencing and Chromosomal Microarray in Short Stature**                                             | 遗传性矮小        | ES/CMA meta-analysis      |
| **2023** | **Lancet Neurology**            | **Evaluation of the feasibility, diagnostic yield, and clinical utility of rapid genome sequencing in infantile epilepsy (Gene-STEPS)**    | 婴儿癫痫/NDD     | rapid WGS，多中心             |
| **2022** | **Lancet Neurology**            | **Whole genome sequencing for the diagnosis of neurological repeat expansion disorders in the UK**                                         | 重复扩增疾病       | WGS                       |
| **2023** | **Journal of Medical Genetics** | **Diagnostic genome sequencing improves diagnostic yield: a prospective single-centre study in 1000 patients with inherited eye diseases** | 遗传性眼病        | **1000例WGS + RNA-seq**    |
| **2025** | **Journal of Medical Genetics** | **Clinical utility of genome sequencing in autism: illustrative examples from a genomic research study**                                   | ASD/NDD      | 202个WGS家庭                 |
| **2024** | **Journal of Medical Genetics** | **WDR45 variants as a major cause for a clinically variable intellectual disability syndrome from early infancy in females**               | ID/NDD       | WGS + XCI                 |
| **2024** | **Journal of Medical Genetics** | **Non-coding CGG repeat expansion in LOC642361/NUTM2B-AS1...**                                                                             | OPDM         | repeat expansion/WGS/LRS  |
| **2024** | **Journal of Medical Genetics** | **Novel variants and genotype-phenotype correlation in a multicentre cohort of GNE myopathy in China**                                     | GNE myopathy | 113例 + WGS + Nanopore LRS |

其中 JAMA Pediatrics 2025 的 long-read 文章尤其值得你看：研究比较了 **235例儿童临床HiFi长读长测序**和513例年龄、表型匹配的标准检测病例；87个LRS诊断中，16个（18.3%）受益于长读长整合能力，包括SV、repeat expansion、甲基化和phasing等。([JAMA Network][1])

JAMA Neurology 2024 的 ES→GS 研究则非常贴合你目前的“**WES未诊断病例进一步WGS**”思路：125名ES未诊断儿童癫痫患者中，GS找到9个诊断/可能诊断结果，其中7个必须依靠GS才能发现。([JAMA Network][2])

---

# 二、The Lancet Neurology：罕见神经遗传病非常值得系统看

## 1. 2022：WGS检测神经系统repeat expansion

### **Whole genome sequencing for the diagnosis of neurological repeat expansion disorders in the UK**

**The Lancet Neurology, 2022;21:234–245**

这是非常重要的一篇。

研究对象是英国疑似神经遗传病患者，利用WGS检测：

* C9orf72
* FMR1
* FXN
* HTT
* DMPK
* ATXN1/2/3/7
* TBP
* CACNA1A
* 等重复扩增位点。

文章指出，传统重复扩增检测往往是**locus-specific**，而WGS有机会在更广泛的遗传病诊断中统一检测。([EM Consulte][3])

这对你以后做：

```text
WGS
 ↓
SNV/Indel
SV
Repeat expansion
CNV
 ↓
rare disease diagnosis
```

很重要。

---

## 2. 2023：Gene-STEPS

### **Evaluation of the feasibility, diagnostic yield, and clinical utility of rapid genome sequencing in infantile epilepsy (Gene-STEPS)**

**The Lancet Neurology, 2023;22:812–825**

国际多中心研究，针对：

> neonatal/infantile-onset epilepsy

开展rapid genome sequencing。

这实际上就是：

```text
婴幼儿癫痫
     ↓
rapid WGS
     ↓
genetic diagnosis
     ↓
clinical utility
```

而且是澳大利亚、加拿大、英国、美国多中心合作。([科学直通车][4])

---

## 3. 2025：C9orf72 repeat expansion

### **Amyotrophic lateral sclerosis caused by hexanucleotide repeat expansions in C9orf72: from genetics to therapeutics**

**The Lancet Neurology, 2025;24:261–274**

重点是：

* repeat expansion
* penetrance
* RNA toxicity
* dipeptide repeat proteins
* TDP-43
* genetic diagnosis
* therapeutics

([科学直通车][5])

---

## 4. 2025：SMA

### **Safety and efficacy of apitegromab in nonambulatory type 2 or type 3 spinal muscular atrophy (SAPPHIRE)**

**The Lancet Neurology, 2025;24:727–739**

SMA是典型的遗传性罕见神经疾病，文章为多中心III期试验。([科学直通车][6])

---

# 三、The Lancet Child & Adolescent Health

这一刊对你的 **NDD/儿童罕见病** 项目非常重要。

## 1. 2024：遗传性发育性癫痫性脑病

### **The expanding field of genetic developmental and epileptic encephalopathies: current understanding and future perspectives**

**The Lancet Child & Adolescent Health, 2024;8:821–834**

重点：

* > 800个DEEs相关基因
* gBRAT-1
* GNAO1
* GRIN family
* HCN family
* gene therapy

这篇非常适合你做NDD课题的**疾病谱背景综述**。([科学直通车][7])

---

## 2. 2024：儿童癌症遗传易感

### **Comparison of clinical selection-based genetic testing with phenotype-agnostic extensive germline sequencing to diagnose genetic predisposition in children with cancer**

**The Lancet Child & Adolescent Health, 2024;8:751–761**

研究比较：

```text
clinical phenotype-driven testing
        VS
phenotype-agnostic extensive germline sequencing
```

这是一个很值得你借鉴的研究设计，因为它实际上对应：

> **“表型驱动WES” vs “广泛无偏WGS/WES”**

([科学直通车][8])

---

## 3. 2024：儿童罕见病支持

### **Holistic support for children with rare disease**

**The Lancet Child & Adolescent Health, 2024**

属于儿童罕见病综合管理方向。([OpenAlex][9])

---

# 四、The Lancet Gastroenterology & Hepatology

这里有一个与你的**罕见病精准诊断**特别相关的研究。

## 1. 2023：单基因IBD

### **Genomic diagnosis and care co-ordination for monogenic inflammatory bowel disease in children and adults**

**The Lancet Gastroenterology & Hepatology, 2023;8:271–286**

这是共识指南。

重点是：

> monogenic IBD

已知超过100种单基因疾病可以表现为IBD。

涉及：

* WGS/WES
* immunodeficiency
* early-onset IBD
* genotype–phenotype
* genomic diagnosis

([科学直通车][10])

对你的NDD项目来说，它提供了一个非常重要的范式：

```text
临床表型
   ↓
怀疑monogenic disease
   ↓
genomic testing
   ↓
分子诊断
   ↓
改变治疗策略
```

---

## 2. 2024：PFIC

### **Maralixibat in progressive familial intrahepatic cholestasis (MARCH-PFIC)**

**The Lancet Gastroenterology & Hepatology, 2024;9:620–631**

PFIC属于遗传性罕见胆汁淤积病。([科学直通车][11])

---

# 五、The Lancet Respiratory Medicine

## 1. 2024：淋巴管平滑肌瘤病

### **Nintedanib for patients with lymphangioleiomyomatosis**

**The Lancet Respiratory Medicine, 2024;12:967–974**

文章明确把 lymphangioleiomyomatosis 描述为：

> ultra-rare disease

属于罕见肺病治疗研究。([科学直通车][12])

---

## 2. 2024：囊性纤维化

### **Compassionate use trials and equitable access to variant-specific treatment for cystic fibrosis**

**The Lancet Respiratory Medicine, 2024;12:842–844**

强调：

> variant-specific treatment

也就是不同CFTR变异与个体化治疗之间的关系。([PubMed][13])

---

# 六、The Lancet Haematology

这一系列里面罕见遗传病非常多。

## 1. 2024：血友病B基因治疗

### **Etranacogene dezaparvovec gene therapy for haemophilia B (HOPE-B)**

**The Lancet Haematology, 2024;11:e265–e275**

24个月随访，单臂III期研究。

([科学直通车][14])

---

## 2. 2022：丙酮酸激酶缺乏症

### **Mitapivat in adult patients with pyruvate kinase deficiency receiving regular transfusions (ACTIVATE-T)**

**The Lancet Haematology, 2022**

丙酮酸激酶缺乏症是经典罕见遗传性溶血性贫血。该研究出现在2022年Lancet Haematology 9(10)期。([科学直通车][15])

---

## 3. 2024：CRISPR/Cas9 gene therapy

### **The dawn of the CRISPR/Cas9 gene therapy era**

**The Lancet Haematology, 2024**

重点讨论：

* gene editing
* CRISPR
* inherited hematologic diseases

([PubMed][16])

---

# 七、The Lancet Diabetes & Endocrinology

## 1. 2023：Rare diseases editorial

### **Rare diseases: individually rare, collectively common**

**The Lancet Diabetes & Endocrinology, 2023;11:139**

直接讨论罕见病。([PubMed][17])

---

## 2. 2025：遗传性肥胖

### **Setmelanotide in patients aged 2–5 years with rare MC4R pathway-associated obesity (VENTURE)**

**The Lancet Diabetes & Endocrinology, 2025;13:29–37**

涉及：

* POMC deficiency
* PCSK1 deficiency
* LEPR deficiency
* Bardet-Biedl syndrome
* MC4R pathway

([科学直通车][18])

---

# 八、JAMA Neurology：与你的NDD队列非常匹配

这部分建议重点读。

## 1. 2023：Polymicrogyria + WES

### **Exome Sequencing and the Identification of New Genes and Shared Mechanisms in Polymicrogyria**

**JAMA Neurology, 2023;80:980–988**

这是非常漂亮的**罕见神经疾病基因发现研究**。

275个家系：

```text
275 families
      ↓
panel / WES
      ↓
known genes
      +
novel associations
      ↓
Matchmaker Exchange
```

最终：

> genetic explanation rate = **32.7% (90/275)**

([JAMA Network][19])

这个模式与你未来做：

> NDD队列 → WES/WGS → candidate genes → phenotype → Matchmaker/knowledge graph

非常接近。

---

## 2. 2024：ES→GS

### **Genome Sequencing After Exome Sequencing in Pediatric Epilepsy**

125名患者：

```text
ES negative
     ↓
GS
     ↓
9 diagnostic / likely diagnostic
     ↓
7 required GS
```

([JAMA Network][2])

---

## 3. 2022：脑瘫

### **Molecular Diagnostic Yield of Exome Sequencing and Chromosomal Microarray in Cerebral Palsy**

**JAMA Neurology, 2022;79:1287–1295**

系统综述和meta-analysis，重点讨论CP中的：

* WES
* CMA
* genetic diagnosis

([JAMA Network][20])

---

# 九、JAMA Pediatrics

## 1. 2025：Clinical Long-read sequencing

### **Clinical Long-Read Sequencing Test for Genetic Disease Diagnosis**

这是我建议你**重点精读**的一篇。

235名LRS患者：

```text
HiFi long-read sequencing
        ↓
SNV
SV
CNV
repeat expansion
methylation
phasing
        ↓
genetic diagnosis
```

而且与513名standard-of-care患者进行了比较。([JAMA Network][1])

对于你现在做：

> **long-read RNA + WGS + NDD**

非常有价值。

---

## 2. 2024：ADHD rare variants

### **Rare De Novo and Inherited Genes in Familial and Nonfamilial Pediatric Attention-Deficit/Hyperactivity Disorder**

使用临床级WES。

77名proband，研究rare damaging inherited/de novo variants。

([JAMA Network][21])

---

## 3. 2023：Cerebral palsy

### **Diagnostic Yield of Exome Sequencing in Cerebral Palsy and Implications for Genetic Testing Guidelines**

系统综述和meta-analysis。

([JAMA Network][22])

---

## 4. 2023：Short stature

### **Molecular Diagnostic Yield of Exome Sequencing and Chromosomal Microarray in Short Stature**

21个研究队列的meta-analysis。

([JAMA Network][23])

---

# 十、JAMA Network Open：你的AI方向尤其值得关注

这本虽然不是传统意义上的“专科子刊”，但属于JAMA Network体系，而且对于你的项目非常重要。

## 2025：LLM + Undiagnosed Diseases Network

### **Large Language Models for Rare Disease Diagnosis at the Undiagnosed Diseases Network**

**JAMA Network Open, 2025;8:e2528538**

这是目前与你提出的：

> **WES + LLM + RAG + Knowledge Graph + Exomiser**

最直接相关的顶级医学期刊文章之一。

研究比较LLM在UDN罕见病诊断中的 differential diagnosis 表现。使用了：

* ChatGPT-4o
* Llama 3.1 8B Instruct
* Undiagnosed Diseases Network

([JAMA Network][24])

**这篇我建议你重点精读。**

---

## 2025：Rare disease drug repurposing

### **Strategies to Advance Drug Repurposing for Rare Diseases**

**JAMA Network Open, 2025;8:e258339**

讨论罕见病药物再利用策略。([JAMA Network][25])

---

# 十一、JAMA Dermatology：罕见皮肤遗传病

## 2024：Palmoplantar keratoderma

### **Clinical and Genetic Findings in Patients With Palmoplantar Keratoderma**

**JAMA Dermatology, 2024**

属于遗传性角化病/罕见皮肤遗传病方向。([JAMA Network][26])

---

## 2024：Recessive dystrophic epidermolysis bullosa

### **Revertant Mosaic Skin Punch Grafting in Recessive Dystrophic Epidermolysis Bullosa**

**JAMA Dermatology, 2024**

研究严重的遗传性大疱性表皮松解症。([JAMA Network][27])

---

# 十二、BMJ / Journal of Medical Genetics：这一部分非常适合你的研究

严格来说，**Journal of Medical Genetics 是BMJ Group旗下非常核心的遗传医学期刊**，如果你关注罕见病遗传诊断，这本实际上应该和JAMA Neurology、Lancet Neurology一起重点看。

---

## 1. 2023：1000例遗传性眼病

### **Diagnostic genome sequencing improves diagnostic yield: a prospective single-centre study in 1000 patients with inherited eye diseases**

**Journal of Medical Genetics, 2024;61:186–?**

这是非常值得你关注的一篇。

数据：

```text
1000 consecutive probands
        ↓
whole-genome sequencing
        +
RNA-seq subset (74)
        ↓
SV calling
+
RNA/DNA integration
```

最终：

> definite genetic diagnosis = **57.4%**

([BMJ Medicine][28])

这和你未来的：

> **WGS + RNA-seq + SV + phenotype**

路线非常接近。

---

# 十三、BMJ JMG：NDD/ID相关

## 1. 2024：WDR45

### **WDR45 variants as a major cause for a clinically variable intellectual disability syndrome from early infancy in females**

研究32名女性developmental delay患者，并结合：

* Sanger
* WGS
* X-chromosome inactivation

([BMJ Medicine][29])

---

## 2. 2025：Autism + WGS

### **Clinical utility of genome sequencing in autism: illustrative examples from a genomic research study**

研究：

> **202 families**

接受WGS结果后，100个家庭至少发现一个与ASD相关的临床意义结果。

([BMJ Medicine][30])

这个对你的**NDD/ASD队列**特别有参考价值。

---

# 十四、BMJ JMG：罕见病基因发现

## 1. Schaaf-Yang syndrome

### **Advancing in Schaaf-Yang syndrome pathophysiology: from bedside to subcellular analyses of truncated MAGEL2**

2022。

MAGEL2 truncating variants，属于超罕见遗传病。([BMJ Medicine][31])

---

## 2. 新骨骼遗传病

### **Heterozygous pathogenic variants involving CBFB cause a new skeletal disorder resembling cleidocranial dysplasia**

2022。

5个无关家系、8个患者，通过遗传分析确定新的疾病机制。([BMJ Medicine][32])

---

## 3. KBG syndrome

### **Deep phenotyping of the neuroimaging and skeletal features in KBG syndrome**

2023。

53例KBG syndrome患者，结合ANKRD11遗传变异与深度表型。([BMJ Medicine][33])

---

## 4. Alström syndrome

### **Genotype–phenotype associations in Alström syndrome: a systematic review and meta-analysis**

2024。

Alström syndrome属于超罕见单基因病。([BMJ Medicine][34])

---

## 5. OPDM repeat expansion

### **Non-coding CGG repeat expansion in LOC642361/NUTM2B-AS1 is associated with a phenotype of oculopharyngodistal myopathy**

2024。

涉及：

> non-coding repeat expansion + rare neuromuscular disease

([BMJ Medicine][35])

---

## 6. GNE myopathy

### **Novel variants and genotype-phenotype correlation in a multicentre cohort of GNE myopathy in China**

2024。

非常有意思，因为它同时使用：

* 113 patients
* WGS
* deep intronic variants
* Nanopore long-read sequencing
* 639-bp insertion

([BMJ Medicine][36])

这篇和你的 **long-read + rare disease** 方向非常匹配。

---

## 7. OGM + genetic diagnosis

### **Retrospective study on the utility of optical genome mapping as a follow-up method in genetic diagnostics**

2025。

7个SOC方法发现SV的病例进一步用OGM分析：

> 6/7病例通过OGM解决。

([BMJ Medicine][37])

---

# 十五、一个非常值得你关注的“新趋势”：Long-read正在进入罕见病临床诊断

把上述论文串起来，会出现非常清晰的技术演进：

```text
                 传统遗传诊断
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
        CMA                     Panel
          │                       │
          └───────────┬───────────┘
                      ↓
                     WES
                      │
                      ↓
                     WGS
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
      SNV             SV          CNV
                      │
                      ↓
                Repeat expansion
                      │
                      ↓
             Long-read sequencing
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      HiFi          ONT          RNA long-read
        │             │             │
        └─────────────┼─────────────┘
                      ↓
          Genome → Transcriptome
                      ↓
               Phenotype / HPO
                      ↓
              AI / LLM / KG
                      ↓
             Molecular diagnosis
```

JAMA Pediatrics 2025的临床HiFi LRS研究、Lancet Neurology的WGS repeat-expansion研究、BMJ JMG的1000例WGS+RNA-seq研究和GNE myopathy的Nanopore长读长研究，正好构成了这条技术路线的不同环节。([JAMA Network][1])

---

# 十六、如果专门针对你的“NDD罕见病队列 + AI诊断”项目，我建议重点建立这个文献矩阵

| 层级                                         | 推荐文章                                          | 对你的项目价值 |
| ------------------------------------------ | --------------------------------------------- | ------- |
| **超大规模NDD队列**                              | NEJM 2023 DDD/rare pediatric disease          | ⭐⭐⭐⭐⭐   |
| **WGS诊断**                                  | NEJM 2024 Genome Sequencing for Rare Diseases | ⭐⭐⭐⭐⭐   |
| **Long-read WGS**                          | JAMA Pediatr 2025 Clinical LRS                | ⭐⭐⭐⭐⭐   |
| **ES→GS**                                  | JAMA Neurol 2024 Pediatric Epilepsy           | ⭐⭐⭐⭐⭐   |
| **NDD基因发现**                                | JAMA Neurol 2023 Polymicrogyria               | ⭐⭐⭐⭐⭐   |
| **rapid WGS**                              | Lancet Neurol 2023 Gene-STEPS                 | ⭐⭐⭐⭐⭐   |
| **Repeat expansion**                       | Lancet Neurol 2022                            | ⭐⭐⭐⭐⭐   |
| **WGS+RNA**                                | JMG 2024 1000 inherited eye disease           | ⭐⭐⭐⭐⭐   |
| **WGS+ASD**                                | JMG 2025 autism                               | ⭐⭐⭐⭐⭐   |
| **WGS+long-read**                          | JMG 2024 GNE myopathy                         | ⭐⭐⭐⭐⭐   |
| **LLM+rare disease**                       | JAMA Netw Open 2025 UDN                       | ⭐⭐⭐⭐⭐   |
| **OGM+SV**                                 | JMG 2025 OGM                                  | ⭐⭐⭐⭐    |
| **Phenotype-driven vs phenotype-agnostic** | Lancet Child 2024                             | ⭐⭐⭐⭐⭐   |
| **Genomic diagnosis → treatment**          | Lancet Gastroenterol 2023 monogenic IBD       | ⭐⭐⭐⭐    |

---

# 十七、对你目前课题，我认为最值得组合成一个“文献逻辑链”

你之前提出的：

> **WES + LLM/RAG + Knowledge Graph + Exomiser + HPO + NDD rare disease cohort**

如果结合这些近四年文章，可以形成：

```text
                   NDD / Rare Disease Cohort
                            │
                            ↓
                    Deep Phenotyping
                    HPO + EHR + imaging
                            │
              ┌─────────────┴──────────────┐
              ↓                            ↓
            WES                           WGS
              │                            │
              ↓                            ↓
       SNV / Indel                    SV / CNV
                                           │
                                  Repeat expansion
                                           │
                                  Long-read WGS
                                           │
                         ┌─────────────────┘
                         ↓
                  Long-read RNA
                         │
                         ↓
             Aberrant isoform / splicing
                         │
                         ↓
       ┌─────────────────┴─────────────────┐
       ↓                                   ↓
   Exomiser                         LLM/RAG/KG
       │                                   │
       └─────────────────┬─────────────────┘
                         ↓
                  Variant prioritization
                         ↓
                  Molecular diagnosis
                         ↓
                  Functional validation
                         ↓
                  Precision treatment
```

这比单纯做一个“**WES + AI诊断模型**”要完整得多。

尤其是你现在已经在做 **PacBio long-read RNA、IsoQuant、atroplex、Isopedia、population pan-transcriptome**，所以你的项目实际上可以再向前走一步：

> **不是只解决“哪个基因有变异”，而是解决“这个变异如何改变真实的人类转录本/isoform，从而解释患者表型”。**

这正好把你现在的两个研究方向：

**① NDD/WES/WGS/AI rare disease diagnosis**

和

**② population long-read transcriptome / pan-transcriptome / isoform**

连接起来。

如果以发表论文为目标，这可能比单独做“LLM辅助WES诊断”更有新意。

[1]: https://jamanetwork.com/journals/jamapediatrics/fullarticle/2838675?utm_source=chatgpt.com "Clinical Long-Read Sequencing Test for Genetic Disease Diagnosis | Genetics and Genomics | JAMA Pediatrics | JAMA Network"
[2]: https://jamanetwork.com/journals/jamaneurology/fullarticle/2824572?utm_source=chatgpt.com "Genome Sequencing After Exome Sequencing in Pediatric Epilepsy | Neurogenetics | JAMA Neurology | JAMA Network"
[3]: https://www.em-consulte.com/revue/LANEUR/21/3/table-des-matieres?prompt=false&utm_source=chatgpt.com "Lancet Neurology, The - Vol 21 - n° 3 - EM consulte"
[4]: https://www.sciencedirect.com/science/article/pii/S1474442223002466?utm_source=chatgpt.com "Evaluation of the feasibility, diagnostic yield, and clinical utility of rapid genome sequencing in infantile epilepsy (Gene-STEPS): an international, multicentre, pilot cohort study - ScienceDirect"
[5]: https://www.sciencedirect.com/science/article/pii/S1474442225000262?utm_source=chatgpt.com "Amyotrophic lateral sclerosis caused by hexanucleotide repeat expansions in C9orf72: from genetics to therapeutics - ScienceDirect"
[6]: https://www.sciencedirect.com/science/article/pii/S147444222500225X?utm_source=chatgpt.com "Safety and efficacy of apitegromab in nonambulatory type 2 or type 3 spinal muscular atrophy (SAPPHIRE): a phase 3, double-blind, randomised, placebo-controlled trial - ScienceDirect"
[7]: https://www.sciencedirect.com/science/article/abs/pii/S2352464224001962?utm_source=chatgpt.com "The expanding field of genetic developmental and epileptic encephalopathies: current understanding and future perspectives - ScienceDirect"
[8]: https://www.sciencedirect.com/science/article/abs/pii/S2352464224001445?utm_source=chatgpt.com "Comparison of clinical selection-based genetic testing with phenotype-agnostic extensive germline sequencing to diagnose genetic predisposition in children with cancer: a prospective diagnostic study - ScienceDirect"
[9]: https://openalex.org/W4391850140?utm_source=chatgpt.com "Holistic support for children with rare disease"
[10]: https://www.sciencedirect.com/science/article/abs/pii/S2468125322003375?utm_source=chatgpt.com "Genomic diagnosis and care co-ordination for monogenic inflammatory bowel disease in children and adults: consensus guideline on behalf of the British Society of Gastroenterology and British Society of Paediatric Gastroenterology, Hepatology and Nutrition - ScienceDirect"
[11]: https://www.sciencedirect.com/science/article/pii/S2468125324000803?utm_source=chatgpt.com "Maralixibat in progressive familial intrahepatic cholestasis (MARCH-PFIC): a multicentre, randomised, double-blind, placebo-controlled, phase 3 trial - ScienceDirect"
[12]: https://www.sciencedirect.com/science/article/abs/pii/S2213260024002170?utm_source=chatgpt.com "Nintedanib for patients with lymphangioleiomyomatosis: a phase 2, open-label, single-arm study - ScienceDirect"
[13]: https://pubmed.ncbi.nlm.nih.gov/39151435/?utm_source=chatgpt.com "Compassionate use trials and equitable access to variant-specific treatment for cystic fibrosis."
[14]: https://www.sciencedirect.com/science/article/abs/pii/S2352302624000061?utm_source=chatgpt.com "Etranacogene dezaparvovec gene therapy for haemophilia B (HOPE-B): 24-month post-hoc efficacy and safety data from a single-arm, multicentre, phase 3 trial - ScienceDirect"
[15]: https://www.sciencedirect.com/journal/the-lancet-haematology/vol/9/issue/10?utm_source=chatgpt.com "The Lancet Haematology | Vol 9, Issue 10, Pages e707-e796 (October 2022) | ScienceDirect.com by Elsevier"
[16]: https://pubmed.ncbi.nlm.nih.gov/38135367/?utm_source=chatgpt.com "The dawn of the CRISPR/Cas9 gene therapy era."
[17]: https://pubmed.ncbi.nlm.nih.gov/36822740/?utm_source=chatgpt.com "Rare diseases: individually rare, collectively common."
[18]: https://www.sciencedirect.com/science/article/abs/pii/S2213858724002730?utm_source=chatgpt.com "Setmelanotide in patients aged 2–5 years with rare MC4R pathway-associated obesity (VENTURE): a 1 year, open-label, multicenter, phase 3 trial - ScienceDirect"
[19]: https://jamanetwork.com/journals/jamaneurology/fullarticle/2807207?utm_source=chatgpt.com "Exome Sequencing and the Identification of New Genes and Shared Mechanisms in Polymicrogyria | Genetics and Genomics | JAMA Neurology | JAMA Network"
[20]: https://jamanetwork.com/journals/jamaneurology/fullarticle/2797273?utm_source=chatgpt.com "Molecular Diagnostic Yield of Exome Sequencing and Chromosomal Microarray in Cerebral Palsy: A Systematic Review and Meta-analysis | Genetics and Genomics | JAMA Neurology | JAMA Network"
[21]: https://jamanetwork.com/journals/jamapediatrics/fullarticle/2812165?utm_source=chatgpt.com "Rare De Novo and Inherited Genes in Familial and Nonfamilial Pediatric Attention-Deficit/Hyperactivity Disorder | Attention Deficit/Hyperactivity Disorders | JAMA Pediatrics | JAMA Network"
[22]: https://jamanetwork.com/journals/jamapediatrics/fullarticle/2801964?utm_source=chatgpt.com "Diagnostic Yield of Exome Sequencing in Cerebral Palsy and Implications for Genetic Testing Guidelines: A Systematic Review and Meta-analysis | Genetics and Genomics | JAMA Pediatrics | JAMA Network"
[23]: https://jamanetwork.com/journals/jamapediatrics/fullarticle/2808913?utm_source=chatgpt.com "Molecular Diagnostic Yield of Exome Sequencing and Chromosomal Microarray in Short Stature: A Systematic Review and Meta-Analysis | Genetics and Genomics | JAMA Pediatrics | JAMA Network"
[24]: https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2837941?utm_source=chatgpt.com "Large Language Models for Rare Disease Diagnosis at the Undiagnosed Diseases Network | Digital Health | JAMA Network Open | JAMA Network"
[25]: https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2833522?utm_source=chatgpt.com "Strategies to Advance Drug Repurposing for Rare Diseases | Health Policy | JAMA Network Open | JAMA Network"
[26]: https://jamanetwork.com/journals/jamadermatology/fullarticle/2826499?utm_source=chatgpt.com "Clinical and Genetic Findings in Patients With Palmoplantar Keratoderma | Genetics and Genomics | JAMA Dermatology | JAMA Network"
[27]: https://jamanetwork.com/journals/jamadermatology/issue/160/10?utm_source=chatgpt.com "October 1, 2024 Issue of JAMA Dermatology | JAMA Network"
[28]: https://jmg.bmj.com/content/jmedgenet/61/2/186.full.pdf?with-ds=yes&utm_source=chatgpt.com "Diagnostic genome sequencing improves diagnostic yield: a prospective single-centre study in 1000 patients with inherited eye diseases"
[29]: https://jmg.bmj.com/content/early/2024/10/28/jmg-2024-110068?utm_source=chatgpt.com "WDR45 variants as a major cause for a clinically variable intellectual disability syndrome from early infancy in females | Journal of Medical Genetics"
[30]: https://jmg.bmj.com/content/early/2025/04/01/jmg-2024-110463?utm_source=chatgpt.com "Clinical utility of genome sequencing in autism: illustrative examples from a genomic research study | Journal of Medical Genetics"
[31]: https://jmg.bmj.com/content/early/2022/09/06/jmg-2022-108690?utm_source=chatgpt.com "Advancing in Schaaf-Yang syndrome pathophysiology: from bedside to subcellular analyses of truncated MAGEL2 | Journal of Medical Genetics"
[32]: https://jmg.bmj.com/content/jmedgenet/early/2022/10/13/jmg-2022-108739.full.pdf?utm_source=chatgpt.com "Heterozygous pathogenic variants involving CBFB cause a new skeletal disorder resembling cleidocranial dysplasia"
[33]: https://jmg.bmj.com/content/early/2023/08/15/jmg-2023-109141?versioned=true&utm_source=chatgpt.com "Deep phenotyping of the neuroimaging and skeletal features in KBG syndrome: a study of 53 patients and review of the literature | Journal of Medical Genetics"
[34]: https://jmg.bmj.com/content/61/1/18.abstract?utm_source=chatgpt.com "Genotype–phenotype associations in Alström syndrome: a systematic review and meta-analysis | Journal of Medical Genetics"
[35]: https://jmg.bmj.com/content/61/4/340?utm_source=chatgpt.com "Non-coding CGG repeat expansion in LOC642361/NUTM2B-AS1 is associated with a phenotype of oculopharyngodistal myopathy | Journal of Medical Genetics"
[36]: https://jmg.bmj.com/content/61/11/1053?utm_source=chatgpt.com "Novel variants and genotype-phenotype correlation in a multicentre cohort of GNE myopathy in China | Journal of Medical Genetics"
[37]: https://jmg.bmj.com/content/62/2/89?utm_source=chatgpt.com "Retrospective study on the utility of optical genome mapping as a follow-up method in genetic diagnostics | Journal of Medical Genetics"




---

# 如何搭建“WES + LLM/RAG + knowledge graph + variant evidence”的多模态罕见病诊断系统

如果你的目标是搭建一个真正用于**NDD/罕见病队列诊断**的系统，我不建议做成“把 WES 结果扔给 GPT，让它猜病”。更合理的架构是：

> **WES/WGS 提供硬证据 → HPO/临床文本提供表型证据 → Knowledge Graph 提供结构化生物医学知识 → RAG 提供可追溯文献证据 → LLM 负责整合、解释和交互 → 最终由规则/证据系统约束诊断结论。**

这与目前比较先进的 DeepRare 思路很接近：其架构把自由文本、HPO 和遗传检测结果作为异构输入，再由 LLM agent 调用专门工具和外部知识源，产生带证据链的 Top-K 诊断。([Nature][1])

---

# 一、我建议你搭建成这套总体架构

```text
                         NDD / Rare Disease Patient
                                    │
                ┌───────────────────┼───────────────────┐
                │                   │                   │
                ▼                   ▼                   ▼
         Clinical records        WES/WGS            Pedigree
         Free-text/HPO           VCF/gVCF             PED
                │                   │                   │
                ▼                   ▼                   │
          LLM Phenotype       Variant Annotation        │
             Agent                   │                   │
                │             ┌─────┴─────┐             │
                ▼             ▼           ▼             ▼
              HPO         Frequency    Functional    Inheritance
             terms        ClinVar      prediction      model
                │             │           │             │
                └─────────────┼───────────┼─────────────┘
                              ▼
                    ┌─────────────────────┐
                    │ Variant/Gene Engine  │
                    │ Exomiser + custom    │
                    └──────────┬──────────┘
                               │
                    Top 50–500 genes/variants
                               │
                ┌──────────────┼──────────────┐
                ▼              ▼              ▼
        Knowledge Graph       RAG          LLM Agents
        Gene-HPO-Disease      PubMed       phenotype
        Gene-Gene             guidelines   genotype
        Variant-Disease       ClinVar      literature
        Drug-Phenotype        OMIM         evidence
                │              │              │
                └──────────────┼──────────────┘
                               ▼
                     Evidence Integration
                               │
                               ▼
                    Candidate Disease/Gene
                               │
                               ▼
                       Variant Evidence
                               │
                               ▼
                    ACMG/AMP-style review
                               │
                               ▼
                 ┌─────────────────────────┐
                 │ Final diagnostic report │
                 │                         │
                 │ Disease                 │
                 │ Gene                    │
                 │ Variant                 │
                 │ Inheritance             │
                 │ Evidence                │
                 │ Confidence              │
                 │ Literature              │
                 └─────────────────────────┘
```

这里最关键的一点是：

**LLM 不应该成为 variant pathogenicity 的最终裁判。**

它应该是**evidence integration / reasoning layer**。

---

# 二、第一层：WES 数据处理

你的输入最好不是直接用原始 BAM，而是：

```text
FASTQ
 ↓
BWA-MEM2
 ↓
BAM
 ↓
GATK
 ↓
SNV/Indel VCF
 ↓
VEP / ANNOVAR
 ↓
annotated VCF
```

如果是 trio：

```text
Father
Mother
Child
   ↓
joint calling
   ↓
trio VCF
   ↓
inheritance analysis
```

然后保留：

```text
CHROM
POS
REF
ALT
GENE
TRANSCRIPT
HGVS.c
HGVS.p
Consequence
AF
gnomAD
ClinVar
CADD
REVEL
AlphaMissense
SpliceAI
```

以及：

```text
GT
AD
DP
GQ
```

---

# 三、第二层：不要让 LLM 直接分析几十万个变异

这是整个系统设计里非常重要的一点。

假设一个 WES：

```text
100,000–300,000 variants
```

不要：

```text
VCF
 ↓
GPT
 ↓
Diagnosis
```

这样既浪费 token，又容易 hallucination。

应该：

```text
WES
 ↓
hard filtering
 ↓
inheritance filtering
 ↓
pathogenicity annotation
 ↓
Exomiser
 ↓
Top 100 genes
 ↓
Top 500 variants
 ↓
LLM
```

---

# 四、第三层：Exomiser应该成为你的“第一代诊断引擎”

这一点非常重要。

Exomiser 本身就是：

> VCF + HPO → variant/gene prioritization

它综合：

* variant frequency
* pathogenicity
* inheritance
* phenotype similarity
* disease-gene association
* model organism phenotype

等信息。([GitHub][2])

例如：

```text
Patient HPO
   │
   ├── HP:0001263
   ├── HP:0001252
   ├── HP:0000729
   └── HP:0002311
          │
          ▼
       Exomiser
          │
          ▼
Gene A       score 0.91
Gene B       score 0.87
Gene C       score 0.82
...
```

而且最近针对 UDN 386 个已诊断病例的分析显示，优化 Exomiser 参数后，WES 中已知诊断变异进入 Top 10 的比例可以从 **67.3% 提高到 88.2%**。([PubMed Central (PMC)][3])

所以：

> **你的 LLM 系统应该和 Exomiser 比，而不是取代 Exomiser。**

---

# 五、第四层：Knowledge Graph 是整个系统的“知识骨架”

我建议建立一个 Rare Disease Knowledge Graph：

```text
                    Disease
                  /    |    \
                 /     |     \
              Gene    HPO    Variant
               |       |       |
               |       |       |
             Protein  Phenotype
               |
             Pathway
               |
             Drug
```

至少建立这些关系：

```text
Gene ──causes──> Disease

Disease ──has_phenotype──> HPO

Gene ──associated_with──> HPO

Variant ──located_in──> Gene

Variant ──associated_with──> Disease

Variant ──has_clinical_significance──> Pathogenic

Gene ──interacts_with──> Gene

Gene ──encodes──> Protein

Protein ──participates_in──> Pathway

Disease ──treated_by──> Drug
```

---

# 六、Knowledge Graph的数据源怎么选？

第一版不要自己从零构建。

建议整合：

### 核心

* HPO
* MONDO
* Orphanet
* OMIM
* ClinVar
* ClinGen
* Gene Ontology
* Reactome
* STRING
* gnomAD

### 遗传诊断

* DECIPHER
* PanelApp
* GenCC
* MIM
* Model organism phenotype

Exomiser 本身已经整合了相当一部分这种 phenotype/gene/disease/model-organism 信息。([PubMed][4])

---

# 七、数据库可以放进 Neo4j

如果你准备真正做一个项目，我推荐：

```text
Neo4j
```

而不是一开始就自己搞复杂的 RDF/SPARQL。

例如：

```text
(:Gene {id:"SHANK3"})
       │
       ├──[:CAUSES]──>
       │
       (:Disease {id:"MONDO:0010726"})
       │
       ├──[:HAS_PHENOTYPE]──>
       │
       (:HPO {id:"HP:0001263"})
```

Variant：

```text
(:Variant {hgvs:"NM_033517.1:c.3679C>T"})
       │
       ├──[:IN_GENE]──>
       (:Gene {id:"SHANK3"})
       │
       └──[:CLINVAR]──>
       (:Evidence {classification:"Pathogenic"})
```

---

# 八、第五层：RAG负责“文献证据”

Knowledge Graph 和 RAG 不应该混为一谈。

### Knowledge Graph

回答：

> **结构化关系是什么？**

例如：

```text
SHANK3 → associated with → autism
```

### RAG

回答：

> **为什么？哪篇文章？病例是什么？证据是什么？**

例如：

```text
SHANK3 variant
     ↓
PubMed
     ↓
case report
     ↓
patient phenotype
     ↓
functional experiment
```

---

# 九、RAG数据库怎么构建？

我建议：

```text
PubMed
ClinVar
ClinGen
OMIM
Orphanet
GeneReviews
case reports
guidelines
```

构建：

```text
PDF/XML/HTML
      ↓
document parsing
      ↓
chunking
      ↓
embedding
      ↓
vector DB
```

Vector DB 可以用：

```text
Qdrant
Milvus
Weaviate
FAISS
```

如果是你自己做科研原型：

> **Qdrant + PostgreSQL + Neo4j**

就已经很好。

---

# 十、不要让RAG只做普通“语义搜索”

这是你系统可以做出创新的地方。

普通 RAG：

```text
query:
SHANK3 autism

       ↓

retrieve 10 papers
```

更好的做法：

```text
Variant:
NM_033517:c.3679C>T

Gene:
SHANK3

Phenotype:
autism
hypotonia
speech delay

Inheritance:
de novo

       ↓

structured query
       +
semantic query
       ↓
RAG
```

最终：

```text
Paper 1
Paper 2
ClinVar
ClinGen
GeneReviews
Case report
```

进行**证据融合**。

---

# 十一、第六层：LLM不要只有一个 Agent

如果你想做高水平研究，我建议：

```text
                Supervisor LLM
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
Phenotype Agent  Variant Agent  Literature Agent
       │              │              │
       ↓              ↓              ↓
      HPO         ACMG evidence     RAG
       │              │              │
       └──────────────┼──────────────┘
                      ↓
                  Gene Agent
                      │
                      ↓
                Disease Agent
                      │
                      ↓
              Evidence Synthesizer
```

这个思路和目前 DeepRare 的多层 agent 架构非常接近。DeepRare 将中央 LLM host、专门 agent server 和外部医学知识源分开，并处理自由文本、HPO 和 VCF 等输入。([Nature][1])

---

# 十二、Phenotype Agent做什么？

例如病历：

> 患儿2岁，语言发育迟缓，肌张力低下，不能独立行走，反复癫痫。

LLM：

```text
Clinical text
      ↓
Phenotype extraction
      ↓
HPO normalization
```

输出：

```text
HP:0001263
Developmental delay

HP:0001252
Hypotonia

HP:0001250
Seizures

HP:0000750
Delayed speech
```

然后再判断：

```text
Present
Absent
Unknown
```

这一点很重要。

因为：

```text
没有描述
```

不等于：

```text
没有这个表型
```

---

# 十三、Variant Agent

Variant Agent 不应该自己“猜”。

它调用：

```text
ClinVar
ClinGen
gnomAD
VEP
CADD
REVEL
AlphaMissense
SpliceAI
LOFTEE
```

然后生成：

```text
Variant Evidence Object
```

例如：

```json
{
  "variant": "NM_033517.1:c.3679C>T",
  "gene": "SHANK3",
  "consequence": "missense",
  "gnomAD_AF": 0.00001,
  "clinvar": "Pathogenic",
  "clinvar_stars": 3,
  "REVEL": 0.91,
  "alphamissense": 0.97,
  "inheritance": "de_novo",
  "phenotype_match": 0.87
}
```

LLM只负责解释这些结构化证据。

---

# 十四、ACMG不要让LLM自由发挥

最好设计成：

```text
Variant
   ↓
Evidence extraction
   ↓
ACMG rules engine
   ↓
PVS1
PS1
PS2
PS3
PM2
PM5
PP3
PP4
...
   ↓
Classification
```

LLM：

```text
解释为什么满足PM2
解释为什么满足PS2
寻找文献支持
生成报告
```

而不是：

```text
GPT：
我认为这个变异是Pathogenic
```

例如 Exomiser 目前已经能利用 ClinVar 星级、phenotype matching、phased VCF 等信息进行部分 ACMG assignment。([Exomiser][5])

---

# 十五、最终应该形成一个 Evidence Graph

这是整个系统最漂亮的部分。

例如：

```text
                    Patient
                       │
             ┌─────────┴─────────┐
             │                   │
         Phenotype            Variant
             │                   │
          HPO terms          SHANK3:p.X
             │                   │
             └────────┬──────────┘
                      │
                    Gene
                   SHANK3
                      │
            ┌─────────┼─────────┐
            │         │         │
         Disease    Protein   Literature
            │         │         │
          ASD       SHANK3     PMIDxxxx
            │                   │
            └─────────┬─────────┘
                      │
                   Evidence
                      │
                      ▼
              Diagnostic hypothesis
```

这样最终每个结论都能回答：

> **为什么认为这个基因/变异是候选？**

---

# 十六、最终输出不要只给一个诊断

建议：

```text
Top 10 diseases
Top 20 genes
Top 50 variants
```

例如：

| Rank | Gene    | Variant | Disease         | HPO score | Variant score | Evidence                | Confidence |
| ---: | ------- | ------- | --------------- | --------: | ------------: | ----------------------- | ---------- |
|    1 | SHANK3  | c.XXX   | Phelan-McDermid |      0.93 |          0.98 | ClinGen + ClinVar + HPO | High       |
|    2 | SYNGAP1 | c.XXX   | SYNGAP1-related |      0.88 |          0.91 | ClinVar + literature    | High       |
|    3 | DDX3X   | c.XXX   | DDX3X-related   |      0.81 |          0.86 | literature              | Medium     |

然后：

```text
Diagnostic conclusion
        ↓
Evidence
        ↓
Literature
        ↓
Uncertainty
        ↓
Recommended next test
```

---

# 十七、如果是 NDD 队列，我建议进一步加入“未解决病例再分析”

这会让你的项目价值明显提高。

```text
                     NDD cohort
                         │
                         ▼
                       WES
                         │
                         ▼
                  Initial analysis
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
           Solved                 Unsolved
              │                     │
              │                     ▼
              │              LLM/RAG/KG
              │                     │
              │              re-ranking
              │                     │
              │          ┌──────────┼──────────┐
              │          ↓          ↓          ↓
              │       novel gene  novel      phenotype
              │                   variant    expansion
              │          │
              └──────────┴──────────┘
                         ↓
                   Reanalysis
                         ↓
                 Diagnosis improvement
```

尤其可以利用：

* 新发表文献
* 新 ClinVar
* 新 HPO
* 新 gene-disease association
* 新 ClinGen evidence

实现：

> **LLM-assisted longitudinal reanalysis**

这个方向比一次性诊断模型更有科研价值。

---

# 十八、我建议你的第一版技术栈

如果是你自己带队做科研，我会这样配：

### 基础生信

```text
BWA-MEM2
GATK
VEP
bcftools
samtools
```

### Variant prioritization

```text
Exomiser
Genomiser
```

Exomiser 可以直接接收 VCF + HPO，并输出排序后的 variants/genes。([GitHub][2])

### Knowledge Graph

```text
Neo4j
```

### RAG

```text
Qdrant
+
BGE-M3 / 同类生物医学embedding
```

### LLM

研究阶段可以：

```text
DeepSeek
Qwen
Llama
```

然后用 API/闭源模型做 benchmark：

```text
GPT
Claude
Gemini
```

### Agent

```text
LangGraph
```

或者自己用：

```text
Python
FastAPI
```

搭建。

---

# 十九、推荐的系统目录结构

可以直接按照这个项目结构开始：

```text
RareDiseaseAI/
│
├── data/
│   ├── patients/
│   ├── phenotype/
│   ├── pedigree/
│   └── vcf/
│
├── annotation/
│   ├── VEP/
│   ├── ClinVar/
│   ├── gnomAD/
│   ├── CADD/
│   ├── REVEL/
│   └── SpliceAI/
│
├── exomiser/
│
├── knowledge_graph/
│   ├── HPO/
│   ├── MONDO/
│   ├── OMIM/
│   ├── Orphanet/
│   ├── ClinGen/
│   └── Neo4j/
│
├── rag/
│   ├── PubMed/
│   ├── Guidelines/
│   ├── CaseReports/
│   ├── embeddings/
│   └── Qdrant/
│
├── agents/
│   ├── phenotype_agent.py
│   ├── variant_agent.py
│   ├── gene_agent.py
│   ├── disease_agent.py
│   ├── literature_agent.py
│   └── supervisor.py
│
├── evidence/
│   ├── ACMG/
│   ├── evidence_graph/
│   └── scoring/
│
├── evaluation/
│   ├── solved_cases/
│   ├── benchmark/
│   └── metrics/
│
└── report/
    └── diagnostic_report.py
```

---

# 二十、最关键的是设计 Benchmark

如果你要发表高水平文章，这一步比搭系统本身还重要。

你最好建立：

> **已确诊 NDD rare disease cohort**

例如：

```text
500–2000 cases
```

其中：

```text
80% training/development
20% independent test
```

但测试集必须保持真正独立。

每个病例：

```text
Clinical text
HPO
WES
Pedigree
Confirmed diagnosis
Causal gene
Causal variant
ACMG classification
```

然后比较：

### Baseline 1

```text
Phenotype → LLM
```

### Baseline 2

```text
HPO → Exomiser
```

### Baseline 3

```text
HPO + WES → Exomiser
```

### Baseline 4

```text
HPO + WES → LLM
```

### Baseline 5

```text
HPO + WES + KG + RAG → LLM
```

### Baseline 6

```text
HPO + WES + KG + RAG
        ↓
   Multi-agent LLM
```

---

# 二十一、评价指标

至少做：

```text
Disease Top-1
Disease Top-3
Disease Top-5
Disease Top-10

Gene Top-1
Gene Top-5
Gene Top-10
Gene Top-20

Variant Top-1
Variant Top-10
Variant Top-50
```

另外做：

```text
diagnostic yield
sensitivity
specificity
MRR
Recall@K
AUROC
AUPRC
```

尤其是：

> **Known diagnostic variant rank**

非常重要。

最近的 Exomiser 优化研究就是用“已知诊断变异进入 Top-10 的比例”作为核心评价指标之一。([PubMed Central (PMC)][3])

---

# 二十二、你这个项目真正有创新空间的地方

如果让我帮你把它定位成一个科研项目，我不会把题目写成：

> “基于大语言模型的罕见病诊断”

太普通。

我会设计成：

> **基于临床表型—基因组—知识图谱—检索增强大语言模型的神经发育障碍罕见病智能诊断与遗传变异优先级系统**

英文可以是：

> **A Multimodal Knowledge-Grounded Large Language Model Framework for Phenotype- and Genome-Informed Diagnosis of Rare Neurodevelopmental Disorders**

核心创新点可以有四个：

**① Multimodal**

```text
Clinical text
+
HPO
+
WES
+
Pedigree
```

**② Knowledge-grounded**

```text
HPO
+
Gene
+
Disease
+
Variant
+
Pathway
```

**③ Evidence-grounded RAG**

```text
ClinVar
ClinGen
PubMed
GeneReviews
Case reports
```

**④ Agentic reasoning**

```text
Phenotype Agent
+
Variant Agent
+
Gene Agent
+
Literature Agent
+
Supervisor
```

而不是单纯：

```text
GPT → diagnosis
```

---

## 二十三、如果现在开始，我建议分三期做

### Phase I：3个月

先不要训练模型。

做：

```text
WES
 ↓
VEP
 ↓
Exomiser
 ↓
HPO
 ↓
Neo4j
 ↓
RAG
 ↓
LLM
```

先把**可解释诊断原型**跑起来。

---

### Phase II：3–6个月

加入：

```text
multi-agent
+
ACMG evidence engine
+
literature RAG
+
pedigree
+
reanalysis
```

建立：

```text
500–1000 solved NDD cases
```

做 benchmark。

---

### Phase III：6–12个月

再考虑：

```text
fine-tuning
LoRA
RL
multimodal model
novel gene discovery
novel variant discovery
```

最终形成：

```text
             NDD Patient
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
   Clinical      WES       Pedigree
       ↓          ↓          ↓
      HPO     Variant      MOI
       │      Evidence       │
       └─────────┬────────────┘
                 ↓
       ┌──────────────────┐
       │ Knowledge Graph  │
       └────────┬─────────┘
                ↓
          Exomiser/KG
                ↓
         Candidate genes
                ↓
       ┌────────┴────────┐
       ↓                 ↓
      RAG            Multi-Agent
       │                 │
       └────────┬────────┘
                ↓
        Evidence Fusion
                ↓
       ACMG/AMP Evidence
                ↓
        Ranked diagnosis
                ↓
       Explainable Report
```

**这套路线尤其适合你前面考虑的 NDD 罕见病队列项目。**它的优势是：即使 LLM 本身并不比 Exomiser 强，也可以把研究问题变成“**LLM 是否能在已有可靠基因组优先级系统之上，通过 RAG + knowledge graph + evidence reasoning 提高复杂/未解决病例的诊断率和可解释性**”。这比单纯 benchmark GPT-4 的科学问题强很多。DeepRare 的最新结果也支持这种“异构数据 + 工具调用 + 可追溯证据”的架构方向。([Nature][1])

如果你准备**实际开始搭建**，我建议第一步就从 **“WES + HPO + Exomiser + Neo4j + Qdrant + LLM”最小可运行版本（MVP）**开始，而不是先训练大模型。

[1]: https://www.nature.com/articles/s41586-025-10097-9?utm_source=chatgpt.com "An agentic system for rare disease diagnosis with traceable reasoning | Nature"
[2]: https://github.com/exomiser/Exomiser?utm_source=chatgpt.com "GitHub - exomiser/Exomiser: A Tool to Annotate and Prioritize Exome Variants · GitHub"
[3]: https://pmc.ncbi.nlm.nih.gov/articles/PMC12539062/?utm_source=chatgpt.com "An optimized variant prioritization process for rare disease diagnostics: recommendations for Exomiser and Genomiser - PMC"
[4]: https://pubmed.ncbi.nlm.nih.gov/35391505/?utm_source=chatgpt.com "Phenotype-driven approaches to enhance variant prioritization and diagnosis of rare disease - PubMed"
[5]: https://exomiser.readthedocs.io/en/latest/acmg_assignment.html?utm_source=chatgpt.com "ACMG Assignment — exomiser 15.1.1 documentation"



---

# 近三年大语言模型在罕见病中的应用的高水平文章

有，而且 **2024–2026 年已经出现一批比较成体系的工作**。如果你关注的是你前面提到的 **NDD/罕见病队列、WES/WGS、表型、基因优先级、临床诊断**，我建议重点看下面这些，而不是泛泛的“ChatGPT 医疗应用”。

我按**文章水平 + 与罕见病遗传诊断的相关性**给你筛了一遍。

## 一、最值得重点看的 10 篇

| 年份       | 期刊                                     | 文章                                                                                                                                                         | LLM应用                            | 推荐    |
| -------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- | ----- |
| **2026** | **Nature**                             | **An agentic system for rare disease diagnosis with large language models**                                                                                | 多智能体 + LLM + HPO + 遗传检测结果 + 工具调用 | ⭐⭐⭐⭐⭐ |
| **2026** | **npj Digital Medicine**               | **Interpretable fine-tuned large language models facilitate making genetic test decisions for rare diseases**                                              | LLM决定 Panel/WES/WGS              | ⭐⭐⭐⭐⭐ |
| **2026** | **European Journal of Human Genetics** | **Systematic benchmarking demonstrates large language models have not reached the diagnostic accuracy of traditional rare-disease decision support tools** | 5213例罕见病，LLM vs Exomiser         | ⭐⭐⭐⭐⭐ |
| **2025** | **Nature Medicine**                    | **A generalist medical language model for disease diagnosis assistance**                                                                                   | MedFound-176B，罕见病诊断              | ⭐⭐⭐⭐⭐ |
| **2025** | **npj Digital Medicine**               | **Enhancing diagnostic capability with multi-agents conversational large language models**                                                                 | 多智能体罕见病诊断                        | ⭐⭐⭐⭐⭐ |
| **2025** | **npj Digital Medicine**               | **Few shot learning for phenotype-driven diagnosis of patients with rare genetic diseases**                                                                | SHEPHERD，表型→基因/疾病                | ⭐⭐⭐⭐⭐ |
| **2025** | **Genome Medicine**                    | **Improving automated deep phenotyping through large language models using retrieval-augmented generation**                                                | RAG-HPO，临床文本→HPO                 | ⭐⭐⭐⭐⭐ |
| **2024** | **American Journal of Human Genetics** | **Assessing the utility of large language models for phenotype-driven gene prioritization in the diagnosis of rare genetic disease**                       | GPT-4/Llama→基因优先级                | ⭐⭐⭐⭐⭐ |
| **2024** | **American Journal of Human Genetics** | **Evaluating large language models on medical, lay-language, and self-reported descriptions of genetic conditions**                                        | 63种遗传病诊断                         | ⭐⭐⭐⭐  |
| **2025** | **npj Digital Medicine**               | **A phenotype-based AI pipeline outperforms human experts in differentially diagnosing rare diseases using EHRs**                                          | EHR→表型→罕见病诊断                     | ⭐⭐⭐⭐  |

下面分别说。

---

# 1. Nature：DeepRare —— 目前非常值得关注

**An agentic system for rare disease diagnosis with large language models**

这是我认为你目前做**罕见病队列 + AI诊断**最应该重点看的文章之一。

[Nature 原文：An agentic system for rare disease diagnosis with large language models](https://www.nature.com/articles/s41586-025-10097-9?utm_source=chatgpt.com)

DeepRare 的核心不是简单：

```text
患者表型
 ↓
GPT
 ↓
疾病
```

而是：

```text
Clinical text
     │
HPO
     │
Genetic testing
     │
     ▼
┌───────────────────┐
│   DeepRare        │
│  multi-agent LLM  │
├───────────────────┤
│ phenotype agent   │
│ genetics agent    │
│ disease agent     │
│ literature/tool   │
│ reasoning agent   │
└─────────┬─────────┘
          ↓
   differential diagnosis
          ↓
   evidence-supported
      diagnosis
```

它能够处理：

* 自由文本临床描述
* HPO
* 遗传检测结果
* 多种专业工具
* 外部知识源
* 多智能体推理

并且输出**带证据支持的候选诊断**。([Nature][1])

### 对你最有价值的地方

你现在如果做：

> **NDD罕见病队列 + WGS/WES + HPO + AI诊断**

DeepRare实际上给出了一个很好的研究框架：

**LLM不直接替代 variant caller，而是作为“临床表型—基因组—知识库”的推理层。**

---

# 2. npj Digital Medicine 2026：RareDAI

**Interpretable fine-tuned large language models facilitate making genetic test decisions for rare diseases**

这篇和你的方向也非常接近。

[npj Digital Medicine 原文：RareDAI](https://www.nature.com/articles/s41746-026-02733-z?utm_source=chatgpt.com)

它研究的不是“这个患者是什么病”，而是一个非常实际的问题：

> **这个患者应该做什么遗传检测？**

例如：

```text
clinical information
       ↓
   LLM / RareDAI
       ↓
 ┌─────┼─────┐
 │     │     │
Panel  WES   WGS
```

模型输入包括：

* 非结构化临床记录
* structured Phecodes
* 临床指南

目标是模拟医生依据 ACMG 等指南进行遗传检测决策。([doi.org][2])

### 对你的启发非常大

你的 NDD 队列完全可以进一步做：

```text
NDD phenotype
      +
family history
      +
HPO
      +
clinical features
      ↓
      LLM
      ↓
Recommended testing strategy
      │
      ├── WES
      ├── WGS
      ├── CNV
      ├── mtDNA
      ├── repeat expansion
      └── long-read sequencing
```

这个方向甚至比单纯“GPT诊断罕见病”更有研究价值。

---

# 3. 2026 EJHG：5213例罕见病 LLM benchmark

这篇我特别建议你看。

**Systematic benchmarking demonstrates large language models have not reached the diagnostic accuracy of traditional rare-disease decision support tools**

[EJHG 原文](https://www.nature.com/articles/s41431-026-02054-5?utm_source=chatgpt.com)

这是一个非常重要的**反面证据**。

研究用了：

> **5213 个罕见遗传病病例**

比较：

* o1-preview
* GPT-4o
* Gemini
* o1-mini
* Meditron
* Meditron3
* Medfound

和：

* **Exomiser**

结果非常值得注意：

| 方法       |     Top-1 |     Top-3 |    Top-10 |
| -------- | --------: | --------: | --------: |
| 最佳LLM    |     23.6% |     31.2% | **36.8%** |
| Exomiser | **35.5%** | **46.3%** | **58.5%** |

也就是说：

> **纯 phenotype → LLM diagnosis，目前仍明显不如专门的 rare-disease decision-support 工具。** ([Nature][3])

这个结论对你的研究设计非常重要。

### 它告诉你不要做：

```text
HPO → GPT → diagnosis
```

然后声称：

> LLM优于Exomiser。

目前很难成立。

更有价值的是：

```text
HPO
 +
WGS/WES
 +
variant evidence
 +
literature
 +
HPO ontology
 +
LLM
 ↓
integrated diagnosis
```

---

# 4. Nature Medicine 2025：MedFound

**A generalist medical language model for disease diagnosis assistance**

这是顶级期刊里非常值得看的 LLM 医疗诊断工作。

[Nature Medicine 原文](https://www.nature.com/articles/s41591-024-03416-6?utm_source=chatgpt.com)

MedFound：

> **176 billion parameters**

训练数据包括：

* 医学文本
* 真实世界临床记录

然后用：

* self-bootstrapping
* chain-of-thought
* preference alignment

训练诊断能力。

重要的是，它专门评估了：

> **long-tailed distribution / rare diseases**

并覆盖多个医学专科。([Nature][4])

它不是一个专门的 rare-disease LLM，但对你研究**“为什么需要医疗专用LLM”**很重要。

---

# 5. npj Digital Medicine 2025：多智能体诊断

**Enhancing diagnostic capability with multi-agents conversational large language models**

[npj Digital Medicine 原文](https://doi.org/10.1038/s41746-025-01550-0?utm_source=chatgpt.com)

这是一个很有意思的方向。

用了：

> **302个罕见病病例**

比较：

```text
GPT-3.5
GPT-4
      vs
Multi-Agent Conversation (MAC)
```

MAC模拟：

```text
Doctor 1
   ↓
Doctor 2
   ↓
Doctor 3
   ↓
Supervisor
   ↓
Final diagnosis
```

最终发现多智能体框架在诊断和推荐进一步检查方面优于单个模型。([doi.org][5])

### 这对 NDD 特别适合

你甚至可以设计：

```text
              NDD patient
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
   Phenotype     Genetics    Neurology
     Agent        Agent       Agent
        │          │          │
        └──────────┼──────────┘
                   ↓
             Evidence Agent
                   ↓
            Supervisor LLM
                   ↓
        ┌──────────┼──────────┐
        ↓          ↓          ↓
    disease      gene       variant
    ranking     ranking     ranking
```

这比单纯“GPT-4诊断NDD”有明显的方法学创新空间。

---

# 6. SHEPHERD：非常值得看，但它不是传统LLM

**Few shot learning for phenotype-driven diagnosis of patients with rare genetic diseases**

[npj Digital Medicine 原文](https://doi.org/10.1038/s41746-025-01749-1?utm_source=chatgpt.com)

SHEPHERD 是 **knowledge-grounded deep learning**，不是传统意义上的 ChatGPT/LLM。

但如果你研究：

> **NDD罕见病诊断 + phenotype + genotype**

这篇非常重要。

它使用：

* phenotype
* candidate genes
* rare disease knowledge graph

实现：

```text
patient phenotype
      ↓
causal gene discovery
      ↓
patients-like-me
      ↓
novel disease presentation
```

并在：

* UDN：465
* MyGene2：146
* DDD：1431

等真实队列上验证。([doi.org][6])

### 为什么我建议你看？

因为它代表了另一条路线：

**不是让 LLM “凭知识猜答案”，而是让 AI 建立在 rare-disease knowledge graph 上。**

这可能比纯 LLM 更适合真正的临床遗传诊断。

---

# 7. Genome Medicine：RAG-HPO

**Improving automated deep phenotyping through large language models using retrieval-augmented generation**

[Genome Medicine 原文](https://link.springer.com/article/10.1186/s13073-025-01521-w?utm_source=chatgpt.com)

这篇对于你做**临床文本 → HPO**特别重要。

作者开发：

> **RAG-HPO**

核心：

```text
Clinical note
      ↓
LLM
      +
Vector database
      ↓
retrieve phenotype concepts
      ↓
HPO ID
```

知识库包含：

> **54,000+ phenotype phrases → HPO IDs**

利用 RAG 减少 LLM hallucination。([Springer][7])

### 这个方向和你的NDD队列高度匹配

例如你有：

```text
病历：
“患儿2岁仍不能独立行走，
语言发育明显落后，
存在肌张力低下……”
```

自动变成：

```text
HP:0001252
HP:0001263
HP:0001290
...
```

然后进入：

```text
HPO
 ↓
Exomiser
 ↓
LLM
 ↓
SHEPHERD
 ↓
Gene ranking
```

---

# 8. AJHG 2024：LLM做基因优先级

这篇是**你一定应该下载下来仔细看的**。

**Assessing the utility of large language models for phenotype-driven gene prioritization in the diagnosis of rare genetic disease**

[AJHG/Cell Press 原文](https://www.sciencedirect.com/science/article/pii/S0002929724002969?utm_source=chatgpt.com)

作者比较：

* GPT-4
* GPT-3.5
* Llama2-70B
* Llama2-13B
* Llama2-7B

在 rare genetic disease phenotype → gene prioritization 上的表现。

GPT-4：

> Top-10：13.9%

> Top-50：17.0%

但仍然低于传统工具。([科学直接][8])

还有一个非常重要的发现：

> **LLM存在明显的“热门基因偏倚”。**

例如：

```text
BRCA1
TP53
PTEN
```

这种文献量极大的基因更容易被 LLM 选中，而真正罕见的疾病基因反而容易漏掉。([PubMed Central (PMC)][9])

这对于 rare disease 非常关键。

---

# 9. AJHG 2024：医学语言 vs 普通语言

**Evaluating large language models on medical, lay-language, and self-reported descriptions of genetic conditions**

[PubMed：Flaharty et al.](https://pubmed.ncbi.nlm.nih.gov/39146935/?utm_source=chatgpt.com)

研究了：

> **63种遗传疾病**

比较：

* medical language
* lay language
* self-reported descriptions

以及：

* GPT-3.5
* GPT-4
* Claude
* Gemini/Bard
* Llama2
* MedLlama2

等。

它特别适合研究：

> **患者自己描述的症状 → LLM → 遗传病候选诊断**

这对未来的患者端 rare disease AI 很有意义。([PubMed][10])

---

# 10. npj Digital Medicine：EHR → 罕见病

**A phenotype-based AI pipeline outperforms human experts in differentially diagnosing rare diseases using EHRs**

[npj Digital Medicine 原文](https://www.nature.com/articles/s41746-025-01452-1?utm_source=chatgpt.com)

这个不是纯 LLM，而是一个很值得你关注的 **AI pipeline**。

数据：

> **2271 cases / 431 rare diseases**

并且有：

> **75 cases / 50 specialist physicians**

进行 human-computer comparison。

PhenoBrain：

```text
EHR
 ↓
phenotype extraction
 ↓
phenotype representation
 ↓
disease ranking
```

最终在部分任务上超过 ChatGPT/GPT-4 和专家医生。([Nature][11])

---

# 还有一个方向：Genetic Transformer / GeneT

如果你特别关心：

> **WES/WGS → candidate variant**

那么这篇值得关注：

**Genetic Transformer: An Innovative Large Language Model Driven Approach for Rapid and Accurate Identification of Causative Variants in Rare Genetic Diseases**

目前检索到的是 **medRxiv**，所以我不会把它和上面的 Nature/AJHG 正式论文放在同一个证据等级。([MedRxiv][12])

它直接把目标从：

```text
phenotype → disease
```

推进到了：

```text
WES/WGS
  ↓
variants
  ↓
LLM
  ↓
candidate causal variants
```

这实际上更接近你做 NDD 队列的需求。

---

# 二、把这些文章放在一起，会看到一个非常清楚的发展路线

近三年其实经历了：

### 第一阶段：ChatGPT 做诊断

```text
Clinical description
        ↓
      GPT-4
        ↓
     Disease
```

代表：

**Flaharty 2024**

---

### 第二阶段：LLM 做 phenotype → gene

```text
HPO
 ↓
LLM
 ↓
Gene ranking
```

代表：

**Kim et al., AJHG 2024**

但问题是：

> **Exomiser 等传统方法仍然更强。**

([科学直接][8])

---

### 第三阶段：LLM + RAG + knowledge graph

```text
Clinical text
      ↓
    LLM
      +
Knowledge graph
      +
RAG
      ↓
HPO / disease / gene
```

代表：

**RAG-HPO**

**SHEPHERD**

---

### 第四阶段：Multi-agent LLM

```text
Phenotype Agent
       +
Genomics Agent
       +
Disease Agent
       +
Literature Agent
       ↓
Supervisor
       ↓
Diagnosis
```

代表：

**MAC**

**DeepRare**

---

### 第五阶段：真正的 genomic decision support

未来更值得做的是：

```text
             Clinical phenotype
                    │
             ┌──────┴──────┐
             ↓             ↓
           HPO          Clinical text
             │             │
             └──────┬──────┘
                    ↓
               LLM/RAG
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       Disease     Gene     Variant
       ranking    ranking    ranking
          │         │         │
          └─────────┼─────────┘
                    ↓
              WES/WGS evidence
                    ↓
             ClinVar/OMIM/
             HPO/gnomAD/
             literature
                    ↓
             Final diagnosis
```

**我认为这个方向比“拿GPT-4诊断罕见病”有明显更高的研究价值。**

---

# 三、如果你是为了你前面提到的“NDD罕见病队列”项目

我会特别推荐你把文献分成下面 **4条技术路线**：

| 方向                                 | 代表工作                 | 适合你的程度 |
| ---------------------------------- | -------------------- | -----: |
| 临床文本 → HPO                         | RAG-HPO              |  ⭐⭐⭐⭐⭐ |
| HPO → Disease/Gene                 | Kim / SHEPHERD       |  ⭐⭐⭐⭐⭐ |
| Multi-agent rare disease diagnosis | MAC / DeepRare       |  ⭐⭐⭐⭐⭐ |
| WES/WGS → Variant/Gene             | GeneT / Exomiser+LLM |  ⭐⭐⭐⭐⭐ |

而且你前面正在考虑的 **NDD罕见病队列诊断**，其实非常适合进一步设计成一个：

> **“HPO + WGS + pedigree + LLM/RAG + knowledge graph + variant evidence”的多模态罕见病诊断系统**

而不是简单做一个 ChatGPT benchmark。

---

## 四、如果让我选“最值得精读”的 6 篇

如果你不想一次看十几篇，我建议按照这个顺序：

**① DeepRare — Nature 2026**
→ 看最新的 **agentic rare-disease diagnosis**

**② 5213 cases LLM benchmark — EJHG 2026**
→ 看 LLM 与 Exomiser 的真实差距

**③ RareDAI — npj Digital Medicine 2026**
→ 看 **LLM + genetic testing decision**

**④ MedFound — Nature Medicine 2025**
→ 看医疗大模型本身怎么训练

**⑤ SHEPHERD — npj Digital Medicine 2025**
→ 看 **knowledge graph + rare disease diagnosis**

**⑥ Kim et al. — AJHG 2024**
→ 看 **LLM phenotype → gene prioritization** 的系统 benchmark

这六篇基本可以把 **2024–2026 年 LLM + 罕见病诊断**的技术演进串起来。([Nature][1])

如果你的目的其实是为**“粤港澳认知与情感神经生物学/NDD罕见病队列”申请项目**做文献依据，那么下一步最有价值的是把这些文章进一步整理成 **2023–2026 年 20–30 篇高水平论文表格**，专门增加 **队列规模、NDD/罕见病类型、WES/WGS、HPO、LLM类型、是否RAG、是否multi-agent、是否真实临床队列、诊断Top-1/Top-10、是否发现新基因、代码/模型是否开放** 这些字段。

[1]: https://www.nature.com/articles/s41586-025-10097-9?utm_source=chatgpt.com "An agentic system for rare disease diagnosis with ..."
[2]: https://doi.org/10.1038/s41746-026-02733-z?utm_source=chatgpt.com "Interpretable fine-tuned large language models facilitate making genetic test decisions for rare diseases | npj Digital Medicine"
[3]: https://www.nature.com/articles/s41431-026-02054-5?utm_source=chatgpt.com "Systematic benchmarking demonstrates large language ..."
[4]: https://www.nature.com/articles/s41591-024-03416-6?utm_source=chatgpt.com "A generalist medical language model for disease diagnosis assistance | Nature Medicine"
[5]: https://doi.org/10.1038/s41746-025-01550-0?utm_source=chatgpt.com "Enhancing diagnostic capability with multi-agents conversational large language models | npj Digital Medicine"
[6]: https://doi.org/10.1038%2Fs41746-025-01749-1?utm_source=chatgpt.com "Few shot learning for phenotype-driven diagnosis of patients with rare genetic diseases | npj Digital Medicine"
[7]: https://link.springer.com/article/10.1186/s13073-025-01521-w?utm_source=chatgpt.com "Improving automated deep phenotyping through large language models using retrieval-augmented generation | Genome Medicine | Springer Nature Link"
[8]: https://www.sciencedirect.com/science/article/pii/S0002929724002969?utm_source=chatgpt.com "Assessing the utility of large language models for phenotype-driven gene prioritization in the diagnosis of rare genetic disease - ScienceDirect"
[9]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11480789/?utm_source=chatgpt.com "Assessing the utility of large language models for phenotype-driven gene prioritization in the diagnosis of rare genetic disease - PMC"
[10]: https://pubmed.ncbi.nlm.nih.gov/39146935/?utm_source=chatgpt.com "Evaluating large language models on medical, lay-language, and self-reported descriptions of genetic conditions - PubMed"
[11]: https://www.nature.com/articles/s41746-025-01452-1?utm_source=chatgpt.com "A phenotype-based AI pipeline outperforms human experts in differentially diagnosing rare diseases using EHRs | npj Digital Medicine"
[12]: https://www.medrxiv.org/content/10.1101/2024.07.18.24310666v1.full?utm_source=chatgpt.com "Genetic Transformer: An Innovative Large Language Model Driven Approach for Rapid and Accurate Identification of Causative Variants in Rare Genetic Diseases | medRxiv"


---

# 罕见病重要文章


我先给你做一个**尽可能全面的第一轮清单**，重点放在 **Nature / Nature Genetics / Nature Medicine / Nature Communications / AJHG / Genetics in Medicine** 等高水平期刊，而且优先选择真正使用了**数千到数万甚至十万级罕见病或相关基因组数据**的研究。



# 一、最值得优先看的 TOP 15

如果你只想先读最重要的一批，我建议下面这 15 篇。

| 年份       | 文章                                                                                                                                                                      | 期刊                        |                                 队列规模 | 核心内容                                                      | 推荐度   |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | -----------------------------------: | --------------------------------------------------------- | ----- |
| **2025** | **Rare disease gene association discovery in the 100,000 Genomes Project**                                                                                              | **Nature**                |     34,851 families / 72,690 genomes | rare variant gene burden、Mendelian disease gene discovery | ⭐⭐⭐⭐⭐ |
| **2023** | **Genetic association analysis of 77,539 genomes reveals rare disease etiologies**                                                                                      | **Nature Medicine**       |                               77,539 | Rareservoir + BeviMed + HPO + gene discovery              | ⭐⭐⭐⭐⭐ |
| **2024** | **Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses**                                                                                     | **Nature Medicine**       |   6,004 families / 9,645 individuals | 跨欧洲罕见病大队列系统 reanalysis                                    | ⭐⭐⭐⭐⭐ |
| **2024** | **Rare coding variant analysis for human diseases across biobanks and ancestries**                                                                                      | **Nature Genetics**       |                              748,879 | 多 biobank + rare coding burden + 多祖源                      | ⭐⭐⭐⭐⭐ |
| **2024** | **Pangenome graphs improve the analysis of structural variants in rare genetic diseases**                                                                               | **Nature Communications** |                  GA4K 574 assemblies | HiFi + pangenome + SV                                     | ⭐⭐⭐⭐⭐ |
| **2025** | **GREGoR: accelerating genomics for rare diseases**                                                                                                                     | **Nature**                |                         GREGoR 多中心资源 | 美国大型罕见病基因组研究体系                                            | ⭐⭐⭐⭐⭐ |
| **2026** | **Automated reanalysis of genomic data for rare disease diagnostics at scale**                                                                                          | **Nature Medicine**       |                        4,735 + 1,089 | Talos 自动 reanalysis                                       | ⭐⭐⭐⭐⭐ |
| **2023** | **Integrated multi-omics for rapid rare disease diagnosis on a national scale**                                                                                         | **Nature Medicine**       |                         290 families | WGS + RNA + long-read + proteomics                        | ⭐⭐⭐⭐⭐ |
| **2023** | **Direct haplotype-resolved 5-base HiFi sequencing for genome-wide profiling of hypermethylation outliers in a rare disease cohort**                                    | **Nature Communications** |                    ~1,000 families背景 | PacBio HiFi + methylation + rare disease                  | ⭐⭐⭐⭐⭐ |
| **2025** | **Long read sequencing enhances pathogenic and novel variation discovery in patients with rare diseases**                                                               | **Nature Communications** | 76 positive controls + 51 unresolved | long-read clinical genomics                               | ⭐⭐⭐⭐  |
| **2024** | **Complex trait associations in rare diseases and impacts on Mendelian variant interpretation**                                                                         | **Nature Communications** |                       3,059 probands | rare disease + PGS + penetrance                           | ⭐⭐⭐⭐  |
| **2024** | **Genetic modifiers of rare variants in monogenic developmental disorder loci**                                                                                         | **Nature Genetics**       |                           UK Biobank | rare variant × common variant modifier                    | ⭐⭐⭐⭐⭐ |
| **2024** | **Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings** | **Nature Genetics**       |      5,652 enrolled / 1,577 analyzed | phenotype + sequencing + MDT                              | ⭐⭐⭐⭐  |
| **2024** | **Examining the role of common variants in rare neurodevelopmental conditions**                                                                                         | **Nature**                |                DDD 7,955 + GEL 3,618 | common variant architecture + rare NDD                    | ⭐⭐⭐⭐⭐ |
| **2025** | **Joint, multifaceted genomic analysis enables diagnosis of diverse, ultra-rare monogenic presentations**                                                               | **Nature Communications** |                             UDN 等多队列 | 跨 cohort 联合分析                                             | ⭐⭐⭐⭐  |

下面逐篇展开。

---

# 二、100,000 Genomes Project：目前最值得研究的罕见病大型队列

## 1. Rare disease gene association discovery in the 100,000 Genomes Project

**Nature, 2025**

这是我认为你应该**重点精读**的一篇。

[Nature 原文](https://www.nature.com/articles/s41586-025-08623-w?utm_source=chatgpt.com)

核心规模：

* 34,851 个 probands/families
* 72,690 genomes
* 226 个 rare diseases
* 4,643,230 个 rare candidate variants

最重要的是它不是简单做 variant filtering，而是建立了：

> **gene-based rare variant burden framework → Mendelian disease association**

最终：

* 165 个已知 disease-gene associations
* **141 个新的潜在 disease-gene associations**
* FDR 0.5%
* 69 个进入重点人工/临床 triage

而且文章把方法整理成了 **geneBurdenRD**。

这篇对你以后做：

**Singleton-WES/WGS → rare variant burden → disease gene discovery**

非常有参考价值。([Nature][1])

---

# 三、77,539 genomes：Rareservoir + BeviMed

## 2. Genetic association analysis of 77,539 genomes reveals rare disease etiologies

**Nature Medicine, 2023**

[Nature Medicine 原文](https://www.nature.com/articles/s41591-023-02211-z?utm_source=chatgpt.com)

这是 100KGP 非常经典的一篇。

规模：

* **77,539 individuals**
* 29,741 probands
* 269 disease classes
* 11.9 million rare exonic/splicing variants

方法：

```text
100KGP
   ↓
Rare variants
   ↓
Rareservoir
   ↓
HPO / disease classification
   ↓
BeviMed
   ↓
gene–disease association
   ↓
experimental / pedigree validation
```

最终：

* 241 已知 association
* **19 个新的 association**

特别值得注意的是，它解决了一个非常重要的问题：

> 大型罕见病队列中，如何把 genotype + phenotype 有效组织起来进行统计关联？

所以如果你想做**大型 singleton-WES rare disease burden**，这篇和 2025 Nature 基本应该连着看。([Nature][2])

---

# 四、Solve-RD：欧洲大型罕见病队列

## 3. Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses

**Nature Medicine, 2025**

[Nature Medicine 原文](https://www.nature.com/articles/s41591-024-03420-w?utm_source=chatgpt.com)

这篇非常重要。

规模：

* **6,004 families**
* 9,645 individuals
* 6,447 affected individuals
* 37 expert centers
* 12 个欧洲国家 + Canada
* > 300 collaborators

数据：

```text
WES
WGS
Pedigree
HPO
Clinical information
        ↓
Systematic reanalysis
        ↓
SNV/Indel
CNV/SV
other variant types
        ↓
Expert review
```

结果：

* 506 families 获得新的 genetic diagnosis
* 552 disease-causing variants
* 67 个涉及新 disease genes
* 187 个来自 ClinVar 新证据
* 210 个来自专家重新分类
* 总体 ad hoc + systematic diagnostic yield 达 **12.6%**

尤其值得关注的是：

> **15.9% 的诊断来自非 SNV/short indel。**

也就是：

* CNV
* SV
* 复杂变异
* 其他非标准 variant

这对你做 **SV / repeat / long-read** 特别有意义。([Nature][3])

---

# 五、GA4K：PacBio HiFi 罕见病队列

这个方向与你目前的 long-read 研究非常接近。

## 4. Pangenome graphs improve the analysis of structural variants in rare genetic diseases

**Nature Communications, 2024**

[Nature Communications 原文](https://www.nature.com/articles/s41467-024-44980-2?utm_source=chatgpt.com)

GA4K：

* **574 assemblies**
* 287 parent-offspring trios
* PacBio HiFi
* 平均约 27×

核心：

```text
PacBio HiFi
     ↓
personal assemblies
     ↓
SV discovery
     ↓
94 public assemblies
     ↓
pangenome graph
     ↓
unified SV callset
     ↓
rare/pathogenic SV prioritization
```

尤其值得注意：

> 这个研究直接把 **rare disease + PacBio HiFi + pangenome + SV** 结合起来。

如果你未来考虑：

**Singleton-WGS / HiFi → SV → rare disease association**

这篇非常值得作为方法学参考。([Nature][4])

---

# 六、GA4K：>1000 pediatric rare disease genomes

## 5. Genomic answers for children: Dynamic analyses of >1000 pediatric rare disease genomes

**Genetics in Medicine, 2022**

严格来说它刚好在“近四年”边缘，但**强烈建议保留**。

规模：

* 960 families
* > 1,000 genomes
* pediatric rare disease

用了：

* WES
* short-read WGS
* PacBio HiFi
* SNV
* SV
* repeat variants
* machine learning
* phenotype
* pedigree

最值得注意的结果：

> HiFi-GS 对 rare coding SV 的发现率比 short-read GS 高 **4 倍以上**。

而 SV 为此前未诊断病例增加了最高约 **13%** 的诊断。([科学直通车][5])

---

# 七、GA4K：rare disease + polygenic background

## 6. Complex trait associations in rare diseases and impacts on Mendelian variant interpretation

**Nature Communications, 2024**

[Nature Communications 原文](https://www.nature.com/articles/s41467-024-52407-1?utm_source=chatgpt.com)

规模：

* **3,059 GA4K probands**
* 1,102 PGS

核心思想非常值得关注：

```text
Rare pathogenic/VUS
        +
Polygenic background
        ↓
Phenotypic severity
        ↓
Penetrance / expressivity
```

发现：

* rare disease patients 的 common-disease polygenic liability 并不是随机的
* PGS 可以解释一部分 variable penetrance
* rare variants + polygenic background 可能共同影响 phenotype

这其实非常接近未来罕见病研究的一个重要趋势：

> **从“单个致病变异”转向“rare variant + common variant background”。**

([Nature][6])

---

# 八、DDD + 100KGP：rare disease 中 common variant 的作用

## 7. Examining the role of common variants in rare neurodevelopmental conditions

**Nature, 2024**

[Nature 原文](https://www.nature.com/articles/s41586-024-08217-y?utm_source=chatgpt.com)

用了：

* DDD：**7,955 patients**
* Genomics England：**3,618 patients**

研究 rare neurodevelopmental disorders 中：

> common variants 是否也参与疾病风险？

非常值得注意，因为传统 Mendelian disease analysis 通常默认：

```text
pathogenic variant
        ↓
disease
```

而这篇实际上在推动：

```text
rare pathogenic variant
        +
common polygenic background
        ↓
disease liability
```

这个方向和 2024 Nature Genetics 的 genetic modifiers 那篇可以配套阅读。([Nature][7])

---

# 九、Nature Genetics：rare variant × common variant modifier

## 8. Genetic modifiers of rare variants in monogenic developmental disorder loci

**Nature Genetics, 2024**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01710-0?utm_source=chatgpt.com)

这是非常值得做方法参考的一篇。

研究：

* UK Biobank
* 599 dominant developmental-disorder genes
* 2–5 个 rare damaging variants 的 cumulative burden
* EA polygenic score

主要结论：

> rare variant burden 与 common polygenic score 可以共同决定 phenotype severity。

也就是说：

**Mendelian disease ≠ 完全由一个变异决定。**

而可能是：

```text
Rare pathogenic variant
        ↓
baseline risk

+

Common variants / PGS
        ↓
modifier

=

实际 phenotype
```

([Nature][8])

---

# 十、748,879 人：跨 biobank + 多祖源 rare variant burden

## 9. Rare coding variant analysis for human diseases across biobanks and ancestries

**Nature Genetics, 2024**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01894-5?utm_source=chatgpt.com)

这个规模非常大：

> **748,879 individuals**

包括：

* All of Us
* 多个大型 biobank
* 155,236 非 European ancestry

研究：

* 601 diseases
* gene-based rare variant tests
* 363 significant associations

这个对于你如果考虑：

> **rare variant burden analysis + 多人群**

非常值得学习。

尤其是文章强调：

**pan-ancestry burden testing** 可以提高 inclusive discovery，但 ancestry-specific sensitivity analysis 仍然非常重要。([Nature][9])

---

# 十一、gnomAD：80万人的罕见病 penetrance

## 10. Exploring penetrance of clinically relevant variants in over 800,000 humans from the Genome Aggregation Database

**Nature Communications, 2025**

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-61698-x?utm_source=chatgpt.com)

规模：

> **807,162 individuals**

研究：

* ClinVar pathogenic variants
* 734 predicted LoF variants
* 77 severe early-onset haploinsufficient disease genes

最有意思的是：

734 个看起来应该导致疾病的 LoF variants 中：

> **701/734（95%）可以找到解释其“表型缺失”的原因。**

这篇非常适合研究：

* penetrance
* phenotypic expansion
* incidental pathogenic variants
* variant interpretation
* healthy carriers

([Nature][10])

---

# 十二、GREGoR：美国大型罕见病研究体系

## 11. GREGoR: accelerating genomics for rare diseases

**Nature, 2025**

[Nature 原文](https://www.nature.com/articles/s41586-025-09613-8?utm_source=chatgpt.com)

GREGoR 本身不是单篇“发现一个 gene”的研究，而是一个非常重要的**大型罕见病研究基础设施**。

它整合：

* WES
* WGS
* long-read WGS
* RNA-seq
* family structure
* phenotype
* genetic findings

目前数据持续更新，已经有多个 release。([GREGoR Consortium][11])

如果你想找：

> **未来可以用于 rare variant / gene discovery / variant prioritization 的大型公共队列**

GREGoR 非常值得关注。

---

# 十三、Nature Medicine 2026：自动化 reanalysis

## 12. Automated reanalysis of genomic data for rare disease diagnostics at scale

**Nature Medicine, 2026**

这是目前非常新的，而且我认为**方法学价值极高**的一篇。

[Nature Medicine 原文](https://www.nature.com/articles/s41591-026-04477-5?utm_source=chatgpt.com)

工具：

> **Talos**

数据：

* 1,089 individuals 用于 validation
* **4,735 previously undiagnosed individuals** 用于实际应用

结果：

> 新增 241 diagnoses，额外 diagnostic yield **5.1%**

其中：

* 32%：new gene–disease relationships
* 22%：new variant-level evidence
* 45%：improved analysis strategies

这个研究说明未来罕见病诊断非常可能变成：

```text
第一次 WES/WGS
       ↓
initial diagnosis
       ↓
每月/定期自动 reanalysis
       ↓
新 gene
新 ClinVar evidence
新 phenotype
新 annotation
新 variant caller
       ↓
不断提高 diagnostic yield
```

([Nature][12])

---

# 十四、全国规模多组学罕见病诊断

## 13. Integrated multi-omics for rapid rare disease diagnosis on a national scale

**Nature Medicine, 2023**

[Nature Medicine 原文](https://www.nature.com/articles/s41591-023-02401-9?utm_source=chatgpt.com)

澳大利亚 Acute Care Genomics：

* **290 families**
* critically ill infants/children

整合：

```text
WGS
 ↓
RNA-seq
 ↓
Long-read
 ↓
enzyme assays
 ↓
proteomics
 ↓
diagnosis
```

结果：

* 初始 WGS diagnostic yield：47%
* 多组学后：**54%**
* 额外发现 19 diagnoses
* 120 patients 的 clinical management 被改变

这篇是**rare disease + multi-omics clinical diagnosis**非常好的代表。([Nature][13])

---

# 十五、PacBio HiFi + methylation：非常值得你关注

## 14. Direct haplotype-resolved 5-base HiFi sequencing for genome-wide profiling of hypermethylation outliers in a rare disease cohort

**Nature Communications, 2023**

[Nature Communications 原文](https://www.nature.com/articles/s41467-023-38782-1?utm_source=chatgpt.com)

这个和你现在做 PacBio/long-read 的技术路线非常接近。

GA4K 约 **1,000 families** 的背景数据。

核心：

> PacBio HiFi 不仅用于 SNV/SV，还可以同时研究 methylation。

文章报告：

* HiFi-GS
* haplotype
* methylation
* rare disease
* hypermethylation outlier

特别适合你如果考虑：

```text
HiFi
 ↓
SNV
SV
CNV
repeat
methylation
phasing
 ↓
rare disease mechanism
```

([Nature][14])

---

# 十六、Long-read clinical rare disease

## 15. Long read sequencing enhances pathogenic and novel variation discovery in patients with rare diseases

**Nature Communications, 2025**

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-57695-9?utm_source=chatgpt.com)

核心：

* 76 positive controls
* 57 methylation-positive samples
* 51 previously short-read-negative patients

在 previously negative cases 中：

> long-read 又发现约 **10% additional diagnoses**。

检测范围：

* SNV
* SV
* methylation
* complex variation

所以这篇非常适合放在：

**short-read → long-read clinical genome sequencing**

这一条文献链里面。([Nature][15])

---

# 十七、超罕见病跨队列联合分析

## 16. Joint, multifaceted genomic analysis enables diagnosis of diverse, ultra-rare monogenic presentations

**Nature Communications, 2025**

[Nature Communications 原文](https://www.nature.com/articles/s41467-025-61712-2?utm_source=chatgpt.com)

核心是：

> 把不同 rare disease cohorts 的数据联合起来。

包括 UDN 等。

这类研究特别重要，因为真正的 Mendelian disease 往往：

```text
某个疾病
↓
1–5 个病例
```

单个 cohort 根本没有统计 power。

因此未来越来越多的是：

```text
UK 100K
+
GA4K
+
GREGoR
+
UDN
+
DDD
+
国际 clinical cohorts
        ↓
cross-cohort analysis
        ↓
gene discovery
```

([Nature][16])

---

# 十八、超罕见病 + next-generation phenotyping

## 17. Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings

**Nature Genetics, 2024**

[Nature Genetics 原文](https://www.nature.com/articles/s41588-024-01836-1?utm_source=chatgpt.com)

德国 TRANSLATE NAMSE：

* 5,652 individuals enrolled
* 1,577 进入 exome analysis

重点：

**NGS + HPO + deep phenotyping + multidisciplinary team**

尤其适合研究：

> phenotype-driven gene discovery

([Nature][17])

---

# 十九、一些“不是典型罕见病 cohort，但非常值得纳入”的大型研究

如果你想把范围扩大到：

> **大型 population biobank 中研究 rare disease / rare variants / Mendelian phenotypes**

那么还有一大批。

---

## 18. Whole-genome sequencing of 490,640 UK Biobank participants

**Nature, 2025**

[Nature 原文](https://www.nature.com/articles/s41586-025-09272-9?utm_source=chatgpt.com)

规模：

> **490,640 WGS**

它不是专门的 rare disease cohort，但对于：

* rare variant frequency
* population controls
* penetrance
* rare disease variant filtering
* structural variants

非常重要。([Nature][18])

---

## 19. Whole-genome sequencing analysis identifies rare, large-effect noncoding variants and regulatory regions associated with circulating protein levels

**Nature Genetics, 2025**

规模：

> ~50,000 UK Biobank WGS

分析：

* 1.1 billion variants
* 123 million aggregate tests
* 2,907 proteins

重点是：

> rare **noncoding variants**

所以对于罕见病未来的：

```text
coding
+
noncoding
+
regulatory
```

分析非常值得参考。([Nature][19])

---

## 20. The impact of rare protein coding genetic variation on adult cognitive function

**Nature Genetics, 2023**

规模：

> **485,930 individuals**

虽然不是罕见病患者队列，但使用 rare coding variants 研究人类表型。

发现 8 个重要 genes。

对于：

> rare coding variant burden → phenotype

这个统计框架值得学习。([Nature][20])

---

# 二十、All of Us：未来很重要的 rare disease population resource

## 21. Genomic data in the All of Us Research Program

**Nature, 2024**

[Nature 原文](https://www.nature.com/articles/s41586-023-06957-x?utm_source=chatgpt.com)

All of Us 的优势不是单纯样本量，而是：

```text
Genomics
+
EHR
+
Phenotype
+
Survey
+
Diverse ancestry
```

尤其适合：

> phenotype-first / genotype-first rare disease research

而且它目前已经包括：

* 数十万 WGS
* structural variants
* long-read WGS
* EHR

所以未来几年 All of Us 很可能产生越来越多的 rare disease genotype-first 研究。([Nature][21])

---

# 二十一、另外几个值得放进你的文献库

### 22. Public platform with 39,472 exome control samples enables association studies without genotype sharing

**Nature Genetics, 2024**

这个不是患者 cohort，但对于：

> rare variant burden / case-control

非常有方法学意义。

它提供 **39,472 exome controls**，解决不同机构做 rare variant association 时 control 不足的问题。([Nature][22])

---

### 23. Pangenome graphs improve the analysis of structural variants in rare genetic diseases

**Nature Communications, 2024**

这个前面已经介绍，是：

> **GA4K + HiFi + pangenome + SV**

方向非常值得你重点参考。([Nature][4])

---

### 24. The expanding diagnostic toolbox for rare genetic diseases

**Nature Reviews Genetics, 2024**

这篇不是 cohort research，而是一篇很好的综述。

重点总结：

* WES
* WGS
* long-read
* OGM
* transcriptome
* epigenome
* proteome
* metabolome

([Nature][23])

如果你想搭建一个完整的 rare disease research framework，这篇适合作为综述入口。

---

# 二十二、把这些论文按“研究路线”重新分类

如果你的真正目的是找**可以模仿的大型罕见病队列研究模式**，我建议不要单纯按照期刊分类，而按照研究问题分类。

## A. 最大型：Rare disease gene discovery

### ★★★★★

**100KGP**

```text
77,539 genomes
      ↓
Rareservoir
      ↓
BeviMed
      ↓
gene-disease association
```

2023 Nature Medicine。([Nature][2])

↓

**100KGP**

```text
72,690 genomes
      ↓
rare variant filtering
      ↓
geneBurdenRD
      ↓
burden test
      ↓
141 new associations
```

2025 Nature。([Nature][1])

这两篇应该**一起读**。

---

# 二十三、B. Rare disease + long-read / SV

这是与你目前研究方向最接近的。

推荐顺序：

### ① GA4K >1000 genomes

**Genomic answers for children**
Genetics in Medicine 2022。([科学直通车][5])

↓

### ② GA4K HiFi + methylation

**Direct haplotype-resolved 5-base HiFi sequencing...**
Nature Communications 2023。([Nature][14])

↓

### ③ GA4K pangenome + SV

**Pangenome graphs improve the analysis of structural variants...**
Nature Communications 2024。([Nature][4])

↓

### ④ clinical long-read

**Long read sequencing enhances pathogenic and novel variation discovery...**
Nature Communications 2025。([Nature][15])

↓

### ⑤ clinical LR-WGS framework

**Toward clinical long-read genome sequencing for rare diseases**
Nature Genetics 2025。([Nature][24])

这一条线非常完整。

---

# 二十四、C. Rare disease + phenotype

重点：

**Solve-RD**

```text
6,004 families
      +
HPO
      +
pedigree
      +
WES/WGS
      ↓
systematic reanalysis
      ↓
506 diagnoses
```

Nature Medicine 2025。([Nature][3])

以及：

**TRANSLATE NAMSE**

```text
5,652 patients
      ↓
deep phenotyping
      +
WES
      ↓
ultra-rare disease diagnosis
```

Nature Genetics 2024。([Nature][17])

---

# 二十五、D. Rare variant burden

如果你真正想找的是：

> **“大型队列如何做 rare variant burden analysis？”**

我建议优先看：

### 第一梯队

1. **Rare disease gene association discovery in the 100,000 Genomes Project** — Nature 2025
2. **Genetic association analysis of 77,539 genomes...** — Nature Medicine 2023
3. **Rare coding variant analysis for human diseases across biobanks and ancestries** — Nature Genetics 2024
4. **Genetic modifiers of rare variants in monogenic developmental disorder loci** — Nature Genetics 2024
5. **The impact of rare protein coding genetic variation on adult cognitive function** — Nature Genetics 2023

---

# 二十六、E. Rare disease + penetrance / variable expressivity

这个方向近几年非常火。

推荐：

1. **Exploring penetrance of clinically relevant variants in over 800,000 humans from gnomAD** — Nature Communications 2025
2. **Complex trait associations in rare diseases and impacts on Mendelian variant interpretation** — Nature Communications 2024
3. **Genetic modifiers of rare variants in monogenic developmental disorder loci** — Nature Genetics 2024
4. **Examining the role of common variants in rare neurodevelopmental conditions** — Nature 2024

它们共同指向一个新的模型：

```text
Pathogenic rare variant
          │
          ├── common variant burden
          │
          ├── genetic modifiers
          │
          ├── environment
          │
          └── epigenetic state
                    ↓
              penetrance
                    ↓
               phenotype
```

这个方向我认为比传统的“找一个 pathogenic variant”更有研究空间。

---

# 二十七、F. Rare disease + 自动化诊断

近两年明显开始从：

> “能不能发现更多 variant？”

转向：

> **“如何让整个 rare disease cohort 自动不断 reanalyze？”**

核心文章：

### 2026 Nature Medicine

**Automated reanalysis of genomic data for rare disease diagnostics at scale**

Talos：

```text
Genome
  ↓
variant prioritization
  ↓
gene-disease evidence
  ↓
inheritance
  ↓
HPO
  ↓
clinical evidence
  ↓
automatic reanalysis
  ↓
new diagnosis
```

4,735 undiagnosed individuals → 241 new diagnoses。([Nature][12])

这个方向很可能是接下来几年非常重要的热点。

---

# 二十八、如果按“队列规模”排名

粗略按照规模，可以分成：

### 10万级

| Cohort      |          规模 | 主要用途                           |
| ----------- | ----------: | ------------------------------ |
| 100KGP      |     ~77,539 | rare disease gene discovery    |
| UK Biobank  |   ~500k WGS | rare variant / controls        |
| gnomAD      |       ~807k | variant frequency / penetrance |
| All of Us   |     数十万 WGS | diverse ancestry / phenotype   |
| 三大 biobanks | **748,879** | rare coding burden             |

### 1万级

| Cohort                       |                        规模 |
| ---------------------------- | ------------------------: |
| Solve-RD                     |                     9,645 |
| DDD                          |                     7,955 |
| 100KGP rare disease analysis |  34,851 probands/families |
| GREGoR                       |                   多中心持续扩展 |
| GA4K                         | >1,000 pediatric families |

### 千级

| Cohort              |             规模 |
| ------------------- | -------------: |
| GA4K                |         >1,000 |
| TRANSLATE NAMSE     | 5,652 enrolled |
| RGP                 |   688 families |
| UDN                 |             数百 |
| Acute Care Genomics |   290 families |

---

# 二十九、如果你是为了设计自己的“大型罕见病队列研究”，我最推荐借鉴的模式

实际上可以把这些文章总结成 **5 种“大队列罕见病研究范式”**：

### 模式 1：Phenotype → Gene

```text
Large RD cohort
      ↓
HPO
      ↓
rare variants
      ↓
gene burden
      ↓
new disease genes
```

代表：

**100KGP 2023/2025**

---

### 模式 2：Genome → SV → Disease

```text
HiFi WGS
   ↓
SNV
SV
CNV
STR
MEI
   ↓
pangenome
   ↓
rare/pathogenic variants
   ↓
diagnosis
```

代表：

**GA4K 2022–2025**

这个与你目前的 PacBio HiFi 工作**最接近**。

---

### 模式 3：Rare variant → Modifier → Phenotype

```text
rare pathogenic variant
          +
common variant PGS
          +
genetic modifiers
          ↓
penetrance
          ↓
clinical phenotype
```

代表：

**Nature 2024 + Nature Genetics 2024 + Nat Commun 2024**

---

### 模式 4：Multi-omics diagnosis

```text
WGS
 +
RNA-seq
 +
methylation
 +
proteomics
 +
metabolomics
       ↓
variant interpretation
       ↓
diagnosis
```

代表：

**Nature Medicine 2023**

---

### 模式 5：持续自动 reanalysis

```text
WES/WGS
   ↓
initial analysis
   ↓
database update
   ↓
new gene
new variant
new phenotype
   ↓
automatic reanalysis
   ↓
new diagnosis
```

代表：

**Nature Medicine 2026 Talos**

---

## 三十、如果结合你现在的研究方向，我会重点推荐这条文献链

你现在如果想做的是**大规模 Singleton-WES / WGS 罕见病致病变异发现**，而且希望以后进一步加入 SV、repeat、AI/phenotype，那么我建议优先建立下面这套文献框架：

```text
                   大型 Rare Disease Cohort
                             │
        ┌────────────────────┼────────────────────┐
        ↓                    ↓                    ↓
   100KGP                 GA4K                 Solve-RD
        │                    │                    │
        ↓                    ↓                    ↓
  Rare burden            HiFi / SV             HPO
        │                    │                    │
        ↓                    ↓                    ↓
 Gene discovery         Pangenome              Reanalysis
        │                    │                    │
        └──────────────┬─────┴────────────────────┘
                       ↓
                Multimodal diagnosis
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
     SNV/SV           STR/TE          Methylation
       ↓               ↓                ↓
       └───────────────┼────────────────┘
                       ↓
             Rare disease diagnosis
                       ↓
             Penetrance / modifiers
                       ↓
               Clinical phenotype
```

其中**最值得你精读的 8 篇**是：

1. **Nature 2025 — Rare disease gene association discovery in the 100,000 Genomes Project** ([Nature][1])
2. **Nature Medicine 2023 — Genetic association analysis of 77,539 genomes reveals rare disease etiologies** ([Nature][2])
3. **Nature Medicine 2025 — Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses** ([Nature][3])
4. **Nature Genetics 2024 — Rare coding variant analysis for human diseases across biobanks and ancestries** ([Nature][9])
5. **Nature Communications 2024 — Pangenome graphs improve the analysis of structural variants in rare genetic diseases** ([Nature][4])
6. **Nature 2024 — Examining the role of common variants in rare neurodevelopmental conditions** ([Nature][7])
7. **Nature Communications 2025 — Exploring penetrance of clinically relevant variants in over 800,000 humans from gnomAD** ([Nature][10])
8. **Nature Medicine 2026 — Automated reanalysis of genomic data for rare disease diagnostics at scale** ([Nature][12])

**尤其是第 1 + 2 篇**，非常适合你关注的 **rare variant burden / gene-level association**；**第 4 + 5 篇**适合 **large cohort + SV/long-read**；**第 6 + 7 篇**则是进一步研究 **penetrance / phenotype modifiers**。

如果你的目的是**“找可以直接照着设计自己文章分析框架的文献”**，下一步我可以进一步给你整理成一张更实用的 **“近四年罕见病大型队列 30–50 篇论文表”**，增加 **队列名称、样本量、WES/WGS/HiFi、singleton/trio、SNV/SV/CNV/STR、HPO、rare burden方法、统计模型、是否有新基因、是否有代码/数据、期刊和 DOI**，这样基本可以直接作为你做课题/写基金的 literature matrix。

[1]: https://www.nature.com/articles/s41586-025-08623-w?utm_source=chatgpt.com "Rare disease gene association discovery in the 100,000 Genomes Project | Nature"
[2]: https://www.nature.com/articles/s41591-023-02211-z?utm_source=chatgpt.com "Genetic association analysis of 77,539 genomes reveals rare disease etiologies | Nature Medicine"
[3]: https://www.nature.com/articles/s41591-024-03420-w?utm_source=chatgpt.com "Genomic reanalysis of a pan-European rare-disease resource yields new diagnoses | Nature Medicine"
[4]: https://www.nature.com/articles/s41467-024-44980-2?utm_source=chatgpt.com "Pangenome graphs improve the analysis of structural variants in rare genetic diseases | Nature Communications"
[5]: https://www.sciencedirect.com/science/article/pii/S1098360022006530?utm_source=chatgpt.com "Genomic answers for children: Dynamic analyses of >1000 pediatric rare disease genomes - ScienceDirect"
[6]: https://www.nature.com/articles/s41467-024-52407-1?utm_source=chatgpt.com "Complex trait associations in rare diseases and impacts on Mendelian variant interpretation | Nature Communications"
[7]: https://www.nature.com/articles/s41586-024-08217-y?utm_source=chatgpt.com "Examining the role of common variants in rare neurodevelopmental conditions | Nature"
[8]: https://www.nature.com/articles/s41588-024-01710-0?utm_source=chatgpt.com "Genetic modifiers of rare variants in monogenic developmental disorder loci | Nature Genetics"
[9]: https://www.nature.com/articles/s41588-024-01894-5?utm_source=chatgpt.com "Rare coding variant analysis for human diseases across biobanks and ancestries | Nature Genetics"
[10]: https://www.nature.com/articles/s41467-025-61698-x?utm_source=chatgpt.com "Exploring penetrance of clinically relevant variants in over 800,000 humans from the Genome Aggregation Database | Nature Communications"
[11]: https://gregorconsortium.org/data?utm_source=chatgpt.com "GREGoR Data for the Scientific Community | GREGoR Consortium"
[12]: https://www.nature.com/articles/s41591-026-04477-5?utm_source=chatgpt.com "Automated reanalysis of genomic data for rare disease diagnostics at scale | Nature Medicine"
[13]: https://www.nature.com/articles/s41591-023-02401-9?utm_source=chatgpt.com "Integrated multi-omics for rapid rare disease diagnosis on a national scale | Nature Medicine"
[14]: https://www.nature.com/articles/s41467-023-38782-1?utm_source=chatgpt.com "Direct haplotype-resolved 5-base HiFi sequencing for genome-wide profiling of hypermethylation outliers in a rare disease cohort | Nature Communications"
[15]: https://www.nature.com/articles/s41467-025-57695-9?utm_source=chatgpt.com "Long read sequencing enhances pathogenic and novel variation discovery in patients with rare diseases | Nature Communications"
[16]: https://www.nature.com/articles/s41467-025-61712-2?utm_source=chatgpt.com "Joint, multifaceted genomic analysis enables diagnosis of diverse, ultra-rare monogenic presentations | Nature Communications"
[17]: https://www.nature.com/articles/s41588-024-01836-1?utm_source=chatgpt.com "Next-generation phenotyping integrated in a national framework for patients with ultrarare disorders improves genetic diagnostics and yields new molecular findings | Nature Genetics"
[18]: https://www.nature.com/articles/s41586-025-09272-9?utm_source=chatgpt.com "Whole-genome sequencing of 490,640 UK Biobank participants | Nature"
[19]: https://www.nature.com/articles/s41588-025-02095-4?utm_source=chatgpt.com "Whole-genome sequencing analysis identifies rare, large-effect noncoding variants and regulatory regions associated with circulating protein levels | Nature Genetics"
[20]: https://www.nature.com/articles/s41588-023-01398-8?utm_source=chatgpt.com "The impact of rare protein coding genetic variation on adult cognitive function | Nature Genetics"
[21]: https://www.nature.com/articles/s41586-023-06957-x?utm_source=chatgpt.com "Genomic data in the All of Us Research Program | Nature"
[22]: https://www.nature.com/articles/s41588-023-01637-y?utm_source=chatgpt.com "Public platform with 39,472 exome control samples enables association studies without genotype sharing | Nature Genetics"
[23]: https://www.nature.com/articles/s41576-023-00683-w?utm_source=chatgpt.com "The expanding diagnostic toolbox for rare genetic diseases | Nature Reviews Genetics"
[24]: https://www.nature.com/articles/s41588-025-02160-y?utm_source=chatgpt.com "Toward clinical long-read genome sequencing for rare diseases | Nature Genetics"


# 基因型和表型关系构建工具


在罕见病队列里，**“基因型 + 表型联合”真正有价值的地方，不是简单把 HPO 当作注释列，而是把表型作为一个概率或相似度证据，与变异的致病性、频率、遗传模式、家系共分离等信息一起进入候选基因/候选变异排序模型**。

目前做得比较成熟、在大型罕见病项目里也比较常用的方法，基本可以分成 5 类：**表型相似度打分、表型驱动基因排序、表型+变异联合排序、贝叶斯/似然模型、AI/NLP 自动表型提取与联合推断**。

我比较推荐你优先关注下面这些工具。

| 工具             | 输入                                  |   是否直接联合 VCF | 核心思想                                                       | 适合场景                   | 推荐度   |
| -------------- | ----------------------------------- | -----------: | ---------------------------------------------------------- | ---------------------- | ----- |
| **Exomiser**   | VCF + PED + HPO                     |            是 | variant pathogenicity + phenotype similarity + inheritance | WES/WGS、trio/singleton | ★★★★★ |
| **Genomiser**  | WGS VCF + HPO                       |            是 | Exomiser + noncoding regulatory variants                   | WGS、非编码变异              | ★★★★★ |
| **LIRICAL**    | HPO + disease/genotype信息            |           部分 | likelihood ratio / Bayesian-style phenotype matching       | 鉴别诊断、疾病排序              | ★★★★★ |
| **Phen2Gene**  | HPO                                 |            否 | phenotype → gene probability/ranking                       | 大队列预筛选                 | ★★★★  |
| **PhenIX**     | VCF + HPO                           |            是 | phenotype similarity + variant filtering                   | Mendelian disease      | ★★★★  |
| **hiPHIVE**    | HPO + gene/model-organism phenotype | 内置于 Exomiser | 人+鼠+鱼+PPI phenotype propagation                            | 新基因发现                  | ★★★★★ |
| **Phenomizer** | HPO                                 |            否 | semantic similarity                                        | differential diagnosis | ★★★★  |
| **AMELIE**     | HPO + candidate genes               |           间接 | NLP 文献挖掘 + phenotype-gene evidence                         | 新基因/VUS解释              | ★★★★  |
| **DeepPVP**    | VCF + HPO                           |            是 | deep learning + phenotype similarity                       | 研究型 prioritization     | ★★★   |
| **Phevor**     | phenotype ontology + gene           |           间接 | ontology propagation                                       | 新基因候选                  | ★★★   |
| **Phenolyzer** | phenotype terms                     |            否 | disease/gene/network integration                           | gene prioritization    | ★★★   |
| **AI-MARRVEL** | variant/gene + phenotype            |         是/部分 | AI + phenotype/genomic evidence                            | 临床候选筛选                 | ★★★★  |

其中如果你是要做**大型罕见病队列的标准分析流程**，我会优先推荐：

> **HPO标准化 → Exomiser → LIRICAL/Phen2Gene辅助 → ACMG/ClinGen → 未解决病例 Genomiser → 定期 reanalysis**

这是目前比较稳妥的一条路线。

---

## 1. Exomiser：目前最值得优先使用

如果你的输入是：

```text
WES/WGS VCF
+
HPO
+
PED
```

那么我认为 **Exomiser 是首选之一**。

它不是单纯看 HPO，也不是单纯看 pathogenicity，而是大致构建：

```text
Variant evidence
│
├─ AF
├─ consequence
├─ REVEL
├─ MVP
├─ AlphaMissense
├─ SpliceAI
└─ ClinVar

        +

Inheritance
│
├─ AD
├─ AR
├─ X-linked
├─ de novo
└─ compound het

        +

Phenotype
│
├─ Patient HPO
├─ Disease HPO
├─ Human gene phenotype
├─ Mouse phenotype
└─ Zebrafish phenotype

        ↓

Final gene / variant score
        ↓
Candidate ranking
```

官方定位就是从 **VCF + HPO** 中寻找最可能的致病变异。([GitHub][1])

而且大型队列已经证明这种策略很有效。在 100,000 Genomes Project 的已诊断病例中，Exomiser 可以把相当大比例的真实诊断排到前几个候选中；一项对 4,877 个诊断病例的分析中，真实诊断位于 Top 1、Top 3、Top 5、Top 10 的比例分别约为 **82.6%、91.3%、92.4%、93.6%**。([PubMed Central (PMC)][2])

这个结果非常说明问题：

> **表型并不是锦上添花，而是能显著缩小候选范围。**

---

# 2. Exomiser 里面最重要的是 hiPHIVE

Exomiser 的 phenotype component 有几个模式：

```text
PhenIX
PHIVE
hiPHIVE
```

其中 **hiPHIVE** 最值得关注。

它不只是：

```text
patient HPO
vs
known disease HPO
```

还会扩展到：

```text
Human phenotype
+
Mouse phenotype
+
Zebrafish phenotype
+
PPI neighbors
```

所以对于：

> **尚未建立明确 human gene–disease association 的新基因**

也有一定识别能力。

这对于你做**罕见病新基因发现**特别重要。

不过很有意思的是，2025 年一项基于 UDN 已解决病例的系统优化研究发现：在已知临床诊断场景中，**human-only hiPHIVE** 的平均表现反而比默认的人+模型动物模式更好；但模型动物/PPI 模式对于少数新近发现基因仍可能把遗漏候选重新捞回来。([PubMed Central (PMC)][3])

所以我比较推荐：

```text
Primary:
human-only hiPHIVE

Secondary / discovery:
full hiPHIVE
human + mouse + zebrafish + PPI
```

而不是只跑一次。

---

# 3. 2025 年 Exomiser 优化研究非常值得参考

这篇我很建议你看：

**An optimized variant prioritization process for rare disease diagnostics: recommendations for Exomiser and Genomiser**

Genome Medicine，2025。

他们使用了：

> 386 个 UDN 已诊断 probands

系统测试了：

* HPO质量
* HPO数量
* VCF质量
* family information
* inheritance
* pathogenicity predictors
* phenotype model

优化之后：

### WGS

真实 coding diagnostic variant 进入：

> **Top 10：49.7% → 85.5%**

### WES

> **Top 10：67.3% → 88.2%**

提升非常明显。([PubMed Central (PMC)][3])

他们推荐的组合特别值得直接借鉴：

```text
Family VCF
+
PED
+
human-only hiPHIVE
+
REVEL
+
MVP
+
AlphaMissense
+
SpliceAI
+
ClinVar whitelist
```

然后人工 review：

> **Top 30 candidates**

([PubMed Central (PMC)][3])

---

# 4. LIRICAL：我认为第二个应该重点使用

LIRICAL 和 Exomiser 思路不完全一样。

它更像：

> **给定患者表型，这个疾病出现这些表型的概率有多大？**

它不是单纯 semantic similarity，而更接近：

```text
Likelihood Ratio

LR =
P(phenotype | disease)
────────────────────
P(phenotype | not disease)
```

然后多个 phenotype：

```text
HPO1
HPO2
HPO3
HPO4
...
 ↓
Likelihood ratios
 ↓
Combined likelihood
 ↓
Disease ranking
```

这有一个非常大的优点：

> **能够利用“有”和“没有”的表型。**

比如：

```text
Patient:
Developmental delay +
Seizure +
Microcephaly +
Hearing loss -

Disease A:
DD +
Seizure +
Microcephaly +
Hearing loss +

Disease B:
DD +
Seizure +
Microcephaly +
Hearing loss -
```

传统 phenotype overlap 可能两者都很像。

LIRICAL 会认为：

> absence of hearing loss

本身也具有诊断信息。

所以如果你的临床数据里有：

```text
present HPO
absent HPO
```

LIRICAL 非常值得加入。

---

# 5. Phen2Gene：非常适合大规模 cohort

Phen2Gene 更简单，但非常实用。

输入：

```text
HP:0001250
HP:0001263
HP:0000252
...
```

输出：

```text
Gene     phenotype_score
GENE1    0.92
GENE2    0.88
GENE3    0.76
...
```

它构建了一个：

> **HPO → Gene knowledgebase**

然后根据不同 HPO 的信息量进行加权。([PubMed Central (PMC)][4])

最大的优势是：

> **非常快。**

原始 benchmark 的中位运行时间不到 1 秒量级，因此非常适合：

```text
10,000 patients
×
HPO
```

先做 phenotype gene prioritization。

之后再和你的 VCF 结果 intersect：

```text
Phen2Gene Top 1000 genes
        ∩
rare damaging variants
        ↓
candidate genes
```

这对于 **singleton WES** 特别实用。

---

# 6. Phenomizer：适合 disease-level differential diagnosis

Phenomizer 是 HPO 官方体系中非常经典的一类方法。

基本逻辑：

```text
Patient HPO profile
        ↓
Semantic similarity
        ↓
OMIM / disease phenotype
        ↓
Disease ranking
```

所以它更适合回答：

> “这个患者最像什么疾病？”

而不是：

> “VCF 中哪个 variant 最可能致病？”

HPO 官方目前也把：

* Exomiser
* Genomiser
* Phenomizer
* Profile Search

作为 phenotype-driven analysis 工具体系的一部分。([人类表型本体][5])

---

# 7. AMELIE：解决“数据库落后于文献”的问题

这类工具非常有意义。

Exomiser 很依赖：

```text
OMIM
Orphanet
HPO annotations
ClinVar
```

但是罕见病领域经常出现：

```text
2026 January:
new gene-disease paper published

↓
OMIM / HPO 尚未完全更新
```

这时候传统方法可能没有 gene–phenotype association。

AMELIE 的优势是：

> **直接从 biomedical literature 中挖掘 gene–disease–phenotype evidence。**

逻辑类似：

```text
Patient HPO
+
Candidate gene
        ↓
Search millions of papers
        ↓
Gene-disease evidence
+
Phenotype match
        ↓
Candidate ranking
```

所以非常适合：

* novel disease genes
* recent literature
* VUS
* 新表型扩展

我会把它放在：

> **Exomiser Top 30–100 candidate 的二次解释阶段**

而不是作为第一层筛选。

---

# 8. DeepPVP

DeepPVP 是：

> phenotype-based variant prioritization + deep learning

把：

```text
variant pathogenicity
+
phenotype similarity
+
gene knowledge
```

联合起来，通过神经网络预测 causal variant。其论文报告相对于若干早期 phenotype-based 方法有性能提升。([PubMed Central (PMC)][6])

不过如果是大规模真实临床 cohort，我目前还是更推荐：

> Exomiser / Genomiser

而 DeepPVP 更适合作为研究性辅助工具。

---

# 9. Genomiser：WGS 的重要补充

如果你只有 WES：

```text
Exomiser
```

基本已经覆盖主要 coding variant。

但如果你有 WGS，我强烈建议增加：

```text
Genomiser
```

因为它可以分析：

```text
promoter
enhancer
UTR
intronic
regulatory
other noncoding variants
```

Genomiser 在 Exomiser 框架基础上加入了：

> ReMM 等 noncoding pathogenicity evidence。

([PubMed Central (PMC)][3])

特别值得注意的是：

### compound heterozygous

比如：

```text
Gene X

maternal:
coding LoF

paternal:
deep intronic splice variant
```

Exomiser 可能只看到：

```text
一个 coding variant
```

无法构成 AR。

Genomiser 可以识别：

```text
coding
+
noncoding
=
compound heterozygous
```

2025 UDN benchmark 就明确观察到了这种情况。([PubMed Central (PMC)][3])

---

# 10. Hong Kong Genome Project 的实际经验也非常有参考价值

2025 年 Hong Kong Genome Project 做了一个非常现实的大规模 benchmark：

> **985 个 rare disease WGS patients**

其中：

* 207 positive
* 778 negative

对 Genomiser 做参数优化。

优化后，在原本 negative cohort 中又发现了一批 noncoding candidates，并带来额外诊断贡献。([PubMed Central (PMC)][7])

这说明：

> **coding-first + noncoding-second**

目前比一开始把全基因组所有变异同时塞进模型更实际。

---

# 11. HPO 本身的质量非常重要

这里其实比选工具还重要。

一个患者不要简单写：

```text
Developmental delay
Abnormality of nervous system
Seizure
Abnormality of brain
```

这几个高度冗余。

最好是：

```text
Global developmental delay
Generalized tonic-clonic seizure
Microcephaly
Hypotonia
Delayed speech and language development
Abnormal EEG
```

也就是说：

> **specific HPO > very broad HPO**

HPO 本身已经包含超过 18,000 个 phenotype terms，并且与大量遗传病注释相连，是目前 rare disease phenotype 标准化的核心体系。([人类表型本体][5])

---

# 12. 不要只记录 positive phenotype

这是很多队列容易忽视的问题。

推荐记录：

```text
HPO_ID
status
onset
severity
age_at_exam
```

例如：

```text
HP:0001250    present    infancy
HP:0000252    present    congenital
HP:0000407    absent     age=10
```

特别是：

> **明确 absent 的 phenotype**

对于 differential diagnosis 非常有价值。

例如：

```text
Disease A:
ID + seizure + deafness

Disease B:
ID + seizure - deafness
```

如果患者明确：

```text
age 15
normal hearing
```

这是一个非常强的 negative evidence。

---

# 13. 表型还应该考虑年龄依赖

不能简单认为：

```text
没有 phenotype
=
negative phenotype
```

比如一个疾病：

```text
Cataract onset: 30–50 years
```

患者：

```text
3 years old
No cataract
```

这个 absent 基本没有信息量。

因此最好记录：

```text
age of onset
age at last examination
```

这也是 phenotype likelihood 方法比简单 Jaccard similarity 更有优势的原因之一。

---

# 14. 推荐一个实际可落地的“Genotype + Phenotype”评分模型

如果你自己做 cohort，而不完全依赖 Exomiser，我会建议构建：

$$
Score_{variant}
=
w_1G+
w_2P+
w_3I+
w_4S+
w_5C
$$

其中：

### G：Genotype pathogenicity

```text
AF
LoF
REVEL
AlphaMissense
SpliceAI
CADD
ClinVar
```

### P：Phenotype similarity

例如：

```text
Resnik similarity
Lin similarity
Information Content
```

### I：Inheritance

```text
de novo
AR
AD
XL
compound het
```

### S：Segregation

```text
affected carrier
unaffected noncarrier
parental origin
```

### C：Clinical evidence

```text
OMIM
ClinGen
Orphanet
GeneReviews
literature
```

最终：

```text
Variant score
      ×
Phenotype score
      ×
Inheritance probability
      ↓
Final posterior-like score
```

比：

```text
CADD > 20
AND
HPO gene overlap
```

要合理得多。

---

# 15. 如果是你这种大型 singleton-WES 队列，我建议这样设计

如果很多病例没有父母，我不会强行照 trio pipeline 做。

可以：

```text
                 Singleton WES
                      │
              ┌───────┴────────┐
              ↓                ↓
          Genotype          Phenotype
              │                │
       Rare variants          HPO
              │                │
     ┌────────┼──────┐         ↓
     ↓        ↓      ↓      Phen2Gene
    LoF    Missense Splice      │
     │        │      │          │
   pLI     REVEL  SpliceAI     ↓
   LOEUF    AM     dbscSNV  gene score
     │        │      │          │
     └────────┴──────┴────┬─────┘
                          ↓
                       Exomiser
                          ↓
                 phenotype × variant
                          ↓
                  Top candidate genes
                          ↓
              ACMG / ClinGen evaluation
                          ↓
                  Candidate diagnosis
```

这一套非常适合批量跑。

---

# 16. Trio 的时候再加入遗传模型

如果有 trio：

```text
Proband
Father
Mother
```

就可以额外加入：

```text
De novo
Compound heterozygous
Homozygous recessive
X-linked
Dominant inherited
Mosaic
```

这样 phenotype + genotype 的威力会进一步增强。

因为：

```text
variant pathogenicity = 0.8
phenotype match = 0.9
de novo compatible = yes
gene known AD = yes
```

这种证据组合远远强于单纯 pathogenicity prediction。

---

# 17. 对新基因发现，不要把 HPO 筛得太死

这是非常重要的一点。

如果你做的是**诊断**：

```text
known disease-gene
+
phenotype match
```

可以给高权重。

但如果你做：

> novel gene discovery

绝对不要：

```text
if gene not in HPO/OMIM:
    remove
```

因为这样一定找不到新基因。

更好的策略：

```text
Tier 1
known disease gene + strong HPO match

Tier 2
known disease gene + partial phenotype

Tier 3
novel gene
+ pathway/PPI/model organism phenotype support

Tier 4
novel biological candidate
```

这就是 hiPHIVE、Phen2Gene gene network、model organism data 有价值的地方。

---

# 18. 表型相似度建议采用 information-content based 方法

不要简单：

$$
Similarity =
\frac{shared\ HPO}{total\ HPO}
$$

因为：

```text
Seizure
```

和：

```text
Abnormality of nervous system
```

信息量完全不一样。

比较合理的是：

$$
IC(t)=-\log P(t)
$$

越罕见、越特异的 phenotype：

> IC 越高。

例如：

```text
Developmental delay
```

信息量较低。

而：

```text
Postaxial polydactyly type A
```

信息量较高。

所以更加推荐：

* Resnik
* Lin
* semantic similarity
* likelihood-ratio

而不是 HPO exact-match count。

---

# 19. 还可以增加 phenotype clustering

如果你有几千到几万 rare disease patients，可以做：

```text
HPO matrix
      ↓
semantic similarity
      ↓
patient-patient similarity
      ↓
clustering
      ↓
phenotypic subgroups
```

然后：

```text
Cluster 1
        ↓
rare variant burden

Cluster 2
        ↓
rare variant burden
```

这个对于**发现新的 disease-gene association**非常有价值。

例如：

```text
5000 undiagnosed patients
↓
HPO clustering
↓
12 patients phenotype highly similar
↓
7 patients rare variants in same gene
```

这种证据比：

```text
全队列直接 burden
```

要强很多。

---

# 20. 更进一步：patient matching

可以建立：

```text
Patient A
HPO + gene X variant

Patient B
HPO + gene X variant

Patient C
HPO + gene X variant
```

然后计算：

$$
S =
PhenotypeSimilarity(A,B)
+
GenotypeSimilarity(A,B)
$$

形成：

> **genotype–phenotype matchmaking**

这也是 Matchmaker Exchange 类体系的核心思想。

对于 ultra-rare disease：

```text
一个中心只有1例
```

非常重要。

---

# 21. 我建议你的大型罕见病 cohort 最终采用“三层模型”

如果让我给你设计，我会用：

```text
Layer 1
Genotype-first
─────────────────────
AF
LoF
missense
splice
CNV
SV
STR
inheritance
ClinVar
ACMG


Layer 2
Phenotype-first
─────────────────────
HPO
specificity
information content
present / absent
onset
Phen2Gene
LIRICAL


Layer 3
Joint inference
─────────────────────
Exomiser
Genomiser
gene-disease evidence
segregation
literature
model organisms
network
```

最后：

```text
                FINAL PRIORITY
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
    Tier 1        Tier 2        Tier 3
   Diagnosis      VUS           Novel gene
```

这会比传统：

```text
MAF < 0.01
CADD > 20
ClinVar pathogenic
```

强很多。

---

## 如果只让我选 5 个

针对你说的“**罕见病队列里基因型如何联合表型提高诊断率**”，我会按下面优先级：

1. **Exomiser**：真正做 VCF + HPO + PED 联合排序，最值得作为主分析工具。([PubMed Central (PMC)][3])
2. **LIRICAL**：基于 likelihood ratio 的 phenotype–disease 诊断排序，尤其适合利用 negative HPO。
3. **Phen2Gene**：HPO→gene，速度非常快，非常适合上万病例预计算 phenotype gene score。([PubMed Central (PMC)][4])
4. **Genomiser**：WGS 未解决病例继续搜索 noncoding / regulatory / compound-het。([PubMed Central (PMC)][3])
5. **AMELIE**：利用最新文献补充 gene–phenotype evidence，特别适合 VUS 和新基因。

对于你如果下一步准备做一个**大型 singleton-WES 罕见病队列**，我尤其建议不要只做“每个病例单独 Exomiser”。更有研究价值的是把它升级成：

> **个体层 phenotype-driven diagnosis + 队列层 phenotype clustering + gene-based rare variant burden + patient matching**

这样既可以提高已有疾病的**诊断率**，又可以进一步做**新 disease gene discovery**。

[1]: https://github.com/exomiser/Exomiser?utm_source=chatgpt.com "GitHub - exomiser/Exomiser: A Tool to Annotate and Prioritize Exome Variants · GitHub"
[2]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9288531/?utm_source=chatgpt.com "Phenotype‐driven approaches to enhance variant prioritization and diagnosis of rare disease - PMC"
[3]: https://pmc.ncbi.nlm.nih.gov/articles/PMC12539062/?utm_source=chatgpt.com "An optimized variant prioritization process for rare disease diagnostics: recommendations for Exomiser and Genomiser - PMC"
[4]: https://pmc.ncbi.nlm.nih.gov/articles/PMC7252576/?utm_source=chatgpt.com "Phen2Gene: rapid phenotype-driven gene prioritization for rare diseases - PMC"
[5]: https://hpo.jax.org/?utm_source=chatgpt.com "Human Phenotype Ontology"
[6]: https://pmc.ncbi.nlm.nih.gov/articles/PMC6364462/?utm_source=chatgpt.com "DeepPVP: phenotype-based prioritization of causative variants using deep learning - PMC"
[7]: https://pmc.ncbi.nlm.nih.gov/articles/PMC12452281/?utm_source=chatgpt.com "Clinical and data-driven optimization of Genomiser for rare disease patients: experience from the Hong Kong Genome Project - PMC"

