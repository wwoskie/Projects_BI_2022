# Project 1 Antibiotic resistance
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


