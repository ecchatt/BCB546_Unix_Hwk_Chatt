# BCB546_Unix_Hwk_Chatt
Unix assignment for BCB546X

##_Data Inspection_

I will upload on both Slack and GitHub (look for the "UNIX_Assignment" folder) two data files for you to inspect and describe with the various UNIX programs we have learned about over the last few weeks. Use these programs to become familiar with the files and to describe their structure and their dimensions (file size, number of columns, number of lines, ect...)

The files are:

1. `fang_et_al_genotypes.txt`: a published SNP data set including maize, teosinte (i.e., wild maize), and Tripsacum (a close outgroup to the genus _Zea_) individuals


 
2. `snp_position.txt`: an additional data file that includes the SNP id (first column), chromosome location (third column), nucleotide location (fourth column) and other information for the SNPs genotyped in the `fang_et_al_genotypes.txt` file

###To find the number of lines, words, and characters in each file
'$wc fang_et_al_genotypes.txt snp_position.txt'
	2783  2744038 11051939 fang_et_al_genotypes.txt
     	984    13198    82763 snp_position.txt
    	3767  2757236 11134702 total

###To find the column header names
'$head -n 1 snp_position.txt' 
SNP_ID	cdv_marker_id	Chromosome	Position	alt_pos	mult_positions	amplicon	cdv_map_feature.name	gene	candidate/random	Genaissance_daa_id	Sequenom_daa_id	count_amplicons	count_cmf	count_gene

'$head -n 1 fang_et_al_genotypes.txt'
	Standard out was too large for meaningful information

###To find the file size in human readable form
'$du -h fang_et_al_genotypes.txt snp_position.txt' 
11M	fang_et_al_genotypes.txt
84K	snp_position.txt

###To find the number of columns
'$awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt' 
986
$awk -F "\t" '{print NF; exit}' snp_position.txt 
15 (note this matches the output from head)

###To double check the previous result from awk to make sure the result was not skewed by multiple lines of header information
'$tail -n +6 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}''
986
'$tail -n +6 snp_position.txt | awk -F "\t" '{print NF; exit}''
15

##_Data Processing_

Our goal is to process these files with UNIX tools in order to format them for a downstream analysis (we won't actually be carrying out this analysis, just preparing the input files...something you'll likely need to do time and again). During this process, we will need to `join` (hint, hint) these data sets so that we have both genotypes and positions in a series of input files. All our files will be formatted such that the first column is "SNP_ID", the second column is "Chromosome", the third column is "Position", and subsequent columns are genotype data from either maize or teosinte individuals.

For maize (Group = ZMMIL, ZMMLR, and ZMMMR in the third column of the `fang_et_al_genotypes.txt` file) we want 20 files in total:

* 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?

* 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -

For teosinte (Group = ZMPBA, ZMPIL, and ZMPJA in the third column of the `fang_et_al_genotypes.txt` file) we want 20 files in total:

* 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?

* 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -

A total of 40 files will therefore be produced.




