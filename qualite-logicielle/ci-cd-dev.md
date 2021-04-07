---
description: Apprendre des technologies pour savoir faire un peu de qualité
---

# CI/CD Dév

🗓 Dernière maj le 14 octobre 2020

Le but de ce TP est de monter une pipeline de test dev maison. Le tp permet de vous apprendre certaine technologie, au menu :

* First day
  * Setup Gitlab
  * Un bout de code ?
  * Une première pipeline
  * Linting de code
* Second day
  * Gitlab : Docker integration
  * Sonarqube

## First day

### Setup Gitlab

* création de compte
* création d'un dépôt de test

### Un bout de code ?

Le mieux pour effectuer ensuite des tests d'intégration et effectuer du déploiement facilement ça serait d'avoir une petite app API REST. Utilisez le langage/framework de votre choix pour coder une API simpliste \(une ou deux entités, avec du GET/POST/DELETE simplement\).

### Une première pipeline

Dans l'interface de Gitlab, sur un dépôt donné, vous trouverez dans le volet de gauche un menu CI/CD, où vous verrez vos pipelines s'exécuter.

Afin de déclencher une pipeline, il faut un fichier `.gitlab-ci.yml` à la racine de votre dépôt git.

Exemple de structure :

```yaml
stages:
  - first_test

first-test:
  stage: first_test
  image: debian
  script:
    - echo 'toto'
```

Tester ce premier fichier `.yml`.

### Linting de code

Créer une nouvelle pipeline qui met en place du linting. Pour ce faire :

* trouver un binaire qui est capable de lint votre code \(par exemple `pylint` pour Python\)
* utiliser une image Docker qui contient ce binaire
* créer une pipeline qui mobilise cette image afin de tester votre code

## Second day

### Gitlab : Docker integration

Il est possible d'utiliser les pipelines de CI/CD pour construire et packager des images Docker. Ces images peuvent ensuite être utilisées à des fins de dév, ou poussées sur une infra de prod.

Gitlab embarque nativement un registre Docker \(un registre permet d'héberger des images Docker, à l'instar du Docker Hub\).

Lors du déroulement d'une pipeline, plusieurs variables d'environnement sont disponibles à l'intérieur du fichier `.gitlab-ci.yml`. On peut par exemple récupérer dynamiquement le nom de la branche qui est en train d'être poussée, l'ID de commit ou encore, l'adresse du registre Docker associé à l'instance Gitlab.

Pour tester tout ça, on va avoir besoin de deux fichiers :

* un Dockerfile
  * décrit comment construire une image Docker
* un `.gitlab-ci.yml` qui fait appel à des commandes `docker` pour construire et pousser les images sur le registre

Exemple de setup simple :

* [Dockerfile](./files/docker-integration/Dockerfile)

```text
FROM alpine

RUN apk update
```

* [.gitlab-ci.yml](./files/docker-integration/.gitlab-ci.yml)

```text
image: docker:18

stages:
  - build
  - push

services:
  - docker:dind

before_script:
  - echo -n $CI_JOB_TOKEN | docker login -u gitlab-ci-token --password-stdin $CI_REGISTRY

Build:
  stage: build
  script:
    - >
      docker build
      --pull
      --build-arg VCS_REF=$CI_COMMIT_SHA
      --build-arg VCS_URL=$CI_PROJECT_URL
      --cache-from $CI_REGISTRY_IMAGE:latest
      --tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
      .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

Tag master as latest:
  variables:
    GIT_STRATEGY: none
  stage: push
  only:
    - master
  script:
    - docker pull $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA $CI_REGISTRY_IMAGE:latest
    - docker push $CI_REGISTRY_IMAGE:latest
```

Testez le build et push automatique de conteneur :

* ajouter ces fichiers à un dépôt Gitlab
* une fois le push effectué et la pipeline terminée, un nouvel onglet est disponible lorsque vous consultez le projet depuis la WebUI `Packages & Registries > Container Registry`

Pour pousser un peu plus, vous pouvez créer un Dockerfile vous-même qui package dans une image Docker votre code et le pousse sur le registry. Si vous avez Docker sur votre machine, vous pourrez ensuite directement récupérer les images avec un `docker pull`.

### Sonarqube

Bon. Pour Sonarqube c'est compliqué d'utiliser l'instance publique gitlab.com et Sonarqube de concert si on a pas accès facilement à une IP publique pour tout le monde.

L'alternative : un `docker-compose.yml` qui monte ce qu'il faut pour reproduire un environnement de travail similaire à Gitlab. Vous pouvez le lancer dans une VM pour avoir cette stack en local et utiliser Sonarqube.

Le `docker-compose.yml` est clairement inspiré du TP infra et embarque :

* Gitea
  * gère des dépôts gits
  * gestion de users/permissions
* Drone
  * Pipeline de CI
* Registre Docker
* Sonarqube
  * Analyse de code

Steps :

* installer Docker et `docker-compose` en suivant la documentation officielle
  * dans une VM Linux si vous pouvez
  * sur votre hôte c'est possible, je vous laisse gérer votre environnement :\)
* récupérer le [docker-compose.yml](./files/sonarqube/docker-compose.yml) et le placer dans un répertoire de travail

```text
# sonarqube : 8080
# gitea : 3000
# drone : 80
version: "3.7"

services:
  gitea:
    image: gitea/gitea:1.10.3
    environment:
      - APP_NAME=Gitea
      - USER_UID=1000
      - USER_GID=1000
      - ROOT_URL=http://ci:3000
      - SSH_DOMAIN=ci
      - SSH_PORT=2222
      - HTTP_PORT=3000
      - DB_TYPE=postgres
      - DB_HOST=gitea-db:5432
      - DB_NAME=gitea
      - DB_USER=ci
      - DB_PASSWD=ci
    restart: always
    volumes:
      - gitea:/data
    ports:
      - "3000:3000"
      - "2222:2222"
    networks:
      ci:
        aliases:
          - "ci"

  gitea-db:
    image: postgres:alpine
    container_name: gitea-db
    restart: always
    volumes:
      - gitea-db:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=gitea
      - POSTGRES_USER=ci
      - POSTGRES_PASSWORD=ci
    networks:
      - ci

  drone-server:
    image: drone/drone:1.2.1
    container_name: drone-server
    ports:
      - 80:80
      - 9000
    volumes:
      - drone:/var/lib/drone/
    restart: always
    depends_on:
      - gitea
    environment:
      - DRONE_RPC_SECRET=9c3921e3e748aff725d2e16ef31fbc42
      - DRONE_OPEN=true
      - DRONE_GITEA=true
      - DRONE_NETWORK=ci
      - DRONE_DEBUG=true
      - DRONE_ADMIN=ci
      - DRONE_USER_CREATE=username:ci,admin:true
      - DRONE_SERVER_PORT=:80
      - DRONE_DATABASE_DRIVER=postgres
      - DRONE_DATABASE_DATASOURCE=postgres://ci:ci@gitea-db:5432/postgres?sslmode=disable
      - DRONE_GIT_ALWAYS_AUTH=false
      - DRONE_GITEA_SERVER=http://gitea:3000
      - DRONE_SERVER_HOST=drone-server:80
      - DRONE_HOST=http://drone-server:80
      - DRONE_SERVER_PROTO=http
      - DRONE_TLS_AUTOCERT=false
      - DRONE_AGENTS_ENABLED=true
    networks:
      - ci

  drone-agent:
    image: drone/agent:1.2.1
    container_name: drone-agent
    command: agent
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - drone-agent:/data
    environment:
      - DRONE_RPC_SERVER=http://drone-server:80
      - DRONE_RPC_SECRET=9c3921e3e748aff725d2e16ef31fbc42
      - DRONE_RUNNER_CAPACITY=1
      - DRONE_RUNNER_NETWORKS=ci
    networks:
      - ci
  sonarqube:
    image: sonarqube
    expose:
      - 9000
    ports:
      - "8080:9000"
    networks:
      - ci
    environment:
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonar
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres
    networks:
      - ci
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
  registry:
    restart: always
    image: 'registry:2'
    ports:
      - "5000:5000"
    environment:
      REGISTRY_AUTH: htpasswd
      REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
      REGISTRY_AUTH_HTPASSWD_REALM: bap
    volumes:
      - registry:/var/lib/registry
      - registry-auth:/auth

volumes:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_bundled-plugins:
  postgresql:
  postgresql_data:
  gitea: {}
  gitea-db: {}
  drone: {}
  drone-agent: {}
  registry:
  registry-auth:

networks:
  ci:
    name: ci
```

* lancer la stack avec un `docker-compose up -d`
* ajouter une ligne à votre fichiers `hosts` pour faire pointer le nom `ci` vers l'IP de la machine qui porte la stack

Les différents WebUI sont dispos sur des ports différents :

* Gitea : `http://ci:3000`
* Drone : `http://ci`
* Sonarqube : `http://ci:8080`

Steps :

* aller sur la WebUI de Gitea
  * onglet `Explore`
  * laisser les paramètres par défaut **SAUF :** n'oubliez pas la création d'un utilisateur admin \(tout en bas\)
  * vous authentifier pour vérifier le bon fonctionnement
  * créer un dépôt et y pousser du code
* aller sur la WebUI de Sonarqube
  * se log avec `admin:admin`
  * aller dans les paramètre du compte `My Account` puis onglet `Security`
  * générer un token et notez le somewhere
* aller sur la WebUI de Drone
  * les credentials sont les mêmes que pour Gitea
  * déclencher une synchro avec Gitea
  * activer le dépôt et configurez-le :
    * cocher "Trusted"
    * définir deux variables :
      * `sonar_host` : `http://sonarqube:9000`
      * `sonar_token` : `<YOUR_TOKEN>`
* ajouter un `.drone.yml` à la racine de votre dépôt
  * [exemple ici](./files/sonarqube/.drone.yml)

```text
ind: pipeline
type: docker
name: code-analysis

steps:
- name: code-analysis
  image: aosapps/drone-sonar-plugin
  settings:
    sonar_host:
      from_secret: sonar_host
    sonar_token:
      from_secret: sonar_token
```

* `git push` tout ça
* vous devriez voir votre code sur Gitea, testé dans Drone, analysé dans Sonarqube

Explorez une peu l'interface de Sonarqube une fois qu'il est en place, beaucoup de métriques intéressantes y remontent.

Drone est quant à lui un outil minimaliste mais puissant. Vous pouvez mettre en place une pipeline similaire à celle de Gitlab sous Drone.

