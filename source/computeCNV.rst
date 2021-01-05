computeCNV
==========

This function is used for computing CNV z-scores.


Parameters
~~~~~~~~~~

.. code:: python

    computeCNV(casetxtInput=None, ctrltxtInput=None, 
               outputdir=None, cytoBandInput=None, 
               threads=1, stepNum=None, caseupstream=None, 
               ctrlupstream=None, verbose=True,)

-  casetxtInput: list, paths of files of GC corrected read counts for case samples.
-  ctrltxtInput: list, paths of files of GC corrected read counts for control samples.
-  outputdir: str, output result folder, None means the same folder as input files.
-  cytoBandInput: str, path of the cytoBand file.
-  threads: int, how many thread to use.
-  stepNum: Step number for folder name.
-  caseupstream: Not used parameter, do not set this parameter.
-  ctrlupstream: Not used parameter, do not set this parameter.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


.. warning::
    We recommend using this function in arm-level CNV detection.

Example usage:

.. code:: python

    # an example for compute arm-level CNV
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

    case_bam = glob.glob("path_to_data/HCC/*.bam")
    ctrl_bam = glob.glob("path_to_data/CTR/*.bam")

    # case
    switchConfigure("cancer")
    case_bamCounter = bamCounter(
        bamInput=case_bam, upstream=True, verbose=verbose, stepNum="case01"
    )
    case_gcCounter = runCounter(
        filetype=0, upstream=True, verbose=verbose, stepNum="case02"
    )
    case_GCCorrect = GCCorrect(
        readupstream=case_bamCounter,
        gcupstream=case_gcCounter,
        verbose=verbose,
        stepNum="case03",
    )

    # ctrl
    switchConfigure("normal")
    ctrl_bamCounter = bamCounter(
        bamInput=ctrl_bam, upstream=True, verbose=verbose, stepNum="ctrl01"
    )
    ctrl_gcCounter = runCounter(
        filetype=0, upstream=True, verbose=verbose, stepNum="ctrl02"
    )
    ctrl_GCCorrect = GCCorrect(
        readupstream=ctrl_bamCounter,
        gcupstream=ctrl_gcCounter,
        verbose=verbose,
        stepNum="ctrl03",
    )

    switchConfigure("cancer")
    res_computeCNV = computeCNV(
        caseupstream=case_GCCorrect,
        ctrlupstream=ctrl_GCCorrect,
        stepNum="ARMCNV",
        verbose=verbose,
    )