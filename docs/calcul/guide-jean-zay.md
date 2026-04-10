# Guide d'utilisation de Jean Zay

(Merci à Daniel Zhu pour cette documentation)

Ce guide s'adresse à toute nouvelle personne amenée à travailler sur le super calculateur [Jean Zay](http://www.idris.fr/jean-zay/cpu/jean-zay-cpu-hw.html) dans le cadre de l'équipe Océanix. Les personnes n'ayant jamais travaillé avec un environnement Unix (Linux ou Mac) peuvent suivre ce document sans problème -- par conséquent, ce guide risque d'être un peu verbeux pour une personne expérimentée.

C'est un petit condensé de petits *tips* qu'on m'a donnés, qui, j'espère, vous feront économiser un temps précieux.

Voir la [documentation](http://www.idris.fr/jean-zay/) de Jean Zay pour plus de détails.

Pour toute question, me contacter sur Slack ou par courriel : daniel.zhu@imt-atlantique.fr, j'essaierai de vous aider au mieux.

## Inscription à Jean Zay

1. faire une demande de création de compte ici: [https://www.edari.fr/before_register](https://www.edari.fr/before_register)
2. les informations utiles
    - adresse labo:<br>
        Laboratoire des Sciences et Techniques de l'information de la Communication et de la Connaissance<br>
        CNRS, UMR 6285<br>
        Technopole Brest-Iroise - CS 83818<br>
        29238 BREST CEDEX 3
    - directeur de labo:<br>
        M. Person Christian<br>
        christian.person@imt-atlantique.fr
    - responsable sécurité de l'utilisateur:<br>
        M. Poupart Eric<br>
        eric.poupart@imt-atlantique.fr<br>
        tel.: 02 29 00 16 67
    - adresse IP:<br>
        192.108.117.17
    - FQDN:<br>
        ssh-01.enst-bretagne.fr
3. demander à Eric Poupart et Christian Person de signer le document numériquement (avec le lien qu'ils auront reçu)
4. imprimer, signer, scanner et renvoyer le document sur la plateforme.
5. imprimer, signer et transmettre la charte informatique ([http://www.idris.fr/media/data/formulaires/charte-informatique.pdf](http://www.idris.fr/media/data/formulaires/charte-informatique.pdf)) à Eric Poupart pour archivage.

## Connexion à Jean Zay

### Connexion à la passerelle

Pour vous connecter à Jean Zay, il faut d'abord passer par la passerelle de l'école à l'aide du protocole SSH ([*Secure Shell*](https://fr.wikipedia.org/wiki/Secure_Shell)).

La passerelle de l'école est un intermédiaire entre votre ordinateur et le reste d'internet, il peut être vu comme un [*proxy*](https://fr.wikipedia.org/wiki/Proxy).

!!! info
    **Hôte de la passerelle**

    La passerelle est accessible depuis l'hôte **ssh.telecom-bretagne.eu** !


Pour se connecter à la passerelle, ouvrir un terminal et saisir la commande suivante :

```shell
ssh XXX@ssh.telecom-bretagne.eu
```

Où `XXX` est votre identifiant campus (école).

Le serveur vous demandera de prouver votre identité en renseignant le mot de passe associé à `XXX`.

### Connexion à Jean Zay

Une fois sur la passerelle, vous pouvez vous connecter au supercalculateur Jean Zay avec la commande suivante :

```shell
ssh YYY@jean-zay.idris.fr
```

Où `YYY` est votre identifiant IDRIS.

!!!warning
    **Première connexion**.

    Attention, à la première connexion, le mot de passe que vous devez utiliser est une combinaison concaténant une chaîne aléatoire et celle que vous avez indiquée à votre inscription (tout est indiqué dans le courriel de bienvenue).

    On vous demandera par la suite de choisir un nouveau mot de passe.

    Consulter [cette page](http://www.idris.fr/jean-zay/pb_mot_de_passe.html) pour en apprendre plus sur les mots de passe.


!!!info
    **Adresse de Jean Zay**

    L'accès à Jean Zay peut se faire via l'hôte générique **jean-zay.idris.fr**.
    Il est à noter que celle-ci vous redirige aléatoirement vers l'une des quatre frontales (hôtes) numérotées de 1 à 4 :

    - **jean-zay1.idris.fr** ;
    - **jean-zay2.idris.fr** ;
    - **jean-zay3.idris.fr** ;
    - **jean-zay4.idris.fr**.

    Dans le cas où vous seriez amené à utiliser certains programmes comme [*tmux*](https://fr.wikipedia.org/wiki/Tmux), il faudra utiliser la même frontale à chaque fois afin de bénéficier de la persistence (un `tmux` lancé depuis la frontale 1 ne sera accessible qu'à la frontale 1).


Vous pouvez savoir sur quelle frontale vous êtes actuellement en regardant le préfixe de l'invité de commande :

```shell
[YYY@jean-zay3: ~]$
```

vous indique que vous êtes sur la frontale 3.

Je vous recommande d'utiliser systématiquement une seule frontale (la 1 par exemple) si vous utilisez souvent `tmux`. Il suffira d'indiquer directement la frontale de votre choix dans votre commande SSH plutôt que l'hôte générique. Par exemple, pour la frontale 1 :

```shell
ssh YYY@jean-zay1.idris.fr
```
### Certificat SSH (optionnel)

Dans l'idéal, le mot de passe que vous définissez à votre première connexion est une chaîne de caractères très complexe et non triviale, générée et stockée par votre gestionnaire de mot de passe favoris, par exemple :

```
\<]`T¿l/#AlU<GaI(uQP[Dm%hâ4370QcA3þOdjWÞUf8IoÀFS9%ÉyK7ìIU2ìdh``4
```

(mais bon vous faites comme vous voulez)

Jean Zay nous recommande d'utiliser des certificat SSH : le principe est de générer un certificat que vous enregistrez soigneusement sur votre `HOMEDIR` de l'école et que vous ne donnez à **personne**. Vous pourrez alors choisir une *phrase* de passe, par exemple "L'ornithorynque fait ses besoins" que vous utiliserez pour vous connecter en lieu et place de votre mot de passe super compliqué. Ce certificat est à renouveler chaque année (il y a aussi un autre type de certificat où vous n'avez même pas besoin de saisir votre mot de passe mais il me semble que c'est spécifiquement pour transférer des fichiers, et à renouveler chaque semaine).

Voir [cette page](http://www.idris.fr/jean-zay/cpu/jean-zay-cpu-connexion-ssh-certificats.html) si ça vous intéresse.

### Transférer des fichiers

#### Code source

Github (Gitlab ou autre) me parait être la solution la plus simple pour transférer du code source entre votre ordinateur et Jean Zay.

Vous pouvez essayer de vous débrouiller aussi pour pouvoir ouvrir directement du code sur Jean Zay depuis votre ordinateur mais ça me paraît compliqué dans la mesure où la passerelle ne permet pas l'*IP forwarding* :thinking_face:.

#### Entre Jean Zay et `extra`

Pour transférer des fichiers entre deux machines distantes, on utilise le
protocole `scp` ([_secure copy_](https://fr.wikipedia.org/wiki/Secure_copy)). Sa syntaxe est la suivante :

```shell
scp SOURCE DESTINATION
```

Où `SOURCE` (resp. `DESTINATION`) est le chemin où se situe le fichier que vous
voulez transférer (resp. le chemin où sera stocké le fichier en question).

En général, l'un des paramètres (`SOURCE` ou `DESTINATION`) désigne un emplacement local (situé sur la
machine où vous saisissez la commande `scp`) et l'autre sur la machine distante. Quand il s'agit d'un emplacement distant, il faut préfixez le chemin par `identifiant@hôte:` comme lorsque vous vous connectez sur un serveur avec SSH.

Vous avez la possibilité de transférer directement un **répertoire** avec tout
son contenu directement, dans ce cas vous devez indiquer le chemin vers le répertoire
et spécifier le paramètre `-r` (pour "récursivement") comme dans l'exemple suivant :

```shell
scp -r mon_répertoire id@hôte:chemin_cible
```

Dans notre cas, il est possible théoriquement depuis la passerelle d'indiquer
deux emplacements distants (Jean Zay et votre machine) mais ça peut s'avérer
fastidieux ; je vous propose plutôt de passer par un intermédiaire, le disque
de stockage proposé par l'école, à savoir `extra` ([voir ici](https://intranet-mee.gitlab-pages.imt-atlantique.fr/stockage/)).

1. Deux situations :
    - Si vous êtes sur votre propre machine (SALSA), montez le disque réseau `extra` sur votre ordinateur (voir [voir cette page](https://intranet-mee.gitlab-pages.imt-atlantique.fr/ubuntu24/#montage-manuel-dun-lecteur-reseau) de l'intranet du département) car il est assez facile de communiquer avec le disque depuis votre ordinateur une fois monté ;
    - Si vous êtes sur un poste fixe sur le campus, vous devriez déjà avoir accès à `extra` donc vous n'aurez rien à faire ;
2. Connectez-vous à la passerelle de l'école si ce n'est pas encore fait,
    - Pour transférer des fichiers des serveurs Jean Zay vers `extra` (pour récupérer des résultats obtenus sur Jean Zay par exemple), taper :
        ```shell
        $ scp YYY@jean-zay.idris.fr:ABC /extra/brest/XXX/XYZ
        ```
    - Inversement, si vous souhaitez transférer des fichiers de `extra` vers Jean Zay, il suffira d'inverser l'ordre des paramètres :
        ```shell
        $ scp /extra/brest/XXX/XYZ YYY@jean-zay.idris.fr:ABC
        ```
    Où `YYY` (resp. `XXX`) est votre identifiant Idris (resp. d'école) et `ABC` (resp. `XYZ`) est le chemin sur Jean Zay du fichier à copier (resp. le chemin de destination sur `extra`, ou **inversement** selon que vous transfériez dans un sens ou vers un autre !).

## Ressources

### Espaces disques

Quand vous vous connectez vous arrivez sur Jean Zay, vous arrivez sur le `HOME` mais vous ne pouvez presque rien y stocker dessus (espace disque très limité).

!!! info
    **Les espaces de disques sur Jean Zay**

    Voir [**la documentation**](http://www.idris.fr/jean-zay/cpu/jean-zay-cpu-calculateurs-disques.html) pour connaître les différents disques accessibles individuellement / par équipe.


Normalement on a un espace `$WORK` mais il était déjà presque plein quand je suis arrivé :cry: du coup on est obligé de tout mettre sur l'espace `$SCRATCH`. C'est principalement ici qu'on va stocker tous nos scripts et éventuellement nos résultats (**penser à bien les récupérer**).

Pour accéder à un espace particulier, par exemple le `$SCRATCH`, saisir :

```shell
cd $SCRATCH
```

!!! danger
    **Le délestage automatique du `$SCRATCH`**

    On a beaucoup de place dans le `$SCRATCH` mais **tout fichier non modifié ni accédé dans les trente derniers jours est automatiquement supprimé** ! (voir bloc suivant)


!!! success
    **Actualiser des fichiers**

    Afin de contrer le délestage automatique du `$SCRATCH`, on peut artificiellement mettre à jour la date de dernière modification de tous les répertoires et fichiers (de façon récursive) :

    1. Se placer dans le `$SCRATCH` :
       ```shell
       cd $SCRATCH
       ```
    2. Saisir la commande suivante :
       ```shell
       find . -execdir touch "{}" +
       ```

    À faire une fois par mois (mais ne *pas* en abuser pour éviter d'éveiller des soupçons... :eyes:). Ça peut prendre un peu de temps s'il y a beaucoup de fichiers à actualiser.


### Cartes graphiques

Sur Jean Zay, on peut utiliser des CPUs (processeurs) ou des GPUs (cartes graphiques) pour l'exécution des scripts.

Cette [page](http://www.idris.fr/jean-zay/cpu/jean-zay-cpu-hw.html) explique très bien quelles sont les unités de calcul mises à notre disposition.

À titre d'information, de novembre 2022 à novembre 2023, nous avons :

- jusqu'à 40 000 heures de calcul sur les GPUs V100 ;
- jusqu'à 5 000 heures de calcul sur les GPUs A100 ;
- jusqu'à 5 000 heures de calcul sur les CPUs.

Si vous demandez un nœud avec 1 GPU V100 pendant 12 heures, 12 heures seront débitées de notre allocation V100 ; si vous demandez 4 GPUs v100 pendant 10 heures, 40 heures seront débitées de notre allocation V100 (car 4x10 heures), etc.

**P. S.** Dans le contexte de Slurm,

- Un *node* (nœud) est une association de plusieurs unités de calcul (CPU ou GPU, RAM, etc) dédiée à un *job* (ton script) : « *Nodes possess resources such as processors, memory, swap, local disk, etc. and jobs consume these resources.* » ;
- Une partition peut être vue comme une file d'attente (pour accéder à un ensemble de nœuds).

Voir [cette page](https://slurm.schedmd.com/cons_res.html) pour plus d'information.

## Environnement Python

Python et Anaconda sont déjà installés sur Jean Zay ! Donc vous pouvez directement créer des [environnements virtuels](https://docs.python.org/fr/3/tutorial/venv.html) pour vos projets.

!!! warning
    **Avant de créer un environnement virtuel**

    Les environnements virtuels que vous allez créer seront stockés dans votre `HOME`, où vous avez très peu de place, dans les répertoires `.conda` et `.local`.

    Il convient de créer et transférer [symboliquement](https://fr.wikipedia.org/wiki/Lien_symbolique) ces deux répertoires vers le `$SCRATCH` :

    ```shell
    mkdir $SCRATCH/.conda
    ln -s $SCRATCH/.conda $HOME
    mkdir $SCRATCH/.local
    ln -s $SCRATCH/.local $HOME
    ```

    Cette opération est à faire une fois :wink: (attention au délestage automatique du `$SCRATCH` !)

    Dans le cas où les répertoires `.conda` et `.local` existent déjà dans votre `HOME`, il suffit de déplacer ces répertoires vers le `$SCRATCH` (utiliser `mv $HOME/{.conda|.local} $SCRATCH/` au lieu de `mkdir $SCRATCH/{.conda|.local}`).


Pour créer un environnement conda, il faut d'abord charger un module Python :

```shell
module load python
```

Puis vous pourrez alors avoir accès aux commandes d'Anaconda (créer un environnement, en charger un, etc) :

```shell
conda create -n monEnvironnement
conda activate monEnvironnement
```

Si vous voulez une version spécifique de Python pour votre environnement, il suffit de l'indiquer lors de sa création :
```shell
conda create -n monEnvironnement python=X.Y
```
Où `X.Y` est la version de Python que vous voulez pour votre environnement.

La commande suivante vous permet de voir quelles versions de Python sont disponibles :

```shell
module avail python
```

Voir [cette page](http://www.idris.fr/jean-zay/cpu/jean-zay-cpu-doc_module.html) pour en savoir plus sur les modules et [celle-ci](http://www.idris.fr/jean-zay/gpu/jean-zay-gpu-python-env.html) pour en savoir plus sur la gestion des environnements virtuels sur Jean Zay.

## Lancer un script avec Jean Zay

Vous ne pouvez pas accéder aux ressources de Jean Zay (CPU et GPU) comme bon vous semble : il va falloir passer par une file d'attente (car beaucoup d'autres personnes en France utilisent aussi Jean Zay).

L'IDRIS utilise [`slurm`](https://fr.wikipedia.org/wiki/SLURM) pour la gestion de l'accès aux ressources.

### Soumission classique d'un script

Si vous souhaitez lancer 4dvarnet(-core), il suffit de lancer dans la commande exécutant le script avec le paramètre `+backend=ZZZ`, où `ZZZ` peut prendre l'une des valeurs suivantes :

- `slurm_1x1` : 1 GPU, pratique pour les tests ;
- `slurm_1x4` : 4 GPUs, plutôt pour les apprentissages.

Voir [cette page](https://github.com/CIA-Oceanix/4dvarnet-core/tree/main/hydra_config/backend) pour connaître les différentes configurations disponibles.

Sinon pour tous les autres scripts, il faut écrire un script SLURM puis lancer une soumission. Cette [page](http://www.idris.fr/jean-zay/gpu/jean-zay-gpu-exec_mono_batch.html) vous explique comment faire (n'hésitez pas si vous avez des questions).

### Mode interactif
Utile pour débogguer/tester des scripts quand on n'a pas de GPU sous la main. Par exemple on peut ne réserver qu'un seul GPU pour tester son script (mais du coup ce n'est pas adapté aux grosses simulations).

D'abord, réserver il faut réserver un nœud :

```shell
 salloc --ntasks=1 --cpus-per-task=10 --gres=gpu:1 --hint=nomultithread -C v100-32g --qos=qos_gpu-t3 -A yrf@v100 --time=10:00:00 --job-name=new_repl
```

Il y aura un peu d'attente le temps qu'un nœud se libère (demander qu'un seul GPU réduit le temps d'attente).

Avec cette commande, on aura jusqu'à 10 heures d'utilisation maximale, mais on n'est pas tenu de les utiliser jusqu'au bout (on peut et on doit les rendre avant le temps imparti afin d'économiser les heures).

Ensuite, quand le nœud vous aura été attribué, vous pourrez lancer vos scripts avec la commande suivante autant de fois que vous voudrez :

```shell
srun python -u VOTRE_SCRIPT.py etc
```

!!! danger
    **Rendre à César ce qui lui appartient**

    Si vous utilisez un *node* en mode interactif en ayant fixé une limite de temps à 10 heures (comme dans l'exemple précédent) mais que vous ne l'utilisez que pendant 2 heures, il faut penser à quitter manuellement pour rendre le temps restant à Jean Zay, car sinon ils seront décomptés inutilement de notre allocation !

    La commande `exit` fait l'affaire (vérifier avec `squeue -u $USER` que le *node* a bien été rendu !).


### Suivi des tâches

!!! info
    **Suivi des tâches**.

    La commande suivante permet de suivre l'état de vos tâches (R=Running, PD=Pending et CG=en cours d'arrêt) :

    ```shell
    squeue -u $USER
    ```

Vous obtiendrez quelque chose de la forme :

```shell
    JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
    28713   gpu_p13 new_repl      YYY  R       0:02      1 TTT
```

La deuxième ligne vous indique que votre tâche porte le numéro 28713 (JOBID), qu'il a été affecté au nœud `TTT` et qu'il est en cours d'exécution (`R`).

!!! info
    **Interrompre un *job***

    La commande `scancel` suivi de l'ID du *job* interrompt l'exécution dudit *job* et rend à Jean Zay le nœud qui vous était jusqu'alors attribué.


Ainsi, dans notre exemple précédent, si on veut interrompre le *job* dont l'ID est 28713, il suffit de saisir :

```shell
scancel 28713
```

Quelque soit l'état (`ST`) du *job*, il passera en `CG` (fin de tâche) puis le noeud sera de nouveau disponible pour d'autres personnes.

!!! info
    **Temps écoulé pour une tâche**

    vous pouvez voir le temps écoulé pour une tâche terminé si vous avez son `JOBID` en utilisant la commande suivante :

    ```shell
    sacct --format="JobID,JobName,Account,Elapsed" -j VVV
    ```

    Où `VVV` est le `JOBID` de votre tâche.


Enfin, vous pouvez suivre l'état de nos allocations avec la commande suivante :
```shell
idracct
```

Voir [cette page](http://www.idris.fr/jean-zay/cpu/jean-zay-cpu-doc_account.html) pour plus d'information.

### Suivi des GPUs
La commande `nvidia-smi` n'est utilisable que dans un nœud qui vous est alloué (elle permet de voir l'état des cartes graphiques sur lesquelles votre script s'exécute).

Pour vous connecter à un nœud, il faut utiliser la commande précédente `squeue -u $USER` : la dernière colonne intitulée « *NODELIST(REASON)* » contient le nom du ou des nœuds qui vous sont alloués si la tâche est en *Running*. Ensuite, il suffira de faire :

```
$ ssh TTT
```

Où `TTT` est le nom du nœud qui est alloué à la tâche dont vous voulez suivre l'état des GPUs.

Vous vous connectez de cette façon dans le nœud où vous pourrez saisir la commande :

```
$ nvidia-smi
```

Et voir l'état et l'usage des GPUs.

## Remerciements

Merci à Hugo G. et à Quentin F. qui m'ont appris plein de petits *tips* que j'ai mis dans ce guide et qu'on peut retrouver sous une forme plus complète et exhaustive sur la [page de documentation](http://www.idris.fr/jean-zay/) de Jean Zay ; merci à Parth pour son soutien émotionnel.
