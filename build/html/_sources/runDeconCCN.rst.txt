runDeconCCN
===========

This function is used for methylation signal deconvolution.


Parameters
~~~~~~~~~~

.. code:: python

    runDeconCCN(mixInput=None, refInput=None, 
                outputdir=None, threads=1, 
                stepNum=None, upstream=None,) 


-  mixInput: Input samples need to be deconvoluted.
-  refInput: reference files. Default from https://www.pnas.org/content/112/40/E5503.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use. In this function, this number is set to 1.
-  upstream: upstream output results, used for pipeline, must from calculate_methyl.
-  stepNum: int or str, step flag for folder name.


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
    res_runDeconCCN = runDeconCCN(upstream=res_calMethy)