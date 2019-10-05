Extracting haplotype information from BAM and VCF file for polyploids
======

## About:
This is an edited version of [extracthairs](https://github.com/vibansal/HapCUT2) in which polyploids are also allowed. The goal of this code is to generate fragment file needed for haplotyping algorithm like sdhap, althap, hapcut, HapMC, Hap10, Haptree and H-popG. 

## To build:

```

git clone https://github.com/smajidian/extract_poly
cd extract_poly
make 
```

The makefile will attempt to build samtools 1.2 and htslib 1.2.1 as git submodules.




## Input:
It requires the following input:
- BAM file for an individual containing reads aligned to a reference genome
- VCF file containing only heterzygous SNVs . Complex SNVs and indels should be handled beforehand. 




## Run for Illumina dataset:


(1) Filtering VCF file (removing homozygous and non-SNP variants)



```
cat variants.vcf | grep -v "0/0" | grep -v "1/1" | grep -v "0/0" | grep -v "mnp" > variants_filtered.vcf

```



(2) Using extractHAIRS to convert BAM file to the compact fragment file format containing only haplotype-relevant information. 

```
./build/extractHAIRS  --bam reads.sorted.bam --VCF variants_filtered.vcf --out fragment_file
```


(3) If you need to use the fragment file for sdhap and althap, use

```
python2 $fragpoly -f fragment_file  -o fragment_file_sdhap -x SDhaP 
```


or for haptree
```
python2 $fragpoly -f fragment_file  -o fragment_file_haptree -x HapTree 
```






## Run for 10x dataset:

(1) Filtering VCF file


(2) use extractHAIRS to convert BAM file to the compact fragment file format containing only haplotype-relevant information. 

```
./build/extractHAIRS --10X 1 --bam reads.sorted.bam --VCF variants_filtered.vcf --out unlinked_fragment_file
```

(3) Link fragments into barcode-specific fragment:
```
python3 utilities/LinkFragments_brcd_based.py  unlinked_fragment_file linked_fragment_file
```


(4) If you need to use the fragment file for sdhap and althap, use

```
python2 $fragpoly -f fragment_file  -o fragment_file_sdhap -x SDhaP 
```


or for haptree
```
python2 $fragpoly -f fragment_file  -o fragment_file_haptree -x HapTree 
```





NOTE: It is required that the BAM reads have the BX (corrected barcode) tag.






## Citation:

[Edge, P., Bafna, V. & Bansal, V. HapCUT2: robust and accurate haplotype assembly for diverse sequencing technologies. Genome Res. gr.213462.116 (2016).](http://genome.cshlp.org/content/early/2016/12/09/gr.213462.116.abstract)

[Extracthairs](https://github.com/vibansal/HapCUT2)

Hap10  





