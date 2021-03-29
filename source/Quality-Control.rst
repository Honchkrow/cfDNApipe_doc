Quality Control
===============

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   fastqc
   qualimap
   fraglenplot
   fraglenplot_comp


The Following example shows how to perform fragment length analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.


fragment length Example usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *
   import glob
   import pysam

   pipeConfigure(
      threads=20,
      genome="hg19",
      refdir=r"path_to_reference/hg19_bowtie2",
      outdir=r"path_to_output/WGS",
      data="WGS",
      type="paired",
      JavaMem="10G",
      build=True,
   )

   verbose = False

   case_bed = ["./HCC/GM1100.bed", "./HCC/H195.bed"]
   ctrl_bed = ["./Healthy/C309.bed", "./Healthy/C310.bed"]

   res_fraglenplot_comp = fraglenplot_comp(
      casebedInput=case_bed, ctrlbedInput=ctrl_bed, caseupstream=True, verbose=verbose)