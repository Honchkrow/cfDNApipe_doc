BQSR
====

This function is for generating new bam with recaled base.


.. note::
   This function is calling gatk ApplyBQSR, please install GATK before using.

   `gatk official docs <https://gatk.broadinstitute.org/hc/en-us/categories/360002310591-Technical-Documentation>`__

Parameters
~~~~~~~~~~

.. code:: python

    BQSR(bamInput=None, recalInput=None, outputdir=None,
         genome=None, ref=None, stepNum=None, 
         upstream=None, threads=1,
         verbose=False, **kwargs)

-  bamInput: list, Input bam files.
-  recalInput: list, Input recal file from BaseRecalibrator.
-  outputdir: str, output result folder, None means the same folder as input files.
-  genome: str, genome version, just be hg19 or hg38.
-  ref: str, reference genome file path.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline, just from BaseRecalibrator. This parameter can be True, which means a new pipeline start.
-  threads: int, how many threads used?
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


.. warning::
    We recommend using this function in SNV detection.
    For a detailed tutorial, please see `SNV detection tutorial <https://honchkrow.github.io/cfDNApipe/#section-6-additional-function-wgs-snvindel-analysis>`__.

Example usage:

.. code:: python

    from cfDNApipe import *
    import glob

    pipeConfigure(
        threads=60,
        genome="hg19",
        refdir=r"path_to_reference/hg19",
        outdir=r"path_to_output/snv_output",
        data="WGS",
        type="paired",
        build=True,
        JavaMem="10G",
    )

    # just set build=True to finish all the works
    Configure.snvRefCheck(folder="path_to_reference/hg19/hg19_snv", build=True)

    # see all the SNV related files
    Configure.getConfig("snv.folder")
    Configure.getConfig("snv.ref")

    # indexed bam files after remove duplicates
    bams = glob.glob("path_to_samples/*.bam")

    res1 = addRG(bamInput=bams, upstream=True)

    res2 = BaseRecalibrator(
        upstream=res1, knownSitesDir=Configure.getConfig("snv.folder")
    )
    res3 = BQSR(upstream=res2)

    res4 = getPileup(
        upstream=res3,
        biallelicvcfInput=Configure.getConfig('snv.ref')["7"],
    )

    res5 = contamination(upstream=res4)

    # In this step, files are split to chromatin
    res6 = mutect2t(
        caseupstream = res5,
        vcfInput=Configure.getConfig('snv.ref')["6"],
        ponbedInput=Configure.getConfig('snv.ref')["8"],
    )

    # 
    res7 = filterMutectCalls(upstream=res6)

    # put all chromatin files together
    res8 = gatherVCF(upstream=res7)

    # split somatic mutations
    res9 = bcftoolsVCF(upstream=res8, stepNum="somatic")

    # split germline mutations
    res10 = bcftoolsVCF(
        upstream=res8, other_params={"-f": "'germline'"}, suffix="germline", stepNum="germline"
    )
    