## Arabidopsis Knowledge Network

### Gene-Protein [Ensembl]
In total 32k Arabidopsis genes (including protein-coding and non-coding RNAs) and 35k proteins including alternative splice variants.
* [GFF3](ftp://ftp.ensemblgenomes.org/pub/current/plants/gff3/arabidopsis_thaliana/Arabidopsis_thaliana.TAIR10.32.gff3.gz)
* [FASTA] (ftp://ftp.ensemblgenomes.org/pub/current/plants/fasta/arabidopsis_thaliana/pep/Arabidopsis_thaliana.TAIR10.pep.all.fa.gz)

### Gene-SNP-Phenotype [Ensembl]
* [BioMart] (http://plants.ensembl.org/biomart/martview?VIRTUALSCHEMANAME=plants_mart&ATTRIBUTES=athaliana_eg_snp.default.snp.refsnp_id|athaliana_eg_snp.default.snp.associated_variant_risk_allele|athaliana_eg_snp.default.snp.distance_to_transcript|athaliana_eg_snp.default.snp.chr_name|athaliana_eg_snp.default.snp.chrom_start|athaliana_eg_snp.default.snp.ensembl_gene_stable_id|athaliana_eg_snp.default.snp.consequence_type_tv|athaliana_eg_snp.default.snp.phenotype_name|athaliana_eg_snp.default.snp.phenotype_description|athaliana_eg_snp.default.snp.p_value&FILTERS=athaliana_eg_snp.default.filters.phenotype_significance."1"|athaliana_eg_snp.default.filters.distance_to_transcript."1000"&VISIBLEPANEL=resultspanel
)
* SNP-Phenotype: significant SNPs from 107 GWAS studies of different phenotypes (122,919 relations)
* SNP-Gene where distance to transcript <1000bp (96,047 relations)
* #SNP=66,816 
* #Gene=27,502 
* #Phenotype=108

### Gene-GO [GOA]
* Include all evidence types
* [GAF 2.0] (http://www.geneontology.org/gene-associations/gene_association.tair.gz)
* Ondex GAF parser requires additional mapping file of feature type -> Ondex Concept Class


### Gene-Gene Interactions [BioGRID]
* [TAB] (http://thebiogrid.org/downloads/archives/Release%20Archive/BIOGRID-3.4.139/BIOGRID-ORGANISM-3.4.139.tab2.zip)
* Version 3.4.139

### Gene-Phenotype [TAIR]
* [TAB] (ftp://ftp.arabidopsis.org/home/tair/User_Requests/Locus_Published_20130305.txt)

### Publications [PubMed]
* [XML] (http://www.ncbi.nlm.nih.gov/pubmed/?term=arabidopsis+thaliana)
* 57,600 PubMed abstracts
* filter publications that are unconnected, i.e. not linked via citation or text-mining

### Gene-Publication [GOA, TAIR]
* [GAF 2.0] (http://www.geneontology.org/gene-associations/gene_association.tair.gz)
* [TAB] (ftp://ftp.arabidopsis.org/home/tair/User_Requests/Locus_Published_20130305.txt)

### Protein-Publication [UniProtKB]
* [At proteome XML] (http://www.uniprot.org/uniprot/?query=proteome:UP000006548&compress=yes&format=xml)
* relations (pub_in): 34,884
* concepts (Publication): 11,550

### Protein-Pathway [AraCyc]
* AraCyc Release 14
* biopax-level2.owl
* Translation.tab

### Gene-TO [Text Mining]
* Created 27k Gene-TO relations that have one or more evidence sentences

### Sequence alignments to other Plants
* Protein@Arabidopsis-Protein@Plants::UniProtKB-SP [Smith-Waterman]
*  Protein-GO [UniProtKB]
*  Protein-Publication [UniProtKB]

### Orthologs in Yeast
*  Protein@Arabidopsis-Protein@Yeast::UniProtKB-SP [Inparanoid]
*  Protein-GO [UniProtKB]
*  Protein-Publication [UniProtKB]
*  Protein-Protein [BioGRID]


