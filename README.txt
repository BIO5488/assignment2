Question 1
bowtie2 -x chr22_idx -U reads.fq -S alignment_file.sam 2> report_file.txt 
grep -v "^@" alignment_file.sam| awk '$2 != 4 {print $0}'>uniquely_mapped_alignment_file.sam #explanation in Comments 

Number of uniquely mapped reads: 7115(26.69%)

Number of multi mapped reads: 10126(37.98%)

Number of unmapped reads: 9420(35.33%)


-
Question 2
CG is the most enriched nucleotide in this dataset. In more detail, single nucleotides A, C, and G are enriched in read.fq than the reference with enrichment scores(fold change) higher than 1. Dinucleotide of AA, AT, CC, CG, GA, GC, GG, TC are enriched in read.fq than the reference with enrichment scores(fold change) higher than 1 but CG is the most enriched compared to other nucleotides.

Single nucleotide enrichment scores:
A : 1.0362139620338378
C : 1.0854251812551212
G : 1.1137595677932663
T : 0.7868097786252916

Dinucleotide enrichment scores:
AA : 1.1548495676422013
AC : 0.9470807240212612
AG : 0.7355022737268961
AT : 1.3319437493655997
CA : 0.8912113345409589
CC : 1.0508820804164807
CG : 3.8718498632509712
CT : 0.7093604002470296
GA : 1.313616652067337
GC : 1.135835963884709
GG : 1.1920619310024492
GT : 0.7299651374274183
TA : 0.6944987899479796
TC : 1.1834792349695444
TG : 0.8377493920020211
TT : 0.4830063004242911

I first calculated the nucleotide frequency of our sequencing reads(read.fq) using nuc_count_FINAL.py. Then, I calculated the nucleotide frequency of the reference genome(chr22.fq) using nuc_count_FINAL.py. Next, I wrote a python script that calculates the enrichment scores by dividing (Nucleotide frequency of our sequencing read) by (Nucleotide frequency of reference genome). For example, AA dinucleotide enrichment is calculated by dividing the AA frequency of our sequencing read by AA frequency of reference. The script is /home/nkong/assignnment2/work/enrichment.py. Finally, I ran the python code by typing "python3 /home/nkong/assignnment2/work/enrichment.py" in the command line.

Chip-seq: Since CG is enriched in our sequence, it is likely that our sequence contains a lot of CpG sites. Knowing the fact that CpG sites are located within and close to sites of about 40% of mammalian gene promoters[Janitz, Karolina, and Michal Janitz. "Assessing epigenetic information." Handbook of Epigenetics. Academic Press, 2011. 173-181.], I think this experiment was designed to target promoter regions using chip-seq. In the chip-seq experiment, we can map promoter regions by using chips that bind to H3K4me3 or transcription factors that bind to promoters or RNA polymerase 2. Also, it might be the reason why only 26.69% of our reads aligned 1 time and 37.98% aligned >1 times since promoter sequence may overlap many times in our genome. 
-
Comments:
This is a comment for command line that I used to make uniquely_mapped_alignment_file.sam file. I used following command; <grep -v "^@" alignment_file.sam| awk '$2 != 4 {print $0}'>uniquely_mapped_alignment_file.sam> To make an output that maps uniquely, I first removed the header by "grep -v "^@" ". Then to remove unmapped reads, I used Flag information in sam file column 2. Flag 4 means unmapped segments, so I removed them by awk '$2 != 4 {print $0}'. Then, there will be uniquely mapped reads and multi-mapped reads. However, according to the bowtie 2 tutorial, in a default mode, after they search for multiple alignments, they report the best one. So, printed reads don't contain information of multi-alignment but contain uniquely mapped information. Therefore, I made the final output with these.

-
Suggestions:

-
