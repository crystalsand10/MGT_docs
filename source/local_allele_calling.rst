.. _local_allele_calling:

***********************************
Generating alleles file locally
***********************************

To reduce the amount of data to be uploaded to the `MGT database <http://mgtdb.unsw.edu.au>` some of the MGT pipeline processing can be performed locally.

These steps include:

#. Species, serovar checking
#. Genome assembly
#. Genome QC
#. Extraction of alleles from genome using known allele fasta file
#. Assignment of 7 gene MLST sequence type

The resulting file is often several orders of magnitude smaller than the raw reads, facilitating rapid upload and analysis.

Installation
################

This pipeline has many dependencies so conda is the best way to handle them all. So the included .yaml file can be used to create the required environment that will need to be activated before running the script

#. Clone the repo

    git clone https://github.com/LanLab/MGT_reads2alleles.git

#. Download latest miniKraken Database:

    From the kraken website - https://ccb.jhu.edu/software/kraken/ (warning 2.9GB!)

    OR

    ``wget https://ccb.jhu.edu/software/kraken/dl/minikraken_20171019_4GB.tgz``

#. unzip archive

#. Add database folder variable with:

    ``export KRAKEN_DEFAULT_DB="/home/user/minikraken_db_folder"``

#. Install conda environment:**

    install miniconda3 -> https://conda.io/miniconda.html

    ``conda create -f /path/to/fq_to_allele.yaml -n deployable_fq_to_genome``

    This may take a while

    ``conda activate deployable_fq_to_genome``


Quickstart
##########

``python reads_to_alleles.py read_1.fastq.gz,read_2.fastq.gz ref_alleles.fasta ref_locations.txt output.fasta``

The above script will run with all other settings including species and serovar as default (see below).


Inputs
####################

**Reads files**

Paired end fastq files (gzipped or not) in format strain_name_1.fastq(.gz) and strain_name_2.fastq(.gz)

**Reference alleles**

Fasta file provided with script containing intact alleles for each locus
(may be initial "1" alleles only or include other intact alleles)

**Reference locations**

Text file provided with script containing position and orientation information for each locus relative to a "reference" genome

Outputs
#######

An Alleles file in fasta format: **strainID_alleles.fasta**. 4 different types of "allele" are recorded.

#. A header stating the **7 gene MLST type** predicted by `mlst <https://github.com/tseemann/mlst>`_
#. A header in the format the locus:0_reason_for_failed_call to denote loci with **uncallable alleles**
#. A header in the format locus:allele to describe **exact matches** to alleles in the reference alleles file
#. A header in the format locus:new with sequence to describe **new intact alleles or alleles with missing data**

This allele file can be submitted (optionally with metadata) to the MGT database for full MGT assignment


Parameters
##########

**usage:**
reads_to_alleles.py [options] inputreads refalleles reflocs outpath

**positional arguments:**

:inputreads: Input paired fastq(.gz) files, comma separated (i.e. name_1.fastq,name_2.fastq )

:refalleles: File path to MGT reference alleles file

:reflocs: File path to MGT allele locations file

:outpath: Path to ouput file name

**optional arguments:**

-h, --help            show this help message and exit
-s SPECIES, --species SPECIES
                    String to find in kraken species confirmation test
                    (default: Salmonella enterica)
--no_serotyping
                    Do not run Serotyping of Salmonella using SISTR (ON by
                    default) (default: None)
-y SEROTYPE, --serotype SEROTYPE
                    Serotype to match in SISTR, semicolon separated
                    (default: Typhimurium;I 4,[5],12:i:-)
-t THREADS, --threads THREADS
                    number of computing threads (default: 4)
-m MEMORY, --memory MEMORY
                    memory available in GB (default: 8)
-f, --force           overwrite output files with same strain name?
                    (default: False)
--min_largest_contig MIN_LARGEST_CONTIG
                    Assembly quality filter: minimum allowable length of
                    the largest contig in the assembly in bp (default:
                    60000)
--max_contig_no MAX_CONTIG_NO
                    Assembly quality filter: maximum allowable number of
                    contigs allowed for assembly (default: 700)
--genome_min GENOME_MIN
                    Assembly quality filter: minimum allowable total
                    assembly length in bp (default: 4500000)
--genome_max GENOME_MAX
                    Assembly quality filter: maximum allowable total
                    assembly length in bp (default: 5500000)
--n50_min N50_MIN     Assembly quality filter: minimum allowable n50 value
                    in bp (default: 20000)
--kraken_db KRAKEN_DB
                    path for kraken db (if KRAKEN_DEFAULT_DB variable has
                    already been set then ignore) (default: )


Examples
--------

**example1:** 

running strain 1234 against salmonella typhimurium MGT with 8 cores and 30gb RAM

    python /path/to/reads_to_alleles.py 1234_1.fastq.gz,1234_2.fastq.gz MGT_alleles_file locus_position_file output_file_name --serotype "Typhimurium;I 4,[5],12:i:-" --species "Salmonella enterica" -t 8 -m 30

**example2:**

running strain abcd against vibrio cholerae MGT with 4 cores and 50gb RAM
(serotyping is currently only for Salmonella)

    python /path/to/reads_to_alleles.py abcd_1.fastq.gz,abcd_2.fastq.gz MGT_alleles_file locus_position_file output_file_name --no_serotyping --species "Vibrio cholerae" -t 4 -m 50

