bismark_deduplicate
===================

This function is used for removing duplicates from bismark output.


.. note::
   This function is calling bismark.

   `bismark official docs <https://rawgit.com/FelixKrueger/Bismark/master/Docs/Bismark_User_Guide.html>`__

Parameters
~~~~~~~~~~

.. code:: python

    bismark_deduplicate(bamInput=None, outputdir=None, 
                        threads=1, paired=True,
                        other_params={}, stepNum=None, 
                        upstream=None, verbose=True)

-  bamInput: list, input bam files.
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

    # bam file must from bismark output and not be sorted!
    bams = ["test1.bam", "test2.bam"]

    bismark_deduplicate(bamInput=bams, paired=True)

