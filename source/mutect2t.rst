mutect2t
========

This function is used for Call somatic SNVs and indels via local assembly of haplotypes using gatk.


.. note::
   This function is calling Mutect2. This function is only suit for paired data.

   `gatk official docs <https://gatk.broadinstitute.org/hc/en-us/categories/360002310591-Technical-Documentation>`__

Parameters
~~~~~~~~~~

.. code:: python

    mutect2t(bamInput=None, ponbedInput=None, vcfInput=None,
        outputdir=None, genome=None, ref=None, threads=1,
        stepNum=None, other_params=None, caseupstream=None,
        ctrlupstream=None, verbose=False, **kwargs)


-  bamInput: list, bam files.
-  ponbedInput: str, panel-of-normals file, you can generating this file from mutect2n or using download   file (like somatic-hg38_1000g_pon.hg38.vcf.gz...).
-  vcfInput: str, like af-only-gnomad.raw.sites.hg19.vcf.gz. this is for gatk parameter "--germline-resource".
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  genome: str, human genome version, just support "hg19" and "hg38"
-  ref: str, reference folderpath.
-  stepNum: int or str, step flag for folder name.
-  other_params: str or dict. other parameters for gatk mutect, default is None.
-  caseupstream: case upstream output results, used for call snp pipeline, just can be contamination / BQSR / addRG. This parameter can be True, which means a new pipeline start.
-  ctrlupstream: ctrl upstream output results, used for pon bed file, just can be createPON. This parameter can be None, which means you need to give me ponbedInput.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.




.. warning::
    We recommend using this function in SNV detection.
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