.. _installation:

***********************************************
Setting up a local application
***********************************************

We provide the website code, so that you can set up your own MGT website to assign MGT's to perhaps assign MGT for your own organism.

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

Clone the MGT repository as:

``https://github.com/crystalsand10/MGT_code_release``

(If you don't have git, download the code as a zip file from the repository location, and then extract it.)

To get the website running a few variables need to be updated for your chosen species of interest.

1. Assuming you want to set up the database now for your organism called NewBacteria, then run the following two commands:

.. code-block:: bash

	find . -type f -exec sed -i.bak “s/Salmonella/NewBacteria/g” {} ;
	find . -type f -exec sed -i.bak “s/salmonella/newBacteria/g” {} ;


Make sure the cases are typed in correctly.

2. Create two databases:

  - 'default': database to store user login information.
  - 'newBacteria' (node the case): database to store the species specific MGT information.


Then set up information in the databases as described below.

2. Setting up the databases
---------------------------

To run the MGT website, atleast two databases are required: one to store a user login information, and second to store MGT isolate information for a single species (if you have multiple species, then you'll have to set up multiple databases).

In the set up below, dbName is the database name, and appName is the code appName. Substitute with which ever names you like (however make sure the names have no spaces). An example is using "Salmonella" as your appName, and "salmonella" as your database name.


a. Log into the installed PostgreSQL server, and create two new databases (if logged in via a terminal, the commands are below):



.. code-block:: SQL

	CREATE DATABASE default;
	CREATE DATABASE <dbName>;



b. Go to the downloaded code, such that you are in the same folder as the file manage.py. Then run database migrations as follows:

	.. code-block:: bash

		python3 manage.py makemigrations <appName>
		python3 manage.py migrate --database=<dbName>

	This will create initial tables.



c. Then create a new postgres user (the website user) and give it restricted access to use the newly created database tables:

.. code-block:: SQL

	CREATE USER mgtWebsite WITH password '<PASSWORD>';
	GRANT SELECT ON ALL TABLES IN SCHEMA public TO mgtWebsite;
	GRANT INSERT, UPDATE, DELETE ON "Salmonella_isolate" TO mgtWebsite;
	GRANT INSERT, UPDATE, DELETE ON "Salmonella_project" TO mgtWebsite;
	GRANT INSERT, UPDATE ON "Salmonella_user" TO mgtWebsite;


d. Add data to the database. You will need to create a number of input files for this purpose and runs scripts as follows:

  1. Update the refFileInfo.json. Sample is available for download at :download:`json <files/refFileInfo.json>`. In this file, provide information for your bacteria (this information is displayed at various points in the website). Multiple chromosomes (for example as found for Vibrio cholerae) can be added.



  .. code-block:: bash

  	python3 populateReference.py ../ Mgt Salmonella Files/refFileInfo.json

  (Location of chromosome files must be supplied in refFileInfo, which will be used to move them to the locations in SETTINGS.py)



  2. Then provide the loci that you'd like your MGT to be based on. An example file is avaiable here. This is a simple table separated file with columns as follows:

  +---------+---------+--------+----+-------+
  | lociName| startPos|	endPos|	dir| chrNum |
  +=========+=========+========+====+=======+
  | | column 2   | column 3  | 

  ``python3 populateLoci.py ../ Mgt Salmonella Files/lociLocationsInRef.txt``

  The input file is a tab separated file,  describing the loci locations as follows:






3. Script to add schemes

``python3 populateSchemes.py ../ Mgt Salmonella Files/schemesInfo.txt Files/Schemes``


The input file is a tab separated file,  describing the loci content in schemes as follows:

Header:
schemeName	uncertainty_th	fn_lociList	displayName	description(optional)


4. Script to set up clonal_complex tables code and add to Tables_cc:

``python3 setUpCcs.py ../ Mgt Salmonella Files/tables_ccs.txt > autoGenCcs``

(Copy and paste the output to Salmonella/models/autoGenCcs and rerun migrations on the app).

Header (1 row for 1 table):

schemeName	tableNum	tableDisplayOrder	displayName
e.g.
stmcgMLST	2	4	"stmcgmlst 10 allele"

e.g. (when the same value is to appear in multiple tables)
stmcgMLST	1,2	10,1	"stmcgmlst 1 allele","stmcgmlst 1 allele"



5. Script to generate allelic_profile tables + the MGT table

``python3 setUpApsAndMgt.py ../ Mgt Salmonella Files/tables_aps.txt > autoGenAps``

(Copy and paste the output to Salmonella/autoGenAps and rerun migrations on the app).

Header:
schemeName	display_order


6. Add alleles:
``python3 addAlleles.py ../ Mgt Salmonella Files/Alleles/``


7. Add snps:

``python3 addSnps.py ../ Mgt Salmonella Files/snpMuts.txt``

Header:

locusId:alleleId	snpMut1,snpMut2...,snpMutN|<empty>

(snpMuts col in standard mutations format)




8. Populate allelic_profile tables above:

``python3 addAllelicProfiles.py ../ Mgt Salmonella Files/schemeToApMapping.txt Files/AllelicProfiles``

Header:
schemeName	alellicProfilesFileName



9. Populate clonal_complex tables and assign them to allelic profiles:

``python3 addClonalComplexes.py ../ Mgt Salmonella  Files/ccInfo.txt Files/ClonalComplexes``

Header (of ccInfo.txt):
schemeName	ccAssignmentToAp	ccMerges	tableNum_orderNum(ccInfo)

Header (of a ccFile):
st	dst	ccOrig




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
