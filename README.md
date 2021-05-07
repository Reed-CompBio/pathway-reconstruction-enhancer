# Signaling Pathway Reconstruction Analysis Streamliner (SPRAS)
[![Test Docker wrappers](https://github.com/Reed-CompBio/spras/actions/workflows/test-docker-wrappers.yml/badge.svg)](https://github.com/Reed-CompBio/spras/actions/workflows/test-docker-wrappers.yml)

SPRAS is a work-in-progress dockerized library of pathway reconstruction enhancement tools.
The framework will contain different pathway reconstruction algorithms that connect genes and proteins of interest in the context of a general protein-protein interaction network, allowing users to run multiple algorithms on their inputs.
See the [GLBIO 2021 slides](https://doi.org/10.6084/m9.figshare.14551476) for more information.
To read more about the specific pathway reconstruction algorithms that may be supported in the future, refer to [our list of algorithms](doc/) within the `doc/` directory.

This repository is very much a work in progress and is not yet stable enough for real data analysis.
The latest features can be found on various development branches.
The instructions below support running SPRAS with a fixed configuration on example data to demonstrate its functionality.
Open a [GitHub issue](https://github.com/Reed-CompBio/spras/issues) or contact [Anthony Gitter](https://www.biostat.wisc.edu/~gitter/) or [Anna Ritz](https://www.reed.edu/biology/ritz/) to provide feedback on this early version of SPRAS.

SPRAS is inspired by tools for single-cell transcriptomics such as [BEELINE](https://github.com/Murali-group/Beeline) and [dynverse](https://github.com/dynverse) that provide a unified interface to many related algorithms.

## Installing and running SPRAS
SPRAS requires
- Files in this repository
- Python
- The Python packages listed in [`environment.yml`](environment.yml)
- Docker

First, download or clone this repository so that you have the Snakefile, example config file, and example data.

The easiest way to install Python and the required packages is with [Anaconda](https://www.anaconda.com/download/).
The Carpentries [Anaconda installation instructions](https://carpentries.github.io/workshop-template/#python) provide guides and videos on how to install Anaconda for your operating system.
After installing Anaconda, you can run
```
conda env create -f environment.yml
conda activate spras
```
to create a conda environment with the required packages and activate that environment.
If you have a different version of Python already, you can install the specified versions of the required packages in your preferred manner instead of using Anaconda.

You also need to install [Docker](https://docs.docker.com/get-docker/).
After installing Docker, start Docker before running SPRAS.

Once you have activated the conda environment and started Docker, you can run SPRAS with the example Snakemake workflow.
From the root directory of the `spras` repository, run the command
```
snakemake --cores 1
```
This will run the SPRAS workflow with the example config file and input files.
Output files will be written to the `output` directory.

You do not need to manually download Docker images from DockerHub before running SPRAS.
The workflow will automatically download any missing images as long as Docker is running.

## Components
**Configuration file**: Specifies which pathway reconstruction algorithms to run, which hyperparameter combinations to use, and which datasets to run them on.

**Snakemake file**: Defines a workflow to run all pathway reconstruction algorithms on all datasets with all specified hyperparameters.

**Dockerized pathway reconstruction algorithms**: Pathway reconstruction algorithms are run via Docker images using the docker-py Python package.
[PathLinker](https://github.com/Murali-group/PathLinker), [Omics Integrator](https://github.com/fraenkel-lab/OmicsIntegrator), and [Omics Integrator 2](https://github.com/fraenkel-lab/OmicsIntegrator2) are the first supported algorithms.
The files to create these Docker images are in the `docker-wrappers` subdirectory along with links to algorithms' original repositories.
The Docker images are available on [DockerHub](https://hub.docker.com/orgs/reedcompbio).

**Python wrapper for calling algorithms**: Wrapper functions provide an interface between the common file formats for input and output data and the algorithm-specific file formats and reconstruction commands.
These wrappers are in the `src/` subdirectory.

**Test code**: Tests for the Docker wrappers.
The tests require the conda environment in `environment.yml` and Docker.
Run the tests with `pytest -s`.

## Docker demo
The `docker-demo` subdirectory is not used by the main pathway reconstruction framework.
It serves as a reference for how to set up Dockerfiles and make Docker run calls.

## Attribution
SPRAS builds on public datasets and algorithms.
If you use SPRAS in a research project, please cite the original datasets and algorithms in addition to SPRAS.
