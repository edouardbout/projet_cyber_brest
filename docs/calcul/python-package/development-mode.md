# Installer un package en mode développement 

L’installation d’un package en mode développement permet :

- D'utiliser un package, sans avoir besoin de le publier.
- De modifier le code source sans avoir à le réinstaller après chaque modification.


Attention :
- En cas de modification du code source du package, les changements sont pris en compte immédiatement avoir besoin de réinstaller le package
- En cas de modification du fichier de configuration `pyproject.toml` (ou `setup.py`), il fat forcer la réinstallation du package pour que la modification soit prise en compte

Pour qu'un package puisse être installé en mode développement, il faut s'assurer que setuptools est bien défini comme backend dans le  pyproject.toml 
``` bash
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

Pour installer un package en mode développement :

- Se placer à la racine du projet contenant le dossier `dist`
- Installer le package en mode développement
``` bash
pip install -e .
```

Contrairement à l'installation standard d'un package, dans le cas présent, seul le dossier de méta-données du package `PACKAGE_NAME-VERSION.dist-info/` et créé dans le dossier `venv/lib/python3.10/site-packages`. Le code source du package est intégré depuis le dossier contenant le code dans le projet.


Pour vérifier que le package ets bien déployé :
``` bash
pip show mypackage
```
Vous devriez voir une ligne indiquant "editable" dans le chemin du package. 

Autre test : 
``` bash
pip list | grep mypackage
```
- Pour afficher des informations sur le package, ouvrir un shell python :
``` bash
python3
```
- Afficher les fichiers du package :
``` python
print(mypackage.__file__)
# ... /mypackage/mypackage/__init__.py
```
- Afficher les modules proposés par le package
``` python
dir(my_package)
# ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', 'myfunction']
```

Pour désinstaller le package
``` bash
pip uninstall mypackage
rm -rf mypackage.egg-info
```

Pour forcer la réinstallation du package, il faut le désinstaller, puis le ré-installer.

