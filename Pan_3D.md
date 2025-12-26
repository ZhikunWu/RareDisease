
## Data
[3D Genome Datasets](https://3dgenome.fsm.northwestern.edu/datasets)

[4D nucleome](https://data.4dnucleome.org/hic-data-overview)

[PRJEB11418](https://www.ncbi.nlm.nih.gov/bioproject/PRJEB11418): Analysis of structural variation and chromatin organization by Hi-C

## Tools

[HiC_tools](https://github.com/mdozmorov/HiC_tools): Hi-C data analysis tools and papers


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


