# Publishing a Package on PyPI

---
## PyPI Platforms

PyPI offers 2 platforms: the official platform and a test platform.

To publish a package on PyPI, you must first create an account on the platform.

The 2 platforms are independent, and account management is not shared. Therefore, you need to create an account on each of these platforms.

- Official Platform:
    - Access: [https://pypi.org/](https://pypi.org/)
    - Account creation: [https://pypi.org/account/register/](https://pypi.org/account/register/)
    - Account management: [https://pypi.org/manage/account/](https://pypi.org/manage/account/)
- Test Platform:
    - Access: [https://test.pypi.org/](https://test.pypi.org/)
    - Account creation: [https://test.pypi.org/account/register/](https://test.pypi.org/account/register/)
    - Account management: [https://test.pypi.org/manage/account/](https://test.pypi.org/manage/account/)

An email is sent to validate the account activation.

When you are using the test platform, you need to be carefull with your package dependencies, because most of the package are not available in the test platform.

!!! Warning

    If you use the test platform, you need to be careful with your package dependencies. Many packages are in older versions or are simply not available on this platform.


---
## 2FA Authentification configuration

La plupart des opérations, comme par exemple la publication d'un package, nécessitent d'avoir activé l'authentification à deux facteurs (2FA).

Pour pouvoir activer le 2FA, il faut disposer d'une application proposant cette fonctionnalité, sur sa machine ou son smartphone, voir [](../../reseau-securite/2fa.md)

- Pour activer 2FA : [https://test.pypi.org/manage/account/two-factor/](https://test.pypi.org/manage/account/two-factor/)
- PyPi supporte 2 modes de gestion du 2FA
  - Avec une application d'authentification
  - Avec un périphérique de sécurité, de type YubiKey

Pypi va générer une liste de code de récupération, qu'il faut sauvegarder. Un des codes de cette liste doit être utilisé pour valider qu'on a bien sauvegardé la liste

Si on utilise une application d'authentification, il faut ensuite cliquer sur "Ajouter 2FA avec une application d'authentification" :

![](./img/pypi-2fa.png#center#shadow)


Pypi proposer un QR code, et une clé d'authentification de l'application, qui seront ensuite utilisée avec l'outil de gestion 2FA :

![](./img/pypi-2fa-code.png#center#shadow)


---
## Publishing Token creation

Publishing a package is done with the user `_token_`, which is a standard.

You then need to generate an API key (Token) to manage authentication. The scope of the Token can be global (all projects associated with the account) or limited to a project.

But to limit the scope, the project must exist. And to create a project, you need a Token...

So, you need to create a token with a global scope and use it to create the project. Then create a token with a scope limited to the project.

- Log in to PyPI and display your account settings. For example, for PyPI test: [https://test.pypi.org/manage/account/](https://test.pypi.org/manage/account/)
- Look for the "API Tokens" section on the page and click "Add API token"
    - Create a Token with the scope "Scope to Entire account"
    - Token name: "global-token"
    - Scope: Entire account (all projects)

!!! Warning

    For security reasons, this token will only appear once. Copy it now.

**You must copy and store this token carefully**

---
## Projet creation (with twine)

Using the `twine` utility and a "global token".

- Package verification (ignore warnings)
``` bash
twine check dist/*
```
- Package creation = upload of the first version
``` bash
export PYPI_TOKEN="pypi-AgENdGVz ....."

twine upload \
  --verbose \
  --repository-url https://test.pypi.org/legacy/ \
  -u __token__ \
  -p "$PYPI_TOKEN" \
  dist/*

# Uploading distributions to https://test.pypi.org/legacy/
# ...
# Uploading  PACKAGE_NAME-VERSION-py3-none-any.whl
#  ...
# INFO     Response from https://test.pypi.org/legacy/:
#          200
# ...
# Uploading PACKAGE_NAME-VERSION.tar.gz
# 
# INFO     Response from https://test.pypi.org/legacy/:
#          200 OK                                                                                                                                  
# ...
# View at:
# https://test.pypi.org/project/PACKAGE_NAME/VERSION/
```

At this stage, the project has been created on https://test.pypi.org/, and it is accessible:

- In your user space: "Your projects"
- Via the URL displayed at the end of the `twine upload` command

---
## Post Project Configuration

### Project Token Management

- Create a Token dedicated to this project
    - Follow the procedure used to create the "global-token"
    - Limit the scope to the project


### Configuration of the `.pypirc` file

Create or update the PyPI repository management file at the root of the project:

``` ini
[distutils]
  index-servers =
    testpypi

[testpypi]
  repository = https://test.pypi.org/legacy/
  username = __token__
```  

Testing the validity of the token and configuration
- With manual token entry
``` bash
twine upload --repository testpypi  dist/*
# Enter your API token:
```
- Or using an environment variable
``` bash
export PYPI_TOKEN="pypi-AgENdGVzdC5weXBpLm9yZwIkMjY2OWE1MTktOTU2Mi00NzFhLWJiOTEtNDY2NDM2N2UxZGI5AAIcWzEsWyJlYnJhdXgtbXktY2FsY3VsYXRvciJdXQACLFsyLFsiYWRmOTg4NGMtM2QxMi00MjMxLTgxZTAtYTM4ZWNkYThhMDVlIl1dAAAGIP7sjcwL3Mcle-_N9Cr2yj-rFurAUOqH0ITYbbZT7nlO"

twine upload \
  --repository testpypi \
  -p "$PYPI_TOKEN" \
  dist/*
```




