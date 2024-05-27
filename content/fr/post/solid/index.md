+++
author = "Hugo Authors"
title = "Projet SOLID - M1"
date = "2023-01-01"
description = "Site Web utilisant Solid pour stoquer et extraire les données"
tags = [
    "JS",
    "Node Js",
    "npm",
    "CSS",
    "HTML",
    "SOLID",
    "Inrupt",
]
categories = [
    "Web",
    "Solid",
    "Master",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
image = "solid.png"
+++

[Lien GitLab](https://gitlab.com/AlyssaShep/ter-espace-personnel-de-donnees.git)

## Description

Ce projet est un site web permettant l'importation sur Solid et l'exploitation de données personnelles de Google, Instagram ou Facebook.

## Introduction

Ce projet permet d'importer les données que l'on récupère auprès de Google, Instagram ou Facebook sur un pod Solid. Nous avons ensuite plusieurs exploitations possibles à partir de ces données stoquées comme les informations générales de profil, les images, etc.

## Solid

![Solid](solid_logo.png)

La technologie Solid (Social Linked Data) est une initiative créée par Tim Berner
Lee pour le World Wide Web Consortium (W3C). Solid vise à redonner aux utilisateurs
le contrôle total de leurs données personnelles en ligne. Cette technologie permet aussi
de créer des espaces personnels de données, appelés ”Pods”, où les utilisateurs peuvent
stocker, gérer et partager leurs informations de manière sécurisée.

Dans Solid, les données sont stockées sur des modules de données personnels, accessibles via le Web. Les agents ont le contrôle sur l'accès à leurs données sur ces modules. Les modules de données peuvent être connectés tant qu'il y a une connexion Internet et des contrôles d'accès appropriés.

Les agents peuvent choisir librement parmi différents fournisseurs pour stocker leurs données. Actuellement, les principaux fournisseurs sont inrupt.net et solidcommunity.net. 

## Fonctionnalités

Pour pouvoir utliser toutes les fonctionnalités de notre application, il faut d'abord faire la première étape sur la page principale : la connexion.

### Accueil

{{< gallery match="images/page_intro/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Sur la page principale, nous avons plusieurs fonctionnalités :

+ la connexion : L'utilisateur peut choisir de se connecter parmi différents fournisseurs pour stocker leurs données. Nous avons lister les principaux fournisseurs: inrupt.net et solidcommunity.net. Apres son choix, il sera redirigé vers le fournisseur.

+ changer le nom rattaché au Pod 

+ trouver le nom d'un pod à partir du WebID (adresse d'un Pod)

+ Un affichage des dossiers à la racine du pod est affiché

+ Ajouter sur le pod une archive ou un dossier des données du réseau conserné (Google, Facebook ou Instagram)

+ Voir les données ajoutées au Pod en fonction du réseau sélectionné

# image liste deroulante partie connexion

## Interprétations des données

### Calendrier

{{< gallery match="images/calendrier/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Nous avons fait un affichage en calendrier pour les messages Facebook et Instagram ainsi que pour les mails de Google. En effet, nous avons opté de faire ceci car tous les utilisateurs n'activent pas forcément la géolocalisation, rendant l'exploitation de données de positions impossibles. On peut consulter le calendrier sur le mois, semaine, jour ou en liste.

### Profil

{{< gallery match="images/profil/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Nous avons choisi de faire un onglet spécifique à la visualisation des informations personnelles. Il regroupe les noms, emails, date de naissance et autres informations.

### Listes des activités 

{{< gallery match="images/ip/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Les "données de trafic" sont les informations générées lors de l'utilisation de réseaux (Ip, etc), ces données sont présentes dans la récupération fournie par Google. Nous avons donc fait un tableau avons les dates d'activités, l'adresse Ip et ce que l'appareil était en train de faire (Gmail, ect).

### Photos

{{< gallery match="images/photos/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Nous pouvons récupérés les photos sur Google et Instagram. Les métadonnées sont affichées lorsqu'on passe la souris sur l'image en question.

### Messages

{{< gallery match="images/messages/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Nous avons exploité les messages de Facebook et donc refait le fil des différentes discussions en spécifiant le nom de la personne qui parle puit son message.

## Démo 

{{< video src="demo/montage_final.mp4" controls="yes" >}}

## Utilisation du projet

### Installations 

+ nodejs
+ npm

### Lancement

```
git clone https://gitlab.com/AlyssaShep/ter-espace-personnel-de-donnees.git
```

```
npm install
```

```
npm run build && npm run start
```

Ouvrez un navigateur à l'adresse : http://localhost:8080/


## Implémentation

### Interface utilisateur

L'interface utilisateur (UI) est la couche visible de l'application, permettant aux utilisateurs d'interagir. Lors de la conception, nous avons pensé à faire une page de connexion au Pods pour obtenir le token permettant de faire les différentes opérations sur un Pod, sans ça, nous ne pouvons rien faire sur un Pod (récupération, modification, ajout de données...). Par la suite, pendant la phase de développement, nous avons ajouté le formatage du nom du Pod et de faire une recherche de nom (pour savoir si un utilisateur avec ce nom là existe). 

### Accueil

#### Connexion

Tout d'abord, pour pouvoir faire une opération sur notre Pod, il faut se connecter. Pour cette partie, nous utilisons 'solid-client-authn-browser' d'Inrupt. Par la suite, il faut choisir avec quoi on va se connecter : 'solidcommunity.net', 'inrupt.net', '.inrupt.com' ou encore 'solidweb.org'. La sélection se fait à partir d'une liste déroulante.

Ceci est possible grâce au code suivant. Cette fonction est utilisée pour initier le processus de connexion. Si l'utilisateur n'est pas déjà connecté, elle appelle la fonction 'login()' pour effectuer la connexion. Elle spécifie également le nom du client et l'URL de redirection pour le processus de connexion.

Cette fonction est utilisée pour gérer la redirection après le processus de connexion. Elle appelle la fonction 'handleIncomingRedirect()' pour terminer le processus de connexion en récupérant les informations de session. Si l'utilisateur est connecté avec succès, elle met à jour la page avec le statut de connexion et le WebID de l'utilisateur.

#### Modification/recherche du nom

Nous avons mis en place une fonction permettant à l'utilisateur de modifier le nom de son Pod. Bien qu'optionnel, cela permet encore une fois à l'utilisateur un contrôle totale sur ses données. 

De plus, nous avons décider de permettre à l'utilisateur d'afficher le nom de son Pod par son webID.

#### Affichage des dossiers

Nous avons une section qui permet de voir tous les dossiers présents à la racine de 'Your storage' présent dans le Pod. En effet, pour la recherche, on a besoin du WebID du Pod et appliquer la fonction getSolidDataset de '@inrupt/solid-client' pour récupérer le jeu de données Solid associé à l'URL du Pod. Avec ce jeu de données, on utilise la fonction 'getThingAll' pour obtenir tous les objets (ou ressources) contenus dedans. Puis, nous appliquons un filtre pour obtenir que les dossiers. Le filtre s'applique sur les url des objets obtenus (l'url d'un dossier finira par '/').

Puis, nous avons implémentés une fonction qui gère l'affichage de dossier. S'il n'y a pas de dossiers, un message d'informations nous le signalera sinon chaque dossiers présent sur le Pod seront listés de façon a ce que l'utilisateur sache toutes les données qu'il stocke pour le moment.

#### Ajout sur le pod

Une autre section permet a l'utilisateur de prendre un fichier de n'importe quel type, un dossier ou une archive et de le mettre sur le Pod dans le dossier public.

#### Affichage des fichiers dans un dossier Pod 

Enfin, dans la dernière section, nous avons un affichage des fichiers et sous-dossier présent sur un dossier contenant les données personnelles récoltées sur Google, Facebook ou encore Instagram. Le dossier en question doit porter le nom du réseau en question et doit être mis à la racine de 'Your storage'. 

Cette fonction est faite en 2 parties et est récursive. En effet, nous avons la même logique que pour la recherche de dossier mais l'url de recherche sera différent : le dossier du réseau étant dans public nous devons faire une recherche de sa présence dedans (UrlWithPublic). 

Nous récuperons ensuite la liste des Url présents dans ce dernier. Cependant, si on ne faisait qu'un passage sur UrlWithPublic, nous aurions que la liste des objets présents et non TOUS les éléments. Ainsi, Nous récuperons tous les éléments de ces sous-dossiers par la fonction getAllInsideFolderRecursive. Si c'est un fichier, la fonction n'ajoutera que le lien du fichier sinon il bouclera jusqu'à ce qu'il tombe sur un fichier (parcours en profondeur). Comme les dossiers présents dans 'Your storage', une fonction d'affichage est présente. Et permet l'apparition d'un bouton 'Voir les interprétations' et ainsi d'être redirigé vers les interprétations.

### Interprétations des données

La seconde partie de ce projet étant l'interprétation des données, nous avons dans un premier temps dû analyser les données récupérés. 
Par interprétation des données on entend la mise en place d'une interface utilisateur permettant à ce dernier de parcourir l'ensemble de ses données de façon visuelle et interactive. Lorsque nous avons récupéré nos données personnelles sur ces réseaux, nous avons constatés que les formats des fichiers n'étaient pas tous les mêmes. Cela a représenté un grands défi à relever dans ce projet. En effet, pour les données Instagram et Facebook, nous avons exclusivement des fichiers JSON. Cependant, ce n'est pas le cas de Google. On retrouve au sein des données Google des formats de type : JSON, CSV, txt, ics, html, vcf, jpg ... Il y a une multitude de formats possibles, ce en partie à cause du Google Drive. 

Dans un premier temps, nous pensions à convertir l'ensemble de nos fichier en RDF. Cependant, nous avons décidé de ne pas convertir nos données en RDF pour plusieurs raisons. Tout d'abord, à cause du nombres de formats indéterminés sur le Google Drive. De plus, Solid nous permet de déposer des données non RDF. Mais surtout à cause des formats majoritaires qui sont le JSON et le CSV. Sans oublier que chaque prédicat dans les fichiers RDF comportent des URI, ce qui peut très rapidement devenir compliqué à mettre en place. 

{{< gallery match="images/logo/*" sortOrder="asc" rowHeight="150" margins="5" thumbnailResizeOptions="600x600 q90 Lanczos" showExif=false previewType="blur" embedPreview=true loadJQuery=true >}}

Le JSON est un format de données textuelles largement utilisé pour structurer et représenter des données de manière lisible par l'homme. Il est souvent utilisé dans le développement web et les API, car il est facilement interprétable par JavaScript et de nombreux autres langages de programmation. Le JSON prend en charge les structures de données complexes telles que les objets et les tableaux, et il est bien adapté pour représenter des hiérarchies de données avec des propriétés clés-valeurs. 

Le CSV quant à lui est un format de données tabulaire simple où chaque ligne représente un enregistrement et les valeurs sont séparées par des virgules (ou parfois par d'autres délimiteurs). Le CSV est largement utilisé pour échanger des données tabulaires entre différentes applications, notamment les feuilles de calcul, les bases de données et les outils d'analyse de données. Il est généralement plus léger en termes de taille de fichier que le JSON, ce qui peut être un avantage dans certains scénarios où l'espace est limité.

Ainsi, ces deux formats sont parfaits pour l'interprétation de données. 

Ce sont donc les formats de données que nous avons choisis de conserver pour la mise en place de l'interprétation et de l'analyse de nos données. 

#### Calendrier

Pour les données du calendrier, en fonction du réseau que l'on sélectionne, on ira chercher des informations différentes sur le Pod. Pour Google, ce sera les mails. Pour Facebook, les likes et pour Instagram, les commentaires.

Il faut transformer ses données pour le calendrier. On utilise nos fonctions : 

+ "mboxToJson(..)" : tranforme les emails en Json

+ "tranformGoogleMailForCalendar(..)" : prend le fichier Json résultat de la fonction précédente (mboXToJson(..)) et fait un affichage que l'on définit pour notre calendrier

+ "transformCommentDataForCalendar(..)" : transforme les commentaires de Facebook ou Instagram sous format Json en notre affichage personnalisé pour le donner au calendrier

Une fois qu'on a les informations sont transformés, on utlise les fonctions displayFileOnCalendar(..) pour faire l'affichage.

#### Profil

Pour l'affichage des informations des profils, on utilise notre fonction getFileProfil(..). Il faut d'abord recupérer les informations sur le Pod, puis on les traite.

+ Pour instagram, les informations sont fragmentées en 4 fichiers. On récupère aussi la photo de profil sur le Pod.

+ Pour Google, les informations ne sont pas fragmentées mais sont dans 1 seul fichier. La photo de profil est aussi sur un fichier à part.

+ Pour Facebook, les informations sont aussi dans 1 seul fichier.

Une fois que les informations sont préparés, on les affiche sur la page de Profil.

#### Liste d'activité

Notre fonction GetDevices(..) permet de récupérer, pour Google, 2 fichiers :

+ la liste des services utilisés

+ la liste des appareils

Il reste plus qu'à afficher les données en tableau pour plus de clareté.


#### Photos

Pour l'affichage des photos, on doit d'abord sélectionner le réseau que l'on souhaite voir grâce à la liste déroulante. Notre fonction "affichagePhotos(..)" va prendre les données consernées sur le Pod Solid puis va les afficher en ajoutant les images une à une sur la page.


#### Messages

Pour l'affichage des messages, on doit les récupérer du Pod Solid. Les données sont stockés sous le format Json. L'utilisateur peut sélectionner la conversation qu'il souhaite voir avec la liste déroulante. Les noms des conversations sont obtenus grâce à la fonction "getConversationName(..)".
Une fois la conversation sélectionné, on affiche les messages avec
la fonction "getFacebookMessage(..)". On extrait l'auteur, le message et la date du message. Puis on l'affiche avec "displayConversation(..)".
