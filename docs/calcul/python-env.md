# Gestion des environnements Python

***[Page en cours de rédaction]***


---
## Gestion des paquets et bibliothèques

La plupart des scripts et applications écrites en Python s'appuient sur des librairies et des paquets additionnels.

Ces paquets et bibliothèques sont publiés dans des dépôts, par exemple  `PyPI`, `Anaconda`, `Conda-forge`, ...

Certains paquets sont soumis à licence, certains dépôts sont payants.

Pour les installer dans un environnement python, on utilise un gestionnaires de paquets, par exemple `pip`, `conda`, `mamba`, ... 

Les gestionnaires permettent d'installer les paquets manuellement, mais aussi à partir d'un fichier de dépendances, généralement `requirements.txt`. Ce fichier contient la liste des paquets à installer et leur version, et est généralement intégré au suivi de version des applications, au même titre que le code source.
 
Enfin, des distributions complètes, intégrant Python, ainsi qu'un ensemble de paquets et bibliothèques, adaptées à des cas d'usage précis, sont également disponibles. Elles s'appuient sur les dépôts, et les gestionnaires de paquets.

Elles permettent d'installer rapidement un environnement clé en main. Par exemple :

- `Anaconda`:  pour la science des donnée (NumPy, pandas, Matplotlib, etc ...),
- `PyPy` :  pour des performances élevées,
- `MicroPython` : pour les systèmes embarqués,
- `MiniConda` : version minimaliste de **Anaconda**,
- `Miniforge` : similaire à Miniconda mais mettant l'accent sur les canaux open source, comme `conda-forge`
- ...

---
## Environnement d'Execution

Il est souvent nécessaire de pouvoir utiliser plusieurs environnements Python avec des caractéristiques différentes sur la même machine.

C'est le cas par exemple pour les systèmes Linux, pour lequel Python est installé par défaut, et est utilisé dans un grand nombre de scripts système. La gestion des librairies et packages complémentaires est donc restreinte aux administrateurs des système.

Ou plus simplement dans tous les environnements, si on travaille sur plusieurs projets, 

Des mécanismes d'environnements virtuels permettent de créer un environnement Python qui correspond aux contraintes d'un script ou d'une application, sans droits d'administration. Par exemple le module python `venv`, ou le gestionnaire de paquet `conda` qui peut gérer à la fois des environnements et des paquets.

---
## Utilisation d'Anaconda et Miniconda

Depuis mars 2024, l'entreprise Anaconda a modifié sa licence, et l'utilisation de certains paquets du canal "defaults" d'Anaconda est désormais soumise à licence pour à la plupart des établissements académiques, et de recherche.

Les établissements concernés (CNRS, Ifremer...) ont reçu des mises en demeure pour le paiement des coûts de licences liés à leur utilisation, et la plupart  recommandent de ne plus utiliser Anaconda. Certains l'interdisent.

Cette modification concerne les dépôt "Anaconda Repository", et donc les distributions `Anaconda` et `Miniconda`. Ces distributions utilisent `conda` comme gestionnaire de paquet.

Il est possible de configurer `conda` pour ne pas utiliser les canaux soumis à licence. Mais ce contournement n'est pour l'instant pas encore totalement efficace quand conda est utilisé avec les distributions.

Par exemple, dans un fichier `.condarc` ou dans un fichier de dépendance :
```
channels:
 - nodefaults
 - conda-forge
channel_priority: strict
```

Une alternative consiste à utiliser une autre distribution, comme `Miniforge`, qui privilégie l'utilisation de canaux Open Source et qui par défaut n'utilise pas les canaux du "Anaconda Repository".

---
## Cas d'usage

Si possible, ne plus utiliser les distributions `Anaconda` et `Miniconda`.

Ne plus utiliser le canal "default" du "Anaconda Repository".

Plutôt que conda, utiliser mamba plutôt que conda, car plus performant, voir même si suffisant micromamba.

Dans la mesure du possible, utiliser `Miniforge`.

Pour des besoins simples sans contraintes sur la version de Python, ou si `Miniforge` n'ets pas disponible,  il est toujours possible d'utiliser le module `venv` de python pour gérer les environnements, et le gestionnaire de paquets de `pip` qui s'appuie sur le dépôt `PyPI`

Dans tous les cas, utiliser un fichier de dépendances, et préciser si possible les version à utiliser. 

---
## Utilisation de conda

---
## Utilisation de mamba

---
## Utilisation du module python venv

Plusieurs approches :

- Regrouper les environnements Virtuels dans un seul dossier, `venvs` par exemple
- Placer l'environnement virtuel avec le code qui l'utilise. On utilise dans ce cas souvent un dossier `.venv` (penser à exclure ce dossier des dossier suivi de votre dépôt si vous utilisez git)


Exemple de création d'un environnements virtuel `my-jupyter`, dans un dossier communs.

- Aller dans un dossier vous appartenant, et dans lequel vous disposez d'espace disque suffisant. par exemple le dossier `/Odyssey/private/VOTRE_LOGIN`,
- Créer un dossier `venvs` pour stocker vos environnements Virtuels
``` bash
mkdir venvs
```
- Créer l'environnements Virtuel my-jupyter avec la commande (l'option --prompt permet de modifier le prompt de la ligne de commande, quand l'environnement est activé)
``` bash
python3 -m venv --prompt my-jupyter  ./venvs/my-jupyter
```

Ensuite, pour utiliser l'environnement, il suffit de l'activer
``` bash
source venvs/my-jupyter/bin/activate
```

Une fois activé, une "bulle" python est créée, indépendante de ce qui est installé sur la machine hôte. Il est alors possible d'installer les paquets et librairies donc vous avez besoins.

Pour vérifier quel binaire python est utilisé :
``` bash
which python
```

---
## Utilisation du gestionnaire de paquets pip

---
## Ressources 

- [https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)