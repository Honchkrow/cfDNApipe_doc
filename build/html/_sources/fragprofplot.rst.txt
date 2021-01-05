fragprofplot
============

This function is used for computing and plotting the fragmentation profile plot.


Parameters
~~~~~~~~~~

.. code:: python

    fragprofplot(casetxtInput=None, ctrltxtInput=None, 
                 outputdir=None, cytoBandInput=None, 
                 stepNum=None, caseupstream=None,
                 ctrlupstream=None)

-  casetxtInput: list, paths of files of input short and long counts for case samples,
                [sample1_short, sample1_long, sample2_short, sample2_long, etc.].
-  ctrltxtInput: list, paths of files of input short and long counts for control samples(format is same as casetxtInput).
-  cytoBandInput: str, path of the cytoBand file.
-  labelInput: list, name of case and control(which will be marked on the output plot), [case_name, control_name]
-  outputdir: str, output result folder, None means the same folder as input files.
-  stepNum: Step number for folder name.
-  caseupstream: Not used parameter, do not set this parameter.
-  ctrlupstream: Not used parameter, do not set this parameter.

.. warning::
    We recommend using this function in short long fragmentation analysis.

Example usage:

.. code:: python

    # an example for short long fragmentation analysis
    from cfDNApipe import *
    import glob

    pipeConfigure2(
        threads=20,
        genome="hg19",
        refdir=r"path_to_genome/hg19",
        outdir=r"path_to_output/pcs_fp",
        data="WGS",
        type="paired",
        JavaMem="8G",
        case="cancer",
        ctrl="normal",
        build=True,
    )

    verbose = False

    # these bed.gz output can be get from bam2bed function in cfDNApipe
    case_bedgz = glob.glob("/data/wzhang/pcs_final/HCC/*.bed.gz")
    ctrl_bedgz = glob.glob("/data/wzhang/pcs_final/Healthy/*.bed.gz")

    # case
    switchConfigure("cancer")
    case_fragCounter = fpCounter(
        bedgzInput=case_bedgz, upstream=True, verbose=verbose, stepNum="case01", processtype=1
    )
    case_gcCounter = runCounter(
        filetype=0, binlen=5000000, upstream=True, verbose=verbose, stepNum="case02"
    )
    case_GCCorrect = GCCorrect(
        readupstream=case_fragCounter,
        gcupstream=case_gcCounter,
        readtype=2,
        corrkey="-",
        verbose=verbose,
        stepNum="case03",
    )

    # ctrl
    switchConfigure("normal")
    ctrl_fragCounter = fpCounter(
        bedgzInput=ctrl_bedgz, upstream=True, verbose=verbose, stepNum="ctrl01", processtype=1
    )
    ctrl_gcCounter = runCounter(
        filetype=0, binlen=5000000, upstream=True, verbose=verbose, stepNum="ctrl02"
    )
    ctrl_GCCorrect = GCCorrect(
        readupstream=ctrl_fragCounter,
        gcupstream=ctrl_gcCounter,
        readtype=2,
        corrkey="-",
        verbose=verbose,
        stepNum="ctrl03",
    )

    switchConfigure("cancer")
    res_fragprofplot = fragprofplot(
        caseupstream=case_GCCorrect,
        ctrlupstream=ctrl_GCCorrect,
        stepNum="FP",
    )
