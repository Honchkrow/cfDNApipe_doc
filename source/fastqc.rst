fastqc
======

This function is used for fastq file quality control.


.. note::
   This function is calling FASTQC.

   `FASTQC official docs <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`__

Parameters
~~~~~~~~~~

.. code:: python

    fastqc(fastqInput=None, fastqcOutputDir=None, 
           threads=1, other_params=None, stepNum=None, 
           upstream=None, verbose=True)

-  fastqInput: list, fastq files.
-  fastqcOutputDir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  other_params: dict, other parameters passing to FASTQC.
                "-parameter": True means "-parameter" in command line.
                "-parameter": 1 means "-parameter 1" in command line.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

   fq1 = ["test1_1.fq", "test2_1.fq"]

   fastqc(fastqInput=fq1)
