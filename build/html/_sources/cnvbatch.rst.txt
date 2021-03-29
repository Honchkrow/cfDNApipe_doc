cnvbatch
========

This function is used for cnv calling using cnvkit.


.. note::
   This function is calling cnvkit.py batch, please install cnvkit before using.

   `cnvkit official docs <https://cnvkit.readthedocs.io/en/stable/>`__

   Talevich, Eric, et al. "CNVkit: genome-wide copy number detection and visualization from targeted DNA sequencing." PLoS computational biology 12.4 (2016): e1004873.

Parameters
~~~~~~~~~~

.. code:: python

    cnvbatch(casebamInput=None, ctrlbamInput=None,
             outputdir=None, genome=None, ref=None, threads=1,
             access=None, annotate=None, reference_cnn=None,
             other_params={"-m": "wgs", "-y": True},
             stepNum=None, caseupstream=None, ctrlupstream=None,
             verbose=False, **kwargs)

-  casebamInput: list, case bam files, only this part of bam files will call cnv.
-  casebamInput: list, ctrl bam files, this bam files is using for generating reference cnn, will not call cnv.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  genome: str, human genome version, just support "hg19" and "hg38"
-  ref: str, reference folderpath.
-  access: str, an "access" file precomputed for the UCSC reference human genome, with some know low-mappability regions excluded. eg: access-5kb-mappable.hg19.bed.
-  annotate: str, Gene annotation databases, eg. refFlat_hg19.txt.
-  reference_cnn: str, reference cnn file, you can using the ready-made cnn file as baseline.
-  stepNum: int or str, step flag for folder name.
-  other_params: str or dict. other parameters for cnvkit batch. default is {"-m": "wgs", "-y": True}, which means WGS data, and the ref is male.
-  caseupstream: case upstream output results, used for call cnv pipeline. This parameter can be True, which means a new pipeline start.
-  ctrlupstream: ctrl upstream output results.


.. warning::
    We recommend using this function in bin-level CNV detection.


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

   bams = ["test1.bam", "test1.bam"]
   verbose = True

    res_cnvbatch = cnvbatch(
        casebamInput=bams,
        caseupstream=True,
        access=Configure.getConfig("access-mappable"),
        annotate=Configure.getConfig("refFlat"),
        verbose=verbose,
        stepNum="CNV01",
    )
    res_cnvPlot = cnvPlot(upstream=res_cnvbatch, verbose=verbose, stepNum="CNV02",)
    res_cnvTable = cnvTable(
        upstream=res_cnvbatch, verbose=verbose, stepNum="CNV03",
    )
    res_cnvHeatmap = cnvHeatmap(
        upstream=res_cnvbatch, verbose=verbose, stepNum="CNV04",
    )

