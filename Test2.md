# Test2 for Khubbar7
## Introductory information for Test

Use these 4 datasets, which are additional samples from the Solenopsis invicta study we used in class labs. 

-SRR6922141

-SRR6922185

-SRR6922187

-SRR6922236

They can be found in:

/pickett_shared/teaching/EPP622_Fall2022/raw_data/solenopsis_invicta_test2

## Fastqc and Trimming

Fastqc is a quick way to run some quality control checks on your raw sequence data. It looks at anything from per base/tile sequence quality, per seq GC content, overrepresented sequences, and adapter content. 
Create a new directory in the ‘analysis_test2’ directory for the purposes of this test:

``` 
cd /pickett_shared/teaching/EPP622_Fall2022/analysis_test2/
mkdir khubbar7 
```

Moving to new directory

``` 
cd khubbar7 
```

Lets keep it *neat* 🥰

Creating and moving to a directory for Fastqc

``` 
mkdir 1_fastqc
cd 1_fastqc
```
Mirroring the files from host folders for FastQC

```
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922141_1.fastq
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922185_1.fastq
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922187_1.fastq
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922236_1.fastq
```

Setting up a loop so we only have to do this once :smile:

```
nano fastqc.sh
```
*This will open nano editor*

Input:
```
for file in *.fastq
do
    basename=$(echo "$file" | sed 's/.fastq//')
    echo $file
    echo $basename

    spack load fastqc

   fastqc ${basename}.fastq


done
```

Lets gooooooo! 🥳 

``` 
bash fastqc.sh
```
Now lets get them out to your console, since we cant open in Spinx:

In a different terminal running on your console, not SPINX

```
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/1_fastqc/SRR6922178_1_fastqc.html ./
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/1_fastqc/SRR6922185_1_fastqc.html ./
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/1_fastqc/SRR6922187_1_fastqc.html ./
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/1_fastqc/SRR6922236_1_fastqc.html ./
```
Open the files on your console with the command or just find them

```
open [filename.html]
```

![image](https://user-images.githubusercontent.com/115577500/195474502-f0f38a04-c7b0-40b6-95fd-6c6ce46a7868.png)

![image](https://user-images.githubusercontent.com/115577500/195474587-fce7612f-9987-4e1c-a59a-bebecc1146f6.png)

![image](https://user-images.githubusercontent.com/115577500/195474617-1314beb3-39b1-4c5d-9ec6-94058d1a8069.png)

![image](https://user-images.githubusercontent.com/115577500/195476365-db45c7d1-32a5-4c37-8dd1-3d595010a7a4.png)


### Now lets trim the files and re-run everything

Sometimes the adapter regions/ the barcode regions contaminate the sequence. Also the sequence throws more errors at the end. Trimming helps reduce these errors, but may not be worth it. You need to look at your FASTqc data before and after trimming. 

Going back a directory and making a new one to keep everything tidy

```
cd ../
mkdir skewer
cd skewer
```

Lets mirror all of those files again, but this time lets pipe it so we dont have to use four command lines

```
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922141_1.fastq | ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922185_1.fastq | ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922187_1.fastq | ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922236_1.fastq
```
The loop was bugging on me but thats okay because I can pipe it :smile:

```
/sphinx_local/software/skewer/skewer -t 2 -l 95 -x AGATCGGAAGAGCGGTTCAGCAGGAATGCCGAGACCGATCTCGTATGCCGTCTTCTGCTTG -Q 30 SRR6922141_1.fastq -o SRR6922141_1 | /sphinx_local/software/skewer/skewer -t 2 -l 95 -x AGATCGGAAGAGCGGTTCAGCAGGAATGCCGAGACCGATCTCGTATGCCGTCTTCTGCTTG -Q 30 SRR6922236_1.fastq -o SRR6922236_1 | /sphinx_local/software/skewer/skewer -t 2 -l 95 -x AGATCGGAAGAGCGGTTCAGCAGGAATGCCGAGACCGATCTCGTATGCCGTCTTCTGCTTG -Q 30 SRR6922185_1.fastq -o SRR6922185_1 | /sphinx_local/software/skewer/skewer -t 2 -l 95 -x AGATCGGAAGAGCGGTTCAGCAGGAATGCCGAGACCGATCTCGTATGCCGTCTTCTGCTTG -Q 30 SRR6922187_1.fastq -o SRR6922187_1
```

Now lets Fastqc the trimmed files, after loading fastqc again because it doesnt wanna stay loaded:

```
spack load fastqc
fastqc SRR6922141_1-trimmed.fastq | fastqc SRR6922236_1-trimmed.fastq | fastqc SRR6922185_1-trimmed.fastq | fastqc SRR6922187_1-trimmed.fastq
```

And exporting the files to your console on a different terminal not logged into spinx:

```
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/skewer/SRR6922141_1-trimmed_fastqc.html ./
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/skewer/SRR6922185_1-trimmed_fastqc.html ./
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/skewer/SRR6922187_1-trimmed_fastqc.html ./
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/skewer/SRR6922236_1-trimmed_fastqc.html ./
```

Now use the open command to open them on your console:

```
open [filename.html]
```
![image](https://user-images.githubusercontent.com/115577500/195476110-fe78a43a-39c3-4ba7-8811-11e63921573c.png)

![image](https://user-images.githubusercontent.com/115577500/195476138-5628c62d-229d-441c-9a30-d30b92f578b5.png)

![image](https://user-images.githubusercontent.com/115577500/195476168-ec565887-2d3a-4351-8ba9-82e19969696c.png)

![image](https://user-images.githubusercontent.com/115577500/195476246-e3eda3fe-d21a-4639-abcd-4aac0f47e9f0.png)


### Heres the reads per sample!

*Reads per sample (before trimming):

SRR6922141: 1148398

SRR6922185: 1315096

SRR6922187: 1081747

SRR6922236: 977855

Reads per sample (after trimming):

SRR6922141: 1068311

SRR6922185: 1254903

SRR6922187: 993372

SRR6922236: 928913


## Lets run a Burrows-Wheeler Alignment!! 🥳

A BWA is a fast and accurate way to take short reads and align them against a large reference genome. This is going to map all the reads and give us a BAM file in return which is a tab delinated file that contains sequence alignment data. 

Setting up the folder to keep things tidy and a subfolder for our files and move all the way into the second folder

(Note, I originally named this folder bwa and then quickly realized that naming folders the same as commands was really dumb so I ` rm -r ` 'ed it and re-created it)

```
cd ../
mkdir bwatest
cd bwatest
mkdir bwafiles
cd bwafiles

```

Now lets pull our reference genome in

```
ln -s ../../../../raw_data/solenopsis_invicta/genome/UNIL_Sinv_3.0.fasta* .
```

I can see this file has already been indexed but if i wanted to index it I would use the `bwa index [filename.fasta]` command

Now back one directory to mirror in our trimmed files 

```
cd ../
```

Lets re-mirror all those files in a pipe

```
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922141_1.fastq | ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922185_1.fastq | ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922187_1.fastq | ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922236_1.fastq
```

Lets try looping BWA to make our lives easy

Open nano

```
nano bwa.sh
```
In the nano editor:

```
for file in *.fastq
do
    basename=$(echo "$file" | sed 's/.fastq//')
    echo $file
    echo $basename

    spack load bwa
    spack load samtools@1.9%gcc@8.4.1

    bwa mem -t 6 \
    bwafiles/UNIL_Sinv_3.0.fasta \
    ${basename}.fastq \
    | samtools view -bSh \
    | samtools sort \
    -@ 3 -m 4G \
    -o ${basename}_sorted.bam


done
```
Lets goooo! 🥳 

Also, grab something to drink- shes going to take a second!

```
bash bwa.sh
```
💃 💃 💃 It ran sucessfully 💃 💃 💃

Lets reload sam tools (version 1.9% GC, 8.4.1) extract those stats in a seperate file:

```
spack load samtools@1.9%gcc@8.4.1
samtools flagstat SRR6922141_1_sorted.bam > SRR6922141_1_sorted.stats | samtools flagstat SRR6922185_1_sorted.bam > SRR6922185_1_sorted.stats | samtools flagstat SRR6922187_1_sorted.bam > SRR6922187_1_sorted.stats | samtools flagstat SRR6922236_1_sorted.bam > SRR6922236_1_sorted.stats
```

Lets 👀 look to see what weve got!

```
cat SRR6922141_1_sorted.stats
```

^^ Do this seperately for all four so its easier to see



### Number of Read Mapped

SRR6922141_1: 1151758

SRR6922185_1: 1317060

SRR6922187_1: 1084886

SRR6922236_1: 979403

### Number of Supp Reads

SRR6922141_1: 3360

SRR6922185_1: 1964

SRR6922187_1: 3139

SRR6922236_1: 1548

## GATK Analysis

GATK stands for Genome Analysis Toolkit and is great for identifying SNPs and insertions/deletions (indels). It will produce a report that will let us see SNPs and Indels. 

Loading java version 8.4.1 and samtools version 1.9

```
spack load openjdk@11.0.8_10%gcc@8.4.1
spack load samtools@1.9
```

Writing sh file for adding reading groups loop

```
nano gatkloop.sh

```
In your nano editor:

```
for f in *_sorted.bam
do
        BASE=$( basename $f | sed 's/_sorted.bam*//g' )
        echo "BASE $BASE"
        
	java -jar /pickett_shared/software/picard-2.27.4/picard.jar \
		AddOrReplaceReadGroups \
		I=${BASE}_sorted.bam \
		O=${BASE}_sorted.RG.bam \
		RGSM=$BASE \
		RGLB=$BASE \
		RGPL=illumina \
		RGPU=$BASE
	samtools index ${BASE}_sorted.RG.bam
done
```

Lets run! 🏃

```
bash gatkloop.sh
```

And double check the files got bigger!

Now lets index!

```
samtools index SRR6922141_1_sorted.RG.bam | samtools index SRR6922185_1_sorted.RG.bam |
 samtools index SRR6922187_1_sorted.RG.bam | samtools index SRR6922236_1_sorted.RG.bam 
```

and lets remove the old bam files that dont have the regroups

```
rm -r SRR6922141_1_sorted.bam SRR6922185_1_sorted.bam SRR6922187_1_sorted.bam SRR6922236_1_sorted.bam
```

making a new gatktest directory and moving to it

```
mkdir gatktest
cd gatktest
```

Linking in the BAM file with read groups, its index, and the reference genome

```
 ln -s ../SRR6922141_1_sorted.RG.bam . | ln -s ../SRR6922185_1_sorted.RG.bam . | ln -s ../SRR6922187_1_sorted.RG.bam . | ln -s ../SRR6922236_1_sorted.RG.bam . | ln -s ../SRR6922141_1_sorted.RG.bam.bai . | ln -s ../SRR6922185_1_sorted.RG.bam.bai . | ln -s ../SRR6922187_1_sorted.RG.bam.bai . | ln -s ../SRR6922236_1_sorted.RG.bam.bai . | ln -s /pickett_shared/teaching/EPP622_Fall2022/raw_data/solenopsis_invicta/genome/UNIL_Sinv_3.0.fasta
 
 ```
 
 Preparing reference for GATK
 
 ```
 /pickett_shared/software/gatk-4.2.6.1/gatk CreateSequenceDictionary -R UNIL_Sinv_3.0.fasta
 samtools faidx UNIL_Sinv_3.0.fasta
 ```
 Lets create a sh file loop for GATK
 
 ```
 nano gatktestloop.sh
 ```
 in nano editor:
 
 ```
 for f in *_sorted.RG.bam
do
        BASE=$( basename $f | sed 's/-trimmed_sorted.RG.bam*//g' )
        echo "BASE $BASE"

        /pickett_shared/software/gatk-4.2.6.1/gatk HaplotypeCaller \
        -R UNIL_Sinv_3.0.fasta \
        -I $f \
        -O ${BASE}.g.vcf \
        -ERC GVCF \
        -bamout ${BASE}_sorted.RG.realigned.bam

done
```
Lets goooo!
```
bash gatktestloop.sh
```
She ran! 😍

Whew! That took 7 mins to run!

Lets make a new directory and then copy our GVCF files in there!

```
mkdir gatkgvcf
cp *.g.vcf /pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/bwatest/gatktest/gatkgvcf
```
Yay! Everything has been moved! Now lets look at the data!

```
spack load bcftools
bcftools stats SRR6922141_1_sorted.RG.bam.g.vcf > SRR6922141_1_sorted.RG.bam.g.vcf.stats.txt
nano SRR6922141_1_sorted.RG.bam.g.vcf.stats.txt
bcftools stats SRR6922185_1_sorted.RG.bam.g.vcf > SRR6922185_1_sorted.RG.bam.g.vcf.stats.txt
nano SRR6922185_1_sorted.RG.bam.g.vcf.stats.txt
bcftools stats SRR6922187_1_sorted.RG.bam.g.vcf > SRR6922187_1_sorted.RG.bam.g.vcf.stats.txt
nano SRR6922187_1_sorted.RG.bam.g.vcf.stats.txt
bcftools stats SRR6922236_1_sorted.RG.bam.g.vcf > SRR6922236_1_sorted.RG.bam.g.vcf.stats.txt
nano SRR6922236_1_sorted.RG.bam.g.vcf.stats.txt
```
That gives us:

SRR6922141_1:

SNPS: 28834

INDELS: 4253

SRR6922185_1:

SNPS: 21044

INDELS: 3513

SRR6922187_1:

SNPS: 27421

INDELS: 3884

SRR6922236_1:

SNPS: 19240

INDELS: 3299


## IGV visualization

We do this to visualize the SNPs and let us double check the data. Its also fun to play with! 😄
Looking at IGV:

Lets go ahead a pull these to our computer and set up IGV.org

On a terminal running on my computer:

(This is going to pull more than what I need but its faster)


```
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/bwatest/gatktest/*bam .
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/bwatest/gatktest/*bai .
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/bwatest/gatktest/gatkgvcf/*g.vcf .
scp khubbar7@sphinx.ag.utk.edu:/pickett_shared/teaching/EPP622_Fall2022/analysis_test2/khubbar7/bwatest/gatktest/UNIL_Sinv_3.0.fasta.fai .4
```

Lets find something to pull up on IGV:

```
cd ../
spack load nano
nano SRR6922141_1_sorted.RG.bam.g.vcf
```
(The old google says you can use ^V to jump up pages in nano)

Line from file:

NC_052664.1     1087705 .       AATTAC  A,<NON_REF>     73.60   .       BaseQRankSum=-0.431;DP=3;ExcessHet=0.0000;MLEAC=1,0;MLEAF=0.500,0.00;MQRankSum=0>

PHOTO BELOW

However, this tells us (genome) (position) (deletion anchored by an A) (reference call) 

It does match with the IGV document. You can see the deletion in the top bar.

![image](https://user-images.githubusercontent.com/115577500/195960015-3c588237-0b3e-419c-b118-e4e2e72883b4.png)


# And shes done! Thank you!


















