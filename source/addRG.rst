addRG
=====

This function is used for adding read group info for BAM file.


.. note::
   This function is calling gatk's AddOrReplaceReadGroups function.

   `gatk official docs <https://gatk.broadinstitute.org/hc/en-us/categories/360002310591-Technical-Documentation>`__

Parameters
~~~~~~~~~~

.. code:: python

   addRG(bamInput=None, outputdir=None, Xmx="4G",
         stepNum=None, threads=1, upstream=None, 
         verbose=False)

-  bamInput: list, input bam files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  Xmx: str, Xmx mem, default is "4g".
-  threads: int, how many thread to use, using multiRun to excute.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results(rmduplicate / deduplicate_bismark / bamsort / bismark), used for pipeline. This parameter can be True, which means a new pipeline start.



Example usage:

.. code:: python

   bams = ["test1.bam", "test1.bam"]

   addRG(bamInput=bams, Xmx="4G")
