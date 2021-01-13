bamsort
=======

This function is used for sorting bam files.


.. note::
   This function is calling samtools.

   `samtools official docs <http://www.htslib.org/>`__

   Li, Heng, et al. "The sequence alignment/map format and SAMtools." Bioinformatics 25.16 (2009): 2078-2079.

Parameters
~~~~~~~~~~

.. code:: python

   bamsort(bamInput=None, outputdir=None, 
           threads=1, stepNum=None, upstream=None)

-  bamInput: list, bam file input.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

   bams = ["test1.bam", "test1.bam"]

   bamsort(bamInput=bams)
