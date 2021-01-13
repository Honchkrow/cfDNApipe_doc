adapterremoval
==============

This function is used for removing adapters in fastq files.


.. note::
   This function is calling AdapterRemoval.

   `adapterremoval official docs <https://adapterremoval.readthedocs.io/en/latest/>`__

   Schubert, Mikkel, Stinus Lindgreen, and Ludovic Orlando. "AdapterRemoval v2: rapid adapter trimming, identification, and read merging." BMC research notes 9.1 (2016): 1-7.

Parameters
~~~~~~~~~~

.. code:: python

   adapterremoval(fqInput1=None, fqInput2=None, outputdir=None, 
                  threads=1, paired=True,
                  adapter1=["AGATCGGAAGAGCACACGTCTGAACTCCAGTCA"],
                  adapter2=["AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT"],
                  other_params={"--qualitybase": 33, "--gzip": True},
                  stepNum=None, upstream=None, verbose=True)

-  fqInput1: list, fastq 1 files or single end files.
-  fqInput2: list, fastq 2 files or None for single end data.
-  outputdir: str, output result folder, None means the same folder as
   input files.
-  threads: int, how many thread to use.
-  paired: boolean, paired end or single end.
-  adapter1: list, adapters for corresponding fqInput1.
-  adapter2: list, adapters for corresponding fqInput2, None for single
   end data.
-  other_params: dict, other parameters passing to command "AdapterRemoval". 
   "-parameter": True means "-parameter" in command
   line. "-parameter": 1 means "-parameter 1" in command line.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False
   means black stdout verbose, much faster.


Example usage:

.. code:: python

   fq1 = ["test1_1.fq", "test2_1.fq"]
   fq2 = ["test1_2.fq", "test2_2.fq"]

   adapterremoval(fqInput1=fq1, fqInput2=fq2, paired=True)
