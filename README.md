

## Summary





## Dependencies

    MAFFT 7.458
    IQ-Tree2 rc2
    RAxML 8.2.12
    ggtree 1.14.6
    pangolin V1.1.8(https://github.com/hCoV-2019/pangolin)
    Biopython
    python 3.6
    R version 3.5.1


## Repo Contents

    data: preprocessed data used for analysis.
    result: result files for figure visualization.
    scripts: python script and R script for analysis and visulization.


## Phylogenetic analysis



1. Multi-Alignment of the SARS-CoV-2 genomes : `mafft --thread 10 --auto $fasta_file > $align_result_file`

2. Iqtree phylogenetic analysis: `iqtree -s $align_trim_file --prefix iqtree_result -m GTR+F+R2 -B 1000 -cmax 30 -redo -T 24`

3. RAXML phylogenetic analysis: `raxmlHPC-PTHREADS -s $align_trim_file -n raxmltree_result -m GTRGAMMAI -f a -x 12345 -N 1000 -p 123456 -T 24 -k`

4. pangolin lineage analysis: `pangolin $fasta_file -t 24`


## Genome assembly and variations calling


1. To obtain consensus sequences and deletion mutations: 
    `snakemake -s GenomeAssembly_And_indelCalling.pipeline.py -p`

2. Calling iSNV and SNP mutations by using script "iSNV_calling.sh" according to https://github.com/generality/iSNV-calling: 
   `bash iSNV_calling.sh 3,4,5`


## variations annotation


1. To convert iSNV table into vcf format(VCFv4.1) that can be recognized by SnpEff software: 
   python ./scripts/Variantions_annotion/iSNVTable_2_vcf_allsample.py -i ./data/ -o $allsamplesVcfPath -r MN908947.3

2. Annoting the mutation by using SnpEff software ,for example: java -jar snpEff.jar ann MN908947 $$allsamplesVcfPath/${sample}.vcf >$outSnpeff/{sample}.snpeffAnno.vcf

3. To Integrate all the samples' mutations annotion files into a single file: python Snpeff_results_integrateTo1file -i $outSnpeff/ -o ./data/allsampMutatAnno.txt




## Citation

If you use data, results or conclusion from this work, please cite:




## Acknowledgement
