# Massive-haplotypes-underlie-ecotypic-differentiation-in-sunflowers
## Подготовительный этап
### Скачивание VCF и индексного файла
```
wget https://ubc-sunflower-genome.s3.us-west-2.amazonaws.com/VCFs/Ha412v2/Annuus.ann_gwas.tranche90_snps_bi_AN50_beagle_AF99.vcf.gz
wget https://ubc-sunflower-genome.s3.us-west-2.amazonaws.com/VCFs/Ha412v2/Annuus.ann_gwas.tranche90_snps_bi_AN50_beagle_AF99.vcf.gz.tbi
```
Файл взят из базы данных HeliantHome http://www.helianthome.org/download/#phenotype
### Фильтрация нужных SNP

Фильтрация нужных SNP
```
bcftools view -R pos.txt -o filtered_snps.vcf -O v Annuus.ann_gwas.tranche90_snps_bi_AN50_beagle_AF99.vcf.gz 
```
"Потерялись" следующие фенотипы после фильтрации:
* Leaf C N ratio  
* Leaf eccentricity   
* Seed distal eccentricity   
* Trichomes density leaf center secondary veins   
* Trichomes density vein average  
Сравнила образцы в VCF файле и образцы, для которых было сделано фенотипирование, они идентичны, в обоих файлах представлено 614 линий

## Перенос на другую сборку
### Скачивание сборки Ha412HOv2.0
```
wget https://ubc-sunflower-genome.s3.amazonaws.com/references/HA412/genome/Ha412HOv2.0-20181130.fasta
```
Активация среды (snplift):
```
source ~/.bashrc
conda activate snplift_env
```
**Меняем snplift_config.sh**
```
# Input files
export OLD_GENOME="/mnt/users/erofeevan/Sunflowerall/Ha412HOv2.0-20181130.fasta"
export NEW_GENOME="/mnt/reference/genomes/heliantus_annuus/GCF_002127325.2/GCF_002127325.2_HanXRQr2.0-SUNRISE_genomic.fna"
# Output files
export INPUT_FILE="/mnt/users/erofeevan/Sunflowerall/filtered_snps.vcf"
export OUTPUT_FILE="/mnt/users/erofeevan/Sunflowerall/filtered_snpsHanXRQr2.0.vcf"
# Поменять флаг - если геном не проиндексирован, то надо флаг 0!
```
**Запустить скрипт**
```
time ./snplift 02_infos/snplift_config.sh
```
Лог:  
SNPLift: Number of SNPs treated at each step  
42      Positions  
42      Scores  
42      Transferable  
SNPLift: Percentage of transferred SNPs:  
100.000%  
SNPLift: Run completed  
End: 20240918_132128  
real    1m27.172s  
user    0m32.662s  
sys     0m24.950s  

## Считаем частоты аллелей
Активация среды, где есть vcftools:
```
source ~/.bashrc
conda activate GWAS-PIPELINE
```
Подсчет частоты аллелей:
```
vcftools --vcf filtered_snpsHanXRQr2.0.vcf --freq --out allele_frequencies
```
Файл с частотами аллелей приложен в репозитории: https://github.com/erofeeva-plastilin/Massive-haplotypes-underlie-ecotypic-differentiation-in-sunflowers/blob/main/allele_frequencies.frq

EMMAX использует файл .tped для хранения информации о генотипах, и в аддитивной модели она кодирует аллели числами (1 и 2). Эффектный аллель в модели — это тот, который закодирован как 2 в файле .tped. Это обычно соответствует мажорному аллелю, то есть тому, который встречается чаще в популяции.

![image](https://github.com/user-attachments/assets/371eea0b-e132-4601-bd64-a576e58ad8e8)

Для фильтрации мощности:
```
awk -F',' '$2 == 45111025 || $2 == 8243444 || $2 == 4467336 || $2 == 21801953 || $2 == 165672035' DTB.csv > DTB.txt
```
```
awk -F',' '$2 == 132685321 || $2 == 139514388 || $2 == 9666517 || $2 == 29938959 || $2 == 165672035' TLN.csv > TLN.txt
```
```
awk -F',' '$2 == 45111025 || $2 == 181210680 || $2 == 118850933 || $2 == 135718758 || $2 == 136571815 || $2 == 165672035 || $2 == 23872475 || $2 == 55053233' DTF.csv > DTF.txt
```
```
awk -F',' '$2 == 196972495' IL.csv > IL.txt
```
```
awk -F',' '$2 == 9551550 || $2 == 65929716' LeafcurvedHeightmaxWidth.csv > LeafcurvedHeightmaxWidth.txt
```
```
awk -F',' '$2 == 123950977 || $2 == 37030734 || $2 == 21801953 || $2 == 165672035 || $2 == 23872475' Primary_branches.csv > Primary_branches.txt
```
```
awk -F',' '$2 == 180416349 || $2 == 165672035 || $2 == 132367715' Plant_height_at_flowering.csv > Plant_height_at_flowering.txt
```
```
awk -F',' '$2 == 124117180 || $2 == 24390217 || $2 == 146135316 || $2 == 180697556 || $2 == 28829950' LIR.csv > LIR.txt
```
```
awk -F',' '$2 == 124355109 || $2 == 158100889 || $2 == 65929716 || $2 == 2652319' LSIE_I.csv > LSIE_I.txt
```
```
awk -F',' '$2 == 84015571' LSIE_II.csv > LSIE_II.txt
```
```
awk -F',' '$2 == 35358089 || $2 == 140484458' LTC.csv > LTC.txt
```
```
awk -F',' '$2 == 86877790' PMVC.csv > PMVC.txt
```
```
awk -F',' '$2 == 5457435 || $2 == 125298226' RGB.csv > RGB.txt
```
```
awk -F',' '$2 == 29970141 || $2 == 52031318 || $2 == 194869668' TR.csv > TR.txt
```
```
awk -F',' '$2 == 143795931' TR1.csv > TR1.txt
```
```
awk -F',' '$2 == 131245450' TR2.csv > TR2.txt
```
```
awk -F',' '$2 == 9786450' SPE.csv > SPE.txt
```

![image](https://github.com/user-attachments/assets/c6d3aae2-1f4d-4778-8a73-99a8a81a8ceb)
Цветом обозначены частоты аллелей (красный - мажорная, синий - минорная), в столюце beta1 зеленым обозначен эффект больше 5.

Результаты фильтрации и поиска бета 1 в папке: https://github.com/erofeeva-plastilin/Massive-haplotypes-underlie-ecotypic-differentiation-in-sunflowers/tree/main/Phenotype
Ссылка на финальную таблицу: https://docs.google.com/spreadsheets/d/11D7-WJqfNM710nf4aFO29UhfeMguLUZKwjyEQFpdTlo/edit?gid=1270894803#gid=1270894803
