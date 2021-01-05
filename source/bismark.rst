bismark
=======

This function is used for mapping WGBS data.


.. note::
   This function is calling bismark, we will set multicore in bismark, do not use prefix paramter in bismark.

   `bismark official docs <https://rawgit.com/FelixKrueger/Bismark/master/Docs/Bismark_User_Guide.html>`__

Parameters
~~~~~~~~~~

.. code:: python

    bismark(seqInput1=None, seqInput2=None, 
            ref=None, outputdir=None, 
            threads=1, paired=True,
            other_params={"-q": True, "--phred33-quals": True, "--bowtie2": True, "--un": True,},
            stepNum=None, upstream=None, verbose=True)

-  seqInput1: list, input _1 fastq files.
-  seqInput2: list, input _2 fastq files, None for single end.
-  ref: bismark reference path.
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

    bismark(seqInput1=fq1, seqInput2=fq2, 
            ref="path_to_bismark_genome",
            threads=30, paired=True)

