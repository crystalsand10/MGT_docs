.. _installation:

***********************************************
Setting up a local application
***********************************************

We provide all code required to be able to set up your own MGT website and perhaps assign MGT for your own organism.

Setting up locally requires python3, pip3 (to install various python packages) and PostgreSQL database system.


===========================
Required software
===========================


1. PostgreSQL (database system)
---------------------------------

Download and install the PostgreSQL database system by following instructions from their website (https://www.postgresql.org).



2. Python and pip
------------------

Python3 can be installed by following instructions on its website: https://www.python.org

Pip3 (python's package manager) can be installed by following instructions on its website: https://pip.pypa.io/en/stable/

Then use pip3 to install the following libraries (substituting <library_name> for the libraries listed below):

``pip3 install <library_name>``

Libraries:

* django
* django-registration
* biopython
* django-tables2
* djangorestframework
* django_registration
* django-countries
* psycopg2-binary

Additionally setting up the website via Apache.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Just a local testing version of the website can be run using django's testing server. However, if you want to permanently host your MGT website, we recommend setting up Apache to run the MGT website. The instructions for which can be found at https://docs.djangoproject.com/en/2.1/howto/deployment/wsgi/modwsgi/.

=====================================
Setting up databases and website
=====================================


1. The code and variables to update
------------------------------------

The code is available at the following git repository.

``https://github.com/crystalsand10/MGT_code_release``

Clone this repository. (If you don't have git, download the code as a zip file from the repository location, and then extract it.)

To get the website running a few variables need to be updated for your chosen species of interest.

1. Assuming you want to set up the database now for your organism called NewBacteria, then run the following two commands:

  .. code-block:: bash

  	find . -type f -exec sed -i.bak “s/Salmonella/NewBacteria/g” {} ;
  	find . -type f -exec sed -i.bak “s/salmonella/newBacteria/g” {} ;


  Make sure the cases are typed in correctly.


2. To run the MGT website, atleast two databases are required: one to store a user login information, and second to store MGT isolate information for a single species (if you have multiple species, then you'll have to set up multiple databases).

  In the set up below, newBacteria is the database name, and NewBacteria is the code application name. Substitute with which ever names you like (however make sure the names have no spaces). An example is using "Salmonella" as your application name, and "salmonella" as your database name.


  Log into the installed PostgreSQL server, and create two new databases (if logged in via a terminal, the commands are below):



  .. code-block:: SQL

  	CREATE DATABASE default;
  	CREATE DATABASE newBacteria;




3. For the remaining variables to be updated follow the README.md in your downloaded MGT folder.


Then set up information in the databases as described below.

2. Setting up the databases
---------------------------




a. Go to the downloaded code, such that you are in the same folder as the file manage.py. Then run database migrations as follows:

  .. code-block:: bash

  	python3 manage.py makemigrations NewBacteria
  	python3 manage.py migrate --database=newBacteria

  This will create initial tables.



b. Then create a new postgres user (the website user) and give it restricted access to use the newly created database tables:

  .. code-block:: SQL

	  CREATE USER mgtWebsite WITH password '<PASSWORD>';
	  GRANT SELECT ON ALL TABLES IN SCHEMA public TO mgtWebsite;
	  GRANT INSERT, UPDATE, DELETE ON "Salmonella_isolate" TO mgtWebsite;
	  GRANT INSERT, UPDATE, DELETE ON "Salmonella_project" TO mgtWebsite;
	  GRANT INSERT, UPDATE ON "Salmonella_user" TO mgtWebsite;


c. Add data to the database. You will need to create a number of input files for this purpose and runs scripts as follows:

  1. Update the refFileInfo.json. Sample is available for download at :download:`json <files/refFileInfo.json>`. In this file, provide information for your bacteria (this information is displayed at various points in the website). Multiple chromosomes (for example as found for Vibrio cholerae) can be added.



    .. code-block:: bash

  	  python3 populateReference.py ../ Mgt Salmonella Files/refFileInfo.json



    (Location of the chromosome file must be supplied in refFileInfo.json, which will be used to move the file(s) to the location provided in SETTINGS.py)

    Note: Chromosome is required, since some bacteria such as Vibrio have more than chromosome.

  2. Then add the loci that you'd like your MGT to be based on. An example file is available :download:`here <files/sampleLoci.txt>`. This is a simple table separated file with columns as follows:



    ``python3 populateLoci.py ../ Mgt Salmonella Files/lociLocationsInRef.txt``

    The input file is a tab separated file,  describing the loci locations as follows:

    | Column 1 = loci tag name
    | Column 2 = loci start position in reference
    | Column 3 = loci end position in reference
    | Column 4 = gene direction with regards to reference
    | Column 5 = chromosome number






  3. Add the schemes

    ``python3 populateSchemes.py ../ Mgt Salmonella Files/schemesInfo.txt Files/Schemes``

	:download:`schemesInfo.txt <files/schemesInfo.txt>` is a tab separated file, describing the following info:

	| Column 1 = Scheme name (e.g. MGT1, MGT2 etc; must not contain space)
	| Column 2 = cut off threshold, i.e. maximum number of missing loci allowed.
	| Column 3 = name of file containing the loci to be included in this scheme (the file contains one loci tag name per line).
	| Column 4 = Scheme database.
	| Column 5 = description of the scheme (optional).




  4. Generate code for clonal complex tables and add clonal complex tables information to Tables_cc:

	``python3 setUpCcs.py ../ Mgt Salmonella Files/tables_ccs.txt > autoGenCcs.out.py``

	The :download:`tables_cc.txt <files/tables_cc.txt>` file contains one clonal cluster information per line:

	| Column 1 = Scheme name (as provided in the previous step).
	| Column 2 = What table should this ODC table be displayed in.
	| Column 3 = The order of display in the corresponding table.
	| Column 4 = The display name of the clonal cluster column.



	e.g.
	stmcgMLST	2	4	"stmcgmlst 10 allele"

	e.g. (when the same value is to appear in multiple tables)
	stmcgMLST	1,2	10,1	"stmcgmlst 1 allele","stmcgmlst 1 allele"


	Once run, copy and add the output (autoGenCcs.out.py) to NewBacteria/models/autoGenCcs.py and rerun migrations (step 2a).




  5. Generate code for the allelic profiles tables, and the MGT table:

	``python3 setUpApsAndMgt.py ../ Mgt Salmonella Files/tables_aps.txt > autoGenAps``

	The :download:`tables_ap.txt <files/tables_ap.txt>` file contains two columns:

	| Column 1 = scheme name
	| Column 2 = scheme display order

	Once again, copy and paste the output to Salmonella/models/autoGenAps.py and rerun migrations (step 2a).



  6. In the next few steps we add data into the various tables. One way to add alleles to the database is:

	``python3 addAlleles.py ../ Mgt Salmonella Files/Alleles/``

	The Alleles folder contains one fasta file for each of the loci. An example is :download:`STMMW_14461.fasta <files/STMMW_14461.fasta>`.


  7. Add snps:

	``python3 addSnps.py ../ Mgt Salmonella Files/snpMuts.txt``

	Here the snpMuts.txt file contains SNP mutations in a standard mutations format described here_.

	.. _here: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1867422/



  8. Populate allelic_profile tables:

	``python3 addAllelicProfiles.py ../ Mgt Salmonella Files/schemeToApMapping.txt Files/AllelicProfiles``

	The :download:`schemeToApMapping.txt <files/schemeToApMapping.txt>` file contains a simple mapping from the scheme name to its corresponding allelic profiles file name.

	The :download:`allelic profiles file<files/MGT2_allelic_profiles.txt>` contains an allelic combination mapped to a unique ST and dST.


  9. Populate clonal complex tables and assign them to allelic profiles:

	``python3 addClonalComplexes.py ../ Mgt Salmonella  Files/ccInfo.txt Files/ClonalComplexes``

	The :download:`ccInfo.txt <files/ccInfo.txt>` contains information regarding the files for each clonal complex, and the clonal complex itself added to the database earlier. The columns are:

	| Column 1 = scheme name
	| Column 2 = A file containing information regarding the clonal complex assignment to an allelic profile (e.g. :download:`MGT7_cc.txt <files/MGT7_cc.txt>`).
	| Column 3 = A file containing information regarding newly computed clonal complex merges (e.g. :download:`MGT7_cc_merges.txt <files/MGT7_cc_merges.txt>`).
	| Column 4 = Format of tableNumber_orderNumer.





10. Register for an account on the web-app.
(Can set up a dummy email server as:)

``python -m smtpd -n -c DebuggingServer localhost:25``


11. Populate isolate tables:

``python3 addIsolates.py ../ Mgt Salmonella Files/isolate_info.tab``

(Specify column names of metadata in right at the start of the script).

Header:
userName	projectName	privacy_status	isolateId	METADATA(cols_tabbed)



12. Populate Hst tables, and assign isolates to hsts:
``python3 addMgts.py ../ Mgt Salmonella Files/hgt_annotations.tab``

Header:
username	projectName	isolateName	schName1	schName2	schName3	...	schNameN


13. Script to generate the ap_cc view table: + (sql code for running directly on the sql server).

``python3 genViewSqlAndClass.py ../ Mgt Salmonella mlstWebsite``

Two files are written out:
1. "runOnDb.sql" : run the two sql statements in postgresSql (can follow the method in 14.).
2. "autoGenView" : copy and paste this to autoGenViews.py in the models folder.

14. Run postgres commands from file:
``psql -U postgres -d salmonella50 -a -f runOnDb.sql``



5. Update settings
-------------------

Open the Mgt/Mgt/settings.py file and change the various settings according to your setup.

Some particular settings to pay attention to are:

changing the settings.py file in the MGT github (databases in lower case and apps in upper case.

- change the database {organism to vibrio, then {name: to postgres for database made earlier in step 4.




7. edit all names and paths to remove salmonella and replace with vibrio using find . -type f -exec sed -i.bak "s/Salmonella/Vibrio/g" {} \;



8. ready to set up MGT database, use read me in MGT for next instructions.


You should set up regular backups of your data in the database.
