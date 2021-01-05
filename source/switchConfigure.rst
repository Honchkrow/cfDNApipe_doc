switchConfigure
===============

Switch Configure for case and control, these two situation have different output directory
parameter confName: one of the Configure name defined in Configure2 (case and ctrl)

Parameters
~~~~~~~~~~

.. code:: python

    switchConfigure(confName=None)


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

    switchConfigure("cancer")

    switchConfigure("normal")




