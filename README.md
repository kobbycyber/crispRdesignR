# crispRdesignR
Software used to design guide RNA sequences for CRISPR/Cas9 genome editing

This software aims to provide all scientifically pertinent information when designing guide RNA sequences for Cas9 genome editing. When provided a target DNA sequence for editing, a genome to check for off-targets in, and a genome annotation file (.gtf) to provide addition information about off-target matches it will out put information for two separate data tables. The first table contains all information on the generated sgRNA themselves (sgRNA sequence, PAM, Direction, Start, End, GC content, Presence of Homopolymers, Self Complementarity, Effciency Score (Doench 2014), and Genomic Matches). The second table contains all information on the found off-target sequences (Original sgRNA Sequence, Chromosome, Start, End, Number of Mismatches, Direction, CFD Scores, Matched Sequence, Gene ID, Gene Name, Sequence Type, and Exon Number). Additionally, a user may provide their own DNA libraries to search for off targets in and use a genome annotation file of their preference.

For a version of this software with a User interface through the Shiny package, see: https://github.com/dylanbeeber/Cas9-Guide-Designer.

## Requirements
This package requires three supplemental files:

Doench_Model_Weights_Singleonly.csv and Doench_Model_Weights_Doubleonly.csv - Two data tables used to assist with efficiency scoring. These must be put in the working directory when using the sgRNA_design function.

CFD_Scoring.csv - A data table that contains the information used to calculate the off-target effects of off-target sequences.

## Dependencies
The stringr package: `install.packages("stringr", repos='http://cran.us.r-project.org')`

The BioStrings and BSgenome packages through Bioconductor: `source("https://bioconductor.org/biocLite.R")`
Then: `biocLite("Biostrings", "BSgenome", "BSgenome.Hsapiens.UCSC.hg19", "AnnotationHub")` The user may choose to download other genomes and use them with the crispRdesignR package.

Devtools (to install this package directly from github): `install.packages("devtools")`
Then ensure you have a working development environment: https://www.r-project.org/nosvn/pandoc/devtools.html
To install crispRdesignR: `install_github(dylanbeeber/crispRdesignR)`

## Usage

All of the required files must present in the working directory, this includes Doench_Model_Weights_Singleonly.csv, Doench_Model_Weights_Doubleonly.csv, CFD_Scoring.csv, and a user supplied genome annotation file (.gtf). Genome annotation files can be found here: https://useast.ensembl.org/info/data/ftp/index.html

##### sgRNA Design

All data is generated with a single function: `sgRNA_design(userseq, genomename, gtfname, calloffs = TRUE, annotateoffs = TRUE)`

`userseq`: The target sequence to generate sgRNA guides for. Can either be a character sequence containing DNA bases or the name of a fasta/text file in the working directory.

`genomename`: The name of a geneome (in BSgenome format) to check for off-targets in. These genomes can be downloaded through BSgenome or compiled by the user.

`gtfname`: The name of a genome annotation file (.gtf) in the working directory to check off-target sequences against.

`calloffs`: If TRUE, the function will search for off-targets in the genome chosen specified by the genomename argument. If FALSE, off-target calling will be skipped.

`annotateoffs`: If TRUE, the function will provide annotations for the off-targets called using the genome annotation file specified by the gtfname argument. If FALSE, off-target annotation will be skipped.

##### sgRNA Data Retrieval

The data on the generated sgRNA sequences can be retrieved with: `getsgRNAdata(x)`

`x`: The raw data generated by `sgRNA_design()`

##### Off-Target Data Retrieval

The additional off-target data can be retrieved with `getofftargetdata(x)`

`x`: The raw data generated by `sgRNA_design()`

###### Example:
`alldata <- sgRNA_design("GGCAGAGCTTCGTATGTCGGCGATTCATCTCAAGTAGAAGATCCTGGTGCAGTAGG", BSgenome.Scerevisiae.UCSC.sacCer2, "Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz")`
`sgRNAdata <- getsgRNAdata(alldata)`
`offtargetdata <- getofftargetdata(alldata)`

###### Example:
`exampledata <- sgRNA_design("DAK1.fasta", BSgenome.Scerevisiae.UCSC.sacCer2, "Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz", calloffs = TRUE, annotateoffs = FALSE)`
