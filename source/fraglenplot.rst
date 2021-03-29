fraglenplot
===========

This function is used for ploting fragment length distribution.


.. note::
   This function do not suit for single end data.

Parameters
~~~~~~~~~~

.. code:: python

    fraglenplot(bedInput=None, outputdir=None, 
                maxLimit=500, threads=None, 
                stepNum=None, upstream=None,
                verbose=True)


-  bedInput: list, input bed files.
-  outputdir: str, output result folder, None means the same folder as input files.
-  maxLimit: int, maximum length to be considered.
-  ratio1, ratio2: proportion statistics break point, default: 150, 400
-  threads: int, how many thread to use.
-  stepNum: int or str, step flag for folder name.
-  upstream: upstream output results, used for pipeline. This parameter can be True, which means a new pipeline start.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.



Example usage:

.. code:: python

   beds = ["test1.bed", "test2.bed"]

   fraglenplot(bedInput=beds)
