# Using a Package

---
## Installing a Package with pip

By default, pip will look for packages to install on the [PyPI.org](https://pypi.org) repository. The command to use is simple:
``` bash
pip install mypackage
```

When a package is deployed on a repository other than PyPI.org, you need to specify the URL of this repository to pip using the `index-url` option either in the pip command, or in a configuration file.

If the package relies on dependencies, there is a high risk that they are only available on pypi.org, and not on the specific project repository. In this case, you need to tell pip to also look for packages on pypi.org, using the `extra-index-url` option.

Configuration for a package in test.pypi.org via command line
``` bash
pip install \
  --index-url https://test.pypi.org/simple/ \
  --extra-index-url https://pypi.org/simple/ \
  mypackage
```

To avoid having to specify the index options each time you use pip, they can be defined globally in the pip configuration file `~/.pip/pip.conf`:
``` bash
[global]
index-url = https://test.pypi.org/simple/
extra-index-url = https://pypi.org/simple/
```