bismark_methylation_extractor
=============================

This function is used for extracting methylation information from bismark output.


.. note::
   This function is calling bismark.

   `bismark official docs <https://rawgit.com/FelixKrueger/Bismark/master/Docs/Bismark_User_Guide.html>`__
   
   Krueger, Felix, and Simon R. Andrews. "Bismark: a flexible aligner and methylation caller for Bisulfite-Seq applications." bioinformatics 27.11 (2011): 1571-1572.

Parameters
~~~~~~~~~~

.. code:: python

    bismark_methylation_extractor(bamInput=None, outputdir=None, threads=1,
                other_params={
                    "--no_overlap": True,
                    "--report": True,
                    "--no_header": True,
                    "--gzip": True,
                    "--bedGraph": True,
                    "--zero_based": True,
                }, paired=True, stepNum=None, upstream=None, verbose=True)

-  bamInput: list, input bam files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  paired: True for paired data, False for single end data.
-  other_params: dict, other parameters passing to FASTQC.
                "-parameter": True means "-parameter" in command line.
                "-parameter": 1 means "-parameter 1" in command line.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

    # bam file must from bismark or bismark_deduplicate output and not be sorted!
    bams = ["test1.bam", "test2.bam"]

    bismark_methylation_extractor(bamInput=bams, paired=True)

