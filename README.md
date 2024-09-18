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
