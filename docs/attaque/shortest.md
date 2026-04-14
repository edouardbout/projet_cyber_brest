# Shortest Path


Suite aux travaux de la DISI pour la mise en œuvre d'un nouveau système d'authentification commun avec ex-Mines Nantes (NAD : New Active Directory), l'ancien serveur d'impression //VSS-WINPRINT est remplacé sur le site Brest par : //imp-br-01.ad.imta.fr

Pour les agents du département MEE travaillant en environnement Linux auto-administré (SALSA) sous Ubuntu 22.04 LTS ou version supérieure, voici une procédure afin d'installer les imprimantes réseau K1 et K2 sur le poste de travail compte tenu du changement dans le système d'information.

## Prérequis


Si ce n'est pas déjà fait, installez le paquet `smbclient`. Pour cela, ouvrir un terminal ++ctrl+alt+t++

```bash
sudo apt-get update && upgrade
sudo apt-get install smbclient system-config-printer cups
```

## Paramètres/Imprimantes/Ajouter une imprimante (connexion au réseau de l'école établie)


![Paramètres Imprimantes](../img/imp-ubuntu/imp-ubuntu-01.png#center#shadow)

- dans le champ de recherche écrire : smb://imp-br-01.ad.imta.fr
- une icône nommée "Imp-br-01.ad.imta.fr" apparait sous "CUPS-BRF-Printer"
- cliquer sur cette icône et sélectionner "Déverrouiller":


![Déverouiller Imprimantes](../img/imp-ubuntu/imp-ubuntu-02.png#center#shadow)

- Entrer votre identifiant (de compte Campus) précédé de "AD/"
- Entre votre mot de passe (compte Campus):

![Authentification](../img/imp-ubuntu/imp-ubuntu-03.png#center#shadow)

- Une liste des imprimantes réseau gérées par le serveur imp-br-01.ad.imta.fr apparait.
- Sélectionner l'imprimante désirée (COPIEUR-K1 ou COPIEUR-K2) pour le Dept. MEE :

![Sélectionner Imprimantes](../img/imp-ubuntu/imp-ubuntu-04.png#center#shadow)

- Sélectionner le fabricant et le modèle (Richo, MP C3004 EX)[^1] et le pilote version PS :

[^1]: Fabricant et pilote correspondant aux COPIEUR-K1 et COPIEUR-K2. Pour les autres imprimantes du campus de Brest, il faut d'abord aller relever les références du fabricant et du modèle indiqués sur l'imprimante...


![Sélectionner le fabricant](../img/imp-ubuntu/imp-ubuntu-05.png#center#shadow)

- Cliquer sur Sélectionner
- L'imprimante apparait dans Paramètres/Imprimantes, avec le nom par défaut "Ricoh-MP-C3004ex" :

![Imprimante ajoutée](../img/imp-ubuntu/imp-ubuntu-06.png#center#shadow)


## Paramétrage de l'imprimante :

- Soit Cliquer sur la petite icône en forme de roue dentée à droite de "Aucune tâche d'impression active"
- Ou ouvrir le logiciel "Imprimantes" (touche "Windows" puis écrire "imprimantes" pour trouver le logiciel)
- Modifier le nom par défaut "Ricoh-MP-C3004ex" en "COPIEUR-K1" (par exemple) :


![Renommer Imprimante](../img/imp-ubuntu/imp-ubuntu-07.png#center#shadow)

- Fermer la fenêtre
- L'imprimante apparait dans Paramètres/Imprimantes, avec le nouveau nom  "COPIEUR-K1" (par exemple)
- Cliquer sur "Paramètres d'imprimante supplémentaires..."
- Ouverture d'une fenêtre "Imprimantes - localhost"
- Faire un clic droit sur COPIEUR-K1 et sélectionner "Propriétés" :


![Propriétés Imprimante](../img/imp-ubuntu/imp-ubuntu-08.png#center#shadow)

- Ouverture d'une fenêtre "Propriétés de l'imprimante - << COPIEUR-K1 >> sur localhost"
- Sélectionner le menu "Paramètres"
- A droite du champ "URI du périphérique :" cliquer sur "Modifier..."

![Modifier propriétés Imprimante](../img/imp-ubuntu/imp-ubuntu-09.png#center#shadow)

- Ouverture de la fenêtre "Modifier l'URI du périphérique"
- Sélectionner "Current device"

- Cocher "Demander à l'utilisateur si une authentification est nécessaire" [^2]

[^2]: une authentification sera demandée à chaque envoi de demande d'impression sur le serveur

ou:

- Cocher "Définir maintenant les détails d'authentification" [^3]

[^3]: une authentification sera effectuée automatiquement à chaque demande d'impression

Dans les modes d'authentification [^2] ou [^3], l'utilisateur doit entrer son identifiant de compte campus précédé de "AD/" et son mot de passe de compte campus :

![Authentification](../img/imp-ubuntu/imp-ubuntu-10.png#center#shadow)

- Cliquer sur "Appliquer"
- Retour à la fenêtre "Imprimantes - localhost"
- Si plusieurs imprimantes sont enregistrées : clic droit + définir par défaut celle que l'on souhaite utiliser le plus souvent (une icône check de couleur verte est positionnée sur l'imprimante définie par défaut)

![Imprimante par défaut](../img/imp-ubuntu/imp-ubuntu-11.png#center#shadow)
