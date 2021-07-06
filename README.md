# 52°North - Huddle - Python Series - Virtual Environments and Dependency Management

## Table of Contents
* [Meta](#meta)
* [Content](#content)
  * [PIP](#pip)
  * [Virtual Environments](#virtual-environments)
    * [venv](#venv)
  * [conda](#conda)
* [Notes](#notes)
* [Links](#links)

## Meta

* **Authors**:
  * [EHJ-52n](https://github.com/ehj-52n)
  * [MartinPontius](https://github.com/MartinPontius)

* **License**: [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

## Content

Two main concepts/tools exists in the python world to cope with dependency management:

* pip - _**p**ackage **i**nstaller for **p**ython_
* virtual environments - "isolated" collection of dependencies (and other requirements)

### PIP

Pip knows two types of dependencies: normal and optional (aka [extras](https://www.python.org/dev/peps/pep-0508/#extras)). Normal dependencies are listed with their name and the required version. Optional dependencies are listed using square brackets, e.g. `my-dependency[extra-1,extra-2]`. Calling `pip install my-dependency[extra-1,extra-2]` will install `my-dependency` and all required dependencies. In addition, everything listed in the two extras `extra-1` and `extra-2` will be installed, too.

#### Install

Install **packages** from [PyPI](https://pypi.org/) (**Py**thon **P**ackage **I**ndex) with the following command:

```sh
pip install pkg_name
```

**Note**: When working with _different_ python versions _on your system_, then we recommend using `pythonX -m pip install <package>` over `pip install <package>` or `pip3 install <package>`.

Will install latest version of pkg_name and all required dependencies to the current used environment (one of user env, a venv, or system-wide).

#### `requirements.txt`

You can maintain a so called [requirements file](https://pip.pypa.io/en/stable/user_guide/#requirements-files) `requirements.txt` with pip, to document **dependency versions**.

##### Development

For development, the `requirements.txt` should only contain the top level packages. These should be listed without version information if possible. If you need a specific version try to list only a minimal version, e.g. using `pkg==1.*` to get all `1.x` versions of package `pkg`, but no `2.x` or newer. See the [requirement specifier specification](https://pip.pypa.io/en/stable/cli/pip_install/#requirement-specifiers) for all possible allowed values. The following command uses the tool `pipdeptree` to list only the top level packages and removes the version lock of your current environment:

```sh
pipdeptree --python .venv/bin/python \
           --warn silence | \
           grep --color=none -E '^[a-zA-Z0-9\-]+' | \
           cut --delimiter='=' --fields=1 \
           > requirements.txt
```

Note, that the parameter `--python` points to a generic `venv` folder. Ensure that this parameter points to the correct location in your set-up for listing only the packages in your environment.

##### Deployment

For production, deployment, version tags and releases, a fixed and reusable environment is required. Hence, the versions of all dependencies must be documented. The following command creates/replaces a new/the existing `requirements.txt` with all packages installed in the current environment:

```sh
pip freeze > requirements.txt
```

#### Dependency Licenses

To document all dependencies with its version and license, you can use the package `pip-license`. In combination with a docker container, you can limit it to the "really required" dependencies:

```sh
docker run --rm --interactive --tty your-image:latest /bin/bash \
 -c "pip install --no-warn-script-location --no-cache-dir pip-licenses > /dev/null && .local/bin/pip-licenses -f markdown"
```

This will print a markdown formatted list of all dependencies. If you don't have a container ready, use the following command:

```sh
docker run --rm --interactive --tty \
      --mount type=bind,source="$(pwd)"/requirements.txt,target=/requirements.txt \
      python:3-slim-buster \
      /bin/bash -c \
          "pip install \
          --quiet \
          --disable-pip-version-check \
          --no-warn-script-location \
          --no-cache-dir \
          -r requirements.txt \
          pip-licenses \
          > /dev/null && \
          pip-licenses -f markdown"
```

#### Update dependencies

```sh
pip install --upgrade --requirement requirements.txt
```

For fixing a certain dependency version, because you want to keep its current version, check the output of `pip list | grep package` for example, and update your requirements file accordingly.

```ini
datacube
click==7.*
geojson
geopandas
pathlib
pkg-resources
rioxarray
urllib3
```

_Here_: The package click must to be locked to 7.x, because the currently used datacube version (=1.8.3) stops working when being used with click 8.x

#### Analyse dependencies

Using `pipdeptree`, the dependency tree of your project can better analysed and visualized than with `pip freeze`.

* Better `requirements.txt`:

  * List only **top level dependencies**:

    ```sh
    pipdeptree --python .venv/bin/python --freeze --warn silence | grep -E '^[a-zA-Z0-9\-]+'
    ```

  * List **dependencies and their relationships** `pip` friendly:

    ```sh
    pipdeptree --python .venv/bin/python --freeze
    ```

    This file will contain a lot of duplicates (no problem for pip).

* Analyse your dependencies:

  * List **complete tree**:

    ```sh
    pipdeptree --python .venv/bin/python
    ```

    This outputs the complete tree and for each dependency a version range in addition to the installed version.

  * List **reverse dependencies**:

    ```sh
    pipdeptree --python .venv/bin/python --reverse --packages click
    ```

#### pip && docker

When using python on docker images, pip comes in handy, but some recommendations are good to follow.

1. Don't use venv because docker already isolates everything.

1. Ignore "_pip run as root_" warnings.

1. Use the following command line parameter:

   * `--disable-pip-version-check`
   * `--no-cache-dir`
   * `-r requirements.txt`

1. Install system requirements beforehand.

1. Use pip to install the latest or required package versions. E.g. debian `python-` packages might not fulfill your version requirements.

#### pip vs. conda

  >pip installs *python* packages within *any* environment; conda installs *any* package within *conda* environments. If all you are doing is installing *Python packages* within an *isolated environment,* conda and pip+virtualenv are mostly interchangeable, [...]

  [https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/](https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/)

### Conda

Package manager which installs packages (not only Python packages!) into conda environments.
[Conda](https://packaging.python.org/key_projects/#conda) is used most often via one of the following two software distributions: Anaconda, Miniconda.

**Note**: conda environments are not necessarily isolated from the host system as they may use site-packages from there. To make sure not to use host site-packages you can set the following environment variable: `export PYTHONNOUSERSITE=True` (this behavior was experienced with Ubuntu+Miniconda).

* **Create** a new conda environment

  ```sh
  conda create --name conda_env_name
  ```

* **Remove** an existing conda environment

  ```sh
  conda remove --name conda_env_name --all
  ```

* **List** all known conda environments

  ```sh
  conda info -e
  ```

* **Activate** a conda environemnt

  ```sh
  source activate conda_env_name
  ```

  An activated conda environment is indicated in the terminal like this

  ```sh
  (conda_env_name)$
  ```

* **Deactivate** a conda environment

  ```sh
  conda deactivate
  ```

* **Clean** a conda environment

  ```sh
  conda clean --all
  ```

### Virtual Environments

#### venv

Lightweight virtual environment with its own Python binary and (isolated) site directories.

* **Create** a new environment

  ```sh
  python3 -m venv path/to/venv
  ```

* **Remove** an existing environment

  ```sh
  rm -r path/to/venv
  ```

* **Activate** an environment

  ```sh
  source path/to/venv/bin/activate
  ```

  An activated environment is indicated in the terminal like this

  ```sh
  (venv_name)$
  ```

  Packages can then be installed, e.g., via [pip](#pip).

* **Deactivate** an environment

  ```sh
  deactivate
  ```

#### pipenv

[`pipenv`](https://pipenv.pypa.io/en/latest/) tries to become the official dependency and environment management tool for python. It combines `pip` und `venv`.

## Notes

* pip
  * freeze
  * requirements.txt
  * --no-dependencies
  * pkg[bla] notation
* Virtual Environments
  * pipenv
  * venv (pea && ped)
  * conda (miniconda, anaconda)
* Online Repositories
  * pypi
  * anaconda.org - channels
    * anaconda
    * conda-forge
* Conflict Management

## Links

* [pip User Guide](https://pip.pypa.io/en/stable/user_guide/)
* [PyPI](https://pypi.org/)
* [pipenv](https://packaging.python.org/key_projects/#pipenv)
* [PEP 405::Python Virtual Environments](https://www.python.org/dev/peps/pep-0405/)
* [PEP 508::Extras](https://www.python.org/dev/peps/pep-0508/#extras)
* [PyDoc::Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html)
* [Python Packaging User Guide » Tutorials » Installing Packages](https://packaging.python.org/tutorials/installing-packages/)
* [Python Packaging User Guide » Tutorials » Managing Application Dependencies](https://packaging.python.org/tutorials/managing-dependencies/)
* [Requirements File](https://pip.pypa.io/en/stable/user_guide/#requirements-files)
* [Requirements File Format](https://pip.pypa.io/en/stable/cli/pip_install/#requirements-file-format)
* [Requirement Specifier Specification](https://pip.pypa.io/en/stable/cli/pip_install/#requirement-specifiers)
* [pip-license](https://pypi.org/project/pip-licenses/)
* [pipdeptree](https://pypi.org/project/pipdeptree/)
* [Conda: Myths and Misconceptions](https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/)
