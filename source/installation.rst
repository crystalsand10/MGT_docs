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

``https://github.com/crystalsand10/MGT_local``

Clone this repository. (If you don't have git, download the code as a zip file from the repository location, and then extract it.)

To get the website running a few variables in the `settings_template.py` file need to be updated for your species of interest and some files with genome and loci information need to be provided. 
More information on this is provided in the README.md file located in the root directory of the codebase.


2. Setting up the databases
---------------------------

After making changes to the settings file, run the `setup_new_database.ssh` script to setup the new databases as follows. 

``./setup/setup_new_database.ssh example_inputs_2.setupPath``

Some prompts for a PostgreSQL password will appear throughout the course of the script, once these are entered, the program will setup the models. 


=====================================
Running the website
=====================================

To run the website locally simply:

``python3 manage.py runserver --settings Mgt.settings_template``

Otherwise in the settings set DEBUG=False and follow the instructions in section "Additionally setting up the website via Apache".

Remember, you should set up regular backups of your data in the database.
