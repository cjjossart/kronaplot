# Kronaplot Examples
This repository includes a high level overview of order of operations for running fastq files through Kraken2 and KronaTools to create a krona plot. This document also includes resources and references for the tools used. Included in this repository are the kraken2 reports and krona plots for publically availables SRA files (SRR32381984, SRR32381983, SRR32924575).

## Overview of order of operations
1. Gather Input Data for Kraken2 Analysis
   a. Collect Raw Fastq Files
   b. Download or Build Kraken2 Database
2. Run Kraken2
3. Update NCBI taxonomic IDs for KronaTools
4. Run KronaTools to Create Krona Plots
5. Interact and Intepret Krona Plots


## 1. Gather Input Data for Kraken2 Analysis
Kraken2 is a taxonomic classification system that uses k-mer matches to achieve high accuracy and fast classification speed (Wood et al., 2014). Kraken2 will match each read of fastq file to a taxonomic ID as best as possible using its matching algorithm. This will give a user the abundance of reads classified with specific taxonomic IDs. 

### a. Gather the Raw Fastq Files to Be Analyzed
Make sure you have the fastqs accessible. They can be raw data from the sequencer or processed to some degree (i.e. trimmed and scrubbed for human reads).

### b. Donwload or Prepare a Kraken2 Database
You can donwload pre-build databases of various sizes and that include what your are looking for here (https://benlangmead.github.io/aws-indexes/k2​). When running Kraken2 you will only classify reads that have matches to the database you are using. It is important to know what bug you are looking for or may have in the reads.
You can also use Kraken2 to build databases. This involves a few extra steps on the commnad line and will use NCBI refseq and/or a custom collection of fastq. The instructions for building a database with Kraken2 are included here (https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown​).


## 2. Run Kraken2
This line of code can be used to run Kraken2 in the command line. You will need to have it downloaded locally or running Kraken2 within a container that has Kraken2 downloaded and all dependencies includes. I normally work within docker containers and find that to be easier that managing dependencies and tools.

```
kraken2 --db k2_pluspf_16gb_20240605 --report sample_1_report.txt --output sample_1_output.txt --gzip-compressed --paired reads/sample_1_R1.fastq.gz reads/sample_1_R2.fastq.gz

```
kraken2 is the kraken2 command to create the kraken2 report​

--db is the kraken2 database parameter

--report is the parameter to define the report name and location​

--output is the parameter to define the standard output file name and location​

--gzip-compressed and –paired refer to the format of the input fastq files​


## 3. Update NCBI Taxonomic IDs for KronaTools
Similar to Kraken2 you will need to download the KronaTools locally or use a designated container for this process. Once you have KronaTools, you will want to update the NCBI taxonomic data to be the most up to date. This can be done by running this line of code. This resource has a more detailed explanation and guide to the code (https://github.com/marbl/Krona/wiki/KronaTools).

```
bash ktUpdateTaxonomy.sh

```


## 4. Run KronaTools to Create Krona Plot
You can now run KronaTools to create Krona Plots from the Kraken2 reports. This line of code will do this process.

```
ktImportTaxonomy -t 5 -m 3 -o combined_krona.html *.report 
```

KtImportTaxonomy is the KronaTools command to create a krona plot from kraken2 reports​

-t is the parameter for the NCBI taxonomic ID column in the kraken2 report​

-m is the parameter for the magnitude (i.e. fragments, reads) column in the kraken2 report​

-o is the parameter to define the output file name and location​

Input of the kraken2 report(s) follows the previous parameters​


## 5. Interact and Interpret Krona Plots
You can now play around with the krona plots and explore the reads and taxonomic calls. Reference this link (https://github.com/marbl/Krona/wiki) for a more detailed walkthrough of interacting with your krona plots.


## Resources
Kraken2​:
Kraken2 Manual: https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown​
Kraken2 pre-built databases: https://benlangmead.github.io/aws-indexes/k2​
Krona Plots​:
Krona Manual: https://github.com/marbl/Krona/wiki​
KronaTools Manual: https://github.com/marbl/Krona/wiki/KronaTools

## References
1. Wood, D.E., Salzberg, S.L. Kraken: ultrafast metagenomic sequence classification using exact alignments. Genome Biol 15, R46 (2014). https://doi.org/10.1186/gb-2014-15-3-r46​
2. Wood, D.E., Lu, J. & Langmead, B. Improved metagenomic analysis with Kraken 2. Genome Biol 20, 257 (2019). https://doi.org/10.1186/s13059-019-1891-0​
3. Ondov, B.D., Bergman, N.H. & Phillippy, A.M. Interactive metagenomic visualization in a Web browser. BMC Bioinformatics 12, 385 (2011). https://doi.org/10.1186/1471-2105-12-385






 


