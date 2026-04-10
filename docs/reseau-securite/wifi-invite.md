# Wifi

## Eduroam

### Sur Windows 10

#### Procédure 1

  - Cliquer sur l'icône de connexion Wifi à droite de la barre des tâches
  - Cliquer sur **eduroam**
  - Renseigner dans le champs login : **login@imta.fr** en remplaçant **login** par votre login IMT Atlantique
  - Renseigner le champs de mot de passe avec celui de votre compte IMT Atlantique.

#### Procédure 2 si la 1 ne fonctionne pas

(vous aurez besoin d'une connexion préalable...)

  - Aller sur [https://cat.eduroam.org/](https://cat.eduroam.org/)
  - Télécharger l'installeur grâce au gros bouton au milieu de la page (trouver IMT Atlantique dans la liste avec le mot clé *Atlantique*)
  - Double cliquer dessus pour l'exécuter
  - Répondre aux questions


### Sur Ubuntu

#### Procédure 1

  - Aller dans **Paramètres** :right_arrow: **Wifi**
  - Sélectionner **eduroam**
  - Dans l'onglet sécurité de la fenêtre de configuration de la connexion à Eduroam mettre :
    - Sécurité : WPA et WPA2 d'entreprise
    - Authentification : EAP sécurisé (PEAP)
    - identité masquée : anonymous@imta.fr
    - cocher Aucun certificat CA requis
    - Version PEAP : automatique
    - Authentification interne : MSCHAPv2
    - Nom d'utilisateur : **login**@imta.fr (remplacer login par votre login IMT Atlantique)
    - Mot de passe : celui de votre compte IMT Atlantique.

#### Procédure 2 si la 1 ne fonctionne pas

(vous aurez besoin d'une connexion préalable...)

  - Aller sur [https://cat.eduroam.org/](https://cat.eduroam.org/)
  - Télécharger l'installeur grâce au gros bouton au milieu de la page (trouver IMT Atlantique dans la liste avec le mot clé *Atlantique*)
  - Ouvrir un terminal (++ctrl+alt+t++) aller dans le répertoire ou se trouve le script d'installation
    - Autoriser l'exécution du script : `chmod +x eduroam-linux-EnsMABPdlL-GENERAL.py`
    - Exécuter le script : `python3 ./eduroam-linux-EnsMABPdlL-GENERAL.py`
    - Répondre aux questions.

## Wifi invité

### Procédure

La connexion d’un invité au réseau Wifi de l’école est possible via la procédure de déclaration accessible par le portail :

[https://intranet.telecom-bretagne.eu/gestion-invites/](https://intranet.telecom-bretagne.eu/gestion-invites/)

### Support

En cas de difficulté, le service d’accès au réseau Wifi s’inscrivant au sein d’une infrastructure gérée par la DISI, il convient de formaliser une demande sous la forme d’un ticket de support adressé par e-mail à :
[support-disi@imt-atlantique.fr](mailto:support-disi@imt-atlantique.fr) en associant les correspondants informatique du département MEE en copie à la demande.
