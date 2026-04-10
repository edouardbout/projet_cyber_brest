# Aide mémoire Linux



## Commandes utiles

### Consulter le manuel des commandes

```bash
man [n] commande
```

Visualisation à l’écran des informations concernant la commande spécifiée. L’affichage est réalisé par un more.
Le manuel est divisé en huit sections:

1 : Les commandes utilisateurs
2 : Les appels systèmes
3 : La librairie des sous-routines
4 : Les formats de fichiers
5 : Les fichiers spéciaux
7 : Les possibilités diverses
8 ou 1m : Les commandes d’administrations système
9 : glossaire

On peut spécifier la section dans laquelle on veut effectuer la recherche (grâce au paramètre n).

Cette commande est probablement la plus utile de toutes.

### lister les process utiliseurs

```bash
ps aux |grep -v '\(root\|gdm\)'
```

### Identifier les utilisateurs du système

```bash
who
```

### Avoir des information sur son compte utilisateur

```bash
id
```

Cette commande retourne les `uid` et `gid`, souvent utile à connaître pour gérer les droits d'accès sur des fichiers/répertoires, ainsi que les groupes auxquels appartient l'utilisateur.

### Changer de mot de passe (SALSA)

La commande `passwd` permet de changer son mot de passe sur une machine SALSA. Pour une machine **campus/campux** il faut changer le mot de passe de son compte école ici : [https://pairuc.imt-atlantique.fr/rucadmin/account/login.php](https://pairuc.imt-atlantique.fr/rucadmin/account/login.php)

### Afficher une chaine de caractères ou le contenu de variables d'environnement

```bash
echo "chaine de caractères"
```
ou pour une variable :### lister les process utiliseurs

```bash
ps aux |grep -v '\(root\|gdm\)'
```


```bash
echo $PATH
```

### Visualiser le contenu d’une répertoire

La commande `ls [OPTION] [FICHIER]` permet l'affichage du contenu de répertoire.

Options utiles

- `-a` : affiche aussi les fichiers cachés
- `-l` : utiliser un format d'affichage long
- `-s` : afficher la taille allouée de chaque fichier en nombre de blocs
- `-h` : avec -l ou -s, afficher les tailles en format lisible (par exemple 1K, 234M, 2G, etc.)
- `-t` : trier selon la date de modification, de la plus récente à la plus ancienne

Par exemple :

```bash
ls -alsht ~/
```

Va afficher le contenu du répertoire `home` de l'utilisateur courant en incluant les fichiers cachés.

### changer de répertoire

La commande `cd /repertoire/de/destination` permet de changer le répertoire dans lequel on se trouve.

!!! note
    le chemin `~/` est équivalent à `/home/$USER`, `$USER` étant l'utilisateur courant.

### Afficher le chemin du répertoire courant

La commande `pwd` permet d'afficher le chemin absolu du répertoire courant.

### Créer des répertoires

La commande `mkdir nouveau-repertoire` permet de créer un répertoire nommé `nouveau-repertoire`. L'option `-p` permet de créer tous les répertoires intermédiaires s'il n'existent pas. Par exemple si `rep1` n'existe pas, la commande `mkdir rep1/rep2` va retourner une erreur. Dans ce cas il est pratique d'utiliser `mkdir -p rep1/rep2`. De même, cette option ne retourne pas d'erreur si le répertoire que l'on veut créer existe déjà.

### Supprimer des fichiers ou des répertoires.

La commande `rm` permet de supprimer des fichiers et des répertoires. Pour supprimer un répertoire, il faut utiliser l'option `-r`. Pour ne pas qu'une confirmation soit demandée, il faut utiliser l'option `-f`. On peut utiliser la wildcard `*` pour supprimer plusieurs fichiers d'un coup

```bash
rm -f fichier.txt

rm -f rep1/*.jpg # supprime tous les fichiers se terminant par .jpg dans le répertoire rep1

rm -rf rep1 # supprime le répertoire rep1 et son contenu.
```

### Visualiser le contenu d’un fichier

La commande `cat` permet d'afficher sur la sortie standard le contenu d'un fichier. Par exemple :

```bash
cat ~/.profile
```

La commande `less` permet d'afficher un fichier de manière intéractive. Il est possible de faire une recherche de chaîne de caractère avec la commande `/`.

### Chercher une chaîne de caractères dans un ou plusieurs fichiers

Dans un fichier :

```bash
grep "chaine" fichier
```

Recursivement dans un répertoire :

```bash
grep -r "chaine"
```

Options :

- `-r` : rechercher récursive
- `-i` : pour ne pas tenir compte des majuscules/minuscules
- `-v` : pour afficher les lignes ne contenant pas l’expression spécifiée.

### Faire une copie d’un fichier

La commande `cp` permet de copier des fichiers ou des répertoires.

```bash
cp source destination
```
copie le fichier source dans le fichier destination. Si le fichier destination n’existe pas, il est créé . Sinon son contenu est écrasé sans avertissement.

Pour copier un répertoire, il faut utiliser l'option `-r` :

```bash
cp -r rep1 rep2
```

### Déplacer ou changer le nom d’un fichier/répertoire

```bash
mv source destination
```
renomme ou déplace le fichier/répertoire source en destination.

### Créer un lien sur un fichier

```bash
ln [-s] cible_du_lien nom_du_lien
```

création un lien sur un fichier ou un répertoire. Un lien est un moyen d’accéder à un même fichier ou un répertoire sous plusieurs noms ou à partir de plusieurs répertoires. Attention un lien n’est pas une copie: si vous modifiez le fichier alors tous les liens sur ce fichier seront modifiés.

Il existe deux sortes de liens: le lien physique et le lien symbolique (avec l’option `-s`). Le lien physique ne peut adresser que des fichiers, alors que le lien symbolique peut aussi lier des répertoires.

Dans le cas de lien physique, pour effacer le fichier, vous devez effacer tous les liens qui pointent sur ce fichier. Par contre pour des liens symboliques, vous pouvez effacer le fichier sans effacer les liens, mais alors ceux-ci seront invalides.

### Rechercher un fichier

```bash
find rep -name nom -print
```

recherche le(s) fichier(s) caractérisé par name (vous pouvez utiliser une expression régulière), à partir du répertoire rep et affiche le résultat.

vous pouvez décrire le fichier à rechercher par une expression régulière, ou indiquer le type de fichiers à chercher ou encore le propriétaire…
vous pouvez aussi exécuter d’autres actions, comme effacer le fichiers… (pour plus de détails, voir le man)

### Permission sur les fichiers

Voir aussi : [https://chmodcommand.com/](https://chmodcommand.com/)

```bash
chmod [-R] mode fichiers
```

change les permissions du ou des fichiers/répertoires. (option `-R` pour le faire récursivement dans tous les sous répertoires et fichier d’un répertoire)

Pour chaque fichier, il y a trois classes d’utilisateurs :

| désignation | définition                                     |
|-------------|------------------------------------------------|
| utilisateur | le propriétaire du fichier (`uid`)             |
| groupe      | le groupe auquel appartient le fichier (`gid`) |
| autre       | tous les autres                                |

Les permissions accordées à ces trois classes sont :

| sigle | signification |
|-------|---------------|
| r     | lecture       |
| w     | écriture      |
| x     | exécution     |


exemple mode désiré : `rwxr-xr–` :

| user | group | other |
|------|-------|-------|
| rwx  | r-x   | r--   |
| 111  | 101   | 100   |
| 7    | 5     | 4     |

Que l'on peut faire avec :
```bash
chmod 754 fichier
```

### Se connecter sur un autre compte

La commande `su` permet de changer d'utilisateur.
```bash
su [-] utilisateur
```
Permet de se connecter en tant que `utilisateur.`. Vous devez saisir le mot de passe du nouvel utilisateur (sauf pour le root). Si vous indiquez `–` alors les fichiers de login (.cshrc ou autre suivant le shell) sont exécutes et vous vous retrouvez dans le home directory du nouveau compte.

### Surveiller les programmes en cours d’exécution

La commande `ps` affiche la liste des processus actifs. Attention les options de cette commande change suivant le système que vous avez, (vérifiez les par le man)

### Tuer un processus

```bash
kill num_process
```
supprime le processus spécifié via son `pid`(vous récuperez le numéro du processus par un ps). Si malgré la commande, le processus n’est pas détruit, essayez `kill -9 num_process`


### Redirection des entrées-sorties

| Redirection | Définition                                                                  |
|-------------|-----------------------------------------------------------------------------|
| <           | l’entrée standard est lu à partir d’un fichier                              |
| >           | La sortie standard est redirigée dans un fichier (RAZ du fichier)           |
| >>          | La sortie standard est redirigée dans un fichier (concaténation du fichier) |
| 2>          | les erreurs sont redirigées dans un fichier                                 |
| 2>&1        | les erreurs sont redirigées dans le même fichier que la sortie standard     |

### Appel de commande dans une autre commande

Il est parfois utile d'appeler une commande 1 dans une autre commande 2 lorsque l'ont besoin du retour de la première directement dans la deuxième. Pour cela il faut écrire cette commande 1 : `$(commande 1)`. Par exemple, dans la documentation [Ubuntu 24](ubuntu24.md#configuration-du-reseau-authentification-8021x), est donnée cette commande :

```bash
nmcli connection add type ethernet con-name "Wired8021x" ifname $(ip ad | grep ^[0-9] | cut -d" " -f 2 | grep ^e | cut -d":" -f1) 802-1x.eap peap 802-1x.identity $USER@imta.fr 802-1x.phase2-auth mschapv2 802-1x.password-flags 2 save yes
```

Cette commande relativement longue qui permet de créer une connexion réseau avec authentification 802.1x, à besoin d'un nom d'interface réseau. Comme les noms d'interface ne sont pas forcément les même d'une machine à l'autre, il faut le trouver pour créer cette connexion. On peut manuellement lister les interfaces et copier coller le résultat dans la commande. Mais on peut imbriquer une commande qui permet de trouver ce nom, c'est ce qui est fait avec :

```bash
$(ip ad | grep ^[0-9] | cut -d" " -f 2 | grep ^e | cut -d":" -f1)
```

### Pipe

On voit dans la commande ci-dessus le caractère `|` appelé *pipe* ou *pipeline*. Cet opérateur permet de rediriger la sortie standard d'une commande vers l'entrée standard de la suivante.

```bash
ip ad | grep ^[0-9]
```

La sortie de la commande `ip ad`, qui liste de manière détaillé les interface réseau, est donnée à la commande `grep ^[0-9]` qui ne va afficher que les lignes qui commencent par un chiffre.

### xargs

voir [https://azurplus.fr/comment-utiliser-la-commande-xargs-sous-linux/](https://azurplus.fr/comment-utiliser-la-commande-xargs-sous-linux/)













## Parallel SSH

Paraller SSH permet d'exécuter des commandes sur plusieurs machines distantes simultanément.

### Installation (dans SALSA) :
```bash
sudo apt install pssh
```

### Utilisation de base

```bash
parallel-ssh -h hosts.txt -i "commande1 -option -argument ; commande2 -option -argument"
```

- `-h hosts.txt` : fichier texte contenant la liste des hosts cibles, un par ligne.
- `-i` : affiche la sortie standard et la sortie d'erreur de l'exécution sur chaque host






## Mise en place d'une clé SSH

Sources :
- [https://www.ssh.com/academy/ssh/keygen](https://www.ssh.com/academy/ssh/keygen)
- [https://www.ssh.com/academy/ssh/copy-id](https://www.ssh.com/academy/ssh/copy-id)

Pour éviter de taper son mot de passe pour se connecter à certaines machines, ou pour automatiser des tâches distantes, il peut-être utile de mettre en place une authentification par clés SSH (paire de clé public/privée). Il existe plusieurs algorithme pour créer des clés SSH, chacun avec un niveau de sécurité différent. Il est conseillé d'utiliser `ed25519` et en cas d'incompatibilité `RSA 4096`.

### Prérequis

Installation du paquet `openssh-client` (sur machine SALSA)

```bash
sudo apt install openssh-client
```

### Création d'une paire de clés

(resp. `ed25519` et `RSA 4096`) :

```bash
ssh-keygen -t ed25519 -f ~/.ssh/name-of-the-key -C "login@fl-mee-br-099.imta.fr"

ssh-keygen -t rsa -b 4096 -f ~/.ssh/name-of-the-key -C "login@fl-mee-br-099.imta.fr"
```

- `-t` : type de clé
- `-f` : emplacement et nom de la clé.
- `-C` : commentaire, souvent le `user@host` sur lequel on crée la clé.

Il est demandé d'entrer une passphrase (i.e mot de passe). Cette passphrase est optionnelle, mais permet d'augmenter le niveau de sécurité. L'inconvénient est qu'il faut entrer cette passphrase pour l'utilisation de la clé. À choisir en fonction de l'utilisation.

### Copie de la clé sur une machine distante

```bash
ssh-copy-id -i ~/.ssh/name-of-the-key.pub login@machine-distante
```

Par exemple pour copier la clé public `~/.ssh/test.pub` sur le login node Jean-Zay `jean-zay.idris.fr` :

```bash
ssh-copy-id -i ~/.ssh/test.pub login@jean-zay.idris.fr
```

### Activation de l'agent SSH et ajout de la clés (privée)

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/test
```

On peut ensuite se connecter à la machine distante sans utiliser son mot de passe, ou alors la passphrase de la clé.






## Authentification portail captif sur serveur Linux SALSA sans bureau graphique

### Prérequis

Soit avoir un accès internet lors de la configuration ou mettre en place le proxy apt-cacher (voir [Configuration apt-cacher](ubuntu24.md#configuration-du-proxy-apt-cacher))


### Installation des paquets nécessaires

```bash
sudo apt install python3-selenium python3-pyvirtualdisplay
```

[pycaptif.zip](../files/pycaptif.zip)

### Récupération des fichiers necessaires

geckodriver de firefox et des scripts de connexion et déconnexion en ligne de commande :

Copiez la sur le serveur, adaptez la commande en ce qui concerne `user` et `server` :

```bash
scp pycaptif.zip user@server:/home/user
```

Connectez-vous au serveur en ssh et décompressez l'archive, toujours en adaptant la commande :

```bash
ssh user@server

unzip pycaptif.zip
```

Mettez en place de geckodriver dans `/usr/bin` :

```bash
sudo cp ~/pycaptif/geckodriver /usr/bin/

sudo chmod 755 /usr/bin/geckodriver
```

Utilisation pour la connexion et la deconnexion à internet.

```bash
python3 ~/pycaptif/get_internet.py

python3 ~/pycaptif/stop_internet.py
```

Entrez votre login et votre mot de passe comme demandé. Ces scripts peuvent être déplacés où bon vous semble.


## Utilisation de `dd` pour faire une clé liveUSB à partir d'une image ISO

```bash
dd if=/chemin/image.iso of=/dev/sdX  bs=4M && sync
```

`sdX` à adapter en fonction du nom de device de la clé. On peut pour cela utiliser `fdisk -l`, qui permet de lister les devices de stockage.




## Récupération d’une IP SALSA d’une machine avec son adresse MAC

Les machine SALSA n'ont pas d'adresse IP fixe. Elle peut changer sans préavis, souvent après redémarrage. Pour pouvoir se connecter en SSH ou via X2go à une machine SALSA, il faut pourtant connaître son IP. Voici une méthode possible.


Prérequis :
- être connecté à une machine sur SALSA
- connaître l’adresse MAC de la machine dont on veut connaître l’adresse IP

Installer apr-scan :

```bash
sudo apt install arp-scan
```
Lancer la commande suivante (adresse MAC pour l’exemple : `30:85:a9:99:a8:25`) :

```bash
sudo arp-scan -t 3000 10.29.232.0/22 | grep "30:85:a9:99:a8:25"
```
Le retour de la commande est : `10.29.232.166 30:85:a9:99:a8:25 ASUSTek COMPUTER INC.`

On sait maintenant que l'adresse IP recherché est `10.29.232.166`, on peut donc se connecter avec :

```bash
ssh user@10.29.232.166
```






## Mise en place d’un RAID 1 logiciel (SALSA)

Source : [https://doc.ubuntu-fr.org/raid_logiciel](https://doc.ubuntu-fr.org/raid_logiciel)

Le RAID 1 est une configuration d'utilisation de disques durs (HDD ou SSD) qui permet de stocker automatiquement les données sur 2 disques simultanément. Ceci prévient la perte de données en cas de panne matériel d'un des disques, et peut permettre une lecture des donnée plus rapide.

Pour l'exemple, les 2 disques, **vierges** et de tailles équivalentes, disposent de chacun une partition occupant tout leur espace `/dev/sda1` et `/dev/sdb1`. Il faudra adapter les commandes à votre configuration matérielle.

```bash
sudo mdadm --create --verbose --assume-clean /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1
```

Pour vérifier :

```bash
cat /proc/mdstat
sudo fdisk -l
sudo mdadm --detail /dev/md0
```

Pour que le volume RAID créé soit chargé automatiquement au démarrage :

```bash
sudo mdadm --monitor --daemonise /dev/md0
```
création d'une partition ext4 sur l'ensemble du volume RAID 1 :

```bash
sudo mkfs.ext4 /dev/md0```
```

mise à jour de la configuration RAID :
```bash
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
sudo update-initramfs -u
```


Montage automatique de la partition au démarrage de la machine :

1. Récupération de l'UUID du volume RAID1 :

```bash
sudo blkid
```
2. Mise à jour du fichier `/etc/fstab` en mettant le bon UUID dans la commande suivante :

```bash
echo 'UUID=l'uid trouvé avec la commande ci-dessus /RAID1 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab
```

Montage des volumes présent dans `/etc/fstab`

```bash
sudo mount -a
```


### compression avec perte :

```bash
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```

Remplacer `screen` par `ebook` ou `printer` pour respectivement dégrader ou garder une qualité maximale.


### Conversion d'un PDF en niveau de gris


```bash
gs -sOutputFile=outfile.pdf -sDEVICE=pdfwrite -sColorConversionStrategy=Gray -dProcessColorModel=/DeviceGray -dCompatibilityLevel=1.4 infile.pdf < /dev/null
```

### PDF Sam

[Pdfsam](https://pdfsam.org/fr/pdfsam-basic/) est un outil multiplateforme qui dispose d'une version libre et gratuite (PDFsam Basic). Cet outil permet des manipulations sur des fichiers PDF :
- Division par pages
- Fusion de documents
- Mélange alternativement de pages de plusieurs documents
- Rotation de pages à l'intérieur d'un document
- Extraction de pages d'un document pour en créer un nouveau

**Installation sur Ubuntu :**

```bash
sudo apt install pdfsam
```

**Installation sur Windows :**

Télécharger la dernière version de l'installeur sur https://github.com/torakiki/pdfsam/releases/

Double cliquer pour lancer l'installation.


## Compression image dans un dossier

Prérequis : installation du paquet `imagemagick` :

```bash
sudo apt install imagemagick
```

Exemple d'utilisation :

```bash
for file in *.jpg; do convert -resize 50% -- "$file" "${file%%.jpg}-resized.jpg"; done
```
réduit la taille de toutes les images *.jpg du dossier courant de 50% en gardant la proportion. On peut donner une taille en remplaçant 50% par LARGEURxHAUTEUR (par exemple 800×600). Il est possible de baisser la qualiter pour diminuer le poids des fichier en ajoutant l’option -quality 97 avant ou après l’option resize. La valeur pour la qualité varie de 1 à 100. 97 est en général une valeur donnant un bon rapport qualité/légèreté.


## Mise en place d'une machine virtuelle Windows 10

Installation de virtualbox :

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install -y virtualbox virtualbox-ext-pack virtualbox-dkms
```

Voir avec les correspondant informatique pour récupérer une VM déjà créée.



## Créer un service **ping** sur un serveur salsa pour garder la connexion ssh

Les machines salsa IP fixe ne sont pas, d’un point de vu réseau, destinées à être des serveurs sur lesquels on se connecte en ssh de manière continue. C’est pourquoi les sessions ssh sur une telle machine freeze et sont difficilement utilisables.

Créer le répertoire nécessaire :


```bash
sudo mkdir -p /usr/lib/systemd/system/
```
créer et éditer le fichier `/usr/lib/systemd/system/pingssh.service` dont le contenu doit être :

```bash
[Unit]
Description=Ping SSH keep alive
#After=network.target
After=network-online.target

[Service]
ExecStart=/bin/bash -c "ping 10.29.232.1"

[Install]
WantedBy=multi-user.target
```

donner les droits exécution au script :

```bash
sudo chmod 755 /usr/lib/systemd/system/pingssh.service
```
autoriser le démarrage de ce service au démarrage de la machine :

```bash
sudo systemctl enable pingssh.service
```

Lancer le service manuellement sans :
```bash
sudo systemctl start pingssh.service
```
Vérifier que le service est lancé :
```bash
sudo systemctl status pingssh.service
```


Ou tout faire d'un coup :
```bash
sudo mkdir -p /usr/lib/systemd/system/
sudo cat << EOT >> /usr/lib/systemd/system/pingssh.service

[Unit]
Description=Ping SSH keep alive
#After=network.target
After=network-online.target

[Service]
ExecStart=/bin/bash -c "ping 10.29.232.1"

[Install]
WantedBy=multi-user.target

EOT

sudo chmod 755 /usr/lib/systemd/system/pingssh.service
sudo systemctl enable pingssh.service
sudo systemctl start pingssh.service
sudo systemctl status pingssh.service
```

## Remplacer une chaîne de caractère dans tous les fichiers d’une arborescence de dossier

Remplacement dans **TOUS** les fichiers de l’arborescence descendante de la chaîne *titi toto* par *tata tutu*
```bash
find . -type f | xargs sed -i -e 's!titi toto!tata tutu!g'
```


## Gestion des droits avec setfacl

```bash
setfacl -m u:login:rx dossier
setfacl -R -m u:login:rx dossier/data/
```

## Résolution automatique sur VM VDI avec XFCE

[https://superuser.com/questions/1183834/no-auto-resize-with-spice-and-virt-manager](https://superuser.com/questions/1183834/no-auto-resize-with-spice-and-virt-manager)


## Convertir un dépôt SVN en dépôt git avec svn2git

`svn2git` permet de convertir un projet SVN en projet Git, en conservant l'historique des commit, et les utilisateurs.

Sur une machine SALSA, installer `svn2git` (si pas déjà fait) :

```bash
sudo apt install svn2git
```

Créez un dépôt sur gitlab.imt-atlantique.fr (ou ailleurs).

Pour l'exemple :

- URL du dépôt SVN : https://redmine.telecom-bretagne.eu/svn/vieux-depot
- URL du dépôt Git : https://gitlab.imt-atlantique.fr/groupe-projet/nouveau-depot


Ouvrir un terminal avec ++ctrl+alt+t++, se déplacer dans un répertoire vide, en créer un si besoin. Une fois dedans :

```bash
svn2git https://redmine.telecom-bretagne.eu/svn/vieux-depot
```

Il faudra éventuellement renseigner un nom d'utilisateur et un mot de passe (ce qui présuppose que l'on ai accès au dépôt à convertir). Cela peut prendre un peu de temps en fonction de la taille du projet.

```bash
git remote add origin https://gitlab.imt-atlantique.fr/groupe-projet/nouveau-depot
git push origin master
```
