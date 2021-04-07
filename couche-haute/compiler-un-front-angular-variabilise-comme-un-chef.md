---
description: Posté le 11/08/2020 par Florent Jaby
---

# 🇬🇧 Compiler un front Angular variabilisé comme un chef

![](https://blog.octo.com/wp-content/uploads/2020/08/angular-on-docker-300x300.png)Si vous vivez dans le présent, voire un peu dans le passé, vous avez sûrement une application de type [SPA](https://blog.octo.com/a-la-decouverte-des-architectures-du-front-3-4-les-single-page-applications/) réalisée avec le framework Angular. Vu que vous vivez dans le présent, vous avez sûrement envie de suivre un processus de développement et de livraison sain, avec promotion d’une même version d’un artefact à travers plusieurs environnements \(test, intégration, recette, préproduction, production, etc.\)

Seulement voilà, Angular, à l’instar de beaucoup d’autres frameworks front-end, vous inflige de construire/compiler votre application une fois par environnement en collant la configuration associée dans un fichier qui porte le nom de l’environnement cible.

Cela pousse en général aux défauts suivants :

* Des secrets ajoutés au dépôt de code dans ces fichiers de configuration
* Un livrable différent par environnement, avec potentiellement des comportements différents
* Aucune injection possible de configuration sans ajouter au code
* Pas possible d’imaginer d’autres environnements sans ajouter au code, par exemple des [_review apps_](https://docs.gitlab.com/ee/ci/review_apps/) pour votre application

> Si vous voulez plus d’informations sur pourquoi ces points sont des défauts, je vous invite à lire les chapitres [3](https://12factor.net/fr/config), [5](https://12factor.net/fr/build-release-run) et [10](https://12factor.net/fr/dev-prod-parity) de [twelve-factor apps](https://12factor.net/fr/) \(_la_ référence en ce qui concerne les applications _Cloud Ready / Native_\).

Nous souhaitons les caractéristiques suivantes pour notre application :

* Constuire un seul artefact pour toutes nos cibles de déploiement
* Injecter la configuration spécifique par les variables d’environnement
* Adapter le comportement de l’application, typiquement la balise [`<base href="...">`](https://angular.io/guide/deployment#the-base-tag) en fonction d’une configuration
* Un déploiement identique à n’importe quel autre type d’application

Sur mon projet, tous nos applicatifs sont packagés avec Docker et deployés soit sur des VMs avec Ansible soit dans un cluster Kubernetes. On a donc cherché à packager notre application Angular de la même manière.

### Utiliser un serveur statique comme image de base <a id="utiliser-un-serveur-statique-comme-image-de-base"></a>

Nous avons choisi [`nginx:alpine`](https://hub.docker.com/_/nginx) comme image de base. Dans notre SI, tous les applicatifs parlent HTTP. Pour faire rentrer notre application web dans le moule, il fallait donc que son image suive les mêmes principes que nos briques d’API par exemple :

* J’écoute sur un `PORT` connu \(ici `80` car la terminaison TLS est faite soit par _ingress_ soit par un _F5_\)
* Je sais sur quel préfixe d’URI je suis exposé via la configuration `BASE_URL` \(par exemple : `https://www.domaine.fr/mon-appli`\)

Voici donc à quoi ressemble notre `Dockerfile` :

```text
# BUILD
FROM node:slim AS build

COPY . /app
WORKDIR /app
RUN npm ci && npx ng build --prod --base-href '\${BASE_URL}'

# RUN
FROM nginx:alpine AS run

COPY ./docker/docker-entrypoint.sh /usr/bin/
COPY ./docker/nginx.front.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/my-app /usr/share/nginx/html

EXPOSE 80
ENV PORT=80
ENV BASE_URL=/
ENV API_BASE_URL # ex: https://api.domaine.fr/mon-api
ENV AUTH_BASE_URL # ex: https://auth.domaine.fr/oidc
ENV AUTH_CLIENT_ID

ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
```

#### Multi Stage Build <a id="multi-stage-build"></a>

Ce `Dockerfile` décrit un [_Multi Stage Build_](https://docs.docker.com/develop/develop-images/multistage-build/). C’est une mécanisme proposé pour utiliser des images de bases différentes pour la phase de construction de l’image à proprement parler et la phase d’execution. On utilise `node:slim` pour construire l’application Angular car nous avons besoin des executables `node`, `npm` et `npx`. Ensuite, nous utilisons l’image de base `nginx:alpine` pour disposer d’un serveur HTTP statique plutôt léger. nous utilisons une configuration spéciale `nginx.front.conf` qui vient de notre base de code. Elle ne fait rien d’intéressant à part le strict minimum, ce qui n’est pas forcément le cas de la configuration de base. Elle sert également à servir le même `index.html` même pour les routes enfants afin que ce soit bien notre application quoi soit chargée à chaque fois et prenne en charge le routage côté navigateur.

Nous pouvons faire ceci car l’output de la commande `ng build` se trouve entièrement dans le dossier `/app/dist/` et ne nécessite aucune execution par le serveur. \(pas besoin non plus de nettoyer les dépendances avec `npm prune --production`\)

La dernière ligne `CMD ["nginx", "-g", "daemon off;"]` quant à elle indique simplement qu’on démarre le serveur _Nginx_ au premier plan avec la configuration que nous lui avons copiée. C’est le dossier `dist/` qui sera servi statiquement pour toutes les requêtes entrantes sur le port 80.

### Personnalisation du `docker-entrypoint` <a id="personnalisation-du-docker-entrypoint"></a>

Je n’ai pas encore parlé de la curieuse ligne `RUN npm ci && npx ng build --prod --base-href '\${BASE_URL}'`. Attention ici, l’objectif n’est pas de faire la substitution de valeur au moment du `docker build` mais bien d’inscrire dans le fichier généré le _placeholder_ `${BASE_URL}`. Il est donc important de respecter l’echappement. L’option `--base-href '\${BASE_URL}'` aura pour conséquence de génerer un `index.html` qui a cette tête là.

```text
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <title>My App</title>
    <base href="${BASE_URL}"> <!-- Notez ici le placeholder -->

    <link rel="icon" type="image/x-icon" href="favicon.ico" />
    <script type="text/javascript" src="assets/config.js"></script>
    <link rel="stylesheet" href="styles.6eceeb0d6ef142ef60a2.css">
  </head>
  <body>
    <app-root></app-root>
    <script src="runtime.c51bd5b1c616d9ffddc1.js" defer></script>
    <script src="polyfills.fb97d1264b2f08af2d29.js" defer></script>
    <script src="main.06f0c73f95c40c9adfb7.js" defer></script>
  </body>
</html>
```

On peut remarquer la ligne `<base href="${BASE_URL}">`. C’est vraiment cette version avec un placeholder qui est stockée dans l’image. En revanche, ce ne sera jamais cette version qui sera servie. En effet, dans notre `Dockerfile` nous avons précisé aussi :

```text
COPY ./docker/docker-entrypoint.sh /usr/bin/
# ...
ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
```

Pour _Docker_, l’`entrypoint` est l’executable qui sera lancée dans le conteneur une fois l’image lancée. Ses arguments seront le contenu de `CMD`. En pratique, nous l’utilisons pour exécuter du code avant de lancer réellement _Nginx_. Voici le contenu de notre `entrypoint`

```text
#!/bin/sh
set -e

INDEX_PATH=/usr/share/nginx/html/index.html
TEMP_INDEX=$(mktemp)

envsubst < ${INDEX_PATH} > ${TEMP_INDEX}
mv ${TEMP_INDEX} ${INDEX_PATH}
chmod a+r ${INDEX_PATH}

exec "$@" # Lancer CMD dans le shell courant
```

`envsubst` est un petit programme, installé sur à peu près toutes les distributions \*nix, qui se charge de remplacer les chaînes de caractères de la forme `${MA_VARIABLE}` par la valeur présente dans l’environnement. On change donc le contenu de notre `index.html` juste avant de lancer _Nginx_ pour modifier cette ligne

```text
    <base href="${BASE_URL}">
```

en cette ligne

```text
    <base href="https://www.domaine.fr/mon-appli/">
```

Pour avoir plus d’informations sur cette commande, tapez `man envsubst` dans votre terminal préféré ![&#x1F609;](https://s.w.org/images/core/emoji/13.0.0/svg/1f609.svg)

> Ici nous bénéficions du fait que notre application n’est exposée que sur un seul préfixe d’URL ce qui n’est pas forcément le cas pour vos applications. Dans un cas pareil, vous pourrez soit déployer 2 fois la même image avec 2 configurations différentes \(complexité dans le déploiement\), ou alors vous orienter vers la solutions de [Server-Side Rendering](https://blog.octo.com/a-la-decouverte-des-architectures-du-front-4-4-les-applications-universelles/) la plus adaptée à votre framework \(complexité dans l’applicatif\)

#### Templatisation de `config.js` <a id="templatisation-de-configjs"></a>

En ce qui concerne la configuration spécifique à notre application \(`API_BASE_URL`, `AUTH_BASE_URL`, _etc._\), nous utilisons un service Angular `ConfigService`

```text
@Injectable()
export class ConfigService implements Configuration {
  baseApiUrl: string
  baseUrl: string
  authBaseUrl: string
  authClientId: string

  constructor () {
    const browserWindow = window || {}
    const browserWindowConfig = browserWindow['config'] || {}
    for (const key in browserWindowConfig) {
      if (browserWindowConfig.hasOwnProperty(key)) {
        this[key] = browserWindowConfig[key]
      }
    }
  }
}
```

Ce service cherche dans l’espace global \(`window`\) un objet nommé `'config'` et en copie toutes les clés. Cet objet est initialisé par notre fichier `assets/config.js` importé depuis notre `index.html`. En revanche, lorsque nous construisons l’image, c’est seulement `template.config.js` que nous incorporons dans notre container. Voici à quoi il ressemble :

```text
(function (window) {
  const config = {
    baseUrl: '${BASE_URL}',
    baseApiUrl: '${API_BASE_URL}',
    authBaseUrl: '${AUTH_BASE_URL}'
    authClientId: '${AUTH_CLIENT_ID}'
  }
  window.config = window.config || config
})(this)
```

Nous pouvons modifier notre `docker-entrypoint.sh` pour utiliser ce template :

```text
#!/bin/sh
set -e

INDEX_PATH=/usr/share/nginx/html/index.html
TEMP_INDEX=$(mktemp)

envsubst < ${INDEX_PATH} > ${TEMP_INDEX}
mv ${TEMP_INDEX} ${INDEX_PATH}
chmod a+r ${INDEX_PATH}

CONFIG_PATH=/usr/share/nginx/html/assets/config.js
CONFIG_TEMPLATE_PATH=/usr/share/nginx/html/assets/template.config.js
if test -f ${CONFIG_TEMPLATE_PATH}; then
  envsubst < ${CONFIG_TEMPLATE_PATH} > ${CONFIG_PATH}
fi

exec "$@" # Lancer CMD dans le shell courant
```

Et zou !

### Reality Check <a id="reality-check"></a>

Maintenant que nous avons fait tout ça, vérifions donc que nos critères sont bien remplis :

* Notre artefact est une image Docker que l’on peut balader sur plusieurs environnements, facilement transmissible.
* Nous pouvons configurer notre application grâce aux variables d’environnements que l’on donne au démarrage de cette image afin d’adapter le comportement de l’application à son déploiement.
* Nous déployons un service HTTP tout simple, il suffit donc de diriger les connexions vers notre _container_.
* Aucune incidence pour travailler localement avec `ng serve`

Cette approche nous permet également de faire plus facilement certaines choses :

* Utiliser un registre docker pour la promotion d’une version de notre application avec les tags
* Faire une configuration aux petits oignons de notre exposition HTTP, notamment en ce qui concerne les stratégies de cache ou des redirections
* Rajouter une protection de type `BasicAuth` à notre application si on veut limiter la population qui y a accès
* Gérer la terminaison TLS directement dans nginx pour un chiffrement en profondeur

Cette manière de packager et livrer nos applications Angular nous a bien aidé, en particulier en rentrant dans le moule de tous nos processus de livraison pour la gestion de la configuration et l’exposition. Je vous recommande d’essayer, en particulier si vous entrez dans une logique de _review apps_ et que vous souhaitez livrer exactement ce que vous avez validé !

J’imagine que cette approche est transposable à d’autres frameworks front. Comment cela se passe-t-il pour vous avec _Vue_, _React_ ou _Ember_ ?

Source : [https://blog.octo.com/compiler-un-front-angular-variabilise-comme-un-chef/](https://blog.octo.com/compiler-un-front-angular-variabilise-comme-un-chef/)

