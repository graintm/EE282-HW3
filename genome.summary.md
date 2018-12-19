## This script will summarize the _D. melanogaster_ genome by downloading it, checking its integrity, and creating three files which provide information about the nucleotide content.

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
cd ~/EE282/HW3/data/processed
cut -f 2 ~/EE282/HW3/data/processed/Dmel.allchromosomes.sized.sorted.txt \
| head -7 \
| cut -f 2 \
| paste -sd+ - \
| bc > Dmel.bpcount.txt

#To find total number of Ns
cd ~/EE282/HW3/data/raw
gunzip Dmel.allchromosomes.fasta.gz 
grep -v "^>" Dmel.allchromosomes.fasta \
| tr -cd N \
| wc -c > ~/EE282/HW3/data/processed/Dmel.Ncount.txt

#To find total number of sequences
grep "^>" Dmel.allchromosomes.fasta \
| tr -cd ">" \
| wc -c > ~/EE282/HW3/data/processed/Dmel.sequencecount.txt
```
### Genome size in bp
The first section of the script:
```
cd ~/EE282/HW3/data/processed
cut -f 2 ~/EE282/HW3/data/processed/Dmel.allchromosomes.sized.sorted.txt \
| head -7 \
| cut -f 2 \
| paste -sd+ - \
| bc > Dmel.bpcount.txt
```
provides the following output (found in the Dmel.bpcount.txt file): 137547960 bp

### This is actually not the correct number of nucleotides... If you just use faSize on your assembly, you can get the correct number. You get an A for trying though. This is much more difficult than just using faSize, but you want to stay clear of having to reinvent the wheel. It is good to double check results though of course. fyi you are shy the # of tn's by 6,178,042 or 4.3%

### N count
The second section:
```
cd ~/EE282/HW3/data/raw
gunzip Dmel.allchromosomes.fasta.gz 
grep -v "^>" Dmel.allchromosomes.fasta \
| tr -cd N \
| wc -c > ~/EE282/HW3/data/processed/Dmel.Ncount.txt
```
provides the following output: 1152978

### Sequence count
The third second:
```
grep "^>" Dmel.allchromosomes.fasta \
| tr -cd ">" \
| wc -c > ~/EE282/HW3/data/processed/Dmel.sequencecount.txt
```
provides the following output: 1870
