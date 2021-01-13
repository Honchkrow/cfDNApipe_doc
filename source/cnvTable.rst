cnvTable
========

This function is used for caculating break, genemetrics and gene for each sample.


.. note::
   This function is calling cnvkit.py batch, please install cnvkit before using.

   `cnvkit official docs <https://cnvkit.readthedocs.io/en/stable/>`__
   
   Talevich, Eric, et al. "CNVkit: genome-wide copy number detection and visualization from targeted DNA sequencing." PLoS computational biology 12.4 (2016): e1004873.

Parameters
~~~~~~~~~~

.. code:: python

    cnvTable(cnsInput=None, cnrInput=None, outputdir=None, 
             breaks=True, breaks_params={"--min-probes": 1, },
             genemetrics=True, 
             genemetrics_params={"--threshold": 0.1, "--min-probes": 3, "-y": True},
             threads=1, stepNum=None, upstream=None,
             verbose=False, **kwargs)

-  cnsInput: list, cns files(copy number segments), generating from cnvkit.py batch.
-  cnrInput: list, cnr files( a table of copy number ratios), generating from cnvkit.py batch.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  breaks: True, analysis breaks? default is True.
-  breaks_params: dict, parameter for cnvkit.py breaks, default is {"--min-probes": 1}
-  genemetrics: True, analysis genemetrics? default is True.
-  genemetrics_params: dict, parameter for cnvkit.py genemetrics, default is {"--threshold": 0.1, "--min-probes": 3, "-y": True}
-  stepNum: int or str, step flag for folder name.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.
-  upstream: upstream output results, used for cnv pipeline, just can be cnvbatch. This parameter can be True, which means a new pipeline start.


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

