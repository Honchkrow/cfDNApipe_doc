pipeConfigure2
==============

This function is used for setting Configures.

.. note::
   This function is designed for case control comparison.

Parameters
~~~~~~~~~~

.. code:: python

    pipeConfigure2(threads=(cpu_count() / 2), genome=None, refdir=None, outdir=None,
                   data=None, type=None, case=None, ctrl=None, build=False,)
                  

-  threads: int, how many thread to use, default: 1. Do not use the entire computational resources.
-  genome: str, which genome you want to use, 'hg19' or 'hg38'.
-  refdir: reference folder for aligner (bowtie2 or bismark).
-  outdir: Overall result folder.
-  data: data type, 'WGBS' or 'WGS'.
-  type: data type, 'paired' or 'single'.
-  JavaMem: Java memory for every thred, "10G" like.
-  case: case NAME for creating case specific folder.
-  ctrl: control NAME for creating control specific folder.


Example usage:

.. code:: python

    from cfDNApipe import *

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


