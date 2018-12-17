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
gunzip Dmel.annotation.gtf.gz
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

### Comments

Mostly O.K.

It's essential to download the pre-computed checksum to compare against. Then the easiest way to check is to use ```md5sum -c``` against the checksum file you downloaded (md5sum.txt in this case). It will automatically go through files listed in it and compute / compare the checksums.

```
$ wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/gtf/md5sum.txt
$ wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/gtf/dmel-all-r6.24.gtf.gz
$ md5sum dmel-all-r6.24.gtf.gz 
5cd5dcfbfff952ea7ce89e26cba89bbd  dmel-all-r6.24.gtf.gz

$ md5sum -c md5sum.txt
dmel-all-r6.24.gtf.gz: OK
```

Also, your life would be made easier using the ```-c``` switch with uniq. I also used ```awk``` instead of ```cut -f```:

```
awk ' $3 == "gene" { print $1 } ' Dmel.annotation.gtf \
| sort \
| uniq -c \
| sort -rnk1,1 \
| head -7 \
> genecount.txt
```
