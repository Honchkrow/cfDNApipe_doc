Large Scale CNV
===============

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   bamCounter
   runCounter
   GCCorrect
   computeCNV

The Following example shows how to perform large scale CNV analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.

CNV Example usage
~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *
   import glob

   pipeConfigure2(
      threads=20,
      genome="hg19",
      refdir=r"reference_genome/hg19",
      outdir=r"output/pcs_armCNV",
      data="WGS",
      type="paired",
      JavaMem="8G",
      case="cancer",
      ctrl="normal",
      build=True,
   )

   verbose = False

   case_bam = glob.glob("path_to_data/HCC/*.bam")
   ctrl_bam = glob.glob("path_to_data/CTR/*.bam")

   # case
   switchConfigure("cancer")
   case_bamCounter = bamCounter(
      bamInput=case_bam, upstream=True, verbose=verbose, stepNum="case01"
   )
   case_gcCounter = runCounter(
      filetype=0, upstream=True, verbose=verbose, stepNum="case02"
   )
   case_GCCorrect = GCCorrect(
      readupstream=case_bamCounter,
      gcupstream=case_gcCounter,
      verbose=verbose,
      stepNum="case03",
   )

   # ctrl
   switchConfigure("normal")
   ctrl_bamCounter = bamCounter(
      bamInput=ctrl_bam, upstream=True, verbose=verbose, stepNum="ctrl01"
   )
   ctrl_gcCounter = runCounter(
      filetype=0, upstream=True, verbose=verbose, stepNum="ctrl02"
   )
   ctrl_GCCorrect = GCCorrect(
      readupstream=ctrl_bamCounter,
      gcupstream=ctrl_gcCounter,
      verbose=verbose,
      stepNum="ctrl03",
   )

   switchConfigure("cancer")
   res_computeCNV = computeCNV(
      caseupstream=case_GCCorrect,
      ctrlupstream=ctrl_GCCorrect,
      stepNum="ARMCNV",
      verbose=verbose,
   )