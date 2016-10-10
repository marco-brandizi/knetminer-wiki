## Steps to update the Arabidopsis knowledge network

1. Run the data download script from [here](https://github.com/KeywanHP/QTLNetMiner/blob/master/common/util/scripts/updateInputData.sh) 
2. Use Ondex GUI to create OXL file for BioGrid interactions

pp = new PathParser(getActiveGraph(), new DelimitedFileReader("N:/ondex-mini/qtlnetminer/organisms/Arabidopsis/biogrid/BIOGRID-ORGANISM-Arabidopsis_thaliana_Columbia-**3.4.141**.tab2.txt", "\\t+", 1));
