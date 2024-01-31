# Inter-chromosomal contacts demarcate genome topology along a spatial gradient
Signature is a bioinformatics pipeline for determining significant genomic interactions in Hi-C/Omni-C datasets. Signature is able to estimate significant interactions for both intra- and inter-chromosomal interactions genome-wide or for a given region of interest. There are three main requirements, the datasets you want to process, the genomic resolution, and the type of analysis. Signature v29 is not prepared to complete trans analyses at resolutions higher than 500KB, or cis analyses at resolutions higher than 50KB. This pipeline is made to run on the Slurm Linux system - the job-submission scripting will need to be modified in the _user-friendly scheduler_ to match your High Performance Computing system's guidelines. 

# Requirments
### Runs on the Slurm Linux system (modify to your High Performance Computing system's guidelines)

   - For trans analysis of 3 cells: ~16Gb memory, ~12hr runtime
   - For cis analysis of 3 cells: ~100Gb, ~3 days runtime

### Modules

   - bedtools/2.24.0
   - R/4.2.1
   - python/2.7.9

# User-friendly scheduler
batch_create_signature_files.sh

# Main Script
Signature.v29.sh

# Main script dependencies 
trans.1vsAll_LWLR.R

trans.pairwise_LWLR.R

cis_LWLR.R

LWLR_train_cis.R

LWLR_train.R

lowest_zscore_1vsAll.R

lowest_zscore_pairwise.R

merger.awk



# Running interaction analysis with Signature
Signature runs with one simple shell script - the _user-friendly scheduler_ - which has been generated to submit your job in batches of 3 cell types. Larger batches can be submitted depnding on your HPC's processing power and memory limits (see _Making user-specific modifications_ section). 

To get started:
1.	Split up the cells/datasets you want to analyze into batches of 3 and assign each batch a number
2.	Create a general directory where all your scripts, schedulers, output folders etc. will be generated
3.	Copy _batch_create_signature_files.sh_ from above into your directory from step 2
4.	Rename the shell script to be specific to your analysis you want
5.	Make appropriate modifications to the EDIT sections for that batch (line 11-26)

# Making user-specific modifications
The _user-friendly scheduler_ is made to process signature in batches of 3, but can be edited to process more or less.

The following adjustments will need to be made:

1. In the shell script, below line 15, add a new line for each additional cell you want to include following the same format ("cell4=     " etc.)
2. In the shell script, below line 37, as you did in step 1, add corresponding lines for each new cell you want to include following the same format ( "echo $cell4 > $path/batch$batch\_cells.txt" etc. )  
3. String replace "$cell1.$cell2.$cell3" with your new cell-variable string
   - Ex. using _sed_ in command line for a batch of 5 cells:  sed -i 's/$cell1.$cell2.$cell3/$cell1.$cell2.$cell3.$cell4.$cell5/g' batch_create_signature_files.sh
