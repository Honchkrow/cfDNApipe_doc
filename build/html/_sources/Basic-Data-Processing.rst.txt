Basic Data Processing
=====================

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   inputprocess
   identifyAdapter
   adapterremoval
   bowtie2
   bismark
   bismark_deduplicate
   bamsort
   rmduplicate
   bam2bed
   Configure
   Configure2
   pipeConfigure
   pipeConfigure2
   switchConfigure
   cfDNAWGBS
   cfDNAWGBS2
   cfDNAWGS
   cfDNAWGS2


Basic data processing part involves steps from raw fastq file to BAM/BED file procedure. 
For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.
For more demos, please see `here`_.


WGBS Example usage
~~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *

   pipeConfigure(
      threads=60,
      genome="hg19",
      refdir=r"path_to_reference/hg19_bismark",
      outdir=r"path_to_output/WGBS",
      data="WGBS",
      type="paired",
      JavaMem="10g",
      build=True,
   )


   verbose = False

   res_inputprocess = inputprocess(inputFolder="path_to_fastq")

   res_fastqc = fastqc(upstream=res_inputprocess, verbose=verbose)

   res_identifyAdapter = identifyAdapter(upstream=res_inputprocess, verbose=verbose)

   res_adapterremoval = adapterremoval(upstream=res_identifyAdapter, verbose=verbose)

   res_bismark = bismark(upstream=res_adapterremoval, verbose=verbose)

   res_deduplicate = bismark_deduplicate(upstream=res_bismark, verbose=verbose)


WGS Example usage
~~~~~~~~~~~~~~~~~

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


pre-set pipeline Example usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *

   pipeConfigure2(
      threads=120,
      genome="hg38",
      refdir="/home/zhangwei/Genome/hg38_bismark",
      outdir="/opt/tsinghua/zhangwei/Pipeline_test/o_WGBS-compare",
      data="WGBS",
      type="paired",
      JavaMem="10G",
      case="cancer",
      ctrl="normal",
      build=True,
   )

   # fragProfile is set to False because the demo data is too small to get valid values
   a, b = cfDNAWGBS2(
      caseFolder="/opt/tsinghua/zhangwei/Pipeline_test/WGBS-compare/case",
      ctrlFolder="/opt/tsinghua/zhangwei/Pipeline_test/WGBS-compare/ctrl",
      caseName="cancer",
      ctrlName="normal",
      idAdapter=True,
      rmAdapter=True,
      dudup=True,
      armCNV=True,
      CNV=True,
      fragProfile=False,
      deconvolution=True,
      OCF=True,
      report=True,
      verbose=False,
   )

   DMR = computeDMR(caseupstream=a.calculate_methyl, ctrlupstream=b.calculate_methyl)



.. _here: https://github.com/XWangLabTHU/cfDNApipe/tree/master/demo/pipeline_test