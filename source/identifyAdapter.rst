identifyAdapter
===============

This function is used for detecting adapters in paired end fastq files.


.. note::
   This function is calling AdapterRemoval. This function is only suit for paired data.

   `adapterremoval official docs <https://adapterremoval.readthedocs.io/en/latest/>`__

   Schubert, Mikkel, Stinus Lindgreen, and Ludovic Orlando. "AdapterRemoval v2: rapid adapter trimming, identification, and read merging." BMC research notes 9.1 (2016): 1-7.

Parameters
~~~~~~~~~~

.. code:: python

    identifyAdapter(fqInput1=None, fqInput2=None, 
                    outputdir=None, threads=1, 
                    other_params=None, stepNum=None, 
                    upstream=None, verbose=True)

-  fqInput1: list, fastq 1 files.
-  fqInput2: list, fastq 2 files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  other_params: dict, other parameters passing to command "AdapterRemoval --identify-adapters".
                "-parameter": True means "-parameter" in command line.
                "-parameter": 1 means "-parameter 1" in command line.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

   fq1 = ["test1_1.fq", "test2_1.fq"]
   fq2 = ["test1_2.fq", "test2_2.fq"]

   identifyAdapter(fqInput1=fq1, fqInput2=fq2)
