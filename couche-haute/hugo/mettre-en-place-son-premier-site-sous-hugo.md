# Mettre en place son premier site sous Hugo

10 MARS 2018 • [HUGO](https://jamstatic.fr/categories/hugo) • 9 MIN • [FRANK TAILLANDIER](https://frank.taillandier.me)\
\
_Source :_ [_https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#mettre-%C3%A0-jour-un-article_](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#mettre-%C3%A0-jour-un-article)__

Pour créer un nouveau projet avec Hugo, [Forestry](https://forestry.io) propose un kit de démarrage en libre téléchargement. Que vous ayez déjà utilisé le générateur de site statique Hugo ou pas, ce kit est intéressant, car il propose une configuration complète et un workflow de développement moderne basé sur les outils de l’écosystème de `npm`. [Chris Macrae](https://twitter.com/chrisdmacrae) nous montre comment s’en servir pour créer votre premier site en moins de 30 minutes.

[Hugo](https://gohugo.io), le générateur de site statique écrit en Go, a pris la communauté de vitesse. Il présente tous les avantages d’un générateur de site statique — 100% flexible, sécurisé et rapide — mais il vole également la vedette quand on [compare ses performances avec celles de Jekyll](https://forestry.io/blog/hugo-vs-jekyll-benchmark/). Le site de [Forestry.io](https://forestry.io) est d’ailleurs développé avec Hugo.

Nous allons voir comment configurer Hugo sur votre ordinateur, comment installer et personnaliser un thème, en ajoutant nos propres fichiers CSS et JavaScript.

Quelle différence avec le guide de démarrage rapide de la documentation d’Hugo ? Nous allons utiliser [notre kit de démarrage](https://github.com/forestryio/hugo-boilerplate) régulièrement mis à jour qui ajoute un workflow de développement moderne à Hugo.

**Sommaire**

* [1. Configurer Hugo](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#1-configurer-hugo)
* [2. Configurer votre site](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#2-configurer-votre-site)
  * [Mettre à jour un article](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#mettre-%C3%A0-jour-un-article)
  * [Créer un nouvel article](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#cr%C3%A9er-un-nouvel-article)
  * [Utiliser un thème](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#utiliser-un-th%C3%A8me)
* [3. Personnaliser votre site](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#3-personnaliser-votre-site)
* [4. Personnaliser votre thème](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#4-personnaliser-votre-th%C3%A8me)
  * [CSS & Javascript personnalisé](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#css--javascript-personnalis%C3%A9)
* [5. Prochaine étape](https://jamstatic.fr/2018/03/10/mettre-en-place-son-premier-site-sous-hugo/#5-prochaine-%C3%A9tape)

## 1. Configurer Hugo <a href="#1-configurer-hugo" id="1-configurer-hugo"></a>

Pou commencer, clonez ou [téléchargez notre kit de démarrage pour Hugo](https://github.com/forestryio/hugo-boilerplate/archive/master.zip), et décompressez l’archive quelque part sur votre ordinateur. Vous avez aussi besoin de [Node.js](https://nodejs.org) et d’[NPM](https://www.npmjs.com), il vous suffit de suivre les indications sur la [page de téléchargement de Node](https://nodejs.org/fr/download/) si vous ne les avez pas déjà installés.

Vous bénéficiez ainsi automatiquement d’une structure de départ pour Hugo. Dans notre kit, elle est stockée dans le dossier `hugo`. À l’intérieur se trouvent divers dossiers qui abritent le contenu de votre site, les gabarits de page et les fichiers CSS, JS, images, etc. L’arborescence de la structure de base ressemble à ceci — j’ai laissé quelques fichiers et dossiers de côté de façon à ce que ce soit plus clair :

```
.
├── hugo/                  // Le site Hugo, avec les fichiers de contenu, de données, statiques.
|   ├── .forestry/         // rassemble les fichiers de configuration pour Forestry.io
|   ├── content/           // Tout le contenu du site est stocké ici
|   ├── data/              // Les fichiers de données du site au format TOML, YAML ou JSON
|   ├── layouts/           // Vos modèles de page
|   |   ├── partials/      // Les fichiers partiels réutilisables de votre site
|   |   ├── shortcodes/    // Les fichiers shortcodes de votre site
|   ├── static/            // Les fichiers statiques de votre site
|   |   ├── css/           // Les fichiers CSS compilés
|   |   ├── img/           // Les images du site.
|   |   ├── js/            // Les fichiers JS compilés
|   |   └── svg/           // Les fichiers SVG vont ici
|   └── config.toml        // Le fichier de configuration d’Hugo
└─── src/
     ├── css               // Les fichiers source CSS/SCSS à compiler vers /css/
     └── js                // Les fichiers source JS à compiler vers /js/
```

Pour démarrer le projet, ouvrez une fenêtre de terminal et positionnez-vous dans le dossier qui contient la structure de départ (`hugo-boilerplate` par défaut) :

`cd chemin/vers/hugo-boilerplate/`

Installez ensuite toutes les dépendances du projet en lançant :

`npm install`

Pour lancer le serveur de développement et ouvrir le site dans votre navigateur, lancez simplement :

`npm start`

## 2. Configurer votre site <a href="#2-configurer-votre-site" id="2-configurer-votre-site"></a>

Nous allons commencer par ajouter de nouveaux contenus au site. Pour ce faire, nous allons devoir mettre à jour le contenu présent dans le dossier `hugo/content`.

### Mettre à jour un article <a href="#mettre-a-jour-un-article" id="mettre-a-jour-un-article"></a>

Commencer par mettre à jour l’exemple d’article fourni dans notre structure de départ. Ouvrez le fichier `hugo/content/posts/example.md` dans votre éditeur de texte. Il est composé d’un en-tête _front matter_ avec un champ titre et d’un texte d’exemple au format markdown.

e>

Cet article n’a pas de date ! Essayez d’en définir une en ajoutant l’entrée suivante dans l’en-tête _Front Matter_ de l’article:

`date: "YYYY-MM-DDTHH:MM:SS-00:00"`

_Remplacez_ `YYYY-MM-DDTHH:MM:SS-00:00` avec une date valide, comme… `2018-01-01T12:42:00-00:00`. Si votre date se situe dans le futur, Hugo ne générera pas cet article en production.

Sauvegardez vos changements puis affichez l’article mis à jour dans votre navigateur à l’adresse [http://localhost:3000/](http://localhost:3000). La date affichée devant le titre de l’article devrait avoir été mise à jour.

### Créer un nouvel article <a href="#creer-un-nouvel-article" id="creer-un-nouvel-article"></a>

Maintenant essayons de créer un nouvel article. Nous utiliserons pour cela la commande fournie avec Hugo pour générer un nouvel article. Dans notre projet, Hugo est déclaré comme une dépendance NPM, nous pouvons donc l’utiliser avec la commande :

`npm run hugo -- --`

Créez votre premier article en lançant :

`npm run hugo -- new posts/mon-premier-article.md`

Cela va créer un nouvel article au format markdown dans `hugo/content/posts/mon-premier-article.md`. Ouvrez ce fichier dans votre éditeur de texte favori.

```
---
title: "Mon Premier Article"
date: "2018-03-09T14:24:17-04:00"
draft: true
---
```

Ce fichier comporte un en-tête Front Matter (des métadonnées structurées relatives à la page) dont on peut tirer parti dans les gabarits de page. Sous le _front matter_, nous pouvons ajouter du contenu au format markdown :

Ajoutez par exemple le contenu suivant dans le fichier et sauvegardez vos changements :

```
## Bienvenue
Pratique ce modèle de projet *Hugo*. j'espère que vous appréciez ce guide !
```

Vous pouvez voir l’article mis à jour dans votre navigateur à l’adresse [http://localhost:3000/posts/mon-premier-article/](http://localhost:3000/posts/mon-premier-article/).

### Utiliser un thème <a href="#utiliser-un-theme" id="utiliser-un-theme"></a>

Pour le moment votre nouveau site n’est pas très beau. Remédions à cela en ajoutant un thème issu de [la galerie de thèmes de Hugo](https://themes.gohugo.io), développé par un des meilleurs contributeurs de la communauté.![Le thème Casper de @vjeantet](https://res.cloudinary.com/jamstatic/image/upload/f\_auto,q\_auto/v1523346998/up\_running\_w\_hugo\_I\_1.jpg)

Le thème Casper de @vjeantet

Nous allons utiliser le thème [Casper](https://github.com/vjeantet/hugo-theme-casper) de [_@vjeantet_](https://github.com/vjeantet). Pour ce faire nous allons ajouter le thème dans le dossier `hugo/themes`, plus exactement dans le dossier `hugo/themes/hugo-theme-casper/`.

Clonez ou [téléchargez le thème](https://github.com/vjeantet/hugo-theme-casper/archive/master.zip) et décompressez l’archive dans `hugo/themes/hugo-theme-casper/`.

Ensuite, mettez à jour la configuration du site aves les options de configuration spécifiques au thème.

Ouvrez le fichier `hugo/config.toml` dans votre éditeur de texte favori et remplacer son contenu par celui-ci :

```
baseURL= "/"
languageCode= "fr"
title= "Hugo Boilerplate"
paginate = 5
copyright = "Tous droits réservés - 2018"
theme = "hugo-theme-casper"
disableKinds = ["taxonomy", "taxonomyTerm"]

[params]
  description = "Bien démarrrer avec Hugo"
  metadescription = "Utilisé dans la balise meta 'description' pour l’accueil et les pages d’index, faute de quoi c'est l’entrée 'description' du front matter de la page qui sera utilisé."
  cover = ""
  author = "VOTRE_NOM"
  authorlocation = "Terre, Galaxie de la Voie Lactée"
  authorwebsite = ""
  authorbio= ""
  logo = ""
  hjsStyle = "default"
  paginatedsections = ["posts"]
```

Reportez-vous à la [documentation du thème](https://github.com/vjeantet/hugo-theme-casper#configuration) pour prendre connaissance de toutes les options de configuration disponibles.

Pour finir, supprimez les exemples de gabarits de page fournis avec notre modèle de départ. Pour cela lancez la commande :

Regardez maintenant dans votre navigateur et vérifiez que votre site a bien été mis à jour !

## 3. Personnaliser votre site <a href="#3-personnaliser-votre-site" id="3-personnaliser-votre-site"></a>

Maintenant que nous avons mis en place un site fonctionnel avec un thème, vous avez probablement envie de le personnaliser.

Nous allons commencer par éditer les paramètres du site dans le fichier `hugo/config.toml`. Mettez à jour les valeurs suivantes comme bon vous semble :

* `title = "Hugo Boilerplate"`
* `description = "Bien démarrer avec Hugo`
* `metadescription = "Utilisé dans la balise meta 'description' pour l’accueil et les pages d’index, faute de quoi c'est l’entrée 'description' du front matter de la page qui sera utilisé"`
* `author = "VOTRE_NOM"`

![Le thème Casper avec du contenu et les styles par défaut](https://res.cloudinary.com/jamstatic/image/upload/f\_auto,q\_auto/v1523346962/casper-theme-default-config.png)

Le thème Casper avec du contenu et les styles par défaut

Bien, ajoutons maintenant une image de fond pour la bannière d’en-tête. Dans le fichier `hugo/config.toml`, vous trouverez une section `[params]`. Modifiez le paramètre `cover` pour qu’il ait la valeur `/img/darius-soodmand-116253.jpg`, sauvegardez vos changements.![Ajout d’une image de fond](https://res.cloudinary.com/jamstatic/image/upload/f\_auto,q\_auto/v1523347009/casper-theme-cover.jpg)

Ajout d’une image de fond

Retournons maintenant voir notre site dans le navigateur. C’est déjà mieux, mais il y a encore du travail.

## 4. Personnaliser votre thème <a href="#4-personnaliser-votre-theme" id="4-personnaliser-votre-theme"></a>

Maintenant que vous avez adapté le site pour le personnaliser en peu, nous allons nous attarder sur l’aspect le plus puissant d’Hugo et de ce kit de démarrage: **un templating simple et puissant**.

Nous venons d’ajouter le thème Casper au site, ce qui permet à Hugo d’utiliser tous les gabarits HTML présents dans le dossier `hugo/themes/hugo-theme-casper/layouts/` lors de la génération du site.

Nous allons maintenant _étendre_ le thème grâce à **l’héritage de gabarits** d’Hugo.

Tous les fichiers de gabarits présents dans le dossier `hugo/layouts/` surchargeront n’importe quel gabarit du même nom présent dans le répertoire des gabarits du thème, nous permettant ainsi de personnaliser notre site sans toucher au thème d’origine.

### CSS & Javascript personnalisé <a href="#css-javascript-personnalise" id="css-javascript-personnalise"></a>

À côté d’_Hugo_, ce kit de démarrage fourni un serveur de développement qui va post-traiter automatiquement les fichiers CSS et JS pour le navigateur. Tous les fichiers CSS, JS, images présents dans le dossier `src/` seront traités automatiquement et déplacés dans le dossier `hugo/static/`.

Ajoutons-les à notre thème de manière à pouvoir le personnaliser comme nous voulons. Nous allons copier des fichiers de gabarits du thème et ajouter les fichiers CSS et JS personnalisés de notre kit dans ces gabarits.

Premièrement, nous allons copier le fichier partiel _header.html_ du thème dans le dossier `hugo/layouts/partials/` :

```
mkdir -p hugo/layouts/partials/
cp hugo/themes/hugo-theme-casper/layouts/partials/header.html hugo/layouts/partials/header.html
```

Ouvrez le fichier `hugo/layouts/partials/header.html` dans votre éditeur de texte et repérez les lignes suivantes :

```
<link rel="stylesheet" type="text/css" href="{{ "css/screen.css" | relURL}}" />
<link rel="stylesheet" type="text/css" href="{{ "css/nav.css" | relURL}}" />
```

Ajoutez en dessous la ligne suivante :

```
<link rel="stylesheet" type="text/css" href="{{ "css/styles.min.css" | relURL}}" />
```

Ensuite, recopions le fichier partiel `footer.html` dans le dossier `hugo/layouts/partials/` de manière à pouvoir ajouter notre fichier JS personnalisé :

```
cp hugo/themes/hugo-theme-casper/layouts/partials/footer.html hugo/layouts/partials/footer.html
```

Ouvrez maintenant le fichier `hugo/layouts/partials/footer.html` et repérez les lignes suivantes :

```
<script type="text/javascript" src="{{"js/jquery.js" | relURL}}">script>
<script type="text/javascript" src="{{"js/jquery.fitvids.js" | relURL}}">script>
<script type="text/javascript" src="{{"js/index.js" | relURL}}">script>
```

Ajoutez juste en dessous:

```
<script type="text/javascript" src="{{"js/scripts.min.js" | relURL}}">script>
```

Maintenant tout notre code CSS et JS personnalisé sera utilisé sur le site.

Faisons un essai en augmentant la hauteur de l’en-tête principal. Ouvrez le fichier `src/css/styles.css` et ajoutez le code suivant à la fin du fichier :

```
.tag-head.main-header {
  height: 80vh;
}
```

![Le résultat final](https://res.cloudinary.com/jamstatic/image/upload/f\_auto,q\_auto/v1523346973/capser-theme-final.jpg)

Le résultat final

Admirez le résultat final dans votre navigateur !

## 5. Prochaine étape <a href="#5-prochaine-etape" id="5-prochaine-etape"></a>

Vous êtes maintenant prêt·e à commencer à créer un site statique avec Hugo !

Vous pouvez continuer à utiliser le thème Casper ou repartir du début en utilisant les modèles du répertoire `hugo/layouts/`.

Les fichiers des modèles de gabarits de page se trouvent dans le [_dépôt de notre kit de démarrage_](https://github.com/forestryio-templates/hugo-boilerplate/tree/master/hugo/layouts) si vous souhaitez repartir de zéro.

Pour en apprendre un peu plus sur Hugo, reportez-vous aux sections suivantes de la documentation officielle :

* [L’organisation des contenus dans Hugo](https://gohugo.io/content-management/organization/)
* [Introduction au langage de templating d’Hugo](https://gohugo.io/templates/introduction/)
* [Les options de configuration d’Hugo](https://gohugo.io/getting-started/configuration/)

Nous verrons dans un prochain article comment configurer le versionnement avec Git pour faciliter l’intégration continue et le déploiement chez différents hébergeurs avec Forestry, un CMS pour les sites statiques générés avec Hugo ou Jekyll.
