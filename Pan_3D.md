
## Data
[3D Genome Datasets](https://3dgenome.fsm.northwestern.edu/datasets)

[4D nucleome](https://data.4dnucleome.org/hic-data-overview)

[PRJEB11418](https://www.ncbi.nlm.nih.gov/bioproject/PRJEB11418): Analysis of structural variation and chromatin organization by Hi-C

## Tools

[HiC_tools](https://github.com/mdozmorov/HiC_tools): Hi-C data analysis tools and papers

[Raichu](https://github.com/XiaoTaoWang/Raichu/)

## Papers


3D genomics across the tree of life reveals condensin II as a determinant of architecture type

Genome-wide detection of enhancer-hijacking events from chromatin interaction data in rearranged genomes

Highly interconnected enhancer communities control lineage-determining genes in human mesenchymal stem cells


## Progress

1.挑选三个家系（Trio），搭建分析流程和评估
PR05	HG00733	HG00731	HG00732	PUR	AMR
SH032	HG00514	HG00512	HG00513	CHS	EAS
Y117	NA19240	NA19239	NA19238	YRI	AFR

评估1：每个样本有两个重复，重复间单独处理，计算所有样本及重复间的相关性，重复间相关性达到一定阈值后可以合并处理（大数据量有利于等位特异Hi-C的发现）；
评估2：分析每个样本的TAD，分析每个Trio内不符合遗传规律的比例，进行错误率的评估；
评估3：将序列比对至phased assembly，检测每个Trio子代特异的TAD，验证流程和方法的可靠性；
评估4：HiFi序列可以同时对SNV和SV进行分phase，但是需要验证两个变异是否完全按照phase的顺序排布；


---

# Evolutionary patterns and functional effects of 3D chromatin structures in butterflies with extensive genome rearrangements

Genome assembly, annotation, and phylogenetic analysis

Drastic genome rearrangements in Graphium butterflies

Identification of 3D chromatin structures of butterflies revealed by Hi-C

Impact of inter-chromosome fusions on 3D chromatin structures

Alteration of 3D chromatin structures with intra-chromosome rearrangements

3D chromatin structure evolution in the Hox gene cluster of insects



## Reconstructing the 3D genome organization of Neanderthals reveals that chromatin folding shaped phenotypic and sequence divergence

Reconstructing the 3D genome organization of archaic hominins

Archaic hominin and modern human genomes exhibit a range of 3D divergence

3D genome organization diverges between AH and MH at 167 genomic loci

Regions with 3D divergence highlight AH-MH phenotypic differences

Relationship between sequence divergence and 3D divergence

Maintenance of 3D genome organization constrained sequence diver- gence in recent hominin evolution






## Hi-C for genome-wide detection of enhancerhijacking rearrangements in routine lymphoid cancer biopsies

FFPE Hi-C identifies topological features across a range of read depths
(range 3.5M–89.5M unique valid pairs)

FFPE Hi-C captures state-selective topological features

FFPE Hi-C detects oncogenic SVs

FFPE Hi-C supports heterologous IGH enhanceroncogene interactions

FFPE Hi-C identifies enhancer-hijacking rearrangements at diverse loci

FFPE Hi-C detects large oncogenic copy-number alterations

Hi-C distinguishes BCL6 gene-activating from BCL6-LCR-donating events

FFPE Hi-C identifies enhancer-donating partners for MYC rearrangements

MYC topology, enhancers, and breakpoints are correlated in DLBCL and MM


## Pan-3D Genome Analysis Reveals the Roles of Structural Variation in Chicken Chromatin Architectures, Domestication and Production Traits

High-Quality Assemblies and Annotation of Fifteen Chicken Genomes

Pan-Gene Family Characteristic of 28 Global Chicken Genomes

Novel Sequence Identification and Structural Variation Landscape in Pan Genome

Construction and Characterization of High-Resolution Pan-3D Genome Atlas

Selection on the TSHR-DIO2 Axis During Chicken Domestication

SV Affected Carcass Traits by Regulating Distal Genes Through Chromatin Loops

Structural Variation Improved Predictive Accuracy of Genomic Selection in Chicken



## Pan-3D genome analysis reveals structural and functional differentiation of soybean genomes

Hi‑C sequencing of 27 soybean accessions

Identification of A/B compartments in individual accessions

TAD identification and characterization in individual soybean accessions

Pan‑analyses of TADs across the 27 soybean accessions

Genomic SVs in the 3D genome

Effect and contribution of SVs to 3D genome variations

3D genome variation and gene expression

3D genome dynamics during domestication and improvement




## TAD conservation in vertebrate genomes is driven by stabilising selection

Identification of syntenic blocks across 12 species

Identification of TADs in the vertebrate fibroblast genomes

TAD number is conserved across vertebrate evolution

Divergence time correlates with TAD border position conservation in vertebrates

Gene position is conserved within TADs

Most syntenic blocks show stabilising selection while a few evolve under genetic drift


## TAD evolutionary and functional characterization reveals diversity in mammalian TAD boundary properties and function

Cross-species comparison of TAD boundaries reveals variation in evolutionary conservation

Genetic and epigenetic properties of boundaries vary depending on their evolutionary conservation

Transposable elements contribute to the evolution of conserved and species-specific TAD boundaries

Species-specific boundaries are over-represented at evolutionary breaks of synteny

Deletion of an ultraconserved TAD boundary results in tissuespecific functional phenotypes

Deletion of a human-specific boundary alters gene expression in human neurons


## Evolutionary analysis of gene ages across TADs associates chromatin topology with whole-genome duplications

Gene ages are not randomly distributed in human and mouse TADs

Gene age co-localization is stronger in TADs than in fixed-size partitions of the human genome

Gene age co-localization patterns are related to TAD insulation

Essential young genes are associated with TADs enriched in old genes

Essential young genes show more 3D interactions with old genes and other essential genes



## Common DNA sequence variation influences 3-dimensional conformation of the human genome
Mapping 3D chromatin conformation across individuals

3D chromatin conformation variations between individuals

Coordinated variation of the 3D genome, epigenome, and transcriptome

Genetic loci influencing 3D chromatin conformation

Contribution of 3D chromatin QTLs to molecular phenotypes and disease risk

## Comparative 3D genome architecture in vertebrates

Initial characteristics of chromosomal conformation across species

Inter‑chromosomal interactome across species

The local spatial context is largely conserved in ten mammals

The TADs are evolutionarily conserved gene expression regulatory units

Some TEs appear highly correlated with chromatin interactions



## Haplotype-Resolved 3D Genomic Landscapes and Their Impacts on Agronomic Traits in Grapevine

Haplotype-Resolved A/B Compartments Landscapes in TS and PN

Haplotype-Resolved TADs in TS and PN

Distinct TAD State Transitions Between Haplotypes

Transcriptional Consequences of Haplotype-Specific TAD State Transitions

Structural Variants Shape Haplotype-Specific 3D Genome Architecture

Phased 3D Genome Variations are Linked to Key Agronomic Traits


## haplotype-specific TAD

2. 使用“合并-回射”法（Pooled-to-Haploid Strategy）
这是目前单倍型研究中最稳健的方法，因为它避免了直接在极低深度数据上“盲找”。

Pooled Calling： 将所有读段（不分单倍型）合并，计算出一套高质量的 “参考 TAD 边界”。

Haploid Extraction： 在单倍型特异性矩阵（Hap1, Hap2）中，分别计算这些参考边界位置的绝缘指数（Insulation Score）。

Differential Testing： 通过统计学检验比较 Hap1 和 Hap2 在同一位置的绝缘强度差异。如果强度差异显著，则判定为单倍型特异性 TAD。




After iterative correction and eigenvector decomposition [28] and quantile normalization [29], we generated 31 Hi-C contact maps at 20-kb resolution (~ 83.12% of bins had at least 1000 intra-chromosomal contacts, which were highly reproducible between biological replicates (median stratumadjusted correlation coefficient, SCC > 0.905; Fig. 1B)


### binning
In this step, an .mcool file will be produced under the coolers-hg38 sub-folder for each .pairs.gz file using cooler. The mcool format is the official Hi-C data format for the 4DN consortium and can be visualized using HiGlass:


