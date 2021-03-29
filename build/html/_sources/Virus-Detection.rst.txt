Virus Detection
===============

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   virusdetect


The Following example shows how to perform virus detection analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.

virus detection Example usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *
   import glob

   pipeConfigure(
      threads=60,
      genome="hg19",
      refdir=r"path_to_reference/hg19",
      outdir=r"path_to_output/virus_output",
      data="WGS",
      type="paired",
      build=True,
      JavaMem="10g",
   )

   # Download and Build Virus Genome
   Configure.virusGenomeCheck(folder="path_to_reference/virus_database", build=True)

   # paired data
   fq1 = glob.glob("path_to_unmapped/*.fq.1.gz")
   fq2 = glob.glob("path_to_unmapped/*.fq.2.gz")

   virusdetect(seqInput1=fq1, seqInput2=fq2, upstream=True)