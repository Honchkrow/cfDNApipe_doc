cfDNAWGS2
=========

This function is used for case/control analysis of paired/single end WGS data.


.. note::
    User must set pipeConfigure2 before using.

Parameters
~~~~~~~~~~

.. code:: python

    cfDNAWGS2(caseFolder=None,
              ctrlFolder=None,
              caseName=None,
              ctrlName=None,
              caseFq1=None,
              caseFq2=None,
              ctrlFq1=None,
              ctrlFq2=None,
              caseAdapter1=["AGATCGGAAGAGCACACGTCTGAACTCCAGTCA"],
              caseAdapter2=["AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT"],
              ctrlAdapter1=["AGATCGGAAGAGCACACGTCTGAACTCCAGTCA"],
              ctrlAdapter2=["AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT"],
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

-  caseFolder: str, input case fastq file folder path. Setting this parameter means disable fastq1 and fastq2.
-  ctrlFolder: str, input control fastq file folder path. Setting this parameter means disable fastq1 and fastq2.
-  caseFq1: list, case fastq1 files.
-  caseFq2: list, case fastq2 files.
-  ctrlFq1: list, control fastq1 files.
-  ctrlFq2: list, control fastq2 files.
-  caseAdapter1: list, adapters for case fastq1 files, if idAdapter is True, this paramter will be disabled.
-  caseAdapter2: list, adapters control for case fastq2 files, if idAdapter is True, this paramter will be disabled.
-  ctrlAdapter1: list, adapters control for fastq1 files, if idAdapter is True, this paramter will be disabled.
-  ctrlAdapter2: list, adapters for fastq2 files, if idAdapter is True, this paramter will be disabled.
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
-  CNV: Compute CNV or not. If True, CNV will be computed for 3 group: case VS reference,
        control VS reference, case VS control.
-  armCNV:  Compute arm level CNV or not.
-  fragProfile: Compute basic fragProfile(long short fragement statistics) or not. This module is not for single end data.
        OCF: Compute OCF or not.
-  report: Generate user report or not.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.
-  box: output will be a box class. that mean the the results can be specified using '.',
        otherwise, the results will be saved as dict.


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

    case, ctrl, comp = cfDNAWGS2(
        caseFolder="path_to_fastqs/raw/case",
        ctrlFolder="path_to_fastqs/raw/ctrl",
        caseName="cancer",
        ctrlName="normal",
        idAdapter=True,
        rmAdapter=True,
        rmAdOP={"--qualitybase": 33, "--gzip": True},
        bowtie2OP={"-q": True, "-N": 1, "--time": True},
        dudup=True,
        CNV=True,
        armCNV=True,
        fragProfile=True,
        OCF=True,
        report=True,
        verbose=False,
    )