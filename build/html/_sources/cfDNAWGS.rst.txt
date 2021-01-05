cfDNAWGS
========

This function is used for processing paired/single end WGS data.


.. note::
    User must set Configure or pipeConfigure before using.
    This function provides basic processing steps, like alignment and bis correction.
    If you want to perform comparison process, please use cfDNAWGBS2.

Parameters
~~~~~~~~~~

.. code:: python

    cfDNAWGBS(inputFolder=None, fastq1=None, fastq2=None,
              adapter1=["AGATCGGAAGAGCACACGTCTGAACTCCAGTCA"],
              adapter2=["AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT"],
              fastqcOP=None,
              idAdapter=True,
              idAdOP=None,
              rmAdapter=True,
              rmAdOP={"--qualitybase": 33, "--gzip": True},
              bismarkOP={"-q": True, "--phred33-quals": True, "--bowtie2": True, "--un": True,},
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
              CNV=False,
              fragProfile=False,
              deconvolution=False,
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
-  bismarkOP: Other parameters used for Bismark, please see class "bismark".
            Default: {"-q": True, "--phred33-quals": True, "--bowtie2": True, "--un": True,}.
-  dudup: Ture or False, remove duplicates for bismark results or not.
-  dudupOP: Other parameters used for bismark_deduplicate, please see class "bismark_deduplicate".
            If dudup is False, this paramter will be disabled.
-  extractMethyOP: Other parameters used for bismark_methylation_extractor, please see class "bismark_methylation_extractor".
                Default: {"--no_overlap": True, "--report": True, "--no_header": True, "--gzip": True,
                            "--bedGraph": True, "--zero_based": True,}
-  methyRegion: Bed file contains methylation related regions.
-  CNV: Compute basic CNV or not.
-  armCNV: Compute basic arm level CNV related values. The arm level cnv detection needs control/healthy samples,
        therefore only operating function "cfDNAWGS" will get no results in report. This function is designed for case control
        study in function cfDNAWGS2.
-  fragProfile: Compute basic fragProfile (long short fragement statistics) related values. This module is not for single end data.
                The fragProfile detection needs control/healthy samples, therefore only operating function "cfDNAWGS" will get no
                results in report. This function is designed for case control study in function cfDNAWGS2.
-  deconvolution: Compute tissue proportion for each sample or not.
-  report: Generate user report or not.
-  verbose: bool, True means print all stdout, but will be slow; False means black stdout verbose, much faster.
-  box: output will be a box class. that mean the the results can be specified using '.',
        otherwise, the results will be saved as dict.


Example usage:

.. code:: python

    from cfDNApipe import *

    pipeConfigure(
        threads=60,
        genome="hg19",
        refdir=r"path_to_genome/hg19_bismark",
            outdir=r"path_to_output/output",
        data="WGBS",
        type="paired",
        build=True,
        JavaMem="10g",
    )

    res = cfDNAWGBS(
        inputFolder=r"path_to_fastqs",
        idAdapter=True,
        rmAdapter=True,
        dudup=True,
        CNV=True,
        armCNV=True,
        fragProfile=True,
        report=True,
        verbose=False,
    )