# Gérer ses mots de passe (Keepass)
## Intérêt d’un gestionnaire de mot de passe

Un gestionnaire de mot de passe permet de stocker ses mots de passe dans un fichier sécurisé par chiffrement symétrique, donc très robuste. Ce fichier peut-être envoyé par mail ou autre moyen de communication sans risque à condition de ne jamais divulguer le mot de passe maître qui sert à chiffrer ce fichier. On peut donc :

- utiliser ce fichier sur tous les ordinateurs/smartphones que l’on utilise en le partageant par exemple sur partage.imt-atlantique.fr ou par mail.
- N’avoir à retenir qu’un seul mot de passe, celui qui permet de déchiffrer le fichier contenant tous ses mots de passe
- Générer des mots de passe uniques et robustes pour chaque site/machine/service que l’on utilise, ce qui accroît la sécurité en cas de compromission d’un mot de passe (suite au piratage d’un site web qui stocke les mots de passe en clair, si si ça existe encore…)

Encore faut-il utiliser un gestionnaire de mot de passe fiable et bien maintenu. Il en existe plusieurs, et celui qui revient souvent est **keepassXC**. KeepassXC utilise le chiffrement AES avec une clé de 256 bits, ce qui est considéré comme très sûr.

## Installation sur windows

Téléchargez KeepassXC sur le site officiel : [https://keepassxc.org/download/#windows](https://keepassxc.org/download/#windows)

Vous avez le choix d’une version portable (utile sur une machine sur laquelle vous n’êtes pas administrateur) que vous pouvez mettre sur une clé USB par exemple. Attention à la legislation de certains pays qui peuvent vous contraindre à donner vos mots de passe. Mieux vaut laisser le fichier de base de données sur votre homedir et vous connecter sur le VPN de l’école.

Si c’est pour votre machine dans Salsa, préférez la version installable en 64bits

## Installation sur Ubuntu

```bash
sudo apt install keepassxc
```

## Utilisation basique
### Premier lancement

- Lors du premier lancement, vous allez créer un fichier de base de donnés qui va contenir les mots de passe (on peut décider d’utiliser plusieurs fichiers pour segmenter ses mots de passe)
- Ensuite vous allez choisir un mot de passe. Choisissez un mot de passe robuste, idéalement 12 caractères ou plus, mélangeant minuscules, majuscules, chiffres et caractères spéciaux (-_/*@$#^!’?~ ….).

### Lancements suivants

À chaque lancement, il vous sera demandé de renseigner l’emplacement du fichier de base de donnés que vous souhaitez utiliser puis d’entrer le mot de passe pour le déchiffrer.

### Création des `Entrées`

Une fois fait, vous arrivez sur la fenêtre principale de l’outil, listant les différents mots de passe. Pour l’instant il n’y en a aucun. Pour en créer un, allez sur `Entrées` :arrow_right: `Ajouter une nouvelle entrée` ou ++ctrl+n++.

Vous pouvez alors renseigner plusieurs champs :

- **Titre** : qui sera visible dans la liste de la fenêtre principale
- **Nom d’utilisateur** : qui sera associé au mot de passe
- **Mot de passe** : que vous pouvez générer automatiquement grâce à l’icône en forme de dé à droite du champ confirmation.
- **Confirmation** : permet d’être sûr que l’on a tapé le mot de passe correctement. Utilisez copier-coller dans le cas d’un mot de passe généré automatiquement
- Pour les mots de passe générés automatiquement, plusieurs options sont possibles :
  - **La longueur** : choisissez la longueur maximum permise par le site web/service visé
  - **Types de caractères** : idem, choisissez le plus possible permis par le site web/service visé
  - **Phrases secrète** : Si besoin, vous pouvez aussi générer des phrases secrètes parfois demandées pour renouveler un mot de passe ou vous identifier en cas de compromission de votre compte.
  - Vous pouvez voir le mot de passe généré en cliquant sur l’icône en forme d’œil à droite du champ «Mot de passe»
- **URL** : adresse du site sur lequel vous utilisez votre couple nom d’utilisateur/mot de passe, pratique car fait office de marque-page web et permet d’ouvrir le navigateur et d’accéder au site directement depuis keepassXC
- **Expiration** : Permet de fixer une date d’expiration du mot de passe pour forcer le renouvellement, optionnel.
- **Notes** : Permet de renseigner ce que vous voulez.

Une fois tous ces champs renseignés, vous pouvez cliquer sur `Appliquer` (enregistrement des informations saisies) ou `OK` (enregistrement des informations saisies et retour à la fenêtre principale).

### Utilisation courante

Ensuite il suffit de faire un clic droit sur une entrée pour au choix ouvrir l’URL (++ctrl+u++) dans le navigateur web, copier le nom d’utilisateur (++ctrl+b++), copier le mot de passe (++ctrl+c++), modifier l’entrée…

Il est intéressant de stocker le fichier chiffré sur [https://cloud.imt-atlantique.fr](https://cloud.imt-atlantique.fr) ou [https://partage.imt.fr](https://partage.imt.fr), pour le sauvegarder et le rendre facilement accessible à toutes ses machines.

### Paramètres

Dans les paramètres `Outils` :arrow_right: `Paramètres`, il est possible entre autres :

- de changer le temps de persistance du presse papier contenant le mot de passe, pour éviter que le mot de passe ne reste trop longtemps disponible.
- de paramétrer les conditions de verrouillage du fichier de base de données
- de paramétrer l’intégration au navigateur
