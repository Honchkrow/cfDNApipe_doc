Configure2
==========

This function is used for setting all the pipe configures.


.. note::
   Configure or Configure2 should be called before analysis.

Parameters
~~~~~~~~~~

.. code:: python

    # get configure names
    Configure2.getConfigs()

    # get configure through name
    Cinfigure2.getConfig(key)

    # set configure through name
    Cinfigure2.setConfig(key, val)

    # additional function: check virus genome
    Configure2.virusGenomeCheck(folder=None, build=False)
    
    # additional function: check SNV reference
    Configure2.snvRefCheck(folder=None, build=False)


-  key, val: key and value.
-  folder: folder path to the virus genome or snv genome files.
-  build: download and build by cfDNApipe or not.



Example usage:

.. code:: python

    from cfDNApipe import *

    pipeConfigure2(
        threads=60,
        genome="hg19",
        refdir="path_to_genome/hg19_bowtie2",
        outdir="path_to_output/pipeline-WGS-comp",
        data="WGS",
        type="paired",
        JavaMem="8G",
        case="cancer",
        ctrl="normal",
        build=True,
    )

    Configure2.getConfigs()

    Cinfigure2.getConfig("genome")

