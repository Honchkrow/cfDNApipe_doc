cnvPlot
=======

This function is used for drawing diagram and scatter plot for each sample.


.. note::
   This function is calling cnvkit.py batch, please install cnvkit before using.

   `cnvkit official docs <https://cnvkit.readthedocs.io/en/stable/>`__

   Talevich, Eric, et al. "CNVkit: genome-wide copy number detection and visualization from targeted DNA sequencing." PLoS computational biology 12.4 (2016): e1004873.

Parameters
~~~~~~~~~~

.. code:: python

    cnvPlot(cnsInput=None, cnrInput=None,
            outputdir=None, diagram=True,
            diagram_params={"--threshold": 0.5, "--min-probes": 3, "-y": True},
            scatter=True,
            scatter_params={"--y-max": 2, "--y-min": -2, "--segment-color": "'red'"},
            threads=1, stepNum=None, upstream=None,
            verbose=False, **kwargs)

-  cnsInput: list, cns files(copy number segments), generating from cnvkit.py batch.
-  cnrInput: list, cnr files( a table of copy number ratios), generating from cnvkit.py batch.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  diagram: True, drawing diagram plot? default is True.
-  diagram_params: dict, parameter for cnvkit.py breaks, default is {"--threshold": 0.5, "--min-probes": 3, "-y": True}
-  scatter: True, drawing scatter plot? default is True.
-  scatter_params: dict, parameter for cnvkit.py scatter, default is {"--y-max": 2, "--y-min": -2, "--segment-color": "'red'"}
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

