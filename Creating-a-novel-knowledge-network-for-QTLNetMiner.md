This document will guide you through connecting a relatively complete knowledge network to load into the server of QTLNetMiner.

## Required knowledge

How to use a shell, how to download entire folders from the WWW, how to create folders, how to run scripts

## Software needed

We only need Ondex which you can download for free [here](http://www.ondex.org/verify.php). For larger networks, it is advisable to use ondex-mini on a server with > 15 GB of RAM (link will be added later)

## Data needed

In this case, we'll work with _Brassica oleracea_ ([see Ensembl](http://plants.ensembl.org/Brassica_oleracea/Info/Index)) which we'll then link to the already created _Arabidopsis thaliana_ network. The taxonomy ID is 109376, you will need to enter that one into the config.xml in QTLNetMiner's server. Download the peptides and the gene gff3 from [here](ftp://ftp.ensemblgenomes.org/pub/plants/release-26/fasta/brassica_oleracea/pep/) and [here](ftp://ftp.ensemblgenomes.org/pub/plants/release-26/gff3/brassica_oleracea/), this will be our simplest network, the one linking genes to their respective proteins.

## Folder structure

Since we will create a few smaller "sub"-networks it is useful to create folders for each subnetwork. "Officially" each organism's project is structured like this (you can see it "in action" [here](https://ondex.rothamsted.ac.uk/QTLNetMiner/releasenotes/))

```
|-- homology (parental folder for all homology related information)
|   |
|   |-- BioMart (this folder contains all homology information from BioMart)
|   x-- Decypher (this folder contains all homology information from manual BLAST or Decypher runs)
|
|-- ontologies (this folder contains all information from GeneOntology, i.e., `ec2go.txt, gene_ontology.1_2.obo, interpro2go.txt, pfam2go.txt` which you can download [here](http://geneontology.org/page/downloads)
| 
|-- organisms (this folder contains all organism-specific information, we will store most work here)
|   |
|   |-- Brassicaoleracea (this is our newly created folder in which we'll work)
|   |   |-- Gene_Protein (this will contain the gene to protein network)
|   |   x-- Protein_Domain (this will contain any protein domain information we can gather)
|   |       |-- Protein_InterPro (all protein domains from InterPro we can find)
|   |       x-- Protein_PFam  (all protein domains from PFam)
|   |
|   |-- Wheat (etc.)
|
|-- pubmed (this folder contains all MEDLINE/PubMed entries for Arabidopsis, download from https://ondex.rothamsted.ac.uk/QTLNetMiner/releasenotes/pubmed/ )
|
|-- references (this folder contains all completed knowledge graphs, at the time of this writing, it's _A. thaliana_ and UniProtKB, you can download all data from https://ondex.rothamsted.ac.uk/QTLNetMiner/releasenotes/references/ )
|
x-- xnets (this folder contains the final workflows which will pull all of the above folders together)
    |-- brassnet (this folder will contain the final workflow to pull all _Brassica oleracea_ data together)
    x-- wheatnet (etc.)
```
