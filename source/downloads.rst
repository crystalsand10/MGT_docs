.. _downloads: 

***********************************************
Data visualisation, download and export
***********************************************

MGTdb enables a number of different visualisations and downloads.

.. image:: images/Visualisation.png
  :width: 2000
  :alt: Visualisations 


===========================
Tabular
===========================
All initially-loaded, or filtered- isolates are shown in a table as below. 

.. image:: images/tabular.png
  :width: 2000
  :alt: Table showing all initially-loaded or filtered isolates

Data shown in screenshot: https://mgtdb.unsw.edu.au/salmonella/isolate-list?country=Australia&searchType=and


Key features of the table are: 

* The table shows 100 isolates at one time.
* All initially-loaded or filtered isolates can be sorted by clicking on a cell in the table header. By default, rows without any values (i.e. null values) are added to the end in the sorted result. 
*  The table shows the isolates and their metadata. By default the table shows STs assigned to the isolate at every MGT level, and ODCs. 
*  The user can switch this view by clicking on the button 'Clonal complexes view' - this updates the table to show CCs at all MGT levels in place of STs. 

.. image:: images/tabular_btnsSTsCCs.png
  :width: 800
  :alt: Table showing all initially-loaded or filtered isolates

*  The cells containing the ST, CC and ODC values are coloured according to the identifier - this can enable recognizing identical values across the table easily. This feature can be switched off by clicking on 'Display color' when required.
*  dST.


===========================
Interactive graphics
===========================
All isolates in the database, or filtered isolates can be graphically summarized. Clicking on 'Graphical view' loads a page which can summarise the data in three ways: 

1. Distribution of counts of isolates with STs, CCs or ODCs over time or location
2. Distribution of counts of isolates with STs, CCs or ODCs over time and location
3. Distribution of isolate counts within particular STs, CCs or ODCs

The first two utilize temporal and spacial metadata associated with isolates. The third does not utilize such metadata, but only utilizes the associated MGT assignments (STs, CCs and ODCs). 

.. image:: images/graphicalView.png
  :width: 2000
  :alt: Graphical View

To visualise any of these graphs, the data needs to be initially loaded by clicking on 'Load data'. The data is fetched, transformed and plotted (the first graphical view is shown below for all public data in the *Salmonella* Typhimurium database).  

.. image:: images/graph1.png
  :width: 2000
  :alt: Graphical View

Key features of these graphs are as follows: 

* Same colored bars indicate the same ST (or CC, ODC) value. 
* Hovering over the bar reveals the ST (or CC, ODC) value. 
* The buttons at the top of the graph enable the graph to be interactively explored. This re-transforms and re-plots the data.
* As not all isolates, for which the plot has been generated, will contain metadata annotations; the counts of those that do contain the required metadata and are included in the plot are indicated below. 
* The displayed graph can be downloaded by clicking on the link 'Download SVG' below the plot. On most browsers, simply clicking on the link can download the SVG, however, on some browsers, right-clicking on the link triggers the download. 

**Note:** It is strongly recommended that these graphs be used with data filtered to a smaller set (compared to the complete data in the databased). Otherwise, depending on your browser and computer, this process may take a long time, as the data for plotting is loaded via javascript onto the users browsers, and rendering a plot with tens-of-thousands of bars may be computationally intensive. 

===========================
Report
===========================

A report can be generated on MGTdb for any given country (or project - if logged in). The report summarizes data at every MGT-level in the past 10 years. 


.. image:: images/report_access.png
  :width: 2000
  :alt: Generating a report on MGTdb

For every organism, the 'Summary report' button in the header navigates to a page, where the user can select any country (or project) and generate the report. 

Once the 'Generate report' button is clicked, MGTdb retrieves isolates with the requested metadata from the database, and generates the charts. The report depicts static charts - where the first four charts summarize data for all MGT levels in general, followed by six charts summarising the data at each MGT level. The figure below shows the first chart from the report. 


.. image:: images/report_chart_1.png
  :width: 2000
  :alt: Report chart 1

By scrolling all the way to the bottom of the report, users can find a button to download the report as an HTML document. 

.. image:: images/report_download_btn.png
  :width: 400
  :alt: Button to download the report


===========================
Downloads
===========================

ST, CC and ODC assignments 
-------------------------------

Allelic profiles
-------------------------------

Alleles
-------------------------------



===========================
Microreact
===========================

===========================
Grapetree
===========================
1. Export data 
---------------

2. Import in grapetree 
-----------------------

1. Download MGT9_allelic profiles (for MST)

a. Add '#' in line 1. 

b. Replace ',' with '\t'. 

c. Import as profile into graphtree & and run MST_v2 algorithm. 


2. Download MGT table (for metadata) 

a. Replace column name 'Isolate' to 'ID'. 

b. Import as metdata into graphtree. 