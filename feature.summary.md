## The following script downloads the _D. melanogaster_ annotation file from flybase.org, checks its integrity, and prints a report listing the total number of genomic features of each type from most to least common:
```
#!/usr/bin/env bash
module load perl
module load R
module load jje/kent/2014.02.19
module load jje/jjeutils/0.1a

createProject HW3 ~/EE282
cd ~/EE282/HW3

wget -O ~/EE282/HW3/data/raw/Dmel.annotation.gtf.gz -q ftp://ftp.flybase.org/genomes/dmel/current/gtf/dmel-all-r6.24.gtf.gz
md5sum Dmel.annotation.gtf.gz > Dmel.annotation.md5.txt

cd ~/EE282/HW3/data/raw
gunzip Dmel.annotation.gtf.gz
cut -f 3 Dmel.annotation.gtf | sort -d | uniq > ~/EE282/HW3/data/processed/genomefeatures.txt

cut -f 3 Dmel.annotation.gtf \
| grep 3UTR \
| wc -w > ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep 5UTR \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep CDS \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep exon \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep ^gene \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep ^miRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep mRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep ncRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep pre_miRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep pseudogene \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep rRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep snoRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep snRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep start_codon \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep stop_codon \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cut -f 3 Dmel.annotation.gtf \
| grep tRNA \
| wc -w >> ~/EE282/HW3/data/processed/feature.count.txt

cd ~/EE282/HW3/data/processed
paste genomefeatures.txt feature.count.txt > labeled.feature.count.txt
sort -rnk 2 labeled.feature.count.txt > labeled.sorted.feature.count.txt

column -t labeled.sorted.feature.count.txt \
| less
```
The output of this script is as follows:
```
exon         187315
CDS          161014
5UTR         46339
3UTR         33358
start_codon  30591
stop_codon   30533
mRNA         30507
gene         17772
ncRNA        2961
miRNA        485
pseudogene   334
tRNA         312
snoRNA       299
pre_miRNA    262
rRNA         115
snRNA        32
```
