## Steps to update the Arabidopsis knowledge network

1. Run the data download script from [here](https://github.com/KeywanHP/QTLNetMiner/blob/master/common/util/scripts/updateInputData.sh) 
2. Use Ondex GUI to create OXL file for Arabidopsis BioGrid interactions
  * Copy content of biogrid_arabidopsis_console.txt and paste into Ondex Console
  * Edit the file path if needed and press enter

  `pp = new PathParser(getActiveGraph(), new DelimitedFileReader("N:/ondex-mini/qtlnetminer/organisms/Arabidopsis/biogrid/BIOGRID-ORGANISM-Arabidopsis_thaliana_Columbia-3.4.141.tab2.txt", "\\t+", 1));`
  * Click "Save graph as..." and enter qtlnetminer/organisms/Arabidopsis/biogrid/At_biogrid_interactions.oxl

2. Use Ondex GUI to create OXL file for yeast BioGrid interactions
  * Copy content of biogrid_yeast_console.txt and paste into Ondex Console
  * Edit the file path if needed and press enter
  `pp = new PathParser(getActiveGraph(), new DelimitedFileReader("N:/ondex-mini/qtlnetminer/references/yeast/BIOGRID-ORGANISM-Saccharomyces_cerevisiae_S288c-3.4.141.tab2.txt", "\\t+", 1));`
  * Click "Save graph as..." and enter qtlnetminer/organisms/references/yeast/Sc_interactions_biogrid.oxl

3. Use Ondex GUI to create OXL file for Decypher Smith-Waterman file




