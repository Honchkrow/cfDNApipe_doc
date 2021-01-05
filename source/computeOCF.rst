computeOCF
==========

This function is used for compute OCF values of input bed files.


Parameters
~~~~~~~~~~

.. code:: python

    computeOCF(casebedInput=None, ctrlbedInput=None, 
               refRegInput=None, outputdir=None, 
               threads=1, caseupstream=None, 
               ctrlupstream=None, labelInput=None, 
               stepNum=None,  verbose=True)
        


-  casebedInput: list, input bed files of case samples.
-  ctrlbedInput: list, input bed files of control samples.
-  refRegInput: str, reference file for OCF computing.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  caseupstream: upstream output results, used for pipeline.
-  ctrlupstream: upstream output results, used for pipeline.
-  stepNum: int or str, step flag for folder name.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


.. warning::
    We recommend using this function in OCF analysis.

Example usage:

.. code:: python

    from cfDNApipe import *
    import glob

    pipeConfigure2(
        threads=20,
        genome="hg19",
        refdir=r"reference_genome/hg19",
        outdir=r"output/pcs_armCNV",
        data="WGS",
        type="paired",
        JavaMem="8G",
        case="cancer",
        ctrl="normal",
        build=True,
    )

    verbose = False

    case_bed = glob.glob("path_to_data/HCC/*.bed")
    ctrl_bed = glob.glob("path_to_data/CTR/*.bed")

    switchConfigure("cancer")

    res_computeOCF = computeOCF(
        casebedInput=case_bed,
        ctrlbedInput=ctrl_bed,
        caseupstream=True,
        verbose=verbose,
    )
    res_OCFplot = OCFplot(upstream=res_computeOCF, verbose=verbose)

