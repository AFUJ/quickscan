# aeSecure - QuickScan

<!-- cspell:ignore aesecure,quickscan,joomla,uncompress -->

![php 8.0](https://img.shields.io/badge/php-8.0-brightgreen?style=flat)

![banner](./banner.svg)

> Script PHP à installer sur votre site (de préférence en localhost pour de meilleures performances) pour analyser les fichiers à la recherche de virus.

**aeSecure QuickScan RECONNAÎT DÉJÀ PLUS DE 47 750 VIRUS (septembre 2023) et utilise des hachages de liste blanche pour éviter d'analyser les fichiers natifs de [WordPress](https://github.com/cavo789/aesecure_quickscan/tree/master/hashes/wordpress) et [Joomla](https://github.com/cavo789/aesecure_quickscan/tree/master/hashes/joomla).**

> ℹ️ **INSTALLATION**
> Vous avez juste besoin d'obtenir une copie de `aesecure_quickscan.php` et rien d'autre ; veuillez lire le [guide d'installation](#installation).

## Table des matières

- [Démo](#démo)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Licence](#licence)

## Démo

Vous pouvez jouer en ligne avec une démo ici : [https://quickscan.avonture.be/](https://quickscan.avonture.be/)

## Installation

aeSecure QuickScan téléchargera automatiquement les fichiers dont il a besoin, donc la seule chose que vous devez faire est d'obtenir une copie du fichier `aesecure_quickscan.php` et rien d'autre.

1. Cliquez sur le lien suivant pour ouvrir le fichier dans une nouvelle fenêtre : [obtenir une copie du script](https://raw.githubusercontent.com/cavo789/aesecure_quickscan/master/aesecure_quickscan.php)
2. Enregistrez le fichier à la racine de votre site web *(de préférence un site local pour des raisons de performance)*

Remarque : vous n'êtes pas obligé de nommer le fichier aesecure_quickscan, vous pouvez par exemple le nommer `scan.php`.

En principe, vous n'avez pas besoin de récupérer d'autres fichiers, seul le script `aesecure_quickscan.php` est suffisant.

### Ancienne version

Si vous avez besoin d'une version pour PHP 7.x, veuillez télécharger celle-ci : [https://raw.githubusercontent.com/cavo789/aesecure_quickscan/fa76e4c01fc8819c32953ad747e5e81aec228df0/aesecure_quickscan.php](https://raw.githubusercontent.com/cavo789/aesecure_quickscan/fa76e4c01fc8819c32953ad747e5e81aec228df0/aesecure_quickscan.php)

## Utilisation

### Démarrer QuickScan

Donc, dans le dossier racine de votre site web, vous avez le fichier `aesecure_quickscan.php` (ou `scan.php`). Pour l'exécuter, il suffit de démarrer un navigateur et d'accéder au fichier par URL par exemple `http://localhost/mon_site/aesecure_quickscan.php`.

Si vous utilisez Docker, vous pouvez également exécuter l'interface en lançant cette commande : `docker run -d -p 8080:80 -u $(id -u):$(id -g) -v "$PWD":/var/www/html php:8.2-apache` dans le dossier où se trouve votre site web et où vous avez copié le fichier `aesecure_quickscan.php`. Une fois cela fait, il suffit de démarrer l'interface en ouvrant votre navigateur et d'accéder à `http://localhost:8080/aesecure_quickscan.php`, attendez quelques secondes et la page du scanner s'affichera.

### Téléchargements automatiques

#### Lorsque l'interface est affichée

aeSecure QuickScan téléchargera automatiquement trois ou quatre fichiers :

- `aesecure_quickscan_lang_en-GB.json` (peut aussi être fr-FR ou nl-BE) qui est le fichier pour votre langue. La langue préférée sera détectée à partir de la configuration de votre navigateur ;
- `aesecure_quickscan_pattern.json` contient des éléments de configuration pour le scanner ;
- `aesecure_quickscan_supported_cms.json` contient une liste de logiciels CMS reconnus par le scanner.

Si votre site web utilise un CMS supporté comme par exemple `Joomla` et une version supportée, un fichier nommé `aesecure_quickscan_XXXXXX.json` sera téléchargé (où `XXXXXX` est par exemple `J!3.9.0`).

Si quelque chose ne va pas avec le téléchargement automatique (pas de support `CURL` par exemple), vous serez invité à télécharger ces fichiers manuellement.

![Téléchargements automatiques](images/files.png)

#### Lorsque 'Obtention de la liste des fichiers' est lancée

aeSecure QuickScan téléchargera automatiquement trois ou quatre fichiers supplémentaires :

- `aesecure_quickscan_blacklist.json` contient le hachage md5 des virus ;
- `aesecure_quickscan_edited.json` contient le hachage md5 des fichiers où des virus ont été ajoutés ;
- `aesecure_quickscan_other.json` contient le hachage md5 des fichiers considérés comme nettoyés (comme des composants bien connus de Joomla) ;
- `aesecure_quickscan_whitelist.json` contient le hachage md5 des fichiers considérés comme nettoyés (fichiers traités manuellement un par un par Christophe, auteur de QuickScan) ;

![Plus de fichiers JSON](images/files_extended.png)

### Découvrir l'interface

![Interface](images/interface.png)

Quelques points :

- Si le site web utilise un CMS supporté, son nom et son numéro de version seront affichés en haut de l'interface *(ce qui signifie également qu'un fichier .JSON a été téléchargé pour ce CMS et cette version)*,
- Vous avez un menu étendu sur le côté gauche de l'interface. Cliquez sur l'icône du menu hamburger pour l'afficher,
- Quelques déclarations de texte sont affichées (cliquez sur l'icône `x` pour les fermer) et,
- L'interface a principalement quatre boutons d'action :

1. Nettoyer les dossiers cache et temp

Pour améliorer la vitesse de l'analyse, les dossiers `/cache` et `/temp` seront d'abord vidés.
Vous devez cliquer sur ce bouton en premier.

2. Obtenir la liste des fichiers

Avant de commencer l'analyse, QuickScan doit savoir combien de fichiers il doit analyser. L'action `Obtention de la liste des fichiers` récupérera la liste de tous les fichiers de votre site et tous les fichiers de la liste blanche seront ignorés. Un fichier de la liste blanche est un fichier que QuickScan sait être propre. Comment ? Parce que le hachage md5 du fichier est mentionné dans un fichier de liste blanche comme le `aesecure_quickscan_J!3.9.0.json` (ou tout autre fichier de liste blanche).

En d'autres termes : `Obtention de la liste des fichiers` ne récupérera que les fichiers qui doivent être analysés. Dans une installation fraîche de Joomla ou WordPress, vous aurez très peu de fichiers à analyser puisque les fichiers natifs, de base, sont dans la liste blanche. Explication : J'ai généré des hachages pour de nombreuses versions (voir https://github.com/cavo789/aesecure_quickscan/tree/master/hashes/joomla et https://github.com/cavo789/aesecure_quickscan/tree/master/hashes/wordpress). Dès que QuickScan peut récupérer un fichier de hachage pour la version de Joomla/Wordpress que vous utilisez, un hachage sera calculé pour chaque fichier de votre site et si ce hachage est reconnu, cela signifie que votre fichier est sain, c'est-à-dire que son contenu est exactement celui présent dans une installation fraîche de Joomla/Wordpress et ne contient donc aucun virus. Dès qu'un fichier de base a été modifié, même avec un simple caractère d'espace, le hachage sera différent et donc non récupéré dans la liste. En conséquence, le fichier sera analysé même s'il fait partie des fichiers "de base" du CMS. Seuls les fichiers non modifiés seront considérés comme sains et non analysés.

Les fichiers **non modifiés** sont dans la liste blanche (s'ils n'ont pas été modifiés, bien sûr).

Comme vous pouvez le voir ci-dessous, dans une installation fraîche de Joomla 3.9.0, le nombre de fichiers à analyser est : zéro. Cela est dû au fait que rien n'a été ajouté au site et donc que 100% des fichiers sont dans notre liste blanche.

![Rien à analyser](images/nothing_to_scan.png)

3. Analyser le site

Les fichiers restants seront analysés et si quelque chose est trouvé sur la base de

- nos modèles (stockés dans `aesecure_quickscan_pattern.json`),
- notre hachage de liste noire (`aesecure_quickscan_blacklist.json`) ou
- notre hachage modifié (`aesecure_quickscan_edited.json`)

![Virus du mien](images/virus_of_mine.png)

Ensuite, le fichier sera affiché, et vous pourrez le mettre dans la liste blanche (si le fichier est propre (c'est-à-dire un faux positif)), ignorer le fichier (fermer simplement l'élément) ou supprimer le fichier.

Remarque : vous pouvez supprimer le fichier uniquement lorsque vous exécutez en mode expert de QuickScan.

4. Supprimer ce script du serveur

Une fois que vous avez analysé votre site ; n'oubliez pas de supprimer le script `aesecure_quickscan.php` et tous les fichiers JSON associés. Le bouton `Supprimer ce script` le fera pour vous.

### Mode expert

En cliquant sur l'icône du menu hamburger en haut à gauche de l'interface, vous afficherez un menu où, par exemple, vous pourrez activer le mode expert.

Dans ce mode, vous pourrez spécifier un chemin (comme faire une analyse d'un sous-dossier) et vous pourrez supprimer un fichier détecté. Un bouton `Supprimer ce fichier` sera affiché dans les résultats de l'analyse.

Vous aurez une autre option :

![Mode expert](images/expert.png)

## Créer des hachages

Vous pouvez créer des hachages en obtenant une copie du fichier `make_hashes.php` de ce dépôt.

Pour Joomla!, téléchargez simplement la version souhaitée en naviguant sur [https://downloads.joomla.org/cms](https://downloads.joomla.org/cms) et obtenez l'archive souhaitée. Procédez exactement de la même manière pour WordPress.

Si vous avez besoin de plus d'une version, téléchargez simplement toutes les versions requises et enregistrez chaque archive dans le dossier `./hashes/joomla`.

Il est important que le nom de fichier de l'archive soit mis à jour et soit simplement la version. Par exemple, renommez `Joomla_5.0.0-Stable-Full_Package.zip` en `5.0.0.zip`.

Dans l'exemple ci-dessous, j'ai téléchargé Joomla 4.4.0 jusqu'à 5.1.0. Les fichiers zip sont dans mon dossier `./hashes/joomla` et je les décompresse en exécutant la commande suivante dans ma console Linux :

```bash
unzip 4.4.0.zip -d ./4.4.0 && rm -f 4.4.0.zip
unzip 4.4.1.zip -d ./4.4.1 && rm -f 4.4.1.zip
unzip 4.4.2.zip -d ./4.4.2 && rm -f 4.4.2.zip
unzip 4.4.3.zip -d ./4.4.3 && rm -f 4.4.3.zip
unzip 5.0.0.zip -d ./5.0.0 && rm -f 5.0.0.zip
unzip 5.0.1.zip -d ./5.0.1 && rm -f 5.0.1.zip
unzip 5.0.2.zip -d ./5.0.2 && rm -f 5.0.2.zip
unzip 5.0.3.zip -d ./5.0.3 && rm -f 5.0.3.zip
unzip 5.1.0.zip -d ./5.1.0 && rm -f 5.1.0.zip
```

Puisque je suis paresseux, voici la commande Linux à exécuter pour obtenir la liste ci-dessus :

```bash
for f in *.zip ; do var=`find "$f"`; echo "unzip $f -d ${f%.*} && rm -f $f"; done
```

Cela fait, je peux maintenant démarrer mon navigateur et le script `make_hashes.php`.

Si vous êtes un utilisateur de Docker, exécutez simplement `docker run -d -p 8080:80 -u $(id -u):$(id -g) -v "$PWD":/var/www/html php:8.2-apache` dans le dossier où vous avez cloné ce dépôt, puis démarrez votre navigateur et ouvrez `http://localhost:8080/make_hash.php`, attendez quelques secondes et c'est terminé.

Le script commencera immédiatement la création des hachages ; il n'y a rien à faire ; il suffit d'attendre.

Après quelques secondes, vous obtiendrez de nouveaux fichiers JSON (un par version) dans `./hashes/joomla`. Vous pouvez maintenant, éventuellement, supprimer les sous-dossiers ; ils ne sont plus nécessaires.

Si vous avez les permissions d'écriture sur le dépôt [https://github.com/cavo789/aesecure_quickscan](https://github.com/cavo789/aesecure_quickscan), poussez simplement les nouvelles signatures pour les rendre publiquement disponibles.

## Licence

[MIT](LICENSE)