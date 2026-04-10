# IT-USER : Procédure d'ajout d'utilisateurs externes

source : [https://pairuc.imt-atlantique.fr/rucadmin/docs/spec-ituser/doc-usager-ituser.pdf](https://pairuc.imt-atlantique.fr/rucadmin/docs/spec-ituser/doc-usager-ituser.pdf)

> Hors des filières classiques associées à des métiers[^1] certains usagers n’entrent pas dans ces filières et n’ont besoin que d’un compte informatique et de quelques services qui sont des compétences DISI. Ces usagers font l’œuvre d’une procédure spécifique IT-USER. Cette procédure permet à un parrain de solliciter la DISI pour l’ouverture d’un compte afin qu’un **IT-USER** coopté puisse accéder à des ressources informatiques, par exemple GITLab ou 3P. Ce document précise au parrain les procédures et règles associées aux IT-USERS.





## Services accessibles à un IT-USER

Une personne ayant un compte IT-USER peut disposer des accès suivants :

- **RUC** : **systématique**, la personne dispose d’un accès à sa fiche RUC, elle peut voir et renseigner les informations personnelles la concernant et exercer ses droits sur celles-ci. Elle peut changer son mot de passe ou utiliser la procédure de récupération de mot de passe. La personne peut se connecter des services qui utilisent le RUC ou ses dérivés pour s’authentifier, c’est par exemple le cas du serveur [gitlab](https://gitlab.imt-atlantique.fr) de l’école.
- **SHIBBOLETH** : **optionnel**, la personne peut s’authentifier avec son compte sur des services qui utilisent l’authentification Shibboleth/CAS de l’école. Ceci peut concerner des services proposés par l’école ou bien par des membres de la fédération.
- **ACCOUNTING** : **optionnel**, la personne pourra se connecter avec son compte sur les machines unix ou windows administrées par la DISI sur les domaines *campux*, *campus* ou *NAD*.
- **NETWORK** : **optionnel**, la personne pourra se connecter au wifi avec son compte. Le compte est valide sur le réseau eduroam.


## Pour un parrain, solliciter l’ajout d’un IT-USER

Chacun peut solliciter l’ajout d’un IT-USER et devenir parrain, dans la mesure où le coopté pour lequel est fait la demande ne peut être pris en charge par une filière standard *PASS*, *PHD* ou *PASSUP*.

Le parrain demandeur formule une demande à la DISI via la gestion de ticket sur [https://support.imt-atlantique.fr/otrs/customer.pl](https://support.imt-atlantique.fr/otrs/customer.pl). Il fournit les informations suivantes :

- le nom du coopté
- son prénom
- sa date de naissance, celle-ci sert à le discerner d’éventuels homonymes
- la date de fin de validité (DFV). Cette date ne peut excéder un an à partir de la date de la demande
- son adresse mail de contact, celle-ci sera visible dans le RUC ;
- la nature du ou des services auquels le IT-USER devra se connecter.

Après prise en charge du ticket, la DISI procède à la création ou à la réactivation du IT-USER et inscrit le demandeur comme son parrain. La DISI ajoute les autorisations nécessaires selon les service souhaités par le parrain.

## Pour un parrain, énumérer ses IT-USERS

Au préalable le parrain doit se connecter à [RucAdmin](https://pairuc.imt-atlantique.fr/rucadmin/account/login.php). Un lien (figure suivante, repère 2) lui est accessible depuis le menu déroulant (repère 1) et permet d’accéder à la page qui énumère ses cooptés.

![Liste des cooptés pour un parrain](../img/it-users/it-users-1.png#center#shadow)

En cliquant sur le nom du coopté on accède à la vue détaillée IT- USER, cette vue est documentée plus bas dans la section [4](#visualiser-et-completer-la-fiche-dun-it-user)


Le statut exact du IT- USER est indiqué (repère  3), il peut s’agir de ituser ou bien de institut dans le cas plus spécifique des personnels de l’Institut Mines Télécom hors IMT Atlantique.

**NB** : un IT-USER peut avoir plusieurs parrains, il peut donc apparaître dans la liste des cooptés de plusieurs personnes.


## Visualiser et compléter la fiche d’un IT-USER

La vue détaillée d’un coopté est présentée sur la figure suivante. Les cadres repérés sur cette figure par 1 et 2 peuvent être renseignés par le parrain alors que le cadre 3 ne peut être renseigné que par la DISI (une erreur survient à l’enregistrement si une modification y est apportée). Le cadre 2 comporte des informations qui peuvent également être amendées par le coopté à partir de sa fiche personnelle.

![Vue détaillée d’un coopté](../img/it-users/it-users-2.png#center#shadow)

Les règles suivantes doivent être respectées par le parrain dans la saisie des informations :

- repère 4 : l’organisation ou la société à laquelle appartient l’individu, par exemple IMT Nord Europe ou Orange Cyberdéfense. **Cette valeur doit être différente de IMT Atlantique, le cas échéant la saisie de IMT Atlantique sera systématiquement corrigée en ACME pour ituser et Institut pour institut**
- repère 6 : la date de départ doit impérativement être renseignée. **Celle-ci ne doit pas excéder un an (365 jours) après la date du jour et sera systématiquement corrigée à la date du jour plus 6 mois dans le cas contraire**
- repère 7 : indiquer la date de naissance est important car cette information est utilisée pour identifier les homonymes et éviter la création de doublons dans le RUC. Toutefois, conformément au RGPD, le coopté est libre de supprimer cette information. **La date de naissance n’est visible que du coopté, des parrains et d’un nombre restreint d’administrateurs du RUC.**

## Cycle de vie des IT-USERS

À l’instar des autres usagers inscrits dans le RUC, les IT- USERS sont informés à la création, à la réactivation et à l’approche de la suppression de leur compte :

- **FOC** : quand il est créé et que toutes les informations requises sont renseignées, le IT-USER reçoit un mail (FOC [^2]) lui demandant de valider la lecture de la charte informatique et d’indiquer sa langue préférée entre l’anglais et le français. Ce mail est envoyé jusqu’à trois fois à intervalle d’une semaine si la validation n’a pas été faite. Une fois la lecture de la charte validée le IT- USER ne recevra plus jamais cette sollicitation
- **Vademecum** : une fois l’étape FOC passée, le IT- USER va recevoir le vademecum. Ce mail lui indique les différentes ressources qui peuvent lui être accessibles, en particulier la procédure de changement de mot de passe et l’accès à sa fiche RUC personnelle. Si un IT-USER est désactivé puis réactivé il peut éventuellement recevoir à nouveau un vademecum, ceci dépend du délai depuis le premier envoi
- **Hasta La Vista** : des mails indiquant la fermeture prochaine du compte sont envoyés à l’IT-USER à l’approche de la date d’invalidation. Les envois interviennent environ trois semaines et un semaine avant la date de clôture. Le premier parrain indiqué pour l’IT-USER reçoit également ces mails, il lui est possible le cas échéant de repousser la date de fermeture du compte en modifiant celle-ci dans la fiche de détail comme indiqué dans la section [4](#visualiser-et-completer-la-fiche-dun-it-user)



[^1]: RH pour les personnels, stagiaires, hébergés et emeritus, DFVS pour les intervenants et élèves et DRI pour les doctorants et assimilés.

[^2]: Fiche d’Ouverture de Compte
