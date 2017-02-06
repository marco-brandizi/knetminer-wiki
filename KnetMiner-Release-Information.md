## Arabidopsis Knowledge Network
Selected datasets are downloaded using a shell script [link]. The datasets are transferred into individual networks and integrated into an Arabidopsis knowledge network (AraKNET) using Ondex workflows [links]. A break down of information types in AraKNET and links to the raw data is listed below.

### Gene-Protein [Ensembl]
In total 32k Arabidopsis genes (including protein-coding and non-coding RNAs) and 35k proteins including alternative splice variants.
* [GFF3](ftp://ftp.ensemblgenomes.org/pub/current/plants/gff3/arabidopsis_thaliana/Arabidopsis_thaliana.TAIR10.34.gff3.gz)
* [FASTA] (ftp://ftp.ensemblgenomes.org/pub/current/plants/fasta/arabidopsis_thaliana/pep/Arabidopsis_thaliana.TAIR10.pep.all.fa.gz)

### Gene-SNP-Phenotype [Ensembl]
* Arabidopsis GWAS data for 107 phenotypes. Phenotypes are grouped into the four categories flowering, developmental, defense and ionomics. For more information see [Atwell et al, 2010, Nature] (http://www.nature.com/nature/journal/v465/n7298/abs/nature08800.html) and [supplementary information] (http://www.nature.com/nature/journal/v465/n7298/extref/nature08800-s1.pdf).
* Data was downloaded from [Ensembl BioMart] (http://plants.ensembl.org/biomart/martview?VIRTUALSCHEMANAME=plants_mart&ATTRIBUTES=athaliana_eg_snp.default.snp.refsnp_id|athaliana_eg_snp.default.snp.associated_variant_risk_allele|athaliana_eg_snp.default.snp.distance_to_transcript|athaliana_eg_snp.default.snp.chr_name|athaliana_eg_snp.default.snp.chrom_start|athaliana_eg_snp.default.snp.ensembl_gene_stable_id|athaliana_eg_snp.default.snp.consequence_type_tv|athaliana_eg_snp.default.snp.phenotype_name|athaliana_eg_snp.default.snp.phenotype_description|athaliana_eg_snp.default.snp.p_value&FILTERS=athaliana_eg_snp.default.filters.phenotype_significance."1"|athaliana_eg_snp.default.filters.distance_to_transcript."1000"&VISIBLEPANEL=resultspanel
)
* SNP-Phenotype relations (122,919 relations) of significant SNPs (as defined by Ensembl, p-value<0.05?) linked to 107 phenotypes; on average 1,150 SNPs per phenotype.
* SNP-Gene relations are based on genes in close proximity to SNPs <1000bp (96,047 relations)
* #SNP=66,816 | #Gene=27,502 | #Phenotype=107
* To see it live, go to [KnetMiner](http://knetminer.rothamsted.ac.uk/Arabidopsis_thaliana/) and search for dormancy, then switch to Map View.

### Gene-GO [GOA]
* Include all evidence types
* [GAF 2.0] (http://www.geneontology.org/gene-associations/gene_association.tair.gz)
* Ondex GAF parser requires additional mapping file of feature type -> Ondex Concept Class


### Gene-Gene Interactions [BioGRID]
* [TAB] (http://thebiogrid.org/downloads/archives/Release%20Archive/BIOGRID-3.4.144/BIOGRID-ORGANISM-3.4.144.tab2.zip)
* Version 3.4.144
* Concepts: 9538 [Gene] | 2326 [Publication]
* Relations: 299 [genetic] | 37756 [physical] | 20044 [pub_in]

### Gene-Phenotype [TAIR]
* [TAB] (ftp://ftp.arabidopsis.org/home/tair/User_Requests/Locus_Published_20130305.txt)

### Publications [PubMed]
* [XML] (http://www.ncbi.nlm.nih.gov/pubmed/?term=arabidopsis+thaliana)
* 57,600 PubMed abstracts
* filter publications that are unconnected, i.e. not linked via citation or text-mining

### Gene-Publication [GOA, TAIR]
* [GAF 2.0] (http://www.geneontology.org/gene-associations/gene_association.tair.gz)
* [TAB] (ftp://ftp.arabidopsis.org/home/tair/User_Requests/Locus_Published_20140331.txt.gz)

### Protein-Publication [UniProtKB, TAIR]
* [At proteome XML] (http://www.uniprot.org/uniprot/?query=proteome:UP000006548&compress=yes&format=xml)
* [TAB] (ftp://ftp.arabidopsis.org/home/tair/User_Requests/Locus_Published_20140331.txt.gz)
* relations (pub_in): 65,780
* concepts (Publication): 15,892

### Protein-Pathway [AraCyc]
* AraCyc Release 14
* biopax-level2.owl
* Translation.tab

### Gene-TO [Text Mining]
* Created 27k Gene-TO sentence based co-occurrences in PubMed abstracts

### Sequence alignments to other Plants
* Protein@Arabidopsis-Protein@Plants::UniProtKB-SP [Smith-Waterman]
  * Concepts: 26663 [Protein]
  * Relations: 100947 [h_s_s]
*  Protein-GO [UniProtKB]
*  Protein-Publication [UniProtKB]

### Orthologs in Yeast
*  Protein@Arabidopsis-Protein@Yeast::UniProtKB-SP [Inparanoid]
*  Protein-GO [UniProtKB]
*  Protein-Publication [UniProtKB]
*  Protein-Protein Interaction [BioGRID]
  * Concepts: 6366 [Protein] | 13725 [Publication]
  * Relations: 462411 [genetic] | 97205 [physical] | 155615 [pub_in]



### Information types contained in Arabidopsis KNET

Concept Class | Concept Count
--------------|------
Pathway | 610
Enzyme	| 8,666
Publication | 70,720
Reaction | 3,193
Protein	| 118,803
Compounds | 3,000
Phenotype | 6,489
MolFunc	| 10,542
Gene | 31,484
SNP | 66,816
BioProc	| 29,400
CelComp	| 4,048
Trait (GWAS) | 108
TO | 1,384
Transport | 95
EC | 2,254
Protcmplx | 193
**Total Concepts** | **368,434**
