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
qtlnetminer
|
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
|   x-- Wheat (etc.)
|
|-- pubmed (this folder contains all MEDLINE/PubMed entries for Arabidopsis, download from https://ondex.rothamsted.ac.uk/QTLNetMiner/releasenotes/pubmed/ )
|
|-- references (this folder contains all completed knowledge graphs, at the time of this writing, it's A. thaliana and UniProtKB, you can download all data from https://ondex.rothamsted.ac.uk/QTLNetMiner/releasenotes/references/ )
|
x-- xnets (this folder contains the final workflows which will pull all of the above folders together)
    |-- brassnet (this folder will contain the final workflow to pull all Brassica oleracea data together)
    x-- wheatnet (etc.)
```

For the rest of this document we'll go through each of these folders.

## Homology
### BioMart

For this section, we'll get the required data from BioMart and transform it into a Ondex network. First, go on [BioMart](http://plants.ensembl.org/biomart/martview/b0290503aa21e7aa6465382793942ba3) and choose "Brassica oleracea". 

Click on "attributes", click on "Homologs", for now we'll get the homologs for _A. thaliana_. Under "Gene", de-select everything under "Gene Attributes" and select only "Protein stable ID". Then open "Orthologs" and select "Arabidopsis thaliana protein stable ID", "Homology type", "% identity", "Arabidopsis thaliana % identity" and "Orthology confidence [0 low, 1 high]". 

Then, scroll back up and click "Results" on the top left corner. You'll see a few example results, click on "Export all results to"'s "Go" button to get all results as a tab-delimited file. The header of that file should look something like this:

```
Protein stable ID	Arabidopsis thaliana protein stable ID	Homology type	% identity	Arabidopsis thaliana % identity	Orthology confidence [0 low, 1 high]
Bo7g103870.1	AT4G15630.1	ortholog_many2many	86	86	1
Bo7g103870.1	AT4G15620.1	ortholog_many2many	84	84	1
Bo7g104070.1	AT4G15880.1	ortholog_one2many	75	71	1
Bo7g104270.1					
Bo7g104470.1	AT4G16490.1	ortholog_one2many	87	84	1
(etc.)
```

Now there are a few lines where the _B. oleracea_ proteins did not show a homology to _A. thaliana_ like 'Bo7g104270.1' here, delete these using any tool or programming language you like. Save that table under `homology/BioMart/Boleracea_Arabidopsis.txt`. At this point, it may be advisable to save the URL of the table you generated - on the BioMart page, click "URL" in the top right corner, and you'll get something like: 

```
http://plants.ensembl.org/biomart/martview?VIRTUALSCHEMANAME=plants_mart_26&ATTRIBUTES=boleracea_eg_gene.default.homologs.ensembl_peptide_id|boleracea_eg_gene.default.homologs.athaliana_eg_homolog_ensembl_peptide|boleracea_eg_gene.default.homologs.athaliana_eg_orthology_type|boleracea_eg_gene.default.homologs.athaliana_eg_homolog_perc_id|boleracea_eg_gene.default.homologs.athaliana_eg_homolog_perc_id_r1|boleracea_eg_gene.default.homologs.athaliana_eg_homolog_is_tree_compliant&FILTERS=&VISIBLEPANEL=resultspanel
```

Clicking on that URL should re-generate your table, useful for the future. **IMPORTANT**: as you can see, this URL uses the database "plants_mart_26". When BioMart updates its databases, the old versions seem to get deleted. So if you try to access this table in the feature and get an error, check which database version they've updated to (30? 33?) and replace the number in plants_mart_26 by the appropriate number. The link should then work again (of course, you will get slightly different results since the database has changed).

Let's load that table into Ondex. Open Ondex using `bash ondex/runme.sh`, click on "Start a new graph", and open the console using "Tools -> Console". The language used in the console is similar to Javascript, here is some example code to create a graph out of it:

```Javascript
p = new PathParser(getActiveGraph(), new DelimitedFileReader("qtlnetminer/homology/BioMart/Boleracea_Arabidopsis.txt", "\\t+",1));
c1 = p.newConceptPrototype(defAccession(0,"ENSEMBL_PLANTS",false), defDataSource("ENSEMBL_PLANTS"), defCC("Protein"));
c2 = p.newConceptPrototype(defAccession(1,"TAIR",false), defDataSource("TAIR"), defCC("Protein"));

p.newRelationPrototype(c1, c2, defRT("ortho"), defEvidence("EnsemblCompara"), defAttribute(2, "Homology_type", "TEXT", false), defAttribute(3, "%Identity_Arabidopsis", "NUMBER", false), defAttribute(4, "%Identity_Boleracea", "NUMBER", false), defAttribute(5, "Orthology_confidence", "NUMBER", false));

s = p.parse();
```

Change the path and the data accessions and sources to the ones you have for your data, this should work for _Brassica_ (new concept prototype called c1) and _Arabidopsis_ (c2). If you've used exactly the same order of columns like the above table, then you don't need to change anything in the creation of the new relation prototype. This will run for about 30s on any regular laptop and create a new graph.

Use Ondex' Keyword filter field on the top right to search for any single gene and see if the graph was created correctly (are the nodes named correctly? Is there a connection between the _Arabidopsis_ nodes and the _Brassica_ nodes?). If everything looks OK, export the graph using "File -> Save graph as.." to `homology/BioMart/Boleracea_Arabidopsis.oxl`. It's also advisable to copy/paste the above piece of code into something like `homology/BioMart/Boleracea_Arabidopsis_console.txt` for later. You've now finished your very first graph!

Let's repeat the same thing using homology to _Brassica rapa_: Use the _Brassica oleracea_ dataset, click "Protein stable IDs", and click _Brassica rapa_'s "Homology type", "% identity", "Brassica rapa % identity" and "Orthology confidence [0 low, 1 high]". You'll get a similar table to the above one, export it to tab-delimited text-file in `homology/BioMart/Boleracea_Brapa.txt`, delete the empty lines, and load it into Ondex using slightly changed code to the above:

```Javascript
p = new PathParser(getActiveGraph(), new DelimitedFileReader("qtlnetminer/homology/BioMart/Boleracea_Brapa.txt", "\\t+",1));
c1 = p.newConceptPrototype(defAccession(0,"ENSEMBL_PLANTS",false), defDataSource("ENSEMBL_PLANTS"), defCC("Protein"));
c2 = p.newConceptPrototype(defAccession(1,"ENSEMBL_PLANTS",false), defDataSource("ENSEMBL_PLANTS"), defCC("Protein"));

p.newRelationPrototype(c1, c2, defRT("ortho"), defEvidence("EnsemblCompara"), defAttribute(2, "Homology_type", "TEXT", false), defAttribute(3, "%Identity_Brapa", "NUMBER", false), defAttribute(4, "%Identity_Boleracea", "NUMBER", false), defAttribute(5, "Orthology_confidence", "NUMBER", false));
s = p.parse();
```

As you can see, the column names have changed. Save the code in `homology/BioMart/Boleracea_Brapa_console.txt`, save the graph in `homology/BioMart/Boleracea_Brapa.oxl`.

OPTIONAL: Depending on the organism you use, you can load as many homologies as you'd like in this step.

### Decypher

This folder contains homologies based on Decypher BLAST runs. You can also use "standard" BLAST here as long as you slightly change the code used in Ondex. For this example, we'll describe a Decypher run against the _Brassica napus_ Darmor proteins from [Genoscope](http://www.genoscope.cns.fr/brassicanapus/data/Brassica_napus.annotation_v5.pep.fa.gz). After running Decypher, we received a table like this:

```
QUERYLOCUS      TARGETLOCUS     SCORE   SIGNIFICANCE    PERCENTALIGNMENT        PERCENTQUERY    PERCENTTARGET   QUERYSTART      QUERYEND        TARGETSTART     TARGETEND       QUERYLENGTH     TARGETLENGTH    ALIGNMENTLENGTH
Bo00285s010.1   GSBRNA2T00118962001     326.25  6.4e-089        40      33      29      110     574     114     577     575     657     475
Bo00285s020.1   GSBRNA2T00102232001     162.15  8.0e-040        65      25      12      1       126     1       130     333     680     131
Bo00285s050.1   GSBRNA2T00088828001     142.12  5.3e-034        90      28      22      82      157     27      102     240     301     76
(etc.)
```

Again, save the table with BLAST results as `homology/Decypher/Boleracea_Bnapus_Decypher_BlastP.tab`. We will use all of this information to connect the concepts in the knowledge graph, this is the script to import the above table into Ondex using the Ondex console:

```Javascript
pp = new PathParser(getActiveGraph(), new DelimitedFileReader("qtlnetminer/homology/Decypher/Boleracea_Bnapus_Decypher_BlastP.tab", "\\t+", 1));

c1 = pp.newConceptPrototype(defAccession(0,"ENSEMBL_PLANTS",false), defDataSource("ENSEMBL_PLANTS"), defCC("Protein"));
c2 = pp.newConceptPrototype(defAccession(1,"GENOSCOPE",false), defDataSource("GENOSCOPE"), defCC("Protein"));

pp.newRelationPrototype(c1, c2, defRT("h_s_s"), defEvidence("Decypher_Terra-BlastP")
, defAttribute(2, "SCORE", "NUMBER", false)
, defAttribute(3, "E-VALUE", "NUMBER", false)
, defAttribute(4, "PERCENTALIGNMENT", "NUMBER", false)
, defAttribute(5, "PERCENTQUERY", "NUMBER", false)
, defAttribute(6, "PERCENTTARGET", "NUMBER", false)
, defAttribute(7, "QUERYSTART", "NUMBER", false)
, defAttribute(8, "QUERYEND", "NUMBER", false)
, defAttribute(9, "TARGETSTART", "NUMBER", false)
, defAttribute(10, "TARGETEND", "NUMBER", false)
, defAttribute(11, "QUERYLENGTH", "NUMBER", false)
, defAttribute(12, "TARGETLENGTH", "NUMBER", false)
, defAttribute(13, "ALIGNMENTLENGTH", "NUMBER", false)
);

s = pp.parse();
```

This will insert all of the BLAST details like e-value etc. as attributes to the relation (edges) in the knowledge graph. For posterity, save the code under `homology/Decypher/Boleracea_Bnapus_Decypher_console.txt` and save the resulting knowledge graph in `homology/Decypher/Boleracea_Bnapus_Decypher_BlastP.oxl`.

OPTIONAL: You can of course run BLAST against many more databases and insert the results here - currently, the _Brassica oleracea_ graph also contains Decypher results using the UniProtKB database.