# Guide d'utilisation de Datarmor

Ce guide s'adresse à toute personne amenée à travailler sur le super calculateur d'Ifremer [Datarmor](https://www.ifremer.fr/fr/infrastructures-de-recherche/le-supercalculateur-datarmor) dans le cadre de l'équipe ODYSSEY. 

Pour une présentation complète de Datarmor, voir : [https://pcdm.ifremer.fr/Equipement](https://pcdm.ifremer.fr/Equipement).

Datarmor est un mésocentre thématique mer/océan. Son utilisation est réservée aux projets en lien avec cette thématique.


---
## **Assistance et contacts**

La personne référente pour Datarmor à MEE est Emmanuel Braux, à contacter par Slack, ou par mail.

La documentation officielle du centre Datarmor est accessible (une fois connecté au réseau Ifremer) via [https://domicile.ifremer.fr/intraric/Mon-informatique/,DanaInfo=w3z.ifremer.fr,SSL,SSO=U+Calcul-et-donnees-scientifiques](https://domicile.ifremer.fr/intraric/Mon-informatique/,DanaInfo=w3z.ifremer.fr,SSL,SSO=U+Calcul-et-donnees-scientifiques)

Pour contacter directement l'assistance informatique d'Ifremer :

- par mail à assistance@ifremer.fr
- par téléphone au 02 98 22 42 07.

---
## **Inscription à Datarmor**

La demande de création d'un compte doit passer par la personne référente MEE :

Les informations nécessaires sont : 

- Nom
- Prénom
- Email
- Date de début
- Date de fin

Pour pouvoir accéder à Datarmor, 2 comptes informatiques vont être crées :

- Un compte d'accès au réseau Ifremer (compte extranet)
- Un compte d'accès au centre de calcul Datarmor (compte intranet)


!!! note

    En cas de nécessité, si vous êtes amené à gérer vous même une demande, envoyez votre demande par mail au support d'Ifremer : `assistance@ifremer.fr`. Précisez bien le contexte, et demandez le rattachement à la personne référente MEE. Ce type de demande doit être réservée à des cas exceptionnels

---
## **Connexion au réseau Ifremer**

---
### Principe

Pour pouvoir accéder à Datarmor, il faut être connecté au réseau d'Ifremer.

Pour se connecter à ce réseau, il faut installer l'outils de VPN Ifremer "Ivanti Secure Access (Pulse Secure).

---
### Installation du VPN pour accéder au réseau Ifremer

Ifremer met à disposition le client VPN "Ivanti Secure Access (Pulse Secure)" dans l'espace partagé : [https://cloud.ifremer.fr/index.php/s/WC9GArY8Eo51yZE](https://cloud.ifremer.fr/index.php/s/WC9GArY8Eo51yZE)

- Pour Windows, utilisez les fichiers PulseSecure*.msi (x64 ou x86, en fonction de la version de Windows).
- Pour MacOS les .dmg. 
- Pour Linux, en fonction de la distribution, les .deb ou .rpm. 

Le VPN est ensuite disponible, soit via une interface graphique `PulseUI`, soit en ligne de commande.

Si possible privilégier l'utilisation via l'interface graphique, plus simple et plus conviviale.


Par exemple pour les systèmes Ubuntu/Debian, télécharger le fichier au format `pulsesecure_xx.x.Rx_amd64.deb`, et l'installer avec la commande `apt` :
``` bash
sudo apt install ./pulsesecure_22.8.R2_amd64.deb
```
*(Le './' est nécessaire pour que apt comprenne qu'il s'agit d'un fichier local)*


---
### Connexion VPN  en mode graphique

Pour créez une nouvelle connexion en mode graphique, cliquer sur le bouton "+" et remplir les informations pour créer un nouveau profil :

- Type : Connecte Secure (VPN)
- Server URL : https://domicile.ifremer.fr/calcul
- Nom : choisir un nom , par exemple `vpn-Ifremer`

Pour s'authentifier, les information du compte extranet sont nécessaires  :

- Login extranet  (au format `PNxxxxx`, **P**rénom, **N**om)
- Mot de passe extranet

---
### Connexion VPN en ligne de commande

Pour lancer une connexion VPN en ligne de commande :
``` bash
/opt/pulsesecure/bin/pulselauncher -U https://domicile.ifremer.fr/calcul -r vpn-Ifremer -u votre_login_extranet
```

!!! note

    En ligne de commande, une fois la connexion établie, `pulselauncher` ne renvoie aucune information, et ne rend pas la main sur le terminal. Il faut donc utiliser un terminal dédié, ou l'exécuter en tache de fond.

---
## **Connexion à Datarmor**

---
### Connexion en SSH

Une fois connecté via le VPN au réseau Ifremer, pour avoir accès au serveur vous permettant de soumettre des jobs sur Datarmor, se connecter en ssh à `datarmor-access.ifremer.fr`. Utiliser le compte intranet (au format "première lettre de votre prénom + votre nom).
``` bash
ssh jdoe@datarmor-access.ifremer.fr
```

La machine `datarmor-access` est une machine assez ancienne Pour les postes de travail récents, il peut être nécessaire d'ajouter des paramètres de connection. Par exemple :
``` bash
ssh jdoe@datarmor-access.ifremer.fr \
  -o KexAlgorithms=+diffie-hellman-group1-sha1 \
  -o HostkeyAlgorithms=+ssh-rsa \
  -o PubkeyAcceptedAlgorithms +ssh-rsa
```

Pour utiliser une paire de clé SSH, pour ne plus avoir à saisir de mot de passe, il faut utiliser une clé au format RSA 4096.

Exemple de mise en place d'une clé :

- Génération de la clé (utiliser l'adresse email personnelle imt-atlantique) :
``` bash
ssh-keygen -t rsa -b 4096 -C "john.doe@imt-atlantique.fr" -f ~/.ssh/id_rsa_datarmor
```
- Copie de la clé, dans le compte sur le serveur Datarmor. Nécessite de s'authentifier avec un mot de passe une dernière fois : 
``` bash
ssh-copy-id -i ~/.ssh/id_rsa_datarmor.pub jdoe@datarmor-access.ifremer.fr
```
- Test de connexion avec la clé :
``` bash 
ssh -i ~/.ssh/id_rsa_datarmor jdoe@datarmor-access.ifremer.fr
```

Pour faciliter la connexion, il est possible de mettre en place une section pour ce serveur, dans le fichier `~/.ssh/config`
``` bash
Host datarmor
    HostName        datarmor-access.ifremer.fr
    User            jdoe
    IdentityFile    ~/.ssh/id_rsa_datarmor
    compression     yes
    KexAlgorithms   +diffie-hellman-group1-sha1
    HostkeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
```
Ce qui permet d'utiliser ensuite la commande simple :
``` bash
ssh datarmor
```

---
### Connexion en mode Web

Il est également possible de se connecter à Datarmor sans être sur le réseau Ifremer, et donc sans passer par le VPN.

Utiliser le portail d'accès distant : [https://domicile.ifremer.fr/calcul](https://domicile.ifremer.fr/calcul)

- S'authentifier avec le compte extranet.
- Cliquer sur "connect datarmor-web"
- Ouvrir une session web avec son compte Datarmor


---
### Connexion à Jupyterhub

Une fois connecté via le VPN au réseau Ifremer, il est possible d'avoir accès à un espace d'exécution de Notebook Jupyter. [https://jupyterhub.ifremer.fr(https://jupyterhub.ifremer.fr)].

Pour l'authentification, utiliser le login Datarmor.

Il est ensuite possible de lancer des Notebooks qui s'exécuterons sur Datarmor.

Plusieurs profils de machines sont disponibles, avec ou sans GPU.

Il est également possible d'accéder à Jupyterhub via le portail d'accès distant.

---
### Connexion aux serveurs DGX

Dans le cadre du CPER AIDA, plusieurs serveurs NVIDIA DGX ont été déployés sur Datarmor, et sont accessibles aux membre de l'équipe ODYSSEY. Chaque serveur DGX propose 8 cartes Nvidia H100.

Ces serveurs sont accessible via Jupyterhub. Ils correspondent aux serveurs "dataaiX"

Il sont également accessibles en SSH : 

- Se connecter en SSH au serveur à `datarmor-access.ifremer.fr`
``` bash
ssh jdoe@datarmor-access.ifremer.fr
```
- Puis depuis ce serveur, se connecter en ssh à l'un des serveur DGX : compute-101-9.ifremer.fr, compute-101-15.ifremer.fr ou  compute-101-23.ifremer.fr
``` bash
ssh compute-101-9.ifremer.fr
```

Pour faciliter la connexion, il est possible de mettre en place une section pour ces serveurs, en utilisant l'option "ProxyJump" pour gérer le serveur de rebond, dans le fichier `~/.ssh/config`
``` bash
Host datarmor
    HostName        datarmor-access.ifremer.fr
    User            jdoe
    IdentityFile    ~/.ssh/ebraux_ed25519
    compression     yes

Host dgx1
    HostName        compute-101-9.ifremer.fr
    User            jdoe
    IdentityFile    ~/.ssh/id_rsa_datarmor
    compression     yes
    ProxyJump datarmor

Host dgx2
    HostName        compute-101-15.ifremer.fr
    User            jdoe
    IdentityFile    ~/.ssh/id_rsa_datarmor
    compression     yes
    ProxyJump datarmor

Host dgx3
    HostName        compute-101-23.ifremer.fr
    User            jdoe
    IdentityFile    ~/.ssh/id_rsa_datarmor
    compression     yes
    ProxyJump datarmor

```
Ce qui permet d'utiliser ensuite, par exemple,  la commande simple :
``` bash
ssh dgx2
```

---
## **Ressources**

### Espaces disques

Les espaces de stockage disponibles sont :

- Espace personnel `/ontap/user/[votre-login]` (anciennement datahome /home*/datahome/[votre-login] )
- Espace de travail `/scale/user/[votre-login]` (anciennement datawork /home*/datawork/[votre-login] )

### Cartes graphiques


## Environnement Python

Les environnement sont basé sur conda. Conda est régulièrement mis à jour sur Datarmor, et les nouvelles versions rajoutées dans le répertoire /appli/anaconda/versions. 

Pour afficher les versions disponibles
``` bash
ls -1 /appli/anaconda/versions
```

Pour activer un environnement conda

- Tester si le shell est `bash`
``` bash
echo $0
# bash
```
- Si ce n'est as le cas, par exemple `-csh`, initialiser un shell bash, et tester à nouveau le shell :
``` bash
bash
```


!!! note

    Pour valider définitivement le shell bash par défaut, modifier, ou créer le fichier ~/.login, et y placer
    ``` bash
    if ( -x /bin/bash ) then
        exec /bin/bash
    endif
    ```


La syntaxe pour accéder aux commandes de conda est :
``` Bash
$ source /appli/anaconda/versions/[numero_version]/etc/profile.d/conda.sh
```
où [numero_version] correspond au tag de la version conda souhaitée.

Lister les environnements disponibles
``` bash
conda info --envs
# # conda environments:
# #
# base                  *  /appli/anaconda/versions/4.7.12
# jupyterhub               /appli/conda-env/jupyterhub
# pyproj-python3           /appli/conda-env/pyproj-python3
# ...
```

- Activer un environnement
``` bash
conda activate /appli/conda-env/pyproj-python3
```

Il est également possible de créer un environnement personnalisé
``` bash
conda create --name testEnv python=3.12
```
Les environnements personnalisés sont stockés par défaut dans le dossier conda-env de votre datahome : `$HOME/conda-env`
