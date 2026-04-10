# Sécuriser le navigateur Firefox

Firefox est le navigateur installé par défaut sur toutes les machines de l’école. C’est le navigateur recommandé par la DISI et les correspondants informatique du département MEE. Le support d’autres navigateurs ne saurait être assuré que dans la mesure du possible, dans le cas où l’utilisation de Firefox serait impossible à cause de contraintes de compatibilité avec d’autres outils indispensable à la mission de l’utilisateur. Cette page expose les procédures de gestion du cache et de sécurisation du navigateur.

## Gestion du cache

### Procédure pour vider le cache

La procédure de vide du cache est recommandée en cas de signalement d’incident de sécurité en lien avec l’utilisation du navigateur Firefox.

Icône Menu de l’application :arrow_right: Paramètres :arrow_right: Vie privée et sécurité :arrow_right: Historique :arrow_right: Effacer l’historique…

![Effacer historique](../img/firefox/firefox-01-historique.png#center#shadow)

Dans la fenêtre **Supprimer tout l’historique**, cocher toutes les cases puis OK :

![Effacer le cache](../img/firefox/firefox-02-effacer.png#center#shadow)

!!! note
    Cette procédure n’efface pas vos marques pages, que vous pouvez conserver par ailleurs.

### Vérifier l’état du cache

Nouvel Onglet :arrow_right: dans la barre d’URL taper l’URL `about:cache`

![Info cache](../img/firefox/firefox-03-info-cache.png#center#shadow)

si le cache est bien vidé, le nombre d’entrée mémoire et disque est à 0

## Recommandations minimales de sécurité (paramétrage de Firefox)

### Supprimer automatiquement les cookies et données des sites à la fermeture de Firefox

Icône Menu de l’application :arrow_right: Paramètres :arrow_right: Vie privée et sécurité

![Effacer cookies](../img/firefox/firefox-04-effacer-coockies.png#center#shadow)

Sous Firefox, icône Menu de l’application : Paramètres :arrow_right: Vie privée et sécurité/Historique

Règles de conservation : **Ne jamais conserver l’historique**

### Ne pas proposer d’enregistrer les identifiants et les mots de passe pour les sites web

![Ne pas sauvegarder les mots de passe](../img/firefox/firefox-05-no-password.png#center#shadow)

### Bloquer les fenêtre popup et prévenir en cas de tentative d’installation de modules complémentaires

![Pas de popup](../img/firefox/firefox-06-no-popup.png#center#shadow)

### Ne pas autoriser la collecte automatique d’information (décocher tout)

![Pas de collect de données](../img/firefox/firefox-07-no-data-collect.png#center#shadow)

### Activer les options de protection contre les contenus dangereux ou indésirables, vérifier les certificats

![Certificats](../img/firefox/firefox-08-certificats.png#center#shadow)

### Extensions recommandées

Des extensions peuvent être ajoutées afin d’améliorer la sécurité de la navigation internet. L’attention de l’utilisateur doit cependant être attirée sur le fait que l’ajout d’extension à un navigateur internet contribue à l’augmentation de sa surface d’attaque. Par conséquent, le choix d’extension devrait se limiter aux objectifs de sécurité de préférence au confort d’utilisation.

#### Forcer le passage en HTTPS

[https://addons.mozilla.org/fr/firefox/addon/https-everywhere/](https://addons.mozilla.org/fr/firefox/addon/https-everywhere/)


#### Suppression des publicités dans les pages web :

[https://addons.mozilla.org/fr/firefox/addon/ublock-origin/](https://addons.mozilla.org/fr/firefox/addon/ublock-origin/)

<!-- #### Protection contre le pistage -->

<!-- [https://addons.mozilla.org/fr/firefox/addon/decentraleyes/](https://addons.mozilla.org/fr/firefox/addon/decentraleyes/) -->

#### Blocage de tracker

[https://addons.mozilla.org/fr/firefox/addon/privacy-badger17/](https://addons.mozilla.org/fr/firefox/addon/privacy-badger17/)

## Généralités
Il est possible de pousser plus loin la sécurisation du navigateur Firefox cependant au prix de l’utilisabilité et au risque de ne plus pouvoir accéder à certains sites ou certaines fonctionnalités de site. Éviter le plus possible d’échanger des informations hors protocole HTTPS.



Il est important de conserver à l’esprit que la navigation internet ne s’effectue jamais sans risque et quelle constitue, avec la messagerie, l’une des deux portes d’entrée principale des attaques contre les systèmes d’information.

[https://www.ssi.gouv.fr/administration/bonnes-pratiques/](https://www.ssi.gouv.fr/administration/bonnes-pratiques/)
