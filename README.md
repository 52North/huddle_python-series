# 52°North - Huddle - Python Series - Virtual Environments and Dependency Management

## Meta

* **Authors**:
  * [EHJ-52n](https://github.com/ehj-52n)

* **License**: [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

## Content

Two main concepts/tools exists in the python world to cope with dependency management:

* pip - _**p**ackage **i**nstaller for **p**ython_
* virtual environments - "isolated" collection of dependencies (and other requirements)

### PIP

Pip knows two types of dependencies: normal and optional (aka [extras](https://www.python.org/dev/peps/pep-0508/#extras)). Normal dependencies are listed with their name and the required version. Optional dependencies are listed using square brackets, e.g. `my-dependency[extra-1,extra-2]`. Calling `pip install my-dependency[extra-1,extra-2]` will install `my-dependency` and all required dependencies. In addition, everything listed in the two extras `extra-1` and `extra-2` will be installed, too.

* **Install packages** from [PyPI](https://pypi.org/) (**Py**thon **P**ackage **I**ndex):

  ```sh
  pip install pkg_name
  ```

  Will install latest version of pkg_name and all required dependencies to the current used environment (one of user env, a venv, or system-wide)

* Document **dependency versions**

  With pip, you can maintain a so called [requirements file](https://pip.pypa.io/en/stable/user_guide/#requirements-files): `requirements.txt`. The following command creates/replaces a new/the existing `requirements.txt` with all packages installed in the current environment:

  ```sh
  pip freeze > requirements.txt
  ```

  The file contains the list of packages and its currently installed version.

* Document **dependency licenses**

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

* **Update dependencies**

  ```sh
  pip freeze | cut --delimeter='=' --fields=1 | xargs --max-args=1 pip install --upgrade
  ```

  Exclude certain dependencies because you want to keep its current version:

  ```sh
  pip freeze | cut --delimeter='=' --fields=1 | grep --invert-match click | xargs --max-args=1 pip install --upgrade
  ```

* **Analyse dependencies**

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

* **pip && docker**

  When using python on docker images, pip comes in handy, but some recommendations are good to follow.

  1. Don't use venv because docker already isolates everything.

  1. Ignore "_pip run as root_" warnings.

  1. Use the following command line parameter:

     * `--disable-pip-version-check`
     * `--no-cache-dir`
     * `-r requirements.txt`

  1. Install system requirements beforehand.

  1. Use pip to install the latest or required package versions. E.g. debian `python-` packages might not fulfill your version requirements.

## Virtual Environments

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
* [PEP 508::Extras](https://www.python.org/dev/peps/pep-0508/#extras)
* [PyDoc::Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html)
* [Python Packaging User Guide » Tutorials » Installing Packages](https://packaging.python.org/tutorials/installing-packages/)
* [Python Packaging User Guide » Tutorials » Managing Application Dependencies](https://packaging.python.org/tutorials/managing-dependencies/)
* [pip-license](https://pypi.org/project/pip-licenses/)
* [pipdeptree](https://pypi.org/project/pipdeptree/)