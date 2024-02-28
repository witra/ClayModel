# Clay Foundation Model

[![Jupyter Book Badge](https://jupyterbook.org/badge.svg)](https://clay-foundation.github.io/model)
[![Deploy Book Status](https://github.com/Clay-foundation/model/actions/workflows/deploy-docs.yml/badge.svg)](https://github.com/Clay-foundation/model/actions/workflows/deploy-docs.yml)
[![Continuous Integration Tests Status](https://github.com/Clay-foundation/model/actions/workflows/test.yml/badge.svg)](https://github.com/Clay-foundation/model/actions/workflows/test.yml)

An open source AI model and interface for Earth.

## Quickstart

Launch into a [JupyterLab](https://jupyterlab.readthedocs.io) environment on

| [Binder](https://mybinder.readthedocs.io/en/latest) | [Planetary Computer](https://planetarycomputer.microsoft.com) | [SageMaker Studio Lab](https://studiolab.sagemaker.aws) |
|:--:|:--:|:--:|
| [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Clay-foundation/model/main) | [![Open on Planetary Computer](https://img.shields.io/badge/Open-Planetary%20Computer-black?style=flat&logo=microsoft)](https://pccompute.westeurope.cloudapp.azure.com/compute/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2FClay-foundation%2Fmodel&urlpath=lab%2Ftree%2Fmodel%2Fplaceholder.ipynb&branch=main) | [![Open in SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/Clay-foundation/model/blob/main/placeholder.ipynb) |

## Installation

### Basic

To help out with development, start by cloning this [repo-url](/../../)

    git clone <repo-url>
    cd model

Then we recommend [using mamba](https://mamba.readthedocs.io/en/latest/installation.html)
to install the dependencies. A virtual environment will also be created with Python and
[JupyterLab](https://github.com/jupyterlab/jupyterlab) installed.

    mamba env create --file environment.yml

> [!NOTE]
> The command above will only work for Linux devices with CUDA GPUs. For installation
> on macOS devices (either Intel or ARM chips), follow the 'Advanced' section in
> https://clay-foundation.github.io/model/installation.html#advanced

Activate the virtual environment first.

    mamba activate claymodel

Finally, double-check that the libraries have been installed.

    mamba list


## Usage

### Running jupyter lab

    mamba activate claymodel
    python -m ipykernel install --user --name claymodel  # to install virtual env properly
    jupyter kernelspec list --json                       # see if kernel is installed
    jupyter lab &


### Running the model

The neural network model can be ran via
[LightningCLI v2](https://pytorch-lightning.medium.com/introducing-lightningcli-v2supercharge-your-training-c070d43c7dd6).
To check out the different options available, and look at the hyperparameter
configurations, run:

    python trainer.py --help

To quickly test the model on one batch in the validation set:

    python trainer.py fit --model ClayMAEModule --data ClayDataModule --config configs/config.yaml --trainer.fast_dev_run=True

To train the model:

    python trainer.py fit --model ClayMAEModule --data ClayDataModule --config configs/config.yaml

More options can be found using `python trainer.py fit --help`, or at the
[LightningCLI docs](https://lightning.ai/docs/pytorch/2.1.0/cli/lightning_cli.html).

## Contributing

### Writing documentation

Our Documentation uses [Jupyter Book](https://jupyterbook.org/intro.html).

Install it with:
In order to avoid installing dependencies directly onto the machine, a Dockerized version of the Clay model has been provided. To run it using [docker compose](https://docs.docker.com/compose/) run the following command from the repo root:
```bash
pip install -U jupyter-book
```

Then build it with:
Alternatively, you can build and run the Docker image using docker directly with:
```bash
jupyter-book build docs/
```

You can preview the site locally with:
In both cases, the default command will be run, which instantiates a [jupyter-lab](https://jupyterlab.readthedocs.io/en/stable/getting_started/starting.html) instance, from which the documentation notebooks can be run. The jupyter notebook will be accssible on the exposed port (`8888`).
### Note: accessing the jupyter-lab instance in the browser requires an authentication token that can be found in the container logs. Look for the following lines:
```bash
python -m http.server --directory _build/html
```

There is a GitHub Action on `./github/workflows/deploy-docs.yml` that builds the site and pushes it to GitHub Pages.
Additionally the project root will be mounted as volume to the running container so any modifications made to the code base locally will be immediately reflected within the docker container.

### Custom commands:

This default command can be overridden with a custom command:
```bash
docker-compose run claymodel {custom command}
```
or using docker directly:
```bash
docker run --rm -it -v $(pwd):/model -p 8888:8888 -e ENV_NAME=claymodel --platform linux/amd64 claymodel:latest {custom command}
```

For example, the `bash` command can be used to access an interactive bash session within the running docker container, with the `micromamba` environment already activated: 
(with docker-compose)
For example, the `bash` command can be used to access an interactive bash session within the running docker container, with the `micromamba` environment already activated:
```bash
# with docker-compose
docker-compose run claymdel bash
(claymodel) mambauser@f04261284e87:/model$
```
```bash
# with straight docker
docker run --rm -it -v $(pwd):/model -p 8888:8888 -e ENV_NAME=claymodel --platform linux/amd64 claymodel:latest bash
(claymodel) mambauser@f04261284e87:/model$
```
