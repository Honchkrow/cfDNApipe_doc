qualimap
========

This function is for bam file statistics, generating the coverage, depth, map ratio in each chromosome, some figures and so on.


.. note::
   Note: this function is calling qualimap, please install qualimap before using.

   `qualimap official docs <http://qualimap.conesalab.org/>`__

   Okonechnikov, Konstantin, Ana Conesa, and Fernando García-Alcalde. "Qualimap 2: advanced multi-sample quality control for high-throughput sequencing data." Bioinformatics 32.2 (2016): 292-294.

Parameters
~~~~~~~~~~

.. code:: python

    qualimap(bamInput=None, outputdir=None, memSize="8G",
             stepNum=None, upstream=None, threads=1,
             verbose=False, other_params=None, **kwargs)

-  bamInput: list, Input bam files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  memSize: str, mem size for java, default is 8G.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline. This parameter can be True, which means a new pipeline start.
-  other_params: dict or str, other_params for qualimap, eg: for panel data, you can set {"--gff":'xx.bed'} for target region analysis.
-  threads: int, how many threads used?
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


Example usage:

.. code:: python

   bams = ["test1.bam", "test1.bam"]

   qualimap(bamInput=bams)