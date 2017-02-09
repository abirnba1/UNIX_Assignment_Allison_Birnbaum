# UNIX_Assignment_Allison_Birnbaum
#I will call this assignment Jon Snow, because I know nothing

*Data inspection

*Lines, words, bytes


$ wc fang_et_al_genotypes.txt
    2783  2744038 11054722 fang_et_al_genotypes.txt

*Disc Usage (Disc space of a file)


$ du fang_et_al_genotypes.txt
10796   fang_et_al_genotypes.txt


$ du -h fang_et_al_genotypes.txt
11M     fang_et_al_genotypes.txt


*Use of head to inspect the document resulted a large amount of data being printed, suggesting this is a large file. Using the ‘less’ command makes it much easier to look through the file.
Columns


$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt

986


*Snp_position.txt


$ wc snp_position.txt

  984 13198 83747 snp_position.txt


$ du -h snp_position.txt

84K     snp_position.txt


$ head -n 1 snp_position.txt

SNP_ID  cdv_marker_id   Chromosome      Position        alt_pos mult_positions  amplicon      cdv_map_feature.name     gene    candidate/random        Genaissance_daa_id      Sequenom_daa_idcount_amplicons count_cmf       count_gene


$ awk -F "\t" '{print NF; exit}' snp_position.txt

15

*Data Processing


$ cut -f 1-4 fang_et_al_genotypes.txt | head

Sample_ID       JG_OTU  Group   abph1.20

SL-15   T-aust-1        TRIPS   ?/?

SL-16   T-aust-2        TRIPS   ?/?

SL-11   T-brav-1        TRIPS   ?/?

SL-12   T-brav-2        TRIPS   ?/?

SL-18   T-cund  TRIPS   ?/?

SL-2    T-dact-1        TRIPS   ?/?

SL-4    T-dact-2        TRIPS   ?/?

SL-6    T-dact-3        TRIPS   ?/?

SL-7    T-dact-4        TRIPS   ?/?


$ grep "ZMMIL" fang_et_al_genotypes.txt > maizeIL.txt | head -n 3


$ grep "ZMMLR" fang_et_al_genotypes.txt > maizeLR.txt


$ grep "ZMMMR" fang_et_al_genotypes.txt  > maizeMR.txt


*Created three files for each type of Maize data then combined them together into one file to transpose


$ cat maizeIL.txt maizeLR.txt maizeMR.txt > maize_genotypes.txt | head -n 1


$  awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt


$ wc -l transposed_maize_genotypes.txt

986 transposed_maize_genotypes.txt


*Repeated for teosinte data and check the number of lines


$ grep "ZMPBA" fang_et_al_genotypes.txt  > teoBA.txt


$ grep "ZMPIL" fang_et_al_genotypes.txt  > teoIL.txt


$ grep "ZMPJA" fang_et_al_genotypes.txt  > teoJA.txt


$ cat teoBA.txt teoIL.txt teoJA.txt > teosinte_genotypes.txt


$  awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt


$ wc -l transposed_teosinte_genotypes.txt

986 transposed_teosinte_genotypes.txt


*Now I have two transposed txt files, one containing all the maize genotypes and another containing the teosinte genotypes 
First must sort the snp_position.txt file before I can join it with the two transposed files


$ sort -c -k1,1 snp_position.txt

sort: snp_position.txt:2: disorder: abph1.20    5976    2       27403404                      abph1    AB042260        abph1   candidate       8393    10474   1       1       1


$ sort -k1,1 transposed_maize_genotypes.txt > sorted_maize.txt


$ sort -k1,1 transposed_teosinte_genotypes.txt > sorted_teosinte.txt


*I tried to join the files using the following code but it did not work


$ join -1 1 -2 1 sorted_snp_position.txt sorted_teosinte.txt > snp_teosinte.txt


*So I tried to cut the sorted snp data and will retry the join


$ cut -f 1-4 sorted_snp_position.txt > short_sort_snp.txt


$ join -1 1 -2 1 -a 1 short_sort_snp.txt sorted_teosinte.txt > snp_teosinte.txt


$ head snp_teosinte.txt

abph1.20 5976 2 27403404

abph1.22 5978 2 27403892

ae1.3 6605 5 167889790

ae1.4 6606 5 167889682

ae1.5 6607 5 167889821

an1.4 5982 1 240498509

ba1.6 3463 3 181362952

ba1.9 3466 3 181362666

bt2.5 5983 4 66290049

bt2.7 5985 4 66290994


$ join -1 1 -2 1 -a 1 short_sort_snp.txt sorted_maize.txt > snp_maize.txt


$ head snp_maize.txt

abph1.20 5976 2 27403404

abph1.22 5978 2 27403892

ae1.3 6605 5 167889790

ae1.4 6606 5 167889682

ae1.5 6607 5 167889821

an1.4 5982 1 240498509

ba1.6 3463 3 181362952

ba1.9 3466 3 181362666

bt2.5 5983 4 66290049

bt2.7 5985 4 66290994


*Joining the files does not seem to be working as there is an issue with the sorted genotypes files. 
To try and work around this I have allowed the join function to print unprintable data from the sported genotype files


$ join -1 1 -2 1 -a 2 short_sort_snp.txt sorted_maize.txt > snp_maize.txt


$ join -1 1 -2 1 -a 2 short_sort_snp.txt sorted_teosinte.txt > snp_teosinte.txt


*I’m not sure this helped my problem


$ wc snp_maize.txt

986 1550978 6240114 snp_maize.txt


$ wc snp_teosinte.txt

986  961350 3873338 snp_teosinte.txt


$ head -n 2 snp_maize.txt

?/? ?/? ?/? ?/? ?/? ?/? C/C ?/? C/C C/C ?/? C/C C


*That didn’t work… I will try again tomorrow on a mac. 
no changes on the mac, problem with the methodology. Used codes provided by T.Nolan

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ head -n1 fang_et_al_genotypes.txt > tn_maize_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ head -n1 fang_et_al_genotypes.txt > tn_teosinte_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ grep -E -w "ZMMIL|ZMMLR|ZMMMR" fang_et_al_genotypes.txt >> tn_maize_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ grep -E -w "ZMPBA|ZMPIL|ZMPJA" fang_et_al_genotypes.txt >> tn_teosinte_genotypes.tx        t

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ awk -f transpose.awk tn_maize_genotypes.txt > tn_transposed_maize_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ awk -f transpose.awk tn_teosinte_genotypes.txt > tn_transposed_teosinte_genotypes.t        xt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ cut -f1,3,4 snp_position.txt > tn_snp_position_short.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ tail -n+4 tn_transposed_maize_genotypes.txt | sort -k1,1 > tn_transposed_maize_genotypes_sorted.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ tail -n+4 tn_transposed_teosinte_genotypes.txt | sort -k1,1 > tn_transposed_teosinte_genotypes_sorted.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ tail -n+2 tn_snp_position_short.txt | sort -k1,1 > tn_snp_position_short_sorted.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ join -t $'\t' -1 1 -2 1 tn_snp_position_short_sorted.txt tn_transposed_teosinte_genotypes_sorted.txt > tn_joined_teosinte_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ join -t $'\t' -1 1 -2 1 tn_snp_position_short_sorted.txt tn_transposed_maize_genotypes_sorted.txt > tn_joined_maize_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ sed 's/?/-/g' tn_joined_maize_genotypes.txt > edited_maize_genotypes.txt

Owner@Allie-Sunshine MINGW64 ~/desktop/BCB546X-Spring2017/UNIX_Assignment (master)
$ sed 's/?/-/g' tn_joined_teosinte_genotypes.txt > edited_teosinte_genotypes.txt

