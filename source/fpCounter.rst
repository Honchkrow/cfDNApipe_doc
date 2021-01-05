fpCounter
=========

This function is used for counting total reads or short-and-long-reads of the input bedgz files.


Parameters
~~~~~~~~~~

.. code:: python

    fpCounter(bedgzInput=None, chromsizeInput=None, 
            blacklistInput=None, gapInput=None, domains=None,
            binlen=None, outputdir=None, threads=1, 
            processtype=None, stepNum=None, 
            upstream=None, verbose=True,)

-  bedgzInput: list, paths of input bedgz files waiting to be processed.
-  chromsizeInput: str, path of chromsize file.
-  blacklistInput: str, used in fragmentation profile, path of blacklist file.
-  gapInput: str, used in fragmentation profile, path of gap file.
-  domains: list, used in fragmentation profile, [minimum_length_of_short_fragments, maximum_length_of_short_fragments,
            minimum_length_of_long_fragments, maximum_length_of_long_fragments]; default is [100, 150, 151, 220].
-  binlen: int, length of each bin; default is 5000000(5Mb) for fragmentation profile, or 100000(100kb) for CNV.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  processtype: int, 1 for fragmentation profile, 2 for CNV.
-  stepNum: Step number for folder name.
-  upstream: Not used parameter, do not set this parameter.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.


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
