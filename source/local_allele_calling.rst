***********************************
Generating Alleles file locally
***********************************


Installation
################

Installation should be done with miniconda to ensure compatible versions of various packages are installed.

To install miniconda go to https://conda.io/en/latest/miniconda.html, download and run the latest python3 installer for your OS


Using miniconda create virtual environment using the included yml file:

    ``conda env create -f /path/to/fq_to_allelev2.yml -n fq2alleles``

Activate the environment:

    ``conda activate fq2alleles``

Quickstart
##########

``python reads_to_alleles.py read_1.fastq.gz,read_2.fastq.gz ref_alleles.fasta ref_locations.txt output.fasta``

The above script will run with all other settings including species and serovar as default (see below).


Inputs
####################

Reads files
-----------
Paired end fastq files (gzipped or not) in format strain_name_1.fastq(.gz) and strain_name_2.fastq(.gz)

Reference alleles
-----------------
Fasta file provided with script containing intact alleles for each locus
(may be initial "1" alleles only or include other intact alleles)

Reference locations
-------------------
Text file provided with script containing position and orientation information for each locus relative to a "reference" genome

Parameters
##########

.. code-block:: python
    usage: reads_to_alleles.py [-h] [-s SPECIES] [--no_serotyping NO_SEROTYPING]
                               [-y SEROTYPE] [-t THREADS] [-m MEMORY] [-f]
                               [--min_largest_contig MIN_LARGEST_CONTIG]
                               [--max_contig_no MAX_CONTIG_NO]
                               [--genome_min GENOME_MIN] [--genome_max GENOME_MAX]
                               [--n50_min N50_MIN] [--kraken_db KRAKEN_DB]
                               inputreads refalleles reflocs outpath

    positional arguments:
      inputreads            Input paired fastq(.gz) files, comma separated (i.e.
                            name_1.fastq,name_2.fastq )
      refalleles            File path to MGT reference alleles file
      reflocs               File path to MGT allele locations file
      outpath               Path to ouput file name

    optional arguments:
      -h, --help            show this help message and exit
      -s SPECIES, --species SPECIES
                            String to find in kraken species confirmation test
                            (default: Salmonella enterica)
      --no_serotyping NO_SEROTYPING
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


Options
####################