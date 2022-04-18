---
description: Apprendre des technologies pour l'infra et savoir le faire avec qualité
---

# CI/CD d'une infra

:calendar\_spiral: Dernière maj le 14 octobre 2020

Le but de ce TP est de monter une pipeline de test d'infra et de code d'infra (Ansible) maison. Le tp permet de vous apprendre certaine technologie, au menu :

* [git](https://git-scm.com)
  * outil de versioning (SCM)
* [Gitea](https://gitea.io/en-us/)
  * application permettant d'héberger des dépôtss Git
  * léger
  * expose une WebUI
* [Drone](https://drone.io)
  * application permettant de mettre en place des pipelines de test/build
  * s'intègre nativement avec Gitea
* [Ansible](https://www.ansible.com)
  * outil de gestion et déploiement de configuration
  * très utilisé aujourd'hui, c'est l'outil de référence en la matière
* [Molecule](https://molecule.readthedocs.io/en/latest/)
  * outil qui se couple à Ansible pour effectuer des tests sur les playbooks
  * en particulier des tests de conformité
* [Docker](https://www.docker.com)
  * application de conteneurisation
  * permet de créer des environnements légers et autonomes
* [0. Setup environment](broken-reference)
  * [Poste de travail](broken-reference)
  * [Machines virtuelles](broken-reference)
  * [Images Docker](broken-reference)
* [I. Setup environnement Git](broken-reference)
* [II. Mise en place de Drone](broken-reference)
* [III. Ansible](broken-reference)
  * [0. Structure du dépôt Ansible](broken-reference)
  * [1. Création de playbooks](broken-reference)
  * [2. Premiers tests](broken-reference)
  * [3. Molecule](broken-reference)
    * [Présentation](broken-reference)
    * [Prise en main](broken-reference)
    * [Setup dans la pipeline](broken-reference)

## 0. Setup environment

### Poste de travail

Pendant le TP, je vous conseille d'utiliser une machine GNU/Linux comme "poste de travail". Si vous avez un GNU/Linux en dur c'est OK, sinon je vous recommande une VM "poste de travail" (un CentOS peut faire l'affaire).\
En soit aucun pb pour utiliser un autre OS, il faut simplement être à l'aise pour l'utilisation de `git`, `docker`, Python `pip`, et `ansible`, sur votre machine.

### Machines virtuelles

L'OS conseillé pour les VMs en cours est CentOS7. Afin de faciliter et accélérer le déploiement, on va utiliser [Vagrant](https://www.vagrantup.com).

Téléchargez Vagrant pour votre OS, puis initialisez une box `centos/7` :

```bash
$ mkdir workdir
$ cd workdir
# Génération d'un Vagrantfile de base
$ vagrant init centos/7

# Allumer la VM
$ vagrant up

# Récupérer un shell dans la machine
$ vagrant ssh

# Détruire la VM
$ vagrant destroy -f
```

Il peut être bon de repackager la box avec certains éléments préconfigurés, afin de gagner du temps à chaque destroy/up, par exemple :

```bash
# Allumage de la VM
$ vagrant up
# Récupération d'un shell dans la VM
$ vagrant ssh

# Mise à jour des dépôts et du système
$ sudo yum update -y

# Installation de paquets additionels
$ sudo yum install -y vim

# Désactivation de SELinux
$ sudo setenforce 0
$ sudo sed -i 's/SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config

# On quitte la VM
$ exit

# On package la VM sous forme d'un .box
$ vagrant package --output cicd.box

# On ajoute la box à Vagrant
$ vagrant box add cicd.box --name cicd
```

> **Je vous conseille notamment d'installer Docker dans votre box repackagée**, on en aura souvent besoin par la suite.

Une fois repackagée, il est possible d'utiliser la box comme base dans un Vagrantfile :

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "cicd"
end
```

### Images Docker

Pour ce qui est des images Docker, on va beaucoup réutiliser d'images déjà packagées par la communauté. Cela nous permettra d'aller un peu plus vite dans le TP, sans perdre du temps à écrire des Dockerfiles et build des images sur mesure.

## I. Setup environnement Git

**EDIT : vous pouvez utiliser le fichier docker-compose.yml :**

```
version: "3.7"

services:
  gitea:
    image: gitea/gitea:1.10.3
    environment:
      - APP_NAME=Gitea
      - USER_UID=1000
      - USER_GID=1000
      - ROOT_URL=http://gitea:3000
      - SSH_DOMAIN=gitea
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
      - ci

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

volumes:
  gitea: {}
  gitea-db: {}
  drone: {}
  drone-agent: {}

networks:
  ci:
    name: ci
```

****\
**Qui permet de monter facilement Gitea + Drone afin d'accélérer vos tests.**

Pour ce qui est du dépôt git, on va utiliser [Gitea](https://gitea.io/en-us/). C'est une app minimaliste développée en go, qui permet d'héberger des dépôts Git.

Loin d'être fully-featured comme un Gitlab, Gitea opte plutôt pour un aspect modulaire : Gitea lui-même ne se charge que de gérer les dépôts git et leurs accès, il faudra lui greffer d'autres applications afin qu'ils profitent de fonctionnalités supplémentaires.

**TO DO : écrire un Vagrantfile qui monte une machines virtuelle CentOS7**

* la VM va porter Gitea, Drone et run des jobs de build/test
* SELinux doit être désactivé
* 2048M RAM sont conseillés
* pour lancer les services, libres à vous :
  * en dur
  * dans des conteneurs Docker (je vous le conseille, plus rapide et plus simple à faire évoluer pour faire joujou pendant le TP)

**TO DO : mettre en place Gitea**

* installer Gitea en suivant la doc officielle
* je vous conseille un conteneur Docker pour plus de flexibilité

> N'oubliez pas de créer un utser admin lors de la première connexion à la WebUI

**TO DO : Effectuer un push** une fois que Gitea est fonctionnel

* créer un premier dépôt de test
* effectuer un premier push pour valider le bon fonctionnement de la solution

## II. Mise en place de Drone

[Drone](https://drone.io) est un outil léger permettant de mettre en place des pipelines de build et de test. Il se couple nativement très bien avec Gitea.

Comme beaucoup d'outils en son genre, il fonctionne sur un principe de master/runner :

* le master expose la GUI, reçoit les ordres de build et de test, et demandent aux runners d'exécuter des tâches
* le runner a pour charger d'exécuter les tâches demandées par le master : c'est lui qui effectue les builds/tests
  * on va utilise un runner de type Docker
  * c'est à dire que le serveur sera capable de lancer des conteneurs Docker afin d'y effectuer des tests

Dans le cadre du TP, ce sera la même VM qui portera le master et un runner Docker.

**TO DO : configurer Drone pour le lier à Gitea**

> Les credentials sont les mêmes que l'utilisateur admin de Gitea.

**TO DO : tester une première pipeline de test**

* créer un dépôt git dans la WebUI de Gitea&#x20;
* synchroniser le dépôt depuis l'interface de Drone
  * préciser que le dépôt est Trusted (paramètres du dépôt dans la WebUI de Drone)
* cloner le projet
* placer à la racine un fichier `.drone.yml` qui contient :

```yaml
kind: pipeline
name: test-pipeline
type: docker

steps:
  - name: say-hello
    image: alpine
    commands:
      - echo 'HI THERE CICD'
```

* push le fichier
* se rendre sur la WebUI de Drone pour voir le résultat

## III. Ansible

Ansible est un outil de gestion et déploiement de configuration. Il permet de contrôler de façon centralisée et uniformisée un parc de machines. C'est désormais la techno open-source de référence pour ce qui est de la gestion de configuration.

Techno très demandée aujourd'hui, nous allons l'utiliser comme base afin de mettre en place des pipelines de CI/CD.

On ne va pas, dans ce cours, approfondir Ansible. Le but est simplement d'avoir des fichiers de code à tester, des applicatons à tester, build, et intégrer, mais aussi manipuler une technologie extrêmement demandée.

Quelques impératifs pour qu'Ansible fonctionne :

* les fichiers Ansible sont au format `yml`
* à l'intérieur de ces fichiers, on décrit ce qu'il faut installer et configurer sur les serveurs de destination
* la machine qui possède les fichiers `.yml` doit pouvoir se connecter en SSH sur les serveurs de destination
* l'utilisateur sur les serveurs de destination doit pavoir accès à des droits root (_via_ `sudo` par exemple)
  * nécessaires pour beaucoup d'actions comme l'installation de paquets

### 0. Structure du dépôt Ansible

Vous devrez organiser votre dépôt Ansible selon un format standard :

| Directory    | Usage                                                                                                                               |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| `inventory/` | Contient l'inventaire des machines et les variables qui y sont liées                                                                |
| `roles/`     | Contient l'ensemble des "roles" Ansible. Un "role" est un ensemble de tasks ayant un sens (par exemple un rôle "Apache" ou "MySQL") |
| `playbooks/` | La glu entre l'inventaire et les rôles : les playbooks décrivent quel rôle appliquer sur quelle machine                             |

### 1. Création de playbooks

**TO DO : setup Ansible**

* s'assurer que Python est installé sur les machines de destination
* s'assurer qu'il existe un utilisateur pouvant accéder aux droits de root à l'aide de `sudo` sur les machines de destination
* configurer un échange de clés SSH entre le poste de travail (qui héberge les fichiers `yml` Ansible) et les machines de destination

**TO DO : un premier playbook**

* créer un inventaire contenant la machine à qualifier
* créer un rôle qui installe et configure un serveur NGINX
* créer un playbook qui permet de déployer le rôle NGINX sur la machine déclarée dans votre inventaire

Le dépôt Ansible est à déposer dans un endroit personnel (le homedir de votre utilisateur par exemple).

Exemple de structure de dépôt Ansible :

```bash
├── ansible.cfg
├── inventory
│   ├── cicd
│   │   └── hosts.yml
├── playbooks
│   └── test.yml
└─── roles
    └── nginx
        └── tasks
            ├── main.yml
            └── install.yml
```

Structure du fichier `inventory/ci/hosts.yml` :

```yaml
all:
  hosts:
    node1:
      ansible_host: <IP_MACHINE>
  children:
    ci:
      hosts:
        node1:
```

Structure du fichier `playbooks/test.yml` :

```yaml
- name: Test playbook
  hosts: ci
  roles:
    - nginx
```

Structure du fichier `roles/nginx/tasks/main.yml` :

```yaml
- name: install epel-release
  package:
    name: epel-release
    state: present

- name: install NGINX
  package:
    name: nginx
    state: present

- name: setup default website
  copy:
    content: |
               <h1>Hello CI</h1>
    dest: /usr/share/nginx/html/index.html

- name: Copy NGINX startup script
  copy:
    src: start_nginx.sh
    dest: /srv/start_nginx.sh
    owner: root
    group: root
    mode: 700

- name: Starts nginx
  command: 
    cmd: /srv/start_nginx.sh
    creates: /var/run/nginx.pid
```

Contenu de `roles/nginx/files/start_nginx.sh`

```bash
#!/bin/bash

if [[ -f /'var/run/nginx.pid' ]]; then
  echo 'NGINX is already running'
  exit 0
else
  echo 'Starting NGINX'
  nginx
fi
```

**TO DO : tester le bon déploiement du service NGINX et vérifier que le serveur Web est bien fonctionnel**

* **NB** : nous travaillerons essentiellement avec des conteneurs pour les tests. Or il n'existe pas de gestion de services (comme systemd) dans les conteneurs Vous ne pourrez donc pas utiliser quelque chose comme `systemctl start nginx` afin de démarrer NGINX. Le script `start_nginx.sh` est donc utilisé pour lancer le serveur.

### 2. Premiers tests

**TO DO : tester le code ansible**

* créer un dépôt dans Gitea, dédié à hébergé votre code Ansible
* créer un fichier `.drone.yml` qui teste la validité des fichier Ansible
  * le linter [ansible-lint](https://github.com/ansible/ansible-lint) est parfait pour ce use-case
  * il existe [plusieurs images Docker avec ansible-lint prépackagé](https://hub.docker.com/r/yokogawa/ansible-lint)
* pousser le fichier `.drone.yml` pour déclencher une pipeline

**TO DO : ajouter un rôle**

* ajouter un rôle qui déploie des utilisateurs, et le pousser sur le dépôt, afin de tester la pipeline

### 3. Molecule

#### Présentation

[Molecule](https://molecule.readthedocs.io/en/latest/) est un outil qui se couple à Ansible afin d'effectuer des tests d'infra et de conformité.

Le but de cette partie est d'effectuer des tests, à l'aide Molecule, sur le déploiement du rôle Ansible.

Le fonctionnement de Molecule est simple :

* créer un environnement de test (conteneur, VM)
* dérouler un rôle dans l'environnement
* vérifier le bon déroulement du playbook

Par "vérifier le bon déroulement du playbook", on entend : vérifier que le playbook passe, que l'environnement est conforme à nos attentes après déroulement du playbook (est-ce que tel paquet a été bien installé ou tel port firewall a été correctement ouver) ou encore vérifier l'idempotence du playbook (en l'exécutant deux fois d'affilée).

#### Prise en main

Afin de prendre en main Molecule, il peut être bon de tester quelques commandes à la main.

**TO DO** : [Installer Molecule](https://molecule.readthedocs.io/en/latest/installation.html#) (je vous recommande l'installation avec `pip`).

Molecule va nous permettre ici de tester le rôle `nginx` que nous venions d'écrire. Pour que Molecule accepte de tester notre rôle, il est nécessaire d'y ajouter quelques fichiers. Molecule permet de créer un rôle possédant une structure qui correspond aux bonnes pratiques Ansible, afin d'être testé correctement. Pour ce faire :

```bash
# Déplacement dans le répertoire qui contient les rôles:
$ cd roles

# On déplace notre ancien rôle
$ mv nginx nginx.old

# Création du nouveau rôle
$ molecule init role nginx

# Récupération des fichiers précédents
$ mv nginx.old/tasks/main.yml nginx/tasks/main.yml
$ mv nginx.old/files/start_nginx.sh nginx/files/start_nginx.sh

# Suppression de l'ancien module
$ rm nginx.old -r
```

Le rôle est alors prêt à être tester :

```bash
$ cd roles/nginx
$ molecule test
```

#### Setup dans la pipeline

**TO DO : créer une pipeline qui utilise Molecule**

* éditer le `drone.yml`&#x20;
* la pipeline doit utiliser une image Docker qui contient Molecule
  * image officielle : `quay.io/ansible/molecule:3.0.8`
* le test de la pipeline doit exécuter un `molecule test`
  * pour ce faire, vous allez devoir permettre au conteneur molecule de lui-même lancer des conteneurs. On peut le faire en montant le socket Docker `/var/run/docker.sock` dans le conteneur Molecule pour qu'il puisse lancer des conteneurs sur l'hôte

Auteur du TP et intervenant professionnel auprès des écoles du supérieur :

{% embed url="https://gitlab.com/it4lik" %}

Note de coté pour **Léolios :**

```
version: "3.7"

services:
  gitea:
    image: gitea/gitea:1.10.3
    environment:
      - APP_NAME=Gitea
      - USER_UID=1000
      - USER_GID=1000
      - ROOT_URL=http://gitea:3000
      - SSH_DOMAIN=gitea
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
      - ci

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

volumes:
  gitea: {}
  gitea-db: {}
  drone: {}
  drone-agent: {}

networks:
  ci:
    name: ci
```
