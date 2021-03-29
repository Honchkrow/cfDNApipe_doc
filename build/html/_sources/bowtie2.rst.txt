bowtie2
=======

This function is used for mapping WGS data.


.. note::
   This function is calling bowtie2.

   `bowtie2 official docs <http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml>`__

   Langmead, Ben, and Steven L. Salzberg. "Fast gapped-read alignment with Bowtie 2." Nature methods 9.4 (2012): 357.

Parameters
~~~~~~~~~~

.. code:: python

    bowtie2(seqInput1=None, seqInput2=None, ref=None, 
            outputdir=None, threads=1, paired=True,
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

    fq1 = ["test1_1.fq", "test2_1.fq"]
    fq2 = ["test1_2.fq", "test2_2.fq"]

    bowtie2(seqInput1=fq1, seqInput2=fq2, 
            ref="path_to_bowtie2_genome",
            threads=30, paired=True)

