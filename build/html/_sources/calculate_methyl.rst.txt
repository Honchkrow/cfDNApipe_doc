calculate_methyl
================

This function is used for computing methylation level from indexed methylation coverage file.


Parameters
~~~~~~~~~~

.. code:: python

    calculate_methyl(tbxInput=None, bedInput=None, 
                     outputdir=None, threads=1, 
                     stepNum=None, upstream=None,)

-  tbxInput: list, input indexed methylation coverage files.
-  bedInput: str, bed file contains genome regions which will be computed for methylation level.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


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
