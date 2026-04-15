# Configuration Windows 10 dans Salsa


La plupart des postes au département ELEC sont dans le réseau SALSA. Sur ce réseau, les machines sont administrées pas l’utilisateur lui-même et non par la DISI comme c’est le cas des postes dans le domanaine AD (ex CAMPUS). C’est donc à vous de prendre en charge la configuration et la maintenance de votre poste.

Les poste Windows 10 dans Salsa sont néanmoins installés en utilisant une procédure fournie par la Disi, et se fait directement par le réseau. Une fois installé, il reste quelques opérations à faire avant d’avoir un poste pleinement fonctionnel.

## Créer un compte administrateur personnel

Quand une machine est générée dans SALSA en windows 10 avec les outils de la DISI, un compte windows administrateur local par défaut est créé (login : administrateur, passwword : telecom). L’utilisateur est fortement encouragé à créer un compte administrateur personnel, et supprimer ce compte par défaut.

Pour cela :

1. Allez sur le `Menu Démarrer` :arrow_right: `Panneau de configuration` :arrow_right: `Comptes d’utilisateurs` :arrow_right: `Gérer un autre compte` :arrow_right: `Créer un nouveau compte`
2. Dans cette fenêtre il faut renseigner le nom du nouveau compte. Il est fortement conseiller, sauf cas spécifique, d’**utiliser son login école. Il faut cocher la case `Administrateur`**. Cliquez sur OK.
3. Dans la fenêtre de gestion des comptes doit apparaître ce nouveau compte. Cliquez dessus, puis sur `Créer un mot de passe`. **Choisissez votre mot de passe école**, ce qui limite le nombre de mots de passe à retenir...
4. Deconnectez vous du compte `administrateur` et connectez vous sur le compte nouvellement créé. Retournez sur la fenêtre de `Gérer un autre compte` cliquez sur le compte `administrateur` et sur `Supprimer le compte` et ensuite sur `Supprimer les fichiers` et enfin sur `Supprimer le compte`.


## Configuration du réseau filaire dans Salsa

Par défaut quand on est dans Salsa, il faut s’authentifier toutes les deux heures pour pouvoir se connecter à internet. C’est légalement obligatoire pour l’école de savoir qui se connecte à internet depuis le réseau interne. Cependant c’est assez peu pratique à l’usage sur un poste de bureau. Il existe une solution qui permet de s’affranchir de cette manipulation. La machine vous authentifie à votre place. Voici les étapes pour la mettre en place :


### Activer le service `Configuration automatique de réseau câblé`

Dans le menu windows 10 taper `services.msc` puis entrée pour ouvrir la fenêtre de gestion des services. Il faut ensuite faire un clic-droit sur `Configuration automatique de réseau câblé` puis :

1. Sélectionner le type de démarrage Automatique
2. Cliquez sur Démarrer
3. Cliquez sur Appliquer puis OK


### Configurer la connexion pour activer l’authentification automatique

Tout est expliqué sur la page [https://intranet.imt-atlantique.fr/assistance-support/informatique/didacticiels/configuration-dans-salsa/](https://intranet.imt-atlantique.fr/assistance-support/informatique/didacticiels/configuration-dans-salsa)


### Perte d’authentification automatique

En cas de problème, refaire toute la configuration :

- Décocher l’option `Activer l’authentification IEEE 802.1x` puis la recocher.
- Dans les paramètres 802.1x, cocher `supprimer les information d’authentification utilisateur` puis renseigner à nouveau son login et son mot de passe en cliquant sur `Enregistrer ident.`.


## Configuration du client VPN

[Configuration VPN](../reseau-securite/vpn.md)


## Accès aux répertoires réseaux et aux imprimantes

Les différentes solutions de stockage de vos fichiers sont listées ici : [Stockage des données](../calcul/storage.md).


### Utilisation de scripts d’automatisation

Un ensemble de scripts d’automatisation ont été écrits pour faciliter le montage des disques réseaux et des imprimante du département MEE. Ils sont normalement présents sur tous les PC windows 10 générés depuis juin 2021.

Ces scripts *drive.bat* et *imp.bat* permettent respectivement de monter les répertoires suivants :

- y: `\\extra.imta.fr\login`
- z: `\\\\ad.imta.fr\homes\login`

et de connecter les imprimantes suivantes

- copieur-k2
- copieur-k1
- IMP-ELEC-002 (noir et blanc)


#### Installation

S'il ne sont pas déjà installés :

- télécharger et extraire cette archive : [https://gitlab.imt-atlantique.fr/jnbazin/script-windows/-/archive/master/script-windows-master.zip](https://gitlab.imt-atlantique.fr/jnbazin/script-windows/-/archive/master/script-windows-master.zip)
- copier les scripts `.ps1` à la racine de C:\ (`C:\MountDrive.ps1` et `C:\MountPrinter.ps1`)
- copier les scripts `.bat` où vous voulez

#### Utilisation

Il suffit de double-cliquer sur les fichiers *drive.bat* et *imp.bat*. Pour l’un comme pour l’autre, une fenêtre s’ouvre pour demander votre login et votre mot de passe. Renseignez ces dernier et validez. Une fenêtre d’invite de commande (fenêtre noir avec du texte…) devrait s’afficher, n’y touchez pas et attendez qu’elle se ferme toute seule. Pour les imprimantes, cela peut prendre plusieurs minutes car cela nécessite l’installation des pilotes, ne touchez à rien.


### Montage manuelle des répertoires réseaux

Il existe plusieurs répertoires réseaux auxquelles vous pouvez accéder :

- `\\ad.imta.fr\homes\login`
- `\\ad.imta.fr\partages\nom_du_partage`
- `\\extra.imta.fr\login`

![Montage manuel de répertoire réseau](../img/win10-montage-manuel.png#center#shadow)


### Connexion manuelle des imprimantes

[https://intranet.imt-atlantique.fr/assistance-support/informatique/didacticiels/imprimer-a-brest-et-a-rennes/](https://intranet.imt-atlantique.fr/assistance-support/informatique/didacticiels/imprimer-a-brest-et-a-rennes/)

## Master MEE 003

Les installations windows au département MEE se font depuis une image windows (master-mee-003.ad.imta.fr) accessible en boot PXE. Sur ce master sont installés des outils supplémentaire par rapport au master de la DISI :

  - [notepad++](https://notepad-plus-plus.org/)
  - [Miktex](https://miktex.org/)
  - [Texstudio](https://www.texstudio.org/)
  - [Mobaxterm](https://mobaxterm.mobatek.net)
  - [LibreOffice](https://fr.libreoffice.org/download/telecharger-libreoffice/)
  - [Git](https://git-scm.com/downloads)
  - [TortoiseGit](https://tortoisegit.org/download/)
  - [Putty](https://www.putty.org/)
  - [KeepassXC](https://keepassxc.org/download/#windows)
  - [PDFSam](https://pdfsam.org/fr/download-pdfsam-basic/)
