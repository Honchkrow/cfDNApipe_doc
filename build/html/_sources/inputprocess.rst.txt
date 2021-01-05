inputprocess
============

This function is used for auto input files arrangement.


.. note::
   This function is designed for pipeline, so user must set Configure before using.

Parameters
~~~~~~~~~~

.. code:: python

    inputprocess(fqInput1=None, fqInput2=None, 
                 inputFolder=None, stepNum=1, 
                 upstream=None)

-  fqInput1: list, fastq files for single end data;  _1 files for paired end data.
-  fqInput2: list, [] for single end data;  _2 files for paired end data.
-  inputFolder: input folder contains all your input fastq data, the program will detect inputs automatically.
-  stepNum: Step number for folder name.
-  upstream: Not used parameter, do not set this parameter.


Example usage:

.. code:: python

   inputprocess(inputFolder="path_to_data")

