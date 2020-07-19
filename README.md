# Diketone
HOW TO DO AUTOMATED DE ANALYSIS

This repository includes the script used to do the automated DE analysis and Update the corresponding entries in the database. 

Please note the script in this repository has been tested in Ubuntu 16.04 LTS.
Structure:
	./<working folder>/
o	AutoReporting.r – the main script
o	DEAnalysisReport.rmd – the script to generate the pdf report
o	UpdateDB.py – the main script for update the database
o	auxiliary/
	Clustering.py – script called by the main script to perform the clustering using BLOSUM matrix
	updateReports.py – the script to update the ReportTable in the database.
	DE.rmd – script used to perform DE analysis
	Sequence_Repopulation.r – script used to do the re-clustering after execution of Clustering.py
	LibraryImage/ – the chemical structures of the libraries
	2020instructions.jpg – the plot explaining how to read 2020 plot.
o	Example.xlsx – the excel file including the information required by the main scripts
Python version & libraries:
	Python3.6 or later
	numpy
	pandas
	collections
	getpass
	pymysql
	scipy
	sklearn
	matplotlib
	math
R libraries:
	dplyr
	tidyr
	rmarkdown
	reticulate
	gtools
	openxlsx
	ggplot2
	knitr
	png
	grid
	kableExtra
	jpeg
	gridExtra
After running of the script:
	if DE result is available, a new folder <tag_i> will created under the <working folder> for each DE set
	under folder <working folder>/<tag_i>/
o	DE¬¬_<tag_i>.html – DE summary in html.
o	DE¬¬_<tag_i>.txt – DE results.
o	Cluster_<tag_i>.csv – clustering results.
o	Cluster_<tag_i>.png – plot of clustering results.
o	rePopulate¬¬_<tag_i>_to_<tag_i>.csv – the re-clustering results.
o	<tag_i>.pdf – the DE report, which will be uploaded to the cloud.
o	volcano<i>.png – the volcano plots included in the DE report
Before you run the script:
1.	Create a folder in your machine as the <working folder>.
2.	Make sure, you have the Python, R and required libraries listed in Python version & Libraries section installed.
3.	Download the repository into your <working folder>.
4.	Prepare the excel file for DE analysis as
a.	Every two rows represent one DE analysis.
b.	Column “tag” – the tag for the DE analysis, which will be used to name the folder and related files. Note: Do NOT put “_” in the tag.
c.	Column “test” and the row under it – the name of the test file, without “.txt”, and the label for that sample used in the report.
d.	Columns “control<i>” and the row under it -- the name of the control file, without “.txt”, and the label for that sample used in the report. If need more columns for control, insert columns after “control3”.
e.	Column “pattern” – the peptide pattern used to filter the peptides.
f.	Column “SoftControl” – the number of the control sets that are used for direct comparison between test and the control. Use “,” to connect the numbers.
g.	Column “extraComp” 
i.	if depletion is included in the process, control1 should be the sample before depletion, control2 should be the sample after depletion, and “Depletion” in column “extraComp”.
ii.	if unmodified is included in the process, control1 should be the sample of input of the unmodified case, control2 should be the sample of unmodified-negative-control, control3 should be the sample of unmodified-target, control4 should be the modified input, and “UnMod” in column “extraComp”.
h.	Column “ModList” – the modification tags in the data files, connected by “,”.
i.	Columns “SDB”, “SDB_start” – leave them empty for company samples.
j.	Column “seqHead” – the modification attached to the peptide sequences in the report. For example, if it is bicyclic library, put “TSL6-” in this column.
k.	Column “blackPos” the positions in the peptide sequences that do not need color encoding. -1 means the last position. If every position requires color coding, leave it blank.
l.	Column “filterCounts” – the ratio used to filter out sequences with too few occurrences.  If a sequence has been observed less than filterCounts*[number of replicates in test] in the sum of the number of replicates of all samples in the DE. Recommended value: 0.75.
m.	Column “subtitle” – the subtitle used in the report.
n.	Column “library” – the name of the library files in <auxiliary>/<LibraryImage>/ without the format attached. If the library image cannot be found in the folder, leave it empty.
o.	Column “tarGel” – the full path of the target gel image. If not available, leave it empty.
Start the program:
1.	Open AutoReporting.r in RStudio, make the following changes:
a.	Line 20, DEtable – the name of the excel file.
b.	Line 21, DataDir --  the folder storing the data file for DE analysis.
2.	Source the script.
After running of the program:
The program will finish in about 1 hour depending on how many DE analysis is required. When it finishes:
1.	Upload the .pdf reports to the 48hd.cloud EC2 folder: /media/DBData/DEClusteringReport
2.	Run the script UpdateDatabase.py, then you need to input the username and password to the database. You also need to input the name of the excel file you used for DE analysis. 
