cfDNAWGBS2
==========

This function is used for case/control analysis of paired/single end WGBS data.


.. note::
    User must set pipeConfigure2 before using.

Parameters
~~~~~~~~~~

.. code:: python

    cfDNAWGBS2(caseFolder=None,
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
               bismarkOP={"-q": True, "--phred33-quals": True, "--bowtie2": True, "--un": True},
               dudup=True,
               dudupOP=None,
               extractMethyOP={
                    "--no_overlap": True,
                    "--report": True,
                    "--no_header": True,
                    "--gzip": True,
                    "--bedGraph": True,
                    "--zero_based": True,
               },
               methyRegion=None,
               armCNV=False,
               CNV=False,
               fragProfile=False,
               OCF=False,
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
-  bismarkOP: Other parameters used for Bismark, please see class "bismark".
                   Default: {"-q": True, "--phred33-quals": True, "--bowtie2": True, "--un": True,}.
-  dudup: Ture or False, remove duplicates for bismark results or not.
-  dudupOP: Other parameters used for bismark_deduplicate, please see class "bismark_deduplicate".
                 If dudup is False, this paramter will be disabled.
-  extractMethyOP: Other parameters used for bismark_methylation_extractor, please see class "bismark_methylation_extractor".
                        Default: {"--no_overlap": True, "--report": True, "--no_header": True, "--gzip": True,
                                  "--bedGraph": True, "--zero_based": True,}
-  methyRegion: Bed file contains methylation related regions.
-  armCNV:  Compute arm level CNV or not.
-  CNV: Compute CNV or not. If True, CNV will be computed for 3 group: case VS reference,
             control VS reference, case VS control.
-  fragProfile: Compute basic fragProfile(long short fragement statistics) or not. This module is not for single end data.
-  deconvolution: Compute tissue proportion for each sample or not.
-  OCF: Compute OCF or not.
-  report: Generate user report or not.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.
-  box: output will be a box class. that mean the the results can be specified using '.',
             otherwise, the results will be saved as dict.


Example usage:

.. code:: python

    from cfDNApipe import *

    pipeConfigure2(
        threads=20,
        genome="hg19",
        refdir="path_to_genome/hg19_bismark",
        outdir="path_to_output/pipeline-WGBS-comp",
        data="WGBS",
        type="paired",
        JavaMem="8G",
        case="cancer",
        ctrl="normal",
        build=True,
    )

    case, ctrl, comp = cfDNAWGBS2(
        caseFolder="path_to_fastqs/raw/case",
        ctrlFolder="path_to_fastqs/raw/ctrl",
        caseName="cancer",
        ctrlName="tumor",
        idAdapter=True,
        rmAdapter=True,
        dudup=True,
        armCNV=True,
        CNV=True,
        fragProfile=True,
        deconvolution=True,
        OCF=True,
        verbose=True,
    )