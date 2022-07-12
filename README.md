[![View on Medium](https://img.shields.io/badge/Medium-View%20on%20Medium-blue?logo=medium)](https://towardsdatascience.com/how-to-structure-a-data-science-project-for-readability-and-transparency-360c6716800)

# Data Science Cookie Cutter
This repo is forked from khuyentran1401 and adjusted with my personal touch.

**Note**: _This template uses poetry. If you prefer using pip, go to the [pip](https://github.com/khuyentran1401/data-science-template/tree/pip) branch instead._
## What is this?
This repository is a template for a data science project. This is the project structure I frequently use for my data science project. 

## Tools used in this project
* [GitHubCLI](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git): Controlling and caching your git credentials
* [pyenv](https://github.com/pyenv/pyenv): Control your Python version
* [Poetry](https://towardsdatascience.com/how-to-effortlessly-publish-your-python-package-to-pypi-using-poetry-44b305362f9f): Dependency management - [article](https://towardsdatascience.com/how-to-effortlessly-publish-your-python-package-to-pypi-using-poetry-44b305362f9f)
* [hydra](https://hydra.cc/): Manage configuration files - [article](https://towardsdatascience.com/introduction-to-hydra-cc-a-powerful-framework-to-configure-your-data-science-projects-ed65713a53c6)
* [pre-commit plugins](https://pre-commit.com/): Automate code reviewing formatting  - [article](https://towardsdatascience.com/4-pre-commit-plugins-to-automate-code-reviewing-and-formatting-in-python-c80c6d2e9f5?sk=2388804fb174d667ee5b680be22b8b1f)
* [DVC](https://dvc.org/): Data version control - [article](https://towardsdatascience.com/introduction-to-dvc-data-version-control-tool-for-machine-learning-projects-7cb49c229fe0)
* [pdoc](https://github.com/pdoc3/pdoc): Automatically create an API documentation for your project
* [mlflow](https://github.com/mlflow/mlflow): Track your ML-experiments

## Project Structure
```bash
.
├── config                      
│   ├── main.yaml                   # Main configuration file
│   ├── model                       # Configurations for training model
│   │   ├── model1.yaml             # First variation of parameters to train model
│   │   └── model2.yaml             # Second variation of parameters to train model
│   └── process                     # Configurations for processing data
│       ├── process1.yaml           # First variation of parameters to process data
│       └── process2.yaml           # Second variation of parameters to process data
├── data            
│   ├── final                       # data after training the model
│   ├── processed                   # data after processing
│   ├── raw                         # raw data
│   └── raw.dvc                     # DVC file of data/raw
├── docs                            # documentation for your project
├── experiments                     # ml-experiments. Typically with subfolder mlfow, tensorboard etc.
├── figures                         # Saved figures from your ML-experiments
├── dvc.yaml                        # DVC pipeline
├── .flake8                         # configuration for flake8 - a Python formatter tool
├── .gitignore                      # ignore files that cannot commit to Git
├── Makefile                        # store useful commands to set up the environment
├── models                          # store models
├── notebooks                       # store notebooks
├── .pre-commit-config.yaml         # configurations for pre-commit
├── pyproject.toml                  # dependencies for poetry
├── README.md                       # describe your project
├── src                             # store source code
│   ├── __init__.py                 # make src a Python module 
│   ├── process.py                  # process data before training model
│   └── train_model.py              # train model
└── tests                           # store tests
    ├── __init__.py                 # make tests a Python module 
    ├── test_process.py             # test functions for process.py
    └── test_train_model.py         # test functions for train_model.py
```

## How to use this project

Install Cookiecutter:

```bash
pip install cookiecutter
```

Create a project based on the template:

```bash
cookiecutter https://github.com/tfha/data-science-template
```

Find detailed explanation of this template [here](https://towardsdatascience.com/how-to-structure-a-data-science-project-for-readability-and-transparency-360c6716800).


## Basic commands

### pyenv

Python versions are stored in /home/<username>/.pyenv/version/
This should be added to your PATH. To see whats inside your path, run in your terminal:

```bash
echo @PATH
```


Install a new Python version

```bash
pyenv install 3.10.5
```

Set the global Python version (NOTE: exit an eventual poetry or pipenv session):

```bash
pyenv global 3.10.5
```

Set the local Python version for your project:

```bash
pyenv local 3.10.5
```

Inspect python versions

```bash
pyenv versions
python --version
pyenv which python
```

### poetry

Poetry has similarities with pipenv but has extended possibilities.

All main dependencies must be specified in a file in your repo called **pyproject.toml**. Subdependencies will be saved in poetry.lock. Originally there can just be a few dependencies in the file, but the file will be automatically updated underway.

If you haven't got a pyprojct.toml in your repo you can generate one calling:

```bash
poetry init --name <name of your package>
```

Activating the virtual environment (NOTE: Before activating, set the python version by call 'pyenv global 3.10.5' or 'pyenv local 3.10.5'):

```bash
poetry shell
```

To exit the environment.

```bash
exit
```

Install dependencies. Running this command for the first time, the program will find the best possible combination of library versions described in **pyproject.toml**. A **poetry.lock** will be automatically genereated with detailed version for all main packages and sub-packages. Running *poetry install* when both are present will install exactly the version defined in **poetry.lock**. Thus, the package version in **pyproject.toml** and **poetry.lock** can be different. By calling this command locally for different team-members will ensure that the exact same package versions will be used.

```bash
poetry install
```

To add new packages **pyproject.toml** and **poetry.lock** will be automatically updated. The latest possible version of the package will be used.

```bash
poetry add "pandas>=1.2.0"
```

To add packages only used in development, and highlighted as such in **pyproject.toml** (will not be included in packaging into pypi):

```bash
poetry add loguru --dev
```


To update packages to latest versions (under the restrictions described in **pyproject.toml**, ie update this file if you want new restrictions) run:

```bash
poetry update
```

To only update a package:

```bash
poetry update pandas matplotlib
```

To remove a package:

```bash
poetry remove pandas
```

To list all available packages (eventually naming a package directly to see the details):

```bash
poetry show
```

Check your **pyproject.toml**:

```bash
poetry check
```

Search for a package in a remote repo:

```bash
poetry search pandas
```

Export the **poetry.lock** file to **requirements.txt** (you don't need it, but anyway):

```bash
poetry export -f requirements.txt --output requirement.txt
```

