# Project 1: Antibiotic resistance
## Lab notebook

All data processing was carried out on Ubuntu 20.04 machine.

### Downloading data:

- cd to working directory ~/dev/project_1_bioinf
- Download genome in fasta:
`wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.fna.gz`
- Download annotation file:
`wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.gff.gz`
- Download raw sequence data:
wget --content-disposition https://figshare.com/ndownloader/articles/10006541/versions/3

-- content-disposition flag used here to keep the original name and file extension.

### Unzipping data:

- `unzip 10006541.zip` to unzip raw data
- `gunzip -c amp_res_1.fastq.gz > amp_res_1.fastq`
- `gunzip -c amp_res_2.fastq.gz > amp_res_2.fastq`

-c flag to keep original file

### Inspecting raw sequence data:

- `head -4 amp_res_1.fastq`

`@SRR1363257.37 GWZHISEQ01:153:C1W31ACXX:5:1101:14027:2198 length=101
GGTTGCAGATTCGCAGTGTCGCTGTTCCAGCGCATCACATCTTTGATGTTCACGCCGTGGCGTTTAGCAATGCTTGAAAGCGAATCGCCTTTGCCCACACG
+
@?:=:;DBFADH;CAECEE@@E:FFHGAE4?C?DE<BFGEC>?>FHE4BFFIIFHIBABEECA83;>>@>@CCCDC9@@CC08<@?@BB@9:CC#######`

Output looks like legitimate fastq-file (same with the second read fastq)

### Inspecting reference fasta-data:

- `head -3 GCF_000005845.2_ASM584v2_genomic.fna`
`>NC_000913.3 Escherichia coli str. K-12 substr. MG1655, complete genome
AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTCTGATAGCAGCTTCTGAACTG`

Output looks like a legitimate fasta file.

- `cat GCF_000005845.2_ASM584v2_genomic.fna`   

Swamps the console with genomic data

### Inspecting number of reads:

- `wc -l amp_res_1.fastq` 

1823504 amp_res_1.fastq

- `wc -l amp_res_2.fastq`

1823504 amp_res_2.fastq

Info about each read is written in 4 lines, so 1823504 / 4 = 455876 - number of reads we got (both for forward and reverse)

### Inspecting raw sequencing data with fastqc:

- Installing fastqc via conda (in a separate env):

`conda install -c bioconda fastqc`

- Running fastqc on data:

`fastqc -o . amp_res_1.fastq amp_res_2.fastq `

We got two html output files: amp_res_1_fastqc.html and amp_res_2_fastqc.html that both content report on read quality.

For forward:

#TODO screenshot

And for reverse:

#TODO screenshot

Both report files indicate that data didn’t pass quality control in per base quality section. Also, for forward read (amp_res_1) per tile quality check was not passed. This may be caused by errors during injecting samples into Illumina cartridge (air bubbles) or by malfunctioning cartridge itself. Second report however (for amp_res_2) passed this control, though with yellow warning indicator.

Total sequences number is identical with the sequence number accesed via command line.

### Filtering the reads:

For reads filtering Trimmomatic was used in PE mod. Trimmomatic was installed via conda

- `conda install trimmomatic`
- `java -jar /path_to_trimmomatic/trimmomatic-0.39-2/trimmomatic.jar PE amp_res_1.fastq.gz amp_res_2.fastq.gz amp_res_1_output_paired.fq.gz amp_res_1_output_unpaired.fq.gz amp_res_2_output_paired.fq.gz amp_res_2_output_unpaired.fq.gz ILLUMINACLIP:/path_to_adapters/trimmomatic-0.39-2/adapters/TruSeq3-PE.fa:2:30:10:2:True LEADING:20 TRAILING:20 SLIDINGWINDOW:10:20 MINLEN:20`

Trimmomatic generated the following output:

`TrimmomaticPE: Started with arguments:
amp_res_1.fastq.gz amp_res_2.fastq.gz amp_res_1_output_paired.fq.gz amp_res_1_output_unpaired.fq.gz amp_res_2_output_paired.fq.gz amp_res_2_output_unpaired.fq.gz ILLUMINACLIP:/path_to_trimmomatic/trimmomatic-0.39-2/adapters/TruSeq3-PE.fa:2:30:10:2:True LEADING:20 TRAILING:20 SLIDINGWINDOW:10:20 MINLEN:20
Multiple cores found: Using 4 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 455876 Both Surviving: 446259 (97.89%) Forward Only Surviving: 9216 (2.02%) Reverse Only Surviving: 273 (0.06%) Dropped: 128 (0.03%)
TrimmomaticPE: Completed successfully`

Gunzipping Trimmomatic result:
- `gunzip -c amp_res_1_output_paired.fq.gz > amp_res_1_output_paired.fq`
- `gunzip -c amp_res_2_output_paired.fq.gz > amp_res_2_output_paired.fq`

Manually checking number of reads:
- `wc -l amp_res_1_output_paired.fq`

1785036 amp_res_1_output_paired.fq

1785036 / 4 = 446259

- `wc -l amp_res_2_output_paired.fq`

1785036 amp_res_2_output_paired.fq

1785036 / 4 = 446259, same here.

Running fasqc check on Trimmomatic output:
- `fastqc -o . amp_res_1_output_paired.fq amp_res_2_output_paired.fq`

For forward:
#TODO insert screenshot
For reverse:
#TODO insert screenshot

Basic statistics on number of reads come along with manually calculated data (both 446259).
Trimming didn’t improve per tile sequence quality (for amp_res_1) and per base sequence content.


