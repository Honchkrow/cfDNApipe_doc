OCFplot
=======

This function is used for plotting the boxplot of OCF values from computeOCF.


Parameters
~~~~~~~~~~

.. code:: python

    OCFplot(caseocfInput=None, ctrlocfInput=None, 
            outputdir=None, threads=1, 
            upstream=None, labelInput=None, 
            stepNum=None,  verbose=True)
        


-  caseocfInput: list, input files of OCF values of case samples.
-  ctrlocfInput: list, input files of OCF values of control samples.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  upstream: upstream output results, used for pipeline.
-  labelInput: list, [name_of_case, name_of_control](e.g. ["HCC", "CTR"])
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

