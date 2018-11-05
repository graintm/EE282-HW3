## This script downloads the _D. melanogaster_ annotation file and prints a report showing the different chromosomes and the number of genes on each.
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
touch chromosomes.txt
echo -e "X \nY \n2L \n2R \n3L \n3R \n4" > chromosomes.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^X \
| cut -f 2 \
| grep -c ^gene > genecount.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^Y \
| cut -f 2 \
| grep -c ^gene >> genecount.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^2L \
| cut -f 2 \
| grep -c ^gene >> genecount.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^2R \
| cut -f 2 \
| grep -c ^gene >> genecount.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^3L \
| cut -f 2 \
| grep -c ^gene >> genecount.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^3R \
| cut -f 2 \
| grep -c ^gene >> genecount.txt

cut -f 1,3 Dmel.annotation.gtf \
| grep ^4 \
| cut -f 2 \
| grep -c ^gene >> genecount.txt

paste chromosomes.txt genecount.txt > ~/EE282/HW3/data/processed/labeled.genecount.txt
cd ~/EE282/HW3/data/processed
less labeled.genecount.txt
```

This script prints the following output:
```
X       2676
Y       113
2L      3501
2R      3628
3L      3464
3R      4202
4       111
```
