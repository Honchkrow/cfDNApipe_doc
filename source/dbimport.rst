dbimport
========

This function is to import the normal samples' mutation VCF into GenomicsDB using gatk.


.. note::
   This function is calling gatk GenomicsDBImport, please install GATK before using.

   `gatk official docs <https://gatk.broadinstitute.org/hc/en-us/categories/360002310591-Technical-Documentation>`__

   McKenna, Aaron, et al. "The Genome Analysis Toolkit: a MapReduce framework for analyzing next-generation DNA sequencing data." Genome research 20.9 (2010): 1297-1303.

Parameters
~~~~~~~~~~

.. code:: python

    dbimport(vcfInput=None, outputdir=None,
             genome=None, ref=None, threads=1,
             stepNum=None, upstream=None, verbose=False)


-  vcfInput: list, vcf files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  genome: str, human genome version, just support "hg19" and "hg38"
-  ref: str, reference folderpath.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for call snp pipeline, just can be mutect2n. This parameter can be True, which means a new pipeline start.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


.. warning::
    We recommend using this function incase-control SNV detection.
    For a detailed tutorial, please see `SNV detection tutorial <https://honchkrow.github.io/cfDNApipe/#section-6-additional-function-wgs-snvindel-analysis>`__.

Example usage:

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