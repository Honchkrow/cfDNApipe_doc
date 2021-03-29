Bin Scale CNV
=============

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   cnvbatch
   cnvHeatmap
   cnvPlot
   cnvTable

   
The Following example shows how to perform bin-scale CNV analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.


bin-scale CNV Example usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *

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

   res_inputprocess = inputprocess(inputFolder="path_to_fastq")

   res_fastqc = fastqc(upstream=res_inputprocess, verbose=verbose)

   res_identifyAdapter = identifyAdapter(upstream=res_inputprocess, verbose=verbose)

   res_adapterremoval = adapterremoval(upstream=res_identifyAdapter, verbose=verbose)

   res_bowtie2 = bowtie2(upstream=res_adapterremoval, verbose=verbose)

   res_bamsort = bamsort(upstream=res_bowtie2, verbose=verbose)

   res_rmduplicate = rmduplicate(upstream=res_bamsort, verbose=verbose)

   res_cnvbatch = cnvbatch(
      caseupstream=res_rmduplicate,
      access=Configure.getConfig("access-mappable"),
      annotate=Configure.getConfig("refFlat"),
      verbose=verbose,
      stepNum="CNV01",
   )
   res_cnvPlot = cnvPlot(upstream=res_cnvbatch, verbose=verbose, stepNum="CNV02",)
   res_cnvTable = cnvTable(upstream=res_cnvbatch, verbose=verbose, stepNum="CNV03",)
   res_cnvHeatmap = cnvHeatmap(upstream=res_cnvbatch, verbose=verbose, stepNum="CNV04",)
