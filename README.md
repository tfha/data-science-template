# Data Science template using Cookie Cutter

This repo is forked from khuyentran1401 and further developed with my personal touch. Commands are made to work in Linux but will generally also work in a Windows environment.

**Note**: _This template uses poetry. If you prefer using pip, go to the [pip](https://github.com/khuyentran1401/data-science-template/tree/pip) branch instead._


## What is this?

This repository is a template for a data science project. This is the project structure I frequently use for my data science project.

## Tools used in this project

* [GitHubCLI](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git): Controlling and caching your git credentials
* [pyenv](https://github.com/pyenv/pyenv): Control your Python version - [article](https://www.rootstrap.com/blog/how-to-manage-your-python-projects-with-pipenv-pyenv/) and [article](https://realpython.com/intro-to-pyenv/)
* [poetry](https://towardsdatascience.com/how-to-effortlessly-publish-your-python-package-to-pypi-using-poetry-44b305362f9f): Dependency management and package build/publish - [article](https://towardsdatascience.com/how-to-effortlessly-publish-your-python-package-to-pypi-using-poetry-44b305362f9f)
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

### system setup in linux

Before specifying different application commands you will need some basic information about the system setup in Linux.

The Linux directories have this information. See also here: [Link](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html):

* usr/bin - all the executable binaries (programs)
* usr/share - all read-only architecture independend files
* bin - containing binaries mainly used for emergency repairs
* etc - configuration files
* ...

A shell/terminal (interactive user interface with an operatin system) will be invoked in 3 ways:

* A **not interactive shell**. These are not controlled by humans and are run on installations, startups etc.
* An **interactive shell**. The standard shell invoked and run by humans
* In addition we have the distinction between **login shell** and **non login shell**. Logging into a system using SSH from your terminal is a **login shell**, whereas the normal way of just starting a shell is a **non login shell**.

The standard system setup for your session is described in your Linux system here:

* **login shell**: `etc/profile`
* **non login shell**: `etc/bash.bashrc` if you are using `bash`as your shell

These setup-files are typically extended (with inherited information) and overridden with user specific information described in `.profile`and `.bashrc` (if you are using `bash` and not `fish`or `zsh`).

The `.bashrc` config file determines the behaviour of your terminal/system in Linux and is invoked every time you start a new shell (terminal) session both with a **login shell** and a **non login shell**.

For a **login shell** one or more files will be called before `.bashrc`. They will be called in this order (ie. similar variables will be overridden):

* `etc/profile`
* `etc/profile` executes scripts in `etc/profile.d`
* `~/.bash_profile` or `~/.bash_login`or `~/.profile` in this order, if they are provided
* `~/.bash_profile`executes `~/.bashrc`
* `~/.bashrc` executes `etc/bash.bashrc`

For a **non login shell** the files will be called in this order:

* `~/.bashrc`
* `~/.bashrc` executes `etc/bash.bashrc`
* `etc/bash.bashrc` executes the scripts in `etc/profile.d`


### pyenv

Python versions are stored in `/home/username/.pyenv/shims`. When running a command like python or pip your operating system search through a list of directories to find an executable file with that name. This list of directories are stored in an environment variable called `PATH`. `PATH`is searched from left to right. When an executable is found this will be run.

A python program executable should be added to your PATH. To see whats inside your path, run in your terminal:

```bash
echo @PATH
```

If this directory is not in your path, you have to add the path in your `.bashrc`file.

1. Open the `.bashrc` file in your home directory (normally,
   `/home/your-name/.bashrc`) in a text editor. Often this will work `nano ~/.bashrc`.
2. Add in general `export PATH="your-dir:$PATH"` to the last line of the file, where
   *your-dir* is the directory you want to add. Here this is `/home/username/.pyenv/shims`
3. Save the `.bashrc` file.
4. Restart your terminal.

**Note**: _the order of listed dirs in path is important._ If there are different
Python versions in your system the pyenv-path must be listed first in order to work. Arrange the order by:


Install a new Python version

```bash
pyenv install 3.10.5
```

Set the global Python version (**NOTE**: _exit an eventual poetry or pipenv session_):

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

