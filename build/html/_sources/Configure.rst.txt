Configure
=========

This function is used for setting all the pipe configures.


.. note::
   Configure or Configure2 should be called before analysis.

Parameters
~~~~~~~~~~

.. code:: python

    # get configure names
    Configure.getConfigs()

    # get configure through name
    Cinfigure.getConfig(key)

    # set configure through name
    Cinfigure.setConfig(key, val)

    # additional function: check virus genome
    Configure.virusGenomeCheck(folder=None, build=False)
    
    # additional function: check SNV reference
    Configure.snvRefCheck(folder=None, build=False)


-  key, val: key and value.
-  folder: folder path to the virus genome or snv genome files.
-  build: download and build by cfDNApipe or not.



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

    Configure.getConfigs()

    Cinfigure.getConfig("genome")

