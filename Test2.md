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
### Create a new directory in the ‘analysis_test2’ directory for the purposes of this test:

``` 
cd /pickett_shared/teaching/EPP622_Fall2022/analysis_test2/
mkdir khubbar7 
```

### Moving to new directory

``` 
cd khubbar7 
```

Lets keep it *neat* 🥰

### Creating and moving to a directory for Fastqc

``` 
mkdir 1_fastqc
cd 1_fastqc
```
### Mirroring the files from host folders for FastQC

```
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922141_1.fastq
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922185_1.fastq
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922187_1.fastq
ln -s ../../../raw_data/solenopsis_invicta_test2/SRR6922236_1.fastq
```

### Setting up a loop so we only have to do this once :smile:

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

### Lets run 🥳 

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

![image](https://user-images.githubusercontent.com/115577500/195474666-152d8c8b-3ec8-4d32-b974-d55de2a79d0d.png)

## Now lets trim the files and re-run everything

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