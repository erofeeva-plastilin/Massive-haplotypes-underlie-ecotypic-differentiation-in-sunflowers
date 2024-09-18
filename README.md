# Massive-haplotypes-underlie-ecotypic-differentiation-in-sunflowers
## Подготовительный этап
### Скачивание VCF
```
wget https://ubc-sunflower-genome.s3.us-west-2.amazonaws.com/VCFs/Ha412v2/Annuus.ann_gwas.tranche90_snps_bi_AN50_beagle_AF99.vcf.gz
```
Файл взят из базы данных HeliantHome http://www.helianthome.org/download/#phenotype
### Фильтрация нужных SNP
Создание индексного файла
```
bcftools index Annuus.ann_gwas.tranche90_snps_bi_AN50_beagle_AF99.vcf.gz
```
Фильтрация нужных SNP
```
bcftools view -R pos.txt -o filtered_snps.vcf -O v Annuus.ann_gwas.tranche90_snps_bi_AN50_beagle_AF99.vcf.gz
```
