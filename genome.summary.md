## This script with summarize the _D. melanogaster_ genome by downloading it, checking its integrity, and creating three files which provide information about the nucleotide content.

```
#!/usr/bin/env bash
module load perl
module load R
module load jje/kent/2014.02.19
module load jje/jjeutils/0.1a

createProject HW3 ~/EE282
cd ~/EE282/HW3

#Downloads D. melanogaster genome and checks md5sum, which can be compared against md5sum file on flybase.org
wget -O ~/EE282/HW3/data/raw/Dmel.allchromosomes.fasta.gz -q ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/dmel-all-chromosome-r6.24.fasta.gz
cd ~/EE282/HW3/data/raw
md5sum Dmel.allchromosomes.fasta.gz > Dmel.genome.md5.txt

faSize -detailed ~/EE282/HW3/data/raw/Dmel.allchromosomes.fasta.gz > ~/EE282/HW3/data/processed/Dmel.allchromosomes.sized.txt
sort -rnk 2,2  ~/EE282/HW3/data/processed/Dmel.allchromosomes.sized.txt > ~/EE282/HW3/data/processed/Dmel.allchromosomes.sized.sorted.txt

#To find total number of nucleotides
cd data/processed
cut -f 2 ~/EE282/HW3/data/processed/Dmel.allchromosomes.sized.sorted.txt \
| head -7 \
| cut -f 2 \
| paste -sd+ - \
| bc > Dmel.bpcount.txt

#To find total number of Ns
cd data/raw
gunzip Dmel.allchromosomes.fasta.gz 
grep -v "^>" Dmel.allchromosomes.fasta \
| tr -cd N \
| wc -c > Dmel.Ncount.txt

#To find total number of sequences
grep "^>" Dmel.allchromosomes.fasta \
| tr -cd ">" \
| wc -c > ~/EE282/HW3/data/processed/Dmel.sequencecount.txt
```
