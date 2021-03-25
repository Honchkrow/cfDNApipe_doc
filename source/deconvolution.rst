deconvolution
===========

This function is used for methylation signal deconvolution.


Parameters
~~~~~~~~~~

.. code:: python

    deconvolution(mixInput=None, refInput=None, 
                  outputdir=None, threads=1, stepNum=None, 
                  upstream=None, marker_path='', scale=0.1, 
                  delcol_factor=10, iter_num=10, 
                  confidence=0.75, w_thresh=10, 
                  unknown=False, is_markers=False, 
                  is_methylation=True)
        


-  mixInput: Input samples need to be deconvoluted.
-  refInput: reference files. Default from https://www.pnas.org/content/112/40/E5503.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use. In this function, this number is set to 1.
-  upstream: upstream output results, used for pipeline, must from calculate_methyl.
-  stepNum: int or str, step flag for folder name.
-  marker_path: str, path to markers, if users select to specify certain markers
-  scale: float, control the convergence of SVR
-  delcol_factor: int, control the extent of removing collinearity
-  iter_num: int, iterative numbers of outliers detection
-  confidence: float, ratio of remained markers in each outlier detection loop
-  w_thresh: int, threshold to cut the weights designer
-  unknown: bool, if there is unknown content
-  is_markers: bool, if users choose to specify their own markers


.. warning::
    We recommend using this function with bismark related functions.


Example usage:

.. code:: python

    from cfDNApipe import *

    pipeConfigure(
        threads=60,
        genome="hg19",
        refdir=r"path_to_reference/hg19_bismark",
        outdir=r"path_to_output/WGBS",
        data="WGBS",
        type="paired",
        build=True,
        JavaMem="10g",
    )

    fq1 = ["test1_1.fq", "test2_1.fq"]
    fq2 = ["test1_2.fq", "test2_2.fq"]

    res_bismark = bismark(seqInput1=fq1, seqInput2=fq2, 
                          ref="path_to_bismark_genome",
                          upstream=True, threads=30, paired=True)

    res_deduplicate = bismark_deduplicate(
        upstream=res_bismark, other_params=dudupOP, verbose=verbose
    )

    res_methyextract = bismark_methylation_extractor(
        upstream=res_deduplicate, other_params=extractMethyOP, verbose=verbose
    )

    res_compressMethy = compress_methyl(upstream=res_methyextract, verbose=verbose)

    res_calMethy = calculate_methyl(
        upstream=res_compressMethy, bedInput=methyRegion, verbose=verbose
    )
    res_deconvolution = deconvolution(upstream=res_calMethy)