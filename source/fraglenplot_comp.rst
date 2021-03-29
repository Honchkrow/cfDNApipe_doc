fraglenplot_comp
================

This function is used for compare the fragments' lengths in case and control samples.


.. note::
   This function do not suit for single end data.

Parameters
~~~~~~~~~~

.. code:: python

    fraglenplot_comp(casebedInput=None, ctrlbedInput=None, 
                     outputdir=None, maxLimit=500, 
                     labelInput=None, threads=1, 
                     stepNum=None, caseupstream=None, 
                     ctrlupstream=None, verbose=True,)


-  casebedInput: list, input bed files of case samples.
-  ctrlbedInput: list, input bed files of control samples.
-  outputdir: str, output result folder, None means the same folder as input files.
-  maxLimit: int, maximum length to be considered.
-  labelInput: list, [name_of_case, name_of_control](e.g. ["HCC", "CTR"])
-  ratio1, ratio2: proportion statistics break point, default: 150, 400
-  threads: int, how many thread to use.
-  stepNum: int or str, step flag for folder name.
-  caseupstream: upstream output results, used for pipeline.
-  ctrlupstream: upstream output results, used for pipeline.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.



Example usage:

.. code:: python

    from cfDNApipe import *
    import glob
    import pysam

    pipeConfigure(
        threads=20,
        genome="hg19",
        refdir=r"path_to_genome/hg19",
        outdir=r"path_to_output/pcs_fraglen",
        data="WGS",
        type="paired",
        JavaMem="10G",
        build=True,
    )

    verbose = False

    case_bed = glob.glob("/data/wzhang/pcs_final/HCC/*.bed")
    ctrl_bed = glob.glob("/data/wzhang/pcs_final/Healthy/*.bed")

    res_fraglenplot_comp = fraglenplot_comp(
        casebedInput=case_bed, ctrlbedInput=ctrl_bed, caseupstream=True, verbose=verbose)

