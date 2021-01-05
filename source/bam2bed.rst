bam2bed
=======

This function is used for converting sorted bam file to bed file.


.. note::
   Read1 and read2 will be merged in paired end data.

Parameters
~~~~~~~~~~

.. code:: python

   bam2bed(bamInput=None, outputdir=None, 
           paired=True, stepNum=None, 
           upstream=None)

-  bamInput: list, input bam files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  paired: boolean, paired end or single end.
-  lenFilter: Whether filter fragment by length, only for paired data.
-  minLen: Min fragment length.
-  maxLen: Max fragment length.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

   bams = ["test1.bam", "test1.bam"]

   bam2bed(bamInput=bams)
