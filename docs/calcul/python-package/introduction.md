# Python Package

---
## Package Structure

To create a Python package, you need to add some information to the code:

- A "functional" description of the module (documentation for users): `README.md` file
- Dependencies: `requirements.txt` file
- Terms of use (license), optional but recommended: `LICENSE` file

Then you need to create a "technical description of the module" and its construction.

Finally, create the package, which involves generating an archive, for example in a specific format. There are several formats (see below).

During the development phases of the package code, it is deployed locally directly from the source code, in [development mode](./development-mode.md).

To distribute it, you need to make it accessible by publishing it on repositories like PyPI (Python Package Index), Conda-forge, GitHub Packages, etc.

There are several formats for the "technical description of the module," especially to manage different package formats or tools.

- For a package in PyPI format, the historical format is a Python file `setup.py`. It is now recommended to use a Toml file: `pyproject.toml` for new projects, unless technical or functional constraints require `setup.py`.
- For packages in Conda format via Anaconda Repository: yaml format file: `meta.yaml`

The main package formats are:

- "source code": requires package compilation: long, increases the risk of errors
    - sdist: source code
- "pre-compiled": for quick installations
    - bdist_egg [obsolete]: pre-compiled
    - bdist_wheel: bdist packaging, in wheel format (.whl)
    - wheel: wheel packaging, in wheel format (.whl)

The most widespread package format currently is wheel: pre-compiled packages, binary format for quick installations. File ".whl"

There are best practices for structuring a package:

- Manage the code in version control, and use the same name for the repository as for the package
- Create unit tests for the code, and place them in a `tests` folder
- Place the code in a `src` subfolder, containing a folder with the package name. In reality, the `src` subfolder is rarely used.

For example, for a package `mycalculator`, containing a module `calculator`:

``` bash
mycalculator/
├── mycalculator/
│   ├── __init__.py
│   ├── calculator.py
├── tests/
│   ├── calculator.py
├── pyproject.toml
├── requirements.txt
├── README.md
```

---
## Choosing the Package Name

The name of a Python package must follow the conventions of PEP 8 and PEP 426 to be compatible with tools and repositories like PyPI (Python Package Index).

Allowed characters are:

- Lowercase letters (a-z)
- Numbers (0-9)
- Underscores (_): only for internal names or modules, but not recommended for the distributed package name.

- Spaces and special characters (like @, #, or !) are not allowed.
- No uppercase characters

It is important to choose a short and descriptive name that clearly reflects the role or functionality of the package.

However, there is an exception for package names distributed on PyPI:

- File names must follow the rule of not having `-`
- But for the package name on PyPI, defined in the package description, it is common to replace `_` with hyphens `-`.

As a result, for example, to import the package `my-package`, you need to use the command `import my_package`. This can then cause confusion.


---
## Packaging Tools

### Historical Packaging: setuptools and setup.py

`setuptools` is the historical tool for creating and publishing Python packages.

It relies on a `setup.py` configuration file.

It allows generating the package and publishing it.

Some limitations:
    - The default file format is sdist, and the wheel format is not fully supported
    - During publication, credentials are sent in clear text over the network.
    - The `setup.py` file can be complex and difficult to maintain.

### Modern Packaging: pyproject.toml, wheel, build, and twine

It relies on a `pyproject.toml` configuration file, which is simpler and more readable (but the `setup.py` file can still be used).

Package generation is managed by a "Build Frontend," a command-line tool that handles package generation, relying on a "build backend" that takes care of package generation and option implementation.

- The most widespread "Build Frontend" is the `build` module, which relies on `wheel`.
- The most used and documented "build backend" with the `build` frontend is `setuptools`.

Since setuptools is the historical tool, it provides essential features for package management: dependency management, advanced features (e.g., setuptools_scm for version management), etc.

The `twine` module is widely used for package publication.

For more informations see [ Packaging with pyproject.toml, wheel, build, and twine](./build-twine.md)

### Alternatives to these tools

- Hatch, and the integrated Hatchling build backend [https://github.com/pypa/hatch](https://github.com/pypa/hatch)
- Poetry, which allows building and publishing the package
- ...

