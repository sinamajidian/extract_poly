Extracting haplotype information from BAM and VCF file for 10x data
======

## About:
This is an edited version of [extracthairs](https://github.com/vibansal/HapCUT2)   in which polyploids are also allowed.





## To build:

```

git clone https://github.com/smajidian/extract_poly_10x
cd extract_poly_10x
make 
```

The makefile will attempt to build samtools 1.2 and htslib 1.2.1 as git submodules.




## Input:
It requires the following input:
- BAM file for an individual containing reads aligned to a reference genome
- VCF file containing only heterzygous SNVs . Complex SNVs and indels should be handled beforehand. 




## To Run:

Assembling haplotypes requires two steps:

(1) use extractHAIRS to convert BAM file to the compact fragment file format containing only haplotype-relevant information. 

```
./build/extractHAIRS --10X 1 --bam reads.sorted.bam --VCF variants.VCF --out unlinked_fragment_file
```

(2) Link fragments into barcode-specific fragment:
```
python3 utilities/LinkFragments_brcd_based.py  unlinked_fragment_file linked_fragment_file
```




NOTE: It is required that the BAM reads have the BX (corrected barcode) tag.






## Citation:

[Edge, P., Bafna, V. & Bansal, V. HapCUT2: robust and accurate haplotype assembly for diverse sequencing technologies. Genome Res. gr.213462.116 (2016).](http://genome.cshlp.org/content/early/2016/12/09/gr.213462.116.abstract)

[Extracthairs](https://github.com/vibansal/HapCUT2)

Hap10  





