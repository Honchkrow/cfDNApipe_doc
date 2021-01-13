rmduplicate
===========

This function is used for removing duplicates in WGS data.


.. note::
   This function is calling gatk, please install GATK before using.

   `gatk official docs <https://gatk.broadinstitute.org/hc/en-us/categories/360002310591-Technical-Documentation>`__

   McKenna, Aaron, et al. "The Genome Analysis Toolkit: a MapReduce framework for analyzing next-generation DNA sequencing data." Genome research 20.9 (2010): 1297-1303.

Parameters
~~~~~~~~~~

.. code:: python

   rmduplicate(bamInput=None, outputdir=None, 
               threads=1, stepNum=None, 
               upstream=None, verbose=True)

-  bamInput: list, bam file input.
-  outputdir: str, output result folder, None means the same folder as input files.
-  Xmx: How many memory will be used for every thread, default: 4G.
-  threads: int, how many thread to use.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

   bams = ["test1.bam", "test1.bam"]

   rmduplicate(bamInput=bams)
