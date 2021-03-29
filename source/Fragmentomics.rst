Fragmentomics
=============

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   computeOCF
   fpCounter
   fragprofplot
   OCFplot
   
The Following example shows how to perform OCF and fragmentation profile analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.


OCF Example usage
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

   case_bed = glob.glob("path_to_data/HCC/*.bed")
   ctrl_bed = glob.glob("path_to_data/CTR/*.bed")

   switchConfigure("cancer")

   res_computeOCF = computeOCF(
      casebedInput=case_bed,
      ctrlbedInput=ctrl_bed,
      caseupstream=True,
      verbose=verbose,
   )
   res_OCFplot = OCFplot(upstream=res_computeOCF, verbose=verbose)



fragmentation profile Example usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

   case_bedgz = glob.glob("path_to_data/HCC/*.bed.gz")
   ctrl_bedgz = glob.glob("path_to_data/CTR/*.bed.gz")

   # case
   switchConfigure("cancer")
   case_fragCounter = fpCounter(
      bedgzInput=case_bedgz, upstream=True, verbose=verbose, stepNum="case01", processtype=1
   )
   case_gcCounter = runCounter(
      filetype=0, binlen=5000000, upstream=True, verbose=verbose, stepNum="case02"
   )
   case_GCCorrect = GCCorrect(
      readupstream=case_fragCounter,
      gcupstream=case_gcCounter,
      readtype=2,
      corrkey="-",
      verbose=verbose,
      stepNum="case03",
   )

   # ctrl
   switchConfigure("normal")
   ctrl_fragCounter = fpCounter(
      bedgzInput=ctrl_bedgz, upstream=True, verbose=verbose, stepNum="ctrl01", processtype=1
   )
   ctrl_gcCounter = runCounter(
      filetype=0, binlen=5000000, upstream=True, verbose=verbose, stepNum="ctrl02"
   )
   ctrl_GCCorrect = GCCorrect(
      readupstream=ctrl_fragCounter,
      gcupstream=ctrl_gcCounter,
      readtype=2,
      corrkey="-",
      verbose=verbose,
      stepNum="ctrl03",
   )

   switchConfigure("cancer")
   res_fragprofplot = fragprofplot(
      caseupstream=case_GCCorrect,
      ctrlupstream=ctrl_GCCorrect,
      stepNum="FP",
   )