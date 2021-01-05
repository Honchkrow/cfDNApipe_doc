computeDMR
==========

This function is used for compute DMR between case and aontrol samples.


Parameters
~~~~~~~~~~

.. code:: python

    computeDMR(casetxtInput=None, ctrltxtInput=None, 
               outputdir=None, threads=1, adjmethod=None, 
               caseupstream=None, ctrlupstream=None, stepNum=None)


-  casetxtInput: list, input methylation level files of case samples.
-  ctrlbedInput: list, input methylation level files of control samples.
-  outputdir: str, output result folder, None means the same folder as input files.
-  threads: int, how many thread to use.
-  adjmethod: str, method of p_value correction, must be "bonferroni", "fdr_bh"(default), "fdr_by" or "holm"
-  caseupstream: upstream output results, used for pipeline.
-  ctrlupstream: upstream output results, used for pipeline.
-  stepNum: int or str, step flag for folder name.




Example usage:

.. code:: python

    # this function is used for bismark output

    from cfDNApipe import *

    pipeConfigure2(
        threads=60,
        genome="hg19",
        refdir="path_to_reference/hg19_bismark",
        outdir="path_to_output/WGBS_single",
        data="WGBS",
        type="single",
        case="HCC",
        ctrl="Healthy",
        JavaMem="10G",
        build=True,
    )

    # case_calMethy and ctrl_calMethy are from function calculate_methyl
    switchConfigure("HCC")
    computeDMR(caseupstream=case_calMethy, ctrlupstream=ctrl_calMethy, adjmethod="fdr_bh")