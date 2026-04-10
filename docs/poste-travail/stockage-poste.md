# Stockage des données

Il est possible de stocker ses fichiers de travail sur le disque dur de votre poste mais c’est à vos risques et périls, car en cas de panne de votre disque dur, vous perdriez ces fichiers. Il est donc conseillé de les mettre sur votre «Homedir», qui est un espace de stockage en réseau, accessible de toutes les machines connectées au réseau interne de l’école. Ce homedir est sécurisé ET sauvegardé : il existe plusieurs copie réparties sur différents serveurs physique géographiquement éloignés et de plus les états précédents de vos données sont sauvegardés. Vous pouvez retrouver votre Homedir d’il y a trois mois maximum. Le quota sur ce Homedir est limité à 12Go. Il existe un autre espace de stockage, sécurisé et sauvegardé : "extra". Il faut demander à jn.bazin@imt-atlantique.fr pour obtenir du quota sur cet espace.


| chemin                                                               | Type                                            | Données Répliqué | Sauvegardé |
|----------------------------------------------------------------------|-------------------------------------------------|------------------|------------|
| \\campus.enst-bretagne.fr\homes\votre_login                          | Personnel                                       | oui              | oui        |
| \\extra.imta.fr\votre_login                                          | Personnel                                       | oui              | oui        |
| \\shares.svc.enst-bretagne.fr\nom                                    | Partage campus                                  | oui              | oui        |
| [https://partage.imt.fr](https://partage.imt.fr)                     | NextCloud, commun à l’IMT                       | oui              | oui        |
| [https://cloud.imt-atlantique.fr](https://cloud.imt-atlantique.fr)   | NextCloud pour IMT Atlantique                   | oui              | oui        |
| [https://gitlab.imt-atlantique.fr](https://gitlab.imt-atlantique.fr) | Gestion de projet de développement basé sur git | oui              | oui        |
| /Odyssey, /Brain, /Codes                                             | Voir [Stockage serveur](../calcul/storage.md)   | RAID6            | non        |

## Répertoire Campus/Homedir

Ce répertoire stocké sur serveur et accessible en réseau, est le répertoire utilisateur par défaut sur les machines administrées par la DISI (Salles de TP libre service, serveurs, Windows campus). Il est aussi possible de l'utiliser depuis des machine auto administrées SALSA.

Ce répertoire est stocké sur les serveurs de la DISI. Les données sont répliquées ET sauvegardées :

  - Les données sont préservées en cas de pannes matérielles même majeures
  - En cas d'erreur de l'utilisateur, ce dernier peut retrouver ses données dans un état antérieur, jusqu'à 3 mois en arrière.

Pour ces raisons, c'est le répertoire à privilégier pour stocker vos travaux en cours.

Pour accéder à ce répertoire depuis une machine :

  - auto administrée Ubuntu 24.04, reportez à cette pages [Configuration Ubuntu 24.04](../poste-travail/ubuntu24.md)
  - auto administrée Windows 10, reportez à cette pages [Configuration Windows 10](../poste-travail/windows10.md)
  - Windows 10 **Campus**, c'est le répertoire **Mes Documents**
  - Ubuntu **Campux**, c'est le répertoire `/homes/$USER` (`$USER` étant votre login IMT Atlantique)

Le quota disponible est fonction du statut de l'utilisateur :

  - **Personnel de l'école (CDD, CDI et doctorants)** : 20Go
  - **Étudiants** : 7Go

## Espace Extra

**Extra** est aussi un espace de stockage en réseau. Bien que répliqué, les données sont sauvegardée mais moins que le homedir. Ce qui veut dire qu'elles sont préservées en cas de panne matérielles, mais pas en cas d'erreur de l'utilisateur, si par exemple il supprime des fichier par erreur.

La DISI alloue du quota que les correspondants informatique peuvent attribuer aux utilisateurs. Il existe des répertoires partagés, et des répertoires propres à chaque utilisateur.

## Nextcloud sur partage.imt.fr

Nextcloud est un logiciel libre permettant de mettre en place un espace de cloud similaire à google drive. Nextcloud permet la synchronisation des données avec un répertoire local, ce qui permet de travailler hors ligne. Une suite bureautique en ligne est aussi disponible, ce qui permet l'édition collaborative.

Une instance de Nextcloud a été déployé à Mines Nord Europe pour tout l'IMT. Il s'agit de [https://partage.imt.fr](https://partage.imt.fr). Cet espace est accessible au personnel et aux étudiants, avec un quota de 10Go.

Installation du client sur Windows : Télécharger l'application : [https://nextcloud.com/fr/install/](https://nextcloud.com/fr/install/) et installer.

Installation du client sur Ubuntu : `sudo apt install nextcloud-desktop`

Il est possible de demander la création d'espace de projet avec un quota en propre, permettant le partage de fichiers sans impacter le quota des utilisateurs impliqués. Pour cela, il faut adresser une demande à [support-nextcloud@imt-nord-europe.fr](mailto:support-nextcloud@imt-nord-europe.fr) en fournissant un nom et la quantité de quota demandée.


## Nextcloud sur sdrive.cnrs.fr

De la même manière, un espace nextcloud de 100Go est mis à disposition des personnels des UMR : [https://sdrive.cnrs.fr/](https://sdrive.cnrs.fr/)

Il faut un compte *Janus* : [https://janus.cnrs.fr](https://janus.cnrs.fr)

## gitlab.imt-atlantique.fr

Gitlab est un outil libre de forge logicielle. Une instance est installée à IMT Atlantique : [https://gitlab.imt-atlantique.fr](https://gitlab.imt-atlantique.fr). Cet outil exploite [https://git-scm.com/](https://git-scm.com/), le gestionnaire de version le plus utilisé.

Il est vivement conseillé d'utiliser [https://gitlab.imt-atlantique.fr](https://gitlab.imt-atlantique.fr) pour versionner ses projets de développement.
