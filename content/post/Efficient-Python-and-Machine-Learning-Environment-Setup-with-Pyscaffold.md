---
title: "Efficient Python and Machine Learning Environment Setup with Pyscaffold, Poetry, and Pyenv"
date: 2023-07-14T20:58:49+05:30
lastmod: 2023-07-14T00:07:00+00:00
draft: false
tags: ["poetry", "pyenv", "pyscaffold", "virtualenv", "set-up"]
categories: ["Machine Learning", "Python"]
---

[# Efficient Python and Machine Learning Environment Setup with Pyscaffold, Poetry, and Pyenv]::

Setting up development environment for python packages and machine learning projects

<p align="center">
<img src="/img/blog/OscarYildiz_essential.jpeg" width="600" height="350" alt="jpeg"/>
</p>


Setting up a python environment for machine learning can be tedious especially because we want to go right into getting our hands dirty with the data and ML algorithms, whether it's just a project directory or you want to package your machine learning code. Python packaging has been made easier by a set of automation tools, in this article, I'll be exploring what I've found to be a rewarding approach of how best to structure a development environment using these readily available tools. This set me up for lessons that led to success.


## Tools
- [Pyscaffold](https://pyscaffold.org/en/stable/) - project files generator
- [Poetry](https://python-poetry.org/docs/) - dependency management
- [Pyenv](https://github.com/pyenv/pyenv) - python version management

To install and get started with these tools, see their linked pages above.


## Project directory structure

[Pyscaffold](https://pyscaffold.org/en/stable/) helps us generate project files with just one command. Assuming we want to name our project/ package “mlproject”:

    putup mlproject

This generates the following directory structure, with all the files needed for any project or repository. You don’t have to manually create general files like README.md, .gitignore and the others:

<p align="center">
<img src="/img/blog/essential_dir_structure.png" width="600" height="350" alt="jpeg"/>
</p>


Python scripts go to `src/mlproject` directory and one can import functions from the package by:

    from mlproject.script import func

A typical machine learning project would have `data` and `notebooks` folders. Create these in the project’s root directory.


## Dependency management, versioning and publishing.

Among available open source tools, this section will focus on poetry given the simplicity and its ease of use.
[Poetry](https://python-poetry.org/docs/) will automatically create a virtual environment and help in managing dependencies for your project. To set this up we initialize poetry with:

    poetry init

This generates two files:

- pyproject.toml - this keeps record of dependencies
- poetry.lock - keeps version information of the dependencies

Installing/ adding subsequent dependencies will be done by:

    poetry add <numpy>

Package development is a gradual process, you want to keep log of major and minor developments for your package. Poetry helps you achieve this effortlessly. On top of semantic versioning approach, poetry allows for `patch`, `minor`, `major`, `prepatch`, `preminor`, `premajor` and `prerelease` version bumps as seen on the [docs](https://python-poetry.org/docs/cli/#version).
To bump the version after adding a patch for example:

    poetry version patch

And this bumps the version number automatically.
Poetry also makes publishing packages to [pypi](https://pypi.org/) very easy.
For more usage instructions and best practices of effectively using poetry refer to its documentation.

## Specifying Python version

Apart from specifying package versions you want to specify a python version for your project. This can be achieved by using [pyenv](https://github.com/pyenv/pyenv) to install the specific version of python. Specify the version inside your project using poetry by simply doing:

    poetry env use <path/to/python/version>
    # eg. poetry env use /home/vincent/.pyenv/versions/3.8.10/bin/python

This makes sure that when you activate poetry shell it uses this specified python version.
NOTE: Doing it otherwise eg. by `pyenv local 3.8.10` will not have poetry point to that python version.


## Best practices?

We see a tests directory among the auto-generated files, [pytest](https://docs.pytest.org/en/7.1.x/contents.html) is a great tool for writing tests for your package and it integrates well with the above listed tools.

Another cool feature for poetry is how well it helps one separate development packages with actual dependencies used by your package, so that when you publish your package, other users don’t install unneeded packages used in development for example jupyterlab which you only use when doing development inside the notebooks, your package never calls package jupyterlab anywhere.
We separate these by specifying an argument `--dev` :

    poetry add --dev jupyterlab

For more on this see the section in poetry documentation [here](https://python-poetry.org/docs/master/managing-dependencies/).

Lastly, tools such as [black](https://black.readthedocs.io/en/stable/) and [pylint](https://pylint.pycqa.org/en/latest/) are great to explore to write well structured code that follow good software engineering principles.

