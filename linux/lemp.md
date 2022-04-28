# Installation LEMP sur Ubuntu 20.04 Digitalocean

Dans ce guide de démarrage rapide, nous allons installer une pile LEMP sur un serveur Ubuntu 20.04.

Pour une version plus détaillée de ce tutoriel, avec plus d'explications sur chaque étape, veuillez vous référer à [Comment installer Linux, Nginx, MySQL, PHP (pile LEMP) sur Ubuntu 20.0](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-20-04)

## Conditions préalables <a href="#conditions-prealables" id="conditions-prealables"></a>

Pour suivre ce guide, vous devrez avoir accès à un serveur Ubuntu 20.04 en tant qu'utilisateur `sudo`.

## Étape 1 - Installation de Nginx <a href="#etape-1-installation-de-nginx" id="etape-1-installation-de-nginx"></a>

Mettez à jour le cache de votre gestionnaire de packages et installez ensuite Nginx avec :

```
sudo apt update
sudo apt install nginx
```

Une fois l'installation terminée, vous devrez ajuster les paramètres de votre pare-feu pour autoriser le trafic HTTP sur votre serveur. Exécutez la commande suivante pour autoriser l'accès externe sur le port `80` (HTTP) :

(_Correction Léolios_ )

```
sudo ufw allow 'Nginx Full'
```

Avec l'ajout de la nouvelle règle de pare-feu, vous pouvez vérifier si le serveur est opérationnel en accédant à l'adresse IP publique ou au nom de domaine de votre serveur depuis votre navigateur web. Vous verrez une page comme celle-ci :

![Page par défaut de Nginx](https://assets.digitalocean.com/articles/lemp\_ubuntu2004/nginx\_default.png)

## Étape 2 – Installer MySQL <a href="#etape-2-installer-mysql" id="etape-2-installer-mysql"></a>

Nous allons maintenant installer MySQL, un système de gestion de base de données populaire utilisé dans les environnements PHP.

Là encore, utilisez `apt` pour acquérir et installer ce logiciel :

```
sudo apt install mysql-server
```

Une fois l'installation terminée, il est recommandé d'exécuter un script de sécurité qui vient préinstallé avec MySQL. Lancez le script interactif en exécutant :

```
sudo mysql_secure_installation
```

Il vous sera demandé si vous souhaitez configurer le `VALIDATE PASSWORD PLUGIN`. Répondez `Y` pour oui, ou tout autre chose pour continuer sans activer. Si vous répondez « oui », il vous sera demandé de choisir un niveau de validation du mot de passe.

Votre serveur vous demandera ensuite de sélectionner et de confirmer un mot de passe pour l'utilisateur **root** de MySQL. Même si la méthode d'authentification par défaut pour l'utilisateur root de MySQL dispense de l'utilisation d'un mot de passe, **même si celui-ci est défini**, vous devez définir ici un mot de passe fort pour plus de sécurité.

Pour le reste des questions, appuyez sur `Y` et appuyez sur la touche `ENTRÉE à chaqu`e invite.

**Note :** Au moment de la rédaction de ce document, la bibliothèque MySQL PHP native `mysqlnd` [ne prend pas en charge](https://www.php.net/manual/en/ref.pdo-mysql.php) `caching_sha2_authentification`,la méthode d'authentification par défaut pour MySQL 8. Pour cette raison, lorsque vous créez des utilisateurs de base de données pour des applications PHP sur MySQL 8, vous devez vous assurer qu'ils sont configurés pour utiliser le mot de passe `mysql_native_password` à la place. Veuillez vous référer à [l'étape 6 de notre guide détaillé LEMP sur Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-20-04#step-6-%E2%80%94-testing-database-connection-from-php-\(optional\)) pour apprendre comment le faire.\


## Étape 3 – Installer PHP <a href="#etape-3-installer-php" id="etape-3-installer-php"></a>

Pour installer les packages `php-fpm` et `php-mysql`, exécutez :

```
sudo apt install php-fpm php-mysql
```

## Étape 4 - Configuration de Nginx pour PHP <a href="#etape-4-configuration-de-nginx-pour-php" id="etape-4-configuration-de-nginx-pour-php"></a>

Dans ce guide, nous allons mettre en place un domaine appelé **your\_domain**, mais vous devez **le remplacer par votre propre nom de domaine**. 

Sur Ubuntu 20.04, Nginx dispose d'un bloc serveur activé par défaut qui est configuré pour servir des documents à partir d'un répertoire à `/var/www/html`. Même si cela fonctionne bien pour un seul site, cela peut devenir difficile à gérer si vous hébergez plusieurs sites. Au lieu de modifier `/var/www/html`, nous allons créer une structure de répertoire au sein de `/var/www` pour le site Web **your\_domain**, en laissant `/var/www/html` en place comme répertoire par défaut à servir si une demande du client ne correspond à aucun autre site.

Créez le répertoire racine Web pour **your\_domain** comme suit :

```
sudo mkdir /var/www/your_domain
```

Ensuite, attribuez la propriété du répertoire avec la variable d'environnement $USER qui fera référence à votre utilisateur actuel du système :

```
sudo chown -R $USER:$USER /var/www/your_domain
```

Ouvrez ensuite un nouveau fichier de configuration dans le répertoire `sites-available` de Nginx en utilisant votre éditeur de ligne de commande préféré. Ici, nous utiliserons `nano` :

```
sudo nano /etc/nginx/sites-available/your_domain
```

Cela créera un nouveau fichier vierge. Collez dans la configuration suivante :

/etc/nginx/sites-available/your\_domain

```
server {
    listen 80;
    server_name your_domain www.your_domain;
    root /var/www/your_domain;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}


```

Une fois que vous avez terminé vos modifications, enregistrez et fermez le fichier. Si vous utilisez `nano`, vous pouvez le faire en appuyant sur `CTRL + X`, puis `y` et `ENTER` pour confirmer.

Activez votre configuration en établissant un lien vers le fichier de configuration à partir du répertoire `sites-enabled` de Nginx :

```
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

Cela indiquera à Nginx d'utiliser la configuration lors du prochain rechargement. Vous pouvez vérifier si votre configuration contient des fautes de syntaxe en tapant :

```
sudo nginx -t
```

Si des erreurs sont signalées, revenez à votre fichier de configuration pour corriger son contenu avant de continuer.

Une fois que vous êtes prêt, rechargez Nginx pour appliquer les modifications :

```
sudo systemctl reload nginx
```

Votre nouveau site web est maintenant actif, mais le root web `/var/www/your_domain` est toujours vide. Créez un fichier `index.html` à cet endroit afin que nous puissions tester si le bloc de serveur fonctionne comme prévu :

```
nano /var/www/your_domain/index.html
```

Incluez le contenu suivant dans ce dossier :

/var/www/your\_domain/index.html

```

  
    <span class="highlight">your_domain</span> website
  
  
    Hello World!

    This is the landing page of your_domain.
  

```

Maintenant, allez dans votre navigateur et accédez au nom de domaine ou à l'adresse IP de votre serveur, comme indiqué dans la directive `server_name` de votre fichier de configuration de bloc de serveur :

```
http://server_domain_or_IP
```

Vous verrez une page comme celle-ci :

![Nginx server block](https://assets.digitalocean.com/articles/lemp\_ubuntu2004/landing\_page.png)

## Étape 5 - Test de PHP avec Nginx <a href="#etape-5-test-de-php-avec-nginx" id="etape-5-test-de-php-avec-nginx"></a>

Nous allons maintenant créer un script de test PHP pour confirmer que Nginx est capable de gérer et de traiter les demandes de fichiers PHP.

Créer un nouveau fichier nommé `info.php` à l'intérieur de votre dossier root web personnalisé : 

```
nano /var/www/your_domain/info.php
```

Cela ouvrira un fichier vierge. Ajoutez le contenu suivant dans le fichier :

/var/www/your\_domain/info.php

```
<?php
phpinfo();
```

`Lorsque vous avez terminé, enregistrez et fermez le fichier.`

`Vous pouvez maintenant accéder à cette page dans votre navigateur Web en consultant le nom de domaine ou l'adresse IP publique que vous avez défini dans votre fichier de configuration Nginx, suivi de /info.php` :

```
http://server_domain_or_IP/info.php
```

Vous verrez apparaître une page Web contenant des informations détaillées sur votre serveur :

![PHPInfo Ubuntu 20.04](https://assets.digitalocean.com/articles/lemp\_ubuntu2004/phpinfo.png)

Après avoir vérifié les informations pertinentes sur votre serveur PHP par le biais de cette page, il est préférable de supprimer le fichier que vous avez créé car il contient des informations sensibles sur votre environnement PHP et votre serveur Ubuntu. Vous pouvez utiliser `rm` pour supprimer ce fichier :

```
sudo rm /var/www/your_domain/info.php
```

## Tutoriels connexes <a href="#tutoriels-connexes" id="tutoriels-connexes"></a>

Voici des liens vers des guides plus détaillés relatifs à ce tutoriel :

* [Configuration initiale du serveur sur Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)
* [Gestion des enregistrements DNS sur DigitalOcean](https://www.digitalocean.com/docs/networking/dns/how-to/manage-records/#a-records)
* [Comment installer Linux, Nginx, MySQL, PHP (pile LEMP) sur Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-20-04)
