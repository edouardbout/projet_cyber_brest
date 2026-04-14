# Creation des attaques

## Création d’un compte utilisateur dans le groupe sudo

Le groupe `sudo` regroupe les utilisateurs autorisés à exécuter des commande avec les droits super utilisateur (Super User DO) Il faut d’abord ouvrir une session avec le compte d’un utilisateur déjà dans le groupe `sudo`. Ouvrir un terminal(++ctrl+alt+t++) puis taper la commande suivant. Il faudra alors renseigner le mot de passe du compte courant. Il est facultatif de remplir les champs concernant les nom, prénom, numéro de téléphone…

``` bash
sudo adduser nouveau_compte
```

En remplaçant `nouveau_compte` par le nom que vous voulez. Il faudra ensuite renseigner un mot de passe pour ce nouveau compte. Si c’est un compte personnel, l’idéal est d’utiliser son login/mot de passe de l’école. Ceci simplifie certaines opération ci-dessous. Puis il faut ajouter l’utilisateur nouvellement créé au groupe sudo avec cette commande :

``` bash
sudo adduser nouveau_compte sudo
```

## Logiciels utiles et mises à jour

### Mise à jour complète

Vérification de la présence de mise à jour, puis mise à jour puis suppression des paquets inutiles et de leur fichiers de config :

``` bash
sudo apt update && sudo apt -y full-upgrade && sudo apt -y autoremove --purge
```


### Paquets utiles à installer (ne prendre que le nécessaire dans la liste)

```bash
sudo apt install -y \
speedcrunch geany libreoffice vlc emacs subversion git putty gimp ffmpeg graphicsmagick \
lftp x2goclient virt-viewer gnome-tweaks keepassxc gparted nextcloud-desktop \
python3-pip python3-dev python3-pygraphviz pkg-config python3-venv graphviz \
build-essential detox aspell aspell-fr flake8 fonts-hack uncrustify svn2git \
curl cifs-utils keyutils cups numlockx openssh-server libpam-mount exfat-fuse \
lsb-base p7zip-rar rar libtinfo6 libncurses6 lm-sensors ocsinventory-agent read-edid \
inxi htop ncdu aptitude smbclient openvpn network-manager-openvpn network-manager-openvpn-gnome
```


Pour LaTeX :
```
sudo apt install -y texlive-full
```

### Pour les PC portables

Utilitaire d'économie d’énergie :

``` bash
sudo apt install tlp
```

### OCS Inventory

OCS Inventory est un outil qui permet de collecter des informations sur les matériel informatique. Un serveur est installé à la DISI, et un agent peut être installé sur chaque machine. L'agent collecte des informations (adresse IP, MAC, modèle de CPU, RAM ...) et les envoie au serveur. C'est utilisé pour automatiser la collecte d'information et permet un dépannage plus rapide. Plus d'info sur le site :[https://ocsinventory-ng.org/?lang=fr](https://ocsinventory-ng.org/?lang=fr)

```bash
sudo apt install ocsinventory-agent read-edid
```
Configuration du paquet :

1. Choix de HTTP
2. Serveur `https://ocs.imta.fr/ocsinventory`

```bash
sudo tee << EOF /etc/ocsinventory/ocsinventory-agent.cfg > /dev/null
server=https://ocs.imta.fr/ocsinventory
wait=3600
nosoftware=1
EOF
```

### Installation de Discord

- Ouvrez un terminal dans le répertoire de votre choix (++ctrl+alt+t++).
- Téléchargez et installez le fichier .deb :

```bash
wget -O discord.deb "https://discordapp.com/api/download?platform=linux&format=deb" && sudo apt install ./discord.deb
```

### Configuration du proxy apt-cacher

Il existe à l’école un apt-cacher, qui est un proxy et un miroir permettant d’installer des paquets ubuntu tout en étant non identifier derrière le portail captif (SALSA). Cela permet de facilement d’installer des paquet en ligne de commande, et d’économiser de la bande passante. Tout paquet déjà installé via ce biais est stocké en local et rendu disponible pour des installations futures.

Il faut créer ou modifier le fichier `/etc/apt/apt.conf.d/01proxy` (en une seule commande) :

```bash
sudo cat > /etc/apt/apt.conf.d/01proxy << EOS
Acquire::http{
     Proxy "http://apt-cacher-01.priv.enst-bretagne.fr:3142";
};
EOS
```

## Configuration Wifi pour Eduroam

Après avoir sélectionner le réseau Wifi Eduroam, il faut choisir les options suivantes (très similaire à la configuration 802.1x ci-dessus, à part pour le nom d’utilisateur) :

| Champ                    | Valeur                       |
|--------------------------|------------------------------|
| Authentification         | `EAP sécurisé (PEAP)`        |
| Certificat               | `Aucun certificat CA requis` |
| Version PEAP             | `automatique`                |
| Authentification interne | `MSCHAPv2`                   |
| Nom d’utilisateur        | `login@imta.fr`              |


## Configuration du réseau : authentification 802.1x

Pour éviter d’avoir à s'authentifier toutes les deux heures sur le réseau SALSA, il est possible de configurer la connexion réseau pour le faire automatiquement, via le 802.1x.

!!! info
    À priori, depuis le passage à Ubuntu 24.04, l'utilisation de l'interface graphique semble fonctionner de nouveau

La configuration est quasi identique à celle d'eduraom. Il faut créer une connexion filaire ou modifier une existante.

- Onglet **Détails** :

| Champ                                     | Valeur      |
|-------------------------------------------|-------------|
| Connexion automatique                     | peu importe |
| Rendre accessible aux autres utilisateurs | Décocher    |
| Connexion avec un quota                   | Décocher    |

- Onglet **Identité**

| Champ          | Valeur                                    |
|----------------|-------------------------------------------|
| Nom            | Au choix, sans caractère spéciaux         |
| Adresse MAC    | Sélectionner celle de l'interface filaire |
| Adresse clonée | Vide                                      |
| MTU            | Automatique                               |

- Onglet **IPv4 et IPv6** : Tout laisser en automatique

- Onglet **Sécurité**

| Champ                    | Valeur                                           |
|--------------------------|--------------------------------------------------|
| Authentification         | `EAP sécurisé (PEAP)`                            |
| Certificat               | `Aucun certificat CA requis`                     |
| Version PEAP             | `automatique`                                    |
| Authentification interne | `MSCHAPv2`                                       |
| Nom d’utilisateur        | `login`                                          |
| Mot de passe             | Choisir *Demander ce mot de passe à chaque fois* |

Il est possible qu'il faille fermer et réouvrir la session.

## Configuration pour accéder au VPN de l’école

[Configuration VPN](../reseau-securite/vpn.md)

## Configuration pour campus distant

1. Se connecter sur [https://campusdistant2.telecom-bretagne.eu/vpn/index.html](https://campusdistant2.telecom-bretagne.eu/vpn/index.html) avec son login/mot de passe.
2. Aller sur **Paramètre de compte** en haut à droite
3. Cliquer sur **Changer  l'application Citix Workspace**
4. Cliquer sur **Utiliser la version simplifiée**
5. Cliquer sur **Bureau** en haut au milieu
6. Cliquer sur **BUREAU Windows CAMPUS**

Vous devriez arriver sur une session Windows Campus. Votre répertoire personnel **Homedir** est dans **Documents**

## Montage automatique au login de lecteurs réseaux via pam_mount

Il est possible de monter automatiquement des disques réseaux au moment du login sur une machine, en utilisant le module pam_mount de la bibliothèque d’authentification pam (pluggable authentification module, utilisé par la plupart des distribution GNU/Linux pour gérer l’authentification des utilisateurs) Prérequis :

1. Installer libpam-mount :
   `sudo apt install libpam-mount`
2. S’assurer que les login et mots de passe de la machine locale soient les même que ceux utilisés pour accéder au disques réseaux

### Modifier les options de sécurité de pam_mount

Il faut modifier le fichier `/etc/security/pam_mount.conf.xml` comme ceci :

Décommenter la ligne contenant `luserconf name=".pam_mount.conf.xml"`  en supprimant `<!-- avant la ligne et -->` après

### Choisir les répertoires que l’on veut monter

On créer un fichier `.pam_mount.conf.xml` dans son répertoire `/home/login` : `touch ~/.pam_mount.conf.xml` Puis coller le contenu suivant :

``` xml
<?xml version="1.0" encoding="utf-8" ?>

<pam_mount>

<volume user="votre_login_ecole" uid="votre_uid_local" gid="votre_gid_local" fstype="cifs" path="//ad.imta.fr/homes/votre_login_ecole" mountpoint="/votre/point/de/montage/local" options="nosuid,nodev" />

<volume user="votre_login_ecole" uid="votre_uid_local" gid="votre_gid_local" fstype="cifs" path="//extra-br.imta.fr/votre_login_ecole" mountpoint="/votre/point/de/montage/local" options="nosuid,nodev" />

</pam_mount>

```


1. Ajouter/supprimer des lignes commençant par `<volume` en fonction de ce que vous voulez monter
2. Remplacer les `votre_login_ecole` par votre login école et les `votre_uid_local` par l’uid de votre session local (si besoin utiliser la commande `id -u`) et les `votre_gid_local` par le gid de votre session local (si besoin utiliser la commande `id -g`)
3. Choisir les points de montages locaux (si les répertoires n’existent pas, `pam_mount` les créés à l’ouverture de la première session et les supprime à la fermeture de la dernière session active. Par exemple `/home/login/campus` pour le homedir campus
4. Ne pas toucher aux options `nosuid` et `nodev`, elle sont requise par la configuration de `pam_mount` par défaut. Si vous voulez les enlever, il faut modifier le fichier `/etc/security/pam_mount.conf.xml`

## Montage manuel d’un lecteur réseau

Prérequis : paquet `cifs-utils` et `keyutils` installé (en super user : `sudo apt install cifs-utils keyutils`)


``` bash
sudo mount -t cifs //emplacement/sur/le/réseau /emplacement/local -o user=login,uid=$USER,domain=ad.imta.fr
```

où :

1. `/emplacement/sur/le/réseau` est l’emplacement du repertoire distant que l’on souhaite monter
2. `/emplacement/local` est l’emplacement dans l’arborescence de la machine local où sera disponible le répertoire distant
3. `login` est le login d’authentification de pour l’emplacement distant (login école pour le homedir par exemple)
4. `userlocal` est le nom du compte utilisateur local de la machine pour lequel on veut que l’emplacement distant soit accessible. Il est conseillé de choisir le même `login` en local que sur son compte école.

Emplacements réseaux souvent utilisés :


- `//ad.imta.fr/homes/login`
- `//extra-br.imta.fr/login`

## Ajouter un disque avec montage automatique au démarrage


Prérequis : avoir un disque disponible (toute donnée présente sera effacée) et installé dans la machine. Avoir les droits root sur la machine. Pour l’exemple ici, il s’agit d’un disque dur mécanique de 500Go.

### Repérage du `device` disque

La commande `sudo fdisk -l` permet de lister tout espace de stockage présent. On peut voir les lignes :

```
Disque /dev/sdb : 465,78 GiB, 500107862016 octets, 976773168 secteurs
Disk model: ST500DM002-1BD14
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 4096 octets
taille d'E/S (minimale / optimale) : 4096 octets / 4096 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0x9e6a29c8
Périphérique Amorçage Début Fin Secteurs Taille Id Type
/dev/sdb1 976773167 976773167 1 512B 5 Étendue
/dev/sdb2 2048 943720447 943718400 450G 83 Linux
La partition 1 ne commence pas sur une frontière de cylindre physique.
```

La première ligne permet d’identifier le disque, qui est la ressource matériel présente via `/dev/sdb`. Vous pouvez aussi utiliser la commande `lsblk`

### Création d’une table de partition et formatage

La commande cfdisk est interactive et permet de créer/modifier/supprimer des partitions. Dans notre cas, on va supprimer toutes les partitions éventuellement présentes et en créer une seule.

1. `sudo cfdisk /dev/sdb`
2. Avec les flèches directionnelles, choisir **Supprimer** pour chaque partition en validant avec Entrée.
3. Une fois toutes les partitions supprimer, choisir **Nouvelle** et garder la taille par défaut, elle occupe tout l’espace disque disponible, et choisir **Primaire**.
4. Puis, choisir **Écrire** pour appliquer les modifications souhaitées.
5. Quitter.

Le formatage en ext4 se fait grâce à la commande suivante : `sudo mkfs.ext4 /dev/sdb1`. Cela peut prendre un peu prendre un peu de temps selon la taille du disque.

### Montage de la partition

Maintenant que le disque est prêt à recevoir des données, il faut le rendre accessible à l’utilisation, il faut le monter.

#### Montage temporaire

Pour l’exemple, on veut rendre le disque accessible via le chemin : `/mnt/data`. On crée le répertoire cible : `sudo mkdir /mnt/data` Puis on monte le disque : `sudo mount /dev/sdb1 /mnt/data` Pour prendre possession du disque : `sudo chown -R $USER /mnt/data`.

#### Montage persistant au reboot

1. Récupérer l’`UUID` de la partition : `sudo blkid /dev/sdb1`. Le retour de la commande est de la forme : `/dev/sdb1: UUID="77af9651-670e-4a42-b137-133bca59a168" TYPE="ext4" PARTUUID="9e6a29c8-01"`
2. Éditer le fichier `fstab` : `sudo nano /etc/fstab` en ajoutant la ligne suivante (NE RIEN MODIFIER D’AUTRE) `UUID=77af9651-670e-4a42-b137-133bca59a168 /mnt/data ext4 errors=remount-ro 0 0` L’UUID utilisé doit être le même que celui récupéré avec blkid. Le montage sera effectif au prochain reboot, ou immédiatement grâce à la commande : `sudo mount /mnt/data`


### Sur une machine Campux (uniquement faisable par un admin)
Création du système de fichier (ext4):

```bash
fdisk -l
cfdisk /dev/sdX
mkfs.ext4 /dev/sdX1
mkdir /usersN
blkid /dev/sdX1
```

Montage via `/etc/fstab`

Dans `/etc/fstab` ajouter la ligne :

```bash
UUID= /usersN ext4 defaults 0 2
```

puis

```bash
mount /usersN
mkdir /usersN/local
chmod 1777 /usersN/local
```

## Mise en place d'un partage NFSv3 avec montage par systemd

### Mise en place du partage NFS

#### installation du serveur NFS
```bash
apt install nfs-kernel-server
```

#### configuration du partage NFS

Édition du fichier `/etc/exports`

```
/chemin/du/repertoire/partage 10.29.182.30(rw,sync,no_subtree_check) 10.29.182.31(rw,sync,no_subtree_check)

systemctl restart nfs-server.service
```

### Mise en place de l'unit de montage systemd sur les clients

Créer le répertoire de montage (optionnel, je crois que systemd le crée à la volé si besoin)

il faut créer un fichier unit systemd ici : `/etc/systemd/system/` **portant le nom composé à partir du chemin de montage**. Par exemple si on veut que le partage soit monté sur `/point/de/montage`, il faut que le fichier s'appelle `point-de-montage.mount` ou avec son chemin complet : `/etc/systemd/system/point-de-montage.mount`.

Dans ce fichier mettre

```
[Unit]
Description=Mount NFS for wonderful shared stuffs
After=network.target

[Mount]
What=ip.du.serveur.nfs:/chemin/du/repertoire/partage
Where=/point/de/montage/local
Type=nfs
Options=_netdev,auto

[Install]
WantedBy=multi-user.target
```

!!! info

    il n'y a pas besoin de créer le point de montage local, systemd s'en occupe à la volée.


Puis il faut faire les commandes de gestion de service systemd en adaptant le nom :
`systemctl daemon-reload`

`systemctl enable point-de-montage.mount`

`systemctl start point-de-montage.mount`

`systemctl status point-de-montage.mount`

`df -h`


## Mise à jour de firmware

```bash
sudo fwupdmgr get-devices
sudo fwupdmgr refresh --force
sudo fwupdmgr get-updates
sudo fwupdmgr update
```

## Installation de TexStudio

Ouvrir un terminal (++ctrl+alt+t++) puis entrer la commande :

``` bash
sudo apt update && sudo apt -y full-upgrade && sudo apt install texstudio texlive-lang-french texlive-fonts-extra
```

## Suppression sécurisé des données présentes sur un disque

Il faut connaître le `device` du disque concerné. On peut le trouver facilement avec la commande :

```
sudo fdisk -l
```

Ensuite il faut faire la commande (avec comme `device` de disque `/dev/sdb`):

```
sudo shred -vfz -n3 /dev/sdb
```
- `-v` : verbose, affiche la progression de la suppression
- `-f` : force, change les permissions d'écriture si nécessaire
- `-z` : zero, écrit des zéros à la dernière passe pour cacher le fait d'avoir utiliser `shred`
- `-n` : nombre de passe, ici 3, qui est la valeur par défaut, et est suffisante
