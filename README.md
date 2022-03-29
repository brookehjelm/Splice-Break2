# Splice-Break2: Mitochondrial DNA Deletion Pipeline with Filtering, Annotation, and Batch Processing Capabilities
Splice-Break2 is updated to include bbmap processing, MapSplice 2 alignment, filtering steps, and MITOMAP annotations not presented in the previous version. Splice-Break2 is also capable of batch processing a large number of fastq files when presented in the same directory, can be used with both single-end and paired-end datasets, and includes an alternate reference sequence option to handle a uncommon (but possible) segmentation fault error that we’ve observed in a small number of NGS flowcell runs.  

For a detailed description of methods, see:
  - Hjelm *et al*. “Splice-Break: exploiting an RNA-seq splice junction algorithm to discover mitochondrial DNA deletion breakpoints and analyses of psychiatric disorders”. https://doi.org/10.1093/nar/gkz164
  - Xu *et al*. “Splice-Break2: Mitochondrial DNA Deletion Pipeline with Filtering, Annotation, and Batch Processing Capabilities” [link TBD]

# Dependencies
## Provided within Splice-Break2 Directory
[bbmap](https://jgi.doe.gov/data-and-tools/bbtools)  
[MapSplice 2 (v2.2.0+)](http://www.netlab.uky.edu/p/bioinfo/MapSplice2)  
[Samtools (v1.8+)](http://www.htslib.org/download)  
## Not Provided
Python (v2.7.5+)  
Java (v1.8.0+)  

# Installation 
- Download `Splice-Break2-v3.0.0_PAIRED-END.tar.gz` or `Splice-Break2-v3.0.0_SINGLE-END.tar.gz` 
- Extract Directory: `tar -xvf Splice-Break2-v3.0.0_PAIRED-END.tar.gz` or `tar -xvf Splice-Break2-v3.0.0_SINGLE-END.tar.gz`
- Display \<SB_PATH\>: `readlink -f Splice-Break2-v3.0.0_PAIRED-END` or `readlink -f Splice-Break2-v3.0.0_SINGLE-END`


# Usage 
## Command Line 
### Paired-End
`./Splice-Break2_paired-end.sh <inputDir> <outputDir> <logDir> <SB_Path> [options]`

### Single-End
`./Splice-Break2_single-end.sh <inputDir> <outputDir> <logDir> <SB_Path> [options]`

## Input Files [FASTQ(s)]
### Paired-End
- Files must end with a read identifier (.R1/.R2; .r1/.r2; .READ1/.READ2; .read1/.read2) and extension (.fq, .fastq, .txt). A period must be placed before the read identifier and extension. Examples are shown below (all possible combinations of read identifiers and extensions are acceptable).
    - \<sample\>.read1.fq and \<sample\>.read2.fq
    - \<sample\>.R1.fastq and \<sample\>.R2.fastq
    - \<sample\>.READ1.txt and \<sample\>.READ2.txt
    
### Single-End
- Files must end with an extension (.fq, .fastq, .txt). A period must be placed before the extension. Examples are shown below.
    - \<sample\>.fq 
    - \<sample\>.fastq 
    - \<sample\>.txt 

## Command Line (Required Directories)

1. \<inputDir\>:    path to directory containing FASTQ file(s)
2. \<outputDir\>:   path to directory where output files will be written
3. \<logDir\>:      path to directory where log files will be written
4. \<SB_Path\>:     path to Splice-Break2 parent directory 
                    (/PATH/Splice-Break2_v3.0.0_PAIRED-END or /PATH/Splice-Break2_v3.0.0_SINGLE-END)

## Command Line (Options)
- --align=[yes/no]:  
  -  **--align=yes is DEFAULT**.  
  -  --align=yes will align fastq files with MapSplice 2 and proceed with annotation and filtering steps of the Splice-Break2 pipeline.  
  -  --align=no will only do pre-alignment steps (bbmap; PCR replicate and adapter removal) but process will stop before MapSplice 2 alignment.  
- --ref=[rCRS/Nsub]:  
  - **--ref=rCRS is DEFAULT.**  
  - --ref=rCRS will use the revised Cambridge Reference Sequence (rCRS; NC_012920.1) of the human mitochondrial genome for alignment.  
  - --ref=Nsub will use a modified version of the rCRS sequence, where the first and last 100 bases have been replaced with N’s so that reads cannot align to these regions. We typically only use the NSub option if initial attempts to align with rCRS fail, and this option is suggested in the  log file when that occurs.  
- --fastq_keep=[yes/no]:  
  -  **--fastq_keep=no is DEFAULT.**  
  -  --fastq_keep=no will remove the replicate FASTQ files in the output directory that have the PCR replicates and adapter removed (e.g., \<sample\>.deduped.fq and \<sample\>.deduped.deAdpat.fq). 
  -  --fastq_keep=yes will retain these files for the user. The number (and %) of reads containing PCR replicates and adapter sequence that were removed from the original FASTQ files are recorded in the log file \<logDir\>, regardless if these files are kept or not.  
- --skip_preAlign=[yes/no]:  
  - **--skip_preAlign=no is DEFAULT.**  
  - --skip_preAlign=no will use bbmap to remove PCR replicates and adapter sequences from FASTQ files prior to alignment. 
  - --skip_preAlign=yes will skip these steps (not suggested unless steps were performed with alternate program).  

**Note: If the user wishes to change any of the command options from the DEFAULT settings (above), all four command options must be specified in ORDER (--align, --ref, --fastq_keep, --skip_preAlign).**




# Versioning
Semantic Versioning 3.0.0

# Contact
**Brooke Hjelm**<br/>
bhjelm@usc.edu<br/>

**Lili Xu**<br/>
lilixu@usc.edu<br/>

**Michelle Webb**<br/>
michelgw@usc.edu<br/>

