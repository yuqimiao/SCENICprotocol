# Reproducible and scalable regulatory network reconstruction from single cell transcriptomics datasets using SCENIC

## Overview

* [Requirements](#requirements)
* [Quick start](#quick-start)
* [Full pipeline documentation](docs/pipeline.md)
* Case studies
  * PBMC 10k dataset (10x Genomics)
    * Full SCENIC analysis, plus filtering, clustering, visualization, and SCope-ready loom file creation:
      * [Jupyter notebook](notebooks/PBMC10k_SCENIC-protocol-CLI.ipynb) 
        | 
        [HTML render](http://htmlpreview.github.io/?https://github.com/aertslab/SCENICprotocol/blob/master/notebooks/PBMC10k_SCENIC-protocol-CLI.html)
    * Extended analysis post-SCENIC:
      * [Jupyter notebook](notebooks/PBMC10k_downstream-analysis.ipynb)
        | 
        [HTML render](http://htmlpreview.github.io/?https://github.com/aertslab/SCENICprotocol/blob/master/notebooks/PBMC10k_downstream-analysis.html)
  * Cancer data sets
    * [Jupyter notebook](notebooks/SCENIC%20Protocol%20-%20Case%20study%20-%20Cancer%20data%20sets.ipynb)
  * Mouse brain data set
    * [Jupyter notebook](notebooks/SCENIC%20Protocol%20-%20Case%20study%20-%20Mouse%20brain%20data%20set.ipynb)
* [References and more information](#references-and-more-information)


## Requirements

The following tools are required to run the steps in this pipeline:
* [Nextflow](https://www.nextflow.io/)
* A container system, either of:
    * [Docker](https://docs.docker.com/)
    * [Singularity](https://www.sylabs.io/singularity/)

The following container images will be pulled by nextflow as needed:
* Docker: [aertslab/pyscenic:latest](https://hub.docker.com/r/aertslab/pyscenic).
* Singularity: [aertslab/pySCENIC:latest](https://www.singularity-hub.org/collections/2033).
* [See also here.](https://github.com/aertslab/pySCENIC#docker-and-singularity-images)


---
## Quick start
### Running the pipeline on the example dataset

#### Download testing dataset

Download a minimum set of SCENIC database files for a human dataset (approximately 78 MB).
This small test dataset takes approiximately 30s to run using 6 threads on a standard desktop computer.

    mkdir example && cd example/
    # Transcription factors:
    wget https://raw.githubusercontent.com/aertslab/SCENICprotocol/master/example/allTFs_hg38.txt 
    # Motif to TF annotation database:
    wget https://raw.githubusercontent.com/aertslab/SCENICprotocol/master/example/motifs.tbl
    # Ranking databases:
    wget https://raw.githubusercontent.com/aertslab/SCENICprotocol/master/example/genome-ranking.feather
    # Finally, get a small sample expression matrix (loom format):
    wget https://raw.githubusercontent.com/aertslab/SCENICprotocol/master/example/expr_mat.loom


#### Running the example pipeline

Either Docker or Singularity images can be used by specifying the appropriate profile (`-profile docker` or `-profile singularity`).

##### Using loom input

    nextflow run aertslab/SCENICprotocol \
        -profile docker \
        --loom_input expr_mat.loom \
        --loom_output pyscenic_integrated-output.loom \
        --TFs allTFs_hg38.txt \
        --motifs motifs.tbl \
        --pyscenic_tag latest \
        --db *feather

By default, this pipeline uses the container tag specified by the `--pyscenic_tag` parameter.
This should currently be set to `latest` to avoid a bug in the pySCENIC v0.9.14 release.
A custom container can be used (e.g. one built on a local machine) by passing the name of this container to the `--pyscenic_container` parameter.

---

## References and more information

### SCENIC
* [SCENIC (R) on GitHub](https://github.com/aertslab/SCENIC)
* [SCENIC website](http://scenic.aertslab.org/)
* [SCENIC publication](https://doi.org/10.1016/j.cell.2018.05.057)
* [pySCENIC on GitHub](https://github.com/aertslab/pySCENIC)
* [pySCENIC documentation](https://pyscenic.readthedocs.io/en/latest/)

### SCope
* [SCope webserver](http://scope.aertslab.org/)
* [SCope on GitHub](https://github.com/aertslab/SCope)
* [SCopeLoomR](https://github.com/aertslab/SCopeLoomR)
* [SCopeLoomPy](https://github.com/aertslab/SCopeLoomPy)

### Scanpy
* [Scanpy on GitHub](https://github.com/theislab/scanpy)
* [Scanpy documentation](https://scanpy.readthedocs.io/)
* [Scanpy publication](https://doi.org/10.1186/s13059-017-1382-0)




