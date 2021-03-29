SNV Detection
=============

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:


   addRG
   BaseRecalibrator
   bcftoolsVCF
   BQSR
   contamination
   createPON
   dbimport
   filterMutectCalls
   gatherVCF
   getPileup
   mutect2n
   mutect2t


The Following example shows how to perform SNV analysis. For up- and down-stream relationship, please see "Up Down Stream Flowchart" part.

SNV Example usage
~~~~~~~~~~~~~~~~~

.. code:: python

   from cfDNApipe import *
   import glob

   # set global configure
   pipeConfigure2(
      threads=100,
      genome="hg19",
      refdir=r"path_to_reference/hg19",
      outdir=r"path_to_output/snv_output",
      data="WGS",
      type="paired",
      case="cancer",
      ctrl="normal",
      JavaMem="10g",
      build=True,
   )

   Configure2.snvRefCheck(folder="path_to_reference/hg19/SNV_hg19", build=True)

   case_bams = glob.glob("path_to_samples/cancer/*.bam")
   ctrl_bams = glob.glob("path_to_samples/normal/*.bam")

   # create PON file from normal samples
   switchConfigure("normal")

   ctrl_addRG = addRG(bamInput=ctrl_bams, upstream=True, stepNum="PON00",)
   ctrl_BaseRecalibrator = BaseRecalibrator(
         upstream=ctrl_addRG,
         knownSitesDir=Configure2.getConfig("snv.folder"),
         stepNum="PON01",
         )
   ctrl_BQSR = BQSR(upstream = ctrl_BaseRecalibrator, stepNum="PON02",)
   ctrl_mutect2n = mutect2n(upstream = ctrl_BQSR, stepNum="PON03",)
   ctrl_dbimport = dbimport(upstream = ctrl_mutect2n, stepNum="PON04",)
   ctrl_createPon = createPON(upstream = ctrl_dbimport, stepNum="PON05",)

   # performing comparison between cancer and normal 
   switchConfigure("cancer")
   case_addRG = addRG(bamInput=case_bams, upstream=True, stepNum="SNV00",)
   case_BaseRecalibrator = BaseRecalibrator(
      upstream=case_addRG,
      knownSitesDir=Configure2.getConfig("snv.folder"),
      stepNum="SNV01",
   )

   case_BQSR = BQSR(
      upstream=case_BaseRecalibrator, 
      stepNum="SNV02")

   case_getPileup = getPileup(
      upstream=case_BQSR,
      biallelicvcfInput=Configure2.getConfig('snv.ref')["7"],
      stepNum="SNV03",
   )
   case_contamination = contamination(
      upstream=case_getPileup,  
      stepNum="SNV04"
   )

   # In this step, ponbedInput is ignored by using caseupstream parameter
   case_mutect2t = mutect2t(
      caseupstream=case_contamination,
      ctrlupstream=ctrl_createPon,
      vcfInput=Configure2.getConfig('snv.ref')["6"],
      stepNum="SNV05",
   )

   case_filterMutectCalls = filterMutectCalls(
      upstream=case_mutect2t,
      stepNum="SNV06"
   )

   case_gatherVCF = gatherVCF(
      upstream=case_filterMutectCalls, 
      stepNum="SNV07"
   )

   # split somatic mutations
   case_somatic = bcftoolsVCF(upstream=case_gatherVCF, stepNum="somatic")

   # split germline mutations
   case_germline = bcftoolsVCF(
      upstream=case_gatherVCF, other_params={"-f": "'germline'"}, suffix="germline", stepNum="germline"
   )
