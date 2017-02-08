# BCB546_Unix_Hwk_Chatt
Unix assignment for BCB546X

##_Data Inspection_

The files are:

1. `fang_et_al_genotypes.txt`: a published SNP data set including maize, teosinte (i.e., wild maize), and Tripsacum (a close outgroup to the genus _Zea_) individuals


 
2. `snp_position.txt`: an additional data file that includes the SNP id (first column), chromosome location (third column), nucleotide location (fourth column) and other information for the SNPs genotyped in the `fang_et_al_genotypes.txt` file

###To find the number of lines, words, and characters in each file
	$wc fang_et_al_genotypes.txt snp_position.txt

2783  2744038 11051939 fang_et_al_genotypes.txt
984    13198    82763 snp_position.txt
3767  2757236 11134702 total

###To find the column header names
	$head -n 1 snp_position.txt

SNP_ID	cdv_marker_id	Chromosome	Position	alt_pos	mult_positions	amplicon	cdv_map_feature.name	gene	candidate/random	Genaissance_daa_id	Sequenom_daa_id	count_amplicons	count_cmf	count_gene

	$head -n 1 fang_et_al_genotypes.txt

Standard out was too large for meaningful information

###To find the file size in human readable form
	$du -h fang_et_al_genotypes.txt snp_position.txt 
11M	fang_et_al_genotypes.txt
84K	snp_position.txt

###To find the number of columns
	$awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt 
986  
	$awk -F "\t" '{print NF; exit}' snp_position.txt 
15 (note this matches the output from head)

###To double check the previous result from awk to make sure the result was not skewed by multiple lines of header information
	$tail -n +6 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'
986
	$tail -n +6 snp_position.txt | awk -F "\t" '{print NF; exit}'
15

##_Data Processing_

All files are formatted such that the first column is "SNP_ID", the second column is "Chromosome", the third column is "Position", and subsequent columns are genotype data from either maize or teosinte individuals.

For maize (Group = ZMMIL, ZMMLR, and ZMMMR in the third column of the `fang_et_al_genotypes.txt` file) we want 20 files in total:

* 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
	Found in the directory "maize_snp_by_chr"

* 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -  
	Found in the directory "maize_snp_by_chr"

For teosinte (Group = ZMPBA, ZMPIL, and ZMPJA in the third column of the `fang_et_al_genotypes.txt` file) we want 20 files in total:

* 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?  
	Found in the directory "teosinte_snp_by_chr"

* 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -  
	Found in the directory "teosinte_snp_by_chr"

##Code to generate files  	

###Remove desired groups from fang_et_al_genotypes.txt
	$grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_groups.txt

###Add header to newly created group of genotypes file
	$head -n 1 fang_et_al_genotypes.txt > header_fang_et_al_genotypes.txt
####THEN
	$cat fang_header teosinte_groups.txt > teosinte_groups_header.txt 

###Transpose the group of genotypes file
	$awk -f transpose.awk teosinte_groups_header.txt > transposed_teosinte_genotypes.txt

###Remove the first 3 rows from the transposed group genotype file
	$tail -n +4 transposed_teosinte_genotypes.txt > trim_teosinte_genotypes.txt

###Sort the two file by SNP ID
	$sort -k1,1 trim_teosinte_genotypes.txt > sort_teosinte_genotypes.txt  

#####or for snp file
	$sort -k1,1 snp_position.txt > sort_snp_position.txt

###Remove head and unneeded columns from the snp_position.txt
	$tail -n +2 sort_snp_position.txt |cut -f 1,3,4 >trim_sort_snp_position.txt

###To join the sorted files
	$join -t $'\t' -1 1 -2 1 no_head_snp_trim.txt sort_Z.txt > join_Z_3.txt

###To pull out the chromosome number
	$awk '$2 ~ /1/' join_Z_2.txt > awk_maize_chr1.txt

###To numerically sort file based on increasing position values
	$sort -k3,3n awk_maize_chr1.txt > sort_maize_chr1.txt

###To sort in reverse
	$sort -k3,3nr awk_maize_chr1.txt > sortr_maize_chr1.txt

###To replace every occurance of a character within a file
	$sed 's/?/-/g' sortr_maize_chr1.txt > sortrs_maize_chr1.txt




