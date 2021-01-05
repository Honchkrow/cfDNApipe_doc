cnvHeatmap
==========

This function is used for drawing heatmap plot including all samples.


.. note::
   This function is calling cnvkit.py batch, please install cnvkit before using.

   `cnvkit official docs <https://cnvkit.readthedocs.io/en/stable/>`__

Parameters
~~~~~~~~~~

.. code:: python

    cnvHeatmap(cnrInput=None,
               outputdir=None, orderfile=None,
               other_params={"-d": True, "-y": True},
               stepNum=None, upstream=None, **kwargs)

-  cnrInput: list, cnr files( a table of copy number ratios), generating from cnvkit.py batch.
-  outputdir: str, output result folder, None means the same folder as input files.
-  orderfile: str, a file contain the cnr file name to reorder the outfile, you can miss this parameter which will ordered as the cnrInput.
-  diagram: True, drawing diagram plot? default is True.
-  other_params: dict, parameter for cnvkit.py heatmap, default is {{"-d": True, "-y": True}. other parameter could setting as this {"-c": "chr8"} as for designate chromosome. {"-g": "EXT1,PXDNL"} is for designate gene analysis.
-  stepNum: int or str, step flag for folder name.
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

