cfDNAWGBS
=========

This function is used for processing paired/single end WGBS data.


.. note::
    User must set Configure or pipeConfigure before using.
    This function provides basic processing steps, like alignment and bis correction.
    If you want to perform comparison process, please use cfDNAWGBS2.

Parameters
~~~~~~~~~~

.. code:: python

    cfDNAWGS(inputFolder=None,
             fastq1=None,
             fastq2=None,
             adapter1=["AGATCGGAAGAGCACACGTCTGAACTCCAGTCA"],
             adapter2=["AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT"],
             fastqcOP=None,
             idAdapter=True,
             idAdOP=None,
             rmAdapter=True,
             rmAdOP={"--qualitybase": 33, "--gzip": True},
             bowtie2OP={"-q": True, "-N": 1, "--time": True},
             dudup=True,
             CNV=False,
             armCNV=False,
             fragProfile=False,
             report=False,
             verbose=False,
             box=True)


-  inputFolder: str, input fastq file folder path. Setting this parameter means disable fastq1 and fastq2.
-  fastq1: list, fastq1 files.
-  fastq2: list, fastq2 files.
-  adapter1: list, adapters for fastq1 files, if idAdapter is True, this paramter will be disabled.
-  adapter2: list, adapters for fastq2 files, if idAdapter is True, this paramter will be disabled.
-  fastqcOP: Other parameters used for fastqc, please see class "fastqc".
-  idAdapter: Ture or False, identify adapters or not. This module is not for single end data.
-  idAdOP: Other parameters used for AdapterRemoval, please see class "identifyAdapter".
        If idAdapter is False, this paramter will be disabled.
-  rmAdapter: Ture or False, remove adapters or not.
-  rmAdOP: Other parameters used for AdapterRemoval, please see class "adapterremoval".
        Default: {"--qualitybase": 33, "--gzip": True}.
        If rmAdapter is False, this paramter will be disabled.
-  bowtie2OP: Other parameters used for Bowtie2, please see class "bowtie2".
        Default: {"-q": True, "-N": 1, "--time": True}.
-  dudup: Ture or False, remove duplicates for bowtie2 results or not.
-  CNV: Compute basic CNV or not. This CNV detection funvtion is using the default genome as reference, so no control samples is accepted.
-  armCNV: Compute basic arm level CNV related values. The arm level cnv detection needs control/healthy samples,
        therefore only operating function "cfDNAWGS" will get no results in report. This function is designed for case control
        study in function cfDNAWGS2.
-  fragProfile: Compute basic fragProfile (long short fragement statistics) related values. This module is not for single end data.
            The fragProfile detection needs control/healthy samples, therefore only operating function "cfDNAWGS" will get no
            results in report. This function is designed for case control study in function cfDNAWGS2.
-  report: Generate user report or not.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.
-  box: output will be a box class. that mean the the results can be specified using '.',
    otherwise, the results will be saved as dict.


Example usage:

.. code:: python

    from cfDNApipe import *

    pipeConfigure(path_to_genomepath_to_output/output
        threads=60,
        genome="hg19",
        refdir=r"path_to_genome/hg19_bowtie2",
        outdir=r"path_to_output/output",
        data="WGS",
        type="paired",
        build=True,
        JavaMem="10g",
    )

    res = cfDNAWGS(
        inputFolder=r"path_to_fastqs",
        idAdapter=True,
        rmAdapter=True,
        dudup=True,
        CNV=True,
        armCNV=True,
        fragProfile=True,
        verbose=False,
    )

