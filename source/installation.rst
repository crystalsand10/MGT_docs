.. _installation:

***********************************************
Setting up a local application
***********************************************

Setting up locally requires python3, pip3 (to install various python packages) and PostgreSQL database system.


===========================
Required software
===========================


1. PostgreSQL (database system)
---------------------------------

Download and install the PostgreSQL database system by following instructions from their website (https://www.postgresql.org).

Once installed, log into PostgreSQL (via the terminal) create a new database:

``CREATE DATABASE organism;``


Then create a new user and give it access to use the newly created database:

``CREATE USER mlstwebsite WITH password '<PASSWORD>';``

``GRANT ALL PRIVILEGES ON DATABASE organism to mlstwebsite;``


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
* psycopg2-binary

Additionally setting up the website on Apache.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Just a local testing version of the website can be run using the django. However, if you want to permanently host your MGTdb website, we recommend using Apache. The instructions for which can be found at ( ... ).


===========================
Setting up MGT database
===========================

Clone the MGT repository as:

``git clone https://github.com/crystalsand10/Mlst.git``

(If you don't have git, download the code as a zip file from the repository location.)

3. Update settings
-------------------

Open the location/..../settings.py file and change the various settings according to your setup.

Some particular settings to pay attention to are:

changing the settings.py file in the MGT github (databases in lower case and apps in upper case.

- change the database {organism to vibrio, then {name: to postgres for database made earlier in step 4.



6. copy and paste all files from mgt/Salmonella into mgt/Vibrio



7. edit all names and paths to remove salmonella and replace with vibrio using find . -type f -exec sed -i.bak "s/Salmonella/Vibrio/g" {} \;



8. ready to set up MGT database, use read me in MGT for next instructions.
