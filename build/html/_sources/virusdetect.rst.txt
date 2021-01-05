virusdetect
===========

This function is used for detect virus in sequencing data.


.. note::
   This this function is calling centrifuge, only used for WGS data.

   `centrifuge official docs <https://ccb.jhu.edu/software/centrifuge/>`__

Parameters
~~~~~~~~~~

.. code:: python

    virusdetect(seqInput1=None, seqInput2=None, 
                ref=None, outputdir=None, 
                threads=1, paired=True,
                other_params={"-q": True, "-N": 1, "--time": True}, 
                stepNum=None, upstream=None,)

-  seqInput1: list, input _1 fastq files.
-  seqInput2: list, input _2 fastq files, None for single end.
-  ref: bowtie2 reference path.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  paired: True for paired data, False for single end data.
-  other_params: dict, other parameters passing to Bismark.
                "-parameter": True means "-parameter" in command line.
                "-parameter": 1 means "-parameter 1" in command line.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline. This parameter can be True, which means a new pipeline start.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

    from cfDNApipe import *
    import glob

    pipeConfigure(
        threads=20,
        genome="hg19",
        refdir=r"path_to_reference/hg19",
        outdir=r"path_to_output/virus_output",
        data="WGS",
        type="paired",
        build=True,
        JavaMem="10g",
    )

    # Download and Build Virus Genome
    Configure.virusGenomeCheck(folder="path_to_reference/virus_database", build=True)

    # paired data
    fq1 = glob.glob("path_to_unmapped/*.fq.1.gz")
    fq2 = glob.glob("path_to_unmapped/*.fq.2.gz")

    virusdetect(seqInput1=fq1, seqInput2=fq2, upstream=True)
