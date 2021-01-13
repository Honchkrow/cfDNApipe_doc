pipeConfigure
=============

This function is used for setting Configures.


Parameters
~~~~~~~~~~

.. code:: python

    pipeConfigure(threads=(cpu_count() / 2), genome=None, 
                  refdir=None, outdir=None, data=None, 
                  type=None, build=False)


                  
-  threads: int, how many thread to use, default: 1. Do not use the entire computational resources.
-  genome: str, which genome you want to use, must be 'hg19' or 'hg38'.
-  refdir: reference folder for aligner (bowtie2 or bismark) and other reference files.
-  outdir: Overall result folder, it usually contains tmpdir, finaldir and repdir.
-  data: Input data type, 'WGBS' or 'WGS'.
-  type: Input sequencing type, 'paired' or 'single'.
-  JavaMem: Java memory for every thred, "10G" like.
-  build: Whether checking reference and building reference once not detect.


Example usage:

.. code:: python

    from cfDNApipe import *
    import glob

    pipeConfigure(
        threads=60,
        genome="hg19",
        refdir=r"path_to_reference/hg19",
        outdir=r"path_to_output/snv_output",
        data="WGS",
        type="paired",
        build=True,
        JavaMem="10G",
    )


