.. _website:

***********************************
Website
***********************************

The MGT website is available at: http://mgtdb.unsw.edu.au
The website enables you to access MGT information for public isolates. Currently, MGT has only been set up for *Salmonella* typhimurium. However, in the near future, MGT will be available for other organisms such as *Vibrio cholerae*, *Salmonella* enterica and Bordetella pertussis.

Through the website, you can submit your isolates and get an MGT assignment.


Navigating the website
======================

The various features of the website are shown below.


Global epidemeology
-------------------

In a species homepage, top 5 ST (Sequence Type) distribution by the continents can be seen. Hovering reveals the ST number as well as the number of isolates with this ST. In the figure below, the top ST value and count is shown for South America (ST=23, count=27).


.. image:: images/globalEpi.png
  :width: 2000
  :alt: in this fig.


Search
------

As you navigate the website, at various points you can come across filtering isolates.

.. image:: images/filterIsolates.png
  :width: 600
  :alt: in this fig.

The isolate filter feature. Isolates can be searched for by metadata, name, MGT-ST assignment and MGT-CC assignment. Isolates can be searched for by multiples of these as well. On searching the results are displayed in a table, as shown in the image below.

.. image:: images/filterIsolates_table.png
  :width: 800
  :alt: in this fig.

These isolates can also be downloaded via the button shown at the bottom of the table. Currently, there is a maximum limit of 10,000 for the download.

On selecting an isolate, its details, can be displayed, as shown below.

.. image:: images/isolateDetails.png
  :width: 600
  :alt: in this fig.

Features of this isolate can be selected by clicking on them (here selected are isolate type: "clinical" and Sequence type: "6"). An exact mach is then performed, and resultant strains are shown in a table (similar to the previous figure).



Account features
================

You can create an account with MGT which enables you to submit isolates, and get an MGT assignment for them.

Creating an account
-------------------

You can set up an account at the website. Simply click on Register at the top navigation bar, and enter in your details.

.. image:: images/register.png
  :width: 400
  :alt: in this fig.

Doing so will send you an email with a link (check your junk email if you don't receive an email within a few minutes). Click or copy and paste the link, and your account will be active, and you can log in with your supplied details.

Note: Certificates will be added soon to make your communication with the website httpsecure.


Uploading isolates
------------------

Once you log in, you can add projects, and isolates to a project.

Creating a project is straightforward. Simply click on 'Projects' in the top navigation bar. Then select the organism you want to create a project for.

.. image:: images/createProj.png
  :width: 400
  :alt: in this fig.

Clicking on '+ Add new project' to create a new project. Only a project name is required to create a project.


Once a project is created, navigate to the project detail. Then clicking on '+ Upload new isolate' enables you to add isolates.

.. image:: images/createIsolate.png
  :width: 400
  :alt: in this fig.

Webpage to enable adding isolate to the MGT database. The relevant information can be supplied here.

Note, that for the files to be uploaded, either Illumina sequenced files forward and reverse should be supplied, or alleles files. The advance of providing alleles file is that the uploaded file is a lot smaller (if internet speeds are an issue).

To generate the allele files locally see section  :ref:`local_allele_calling`. Apart of uploaded files, Collection year, Country, Countinent, Privacy status and Isolate name are compulsory fields.

Once information is received, 

This then sets up a job on our server. The results are returned to you as soon as possible.

Privacy
-------

We take your uploaded isolates privacy very seriously. Your isolates are made public only if you specify. Furthermore, if you delete your isolates, then all associated isolate meta-data and the uploaded files are deleted.

* To delete any single isolate:

1. Go to your Projects

2. Search for your isolate

3. View its details


* To delete all isolates in a project:
