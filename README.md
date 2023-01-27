*** v1, 27 Jan 2023, Harald Janovjak, harald.janovjak@flinders.edu.au***

**Preamble:**
This document provides information on filters, formulas and macros accompanying the research article Gangemi et al.


**Part A. cKSR analysis (Figures 1 and 2, and Supplementary Figures S1 and S3):**
This analysis was conducted in MS Excel using custom filters and formulas.
As an example, Tyr kinase cKSRs data and analysis is provided here in two files:
1) “general-cKSR_analysis_Tyr_kinases.xlsx”
2) “site-specific-cKSR_analysis_Tyr_kinases.xlsx”
Each processing step in the files is commented and these files also indicate where data was exported for the respective figures of the manuscript.


**Part B. Co-expression analysis:**
This analysis was conducted in C in the Igor Pro environment.
Since Igor Pro may not be available to all readers, pseudocode is provided below with the full code available in the file “coexpression-analysis-proteomics-Tyr-kinases.pxp” along with sample data (the file size limit of Github prevents uploading of all data, so only a subset of protein expression data is included and using Tyr kinase general cKSRs as an example).

*Data columns and definitions:*
Inputwave is a data column that in each row lists all kinases that phosphorylate a specific substrate (general cKSR) or site (site-specific cKSR); this corresponds to column U (in “general-cKSR_analysis_Tyr_kinases.xlsx”) or V (in “site-specific-cKSR_analysis_Tyr_kinases.xlsx”) and the Supplementary Tables S1-S4 of the manuscript.
Scorewave is a results column generated that contains the co-expression score (as defined in Figure 2E of the manuscript); this corresponds to the final data compiled in the manuscript Figure 2E and Supplementary Figures S6 and S7.

*Pseudocode:*
Step 1: Convert expression data to a binary format (1: gene/protein is expressed, 0: gene/protein is not expressed) using a defined cut-off.

Function make_binary()
   Find the names of all data columns (each column is a gene/protein, each row a cell line)
string1 = WaveList("*",";","")

   Loop through all data columns
do
	   Define the threshold, e.g. the median in proteomics analysis
	threshold = StatsMedian(wave1_l)

	   To make the column binary:
   first subtract the threshold (now all sub-threshold numbers are negative)
   divide by a big number
   (above sub-threshold numbers are very small)
   (sub-threshold numbers are between -1 and 0)
   apply ceil() command rounding numbers between -1 and 0 to 0
	ceil()
  
	i=i+1
while(i<ItemsInList(string1))
end


Step 2: Count for each substrate if at least two kinases are co-expressed
Function test_coexpression()
   Loop through all lines in inputwave (see above for definition of data columns)
do
	   Add the binary data from all kinase data columns that are found in an inputwave row
	k=0
	do
		   Extract name of each kinase data column
		string2 = StringFromList(k, string1)
		if(waveexists($string2)>0)
		   Add the corresponding binary wave
wave wavetoadd_l = $string2
		tempwave_l += wavetoadd_l
		endif
		   Go to the next kinase
		k=k+1
	while(k<=ItemsInList(string1)-1)
	
	   Count how many lines in tempwave are >=2, via conversion to binary data
	(code as above)

	i=i+1
while(i<numpnts(inputwave))
end
