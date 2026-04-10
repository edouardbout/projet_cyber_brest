# Packaging with pyproject.toml, wheel, build, and twine

---
## Configuring the Package with the `pyproject.toml` File

This file will contain the information describing the package.

It is a modern alternative to `setup.py`. Be careful, using these two files simultaneously is possible but can cause conflicts.

The minimum information includes its name, version, and the tools to use for building it.

But it is possible to specify many options, such as dependencies, license, keywords associated with the package, etc.

Example file:
``` ini
[tool.setuptools]
packages = ["mypackage"]

[project]
name = "mypackage"
version = "1.0.0"
authors = [{name = "My Name", email = "my-name@email.com"}]
description = "My demo package"
readme = "README.md"
requires-python = ">=3.7"
dependencies = ["numpy"]
```

---
## Creating the Package with `Build`

The `python -m build` command is used to build a Python package.

It generates source distribution (.tar.gz) and wheel (.whl) formats.

- Installing `build`
``` bash
pip install build
```
- Building the package
``` bash
python -m build
```

The build will:

- Check for the presence of configuration files (pyproject.toml, setup.py, setup.cfg).
- Create an isolated environment to avoid interference with the global environment.
- Build the distributions in the `dist/` directory:
    - Wheel (.whl): Binary format optimized for quick installation with pip.
    - Source (.tar.gz): Contains the raw source code.
- Build the package metadata in the `src/PACKAGE_NAME.egg-info/` directory.

``` bash
dist/
│── mypackage-1.0.0-py3-none-any.whl  # Wheel package
│── mypackage-1.0.0.tar.gz            # Source archive
```

Even though the .egg format is obsolete, the `mon_module.egg-info/` folder is still used by the "backend builder," setuptools, to store package metadata.

``` bash
src/mypackage.egg-info/
├── dependency_links.txt  # Links to dependencies
├── entry_points.txt      # Package entry points
├── PKG-INFO              # Metadata (name, version, description, etc.)
├── requires.txt          # List of dependencies
└── top_level.txt         # Name of the main module
```

---
## Publishing Packages with Twine

To publish a package, it must first be generated with `python -m build`: creating the `dist/` folder containing the package files (.whl and .tar.gz).

You also need to generate a token for access to the publication platform.

- Installing `Twine`
``` bash
pip install twine
```
- Verifying the package
``` bash
twine check dist/*
```

- Example of [Publishing on PyPI](./pypi.md)

---
## Sources

- [https://packaging.python.org/en/latest/tutorials/packaging-projects/](https://packaging.python.org/en/latest/tutorials/packaging-projects/)
- [https://packaging.python.org/en/latest/glossary/](https://packaging.python.org/en/latest/glossary/)