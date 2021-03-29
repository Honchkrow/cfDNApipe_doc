Methylation Analysis
====================

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   bismark_methylation_extractor
   calculate_methyl
   compress_methyl
   computeDMR
   deconvolution

The Following example shows how to perform deconvolution analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.


deconvolution Example usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   import glob
   from cfDNApipe import *

   pipeConfigure2(
      threads=100,
      genome="hg19",
      refdir=r"path_to_reference/hg19_bismark",
      outdir=r"path_to_output/WGBS",
      data="WGBS",
      type="single",
      case="HCC",
      ctrl="Healthy",
      JavaMem="10G",
      build=True,
   )

   hcc = glob.glob("/WGBS/HCC/intermediate_result/step_06_compress_methyl/*.gz")
   ctr = glob.glob("/WGBS/Healthy/intermediate_result/step_06_compress_methyl/*.gz")

   verbose = False

   switchConfigure("HCC")
   hcc1 = calculate_methyl(tbxInput=hcc, bedInput="plasmaMarkers_hg19.bed", upstream=True, verbose=verbose)
   hcc2 = deconvolution(upstream=hcc1)

   switchConfigure("Healthy")
   ctr1 = calculate_methyl(tbxInput=ctr, bedInput="plasmaMarkers_hg19.bed", upstream=True, verbose=verbose)
   ctr2 = deconvolution(upstream=ctr1)