#UNIX Assignment

###Attributes of fang_et_al_genotypes

Here is my snippet of code used for data inspection:

`wc fang_et_al_genotypes.txt`

`awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt`

`du -h fang_et_al_genotypes.txt`

By inspecting this file I learned that:

1. The .txt file contains 2744038 words, 2783 lines, and 11051939 characters
2. The .txt file is comprised of 986 columns
3. The .txt file size is 6.1M

###Attributes of snp_position.txt

Here is my snippet of code used for data inspection:

`wc snp_position.txt`

`awk -F "\t" '{print NF; exit}' snp_position.txt`

`du -h snp_position.txt`

By inspecting this file I learned that:

1. The .txt file contains 13198 words, 984 lines, and 82763 characters
2. The .txt file is comprised of 15 columns 
3. The .txt file size is 38 K 

##Data Processing

###Maize Data

Here is my snippet of code used for data processing:

`awk 'NR==1 || /ZMM/' fang_et_al_genotypes.txt | cut -f 4-986 > Maize_phenotypes.txt`

`awk -f transpose.awk Maize_phenotypes.txt > transposed_maize_genotypes.txt`

`sort -k1,1 transposed_maize_genotypes.txt > Maize_sorted.txt`

`cut -f1,3,4 snp_position.txt | sed '1d;$d' | sort -k1,1 > snp_position_sorted.txt`

`join -t $'\t' -1 1 -2 1 snp_position_sorted.txt Maize_sorted.txt > Maize_snp_sorted.txt`

`awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Maize_snp_sorted.txt | awk 'NR==1 || /unknown/' > unknown_maize_snp.txt`

`awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Maize_snp_sorted.txt | awk 'NR==1 || /multiple/' > multiple_maize_snp.txt`

`for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' maize_snp_sorted.txt | sort -k3,3n | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_maize_increasing.txt; done`

`for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' maize_snp_sorted.txt | sort -k3,3nr | sed 's/?/-/g' | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_maize_decreasing.txt; done`

Here is my brief description of what this code does:

* SNP_ID and SNP data for maize (group = ZMMIL, ZMMLR, and ZMMMR) is extracted from fang_et_al_genotypes.txt with header, transposed and sorted based on SNP_ID. The header and columns other than SNP_ID, Chromsome and Position are removed from snp_position.txt and sorted based on SNP_ID. The resulting two sorted files created are subsequently joined.
* For all SNPs with unknown or multiple positions in the genome (third column), their data is extracted from the joined file after a header indicating SNP_ID, Chromosome, Position and Genotype Data is added.
* A for loop is utilized to separate the data based on chromosome, sorted based on increasing or decreasing positions on the third column and a header is added to indicate SNP_ID, Chromosome, Position and Genotype Data within the file. FOr files with decreasing positions, missing data encoded by ? is replaced by - within the for loop. 

###Teosinte Data

Here is my snippet of code used for data processing:

`awk 'NR==1 || /ZMP/' fang_et_al_genotypes.txt | cut -f 4-986 > Teosinte_phenotypes.txt`

`awk -f transpose.awk Teosinte_phenotypes.txt > transposed_teosinte_genotypes.txt`

`sort -k1,1 transposed_teosinte_genotypes.txt > Teosinte_sorted.txt`

`join -t $'\t' -1 1 -2 1 snp_position_sorted.txt Teosinte_sorted.txt > Teosinte_snp_sorted.txt`

`awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Teosinte_snp_sorted.txt | awk 'NR==1 || /unknown/' > unknown_teosinte_snp.txt`

`awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Teosinte_snp_sorted.txt | awk 'NR==1 || /multiple/' > multiple_teosinte_snp.txt`

`for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' Teosinte_snp_sorted.txt | sort -k3,3n | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_teosinte_increasing.txt; done`

`for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' Teosinte_snp_sorted.txt | sort -k3,3nr | sed 's/?/-/g' | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_teosinte_decreasing.txt; done`

Here is my brief description of what this code does:

* SNP_ID and SNP data for teosinte (group = ZMPBA, ZMPIL, and ZMPJA) is extracted from fang_et_al_genotypes.txt with header, transposed and sorted based on SNP_ID. This sorted file, alongside the snp_position.txt sorted file mentioned in "Maize Data", are subsequently joined.
* For all SNPs with unknown or multiple positions in the genome (third column), their data is extracted from the joined file after a header indicating SNP_ID, Chromosome, Position and Genotype Data is added.
* A for loop is utilized to separate the data based on chromosome, sorted based on increasing or decreasing positions on the third column and a header is added to each column to describe the data within that colum. FOr files with decreasing positions, missing data encoded by ? is replaced by - within the for loop. 
