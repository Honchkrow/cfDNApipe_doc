GCCorrect
=========

This function is used for processing GC correction on read count data in wig or csv/txt files.


Parameters
~~~~~~~~~~

.. code:: python

    GCcorrect(readInput=None, gcwigInput=None, 
              readtype=None, corrkey=None, 
              outputdir=None, threads=1, 
              stepNum=None, readupstream=None, 
              gcupstream=None, verbose=True,)
        

-  readInput: list, paths of input files of read counts.
-  gcwigInput: list, paths of wig files of gc contents.
-  readtype: int, file type of readInput, 1 for .wig, 2 for .txt/.csv.; 1 is set by default.
-  corrkey: char, type of GC correction, "-" for minus, "/" for divide, "0" for process without GC correction; "/" is set by default
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  stepNum: Step number for folder name.
-  readupstream: upstream output results, used for pipeline.
-  gcupstream: upstream output results, used for pipeline.
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