---
description: >-
  Introduction sur l'un des outils de cyber-sécurité le plus utiliser dans le
  pentest
---

# BurpSuite - Intercepter toutes les requêtes HTTP

## Introduction à BurpSuite

Burp Suite est une suite d’outils développée en Java par la société PortSwigger Ltd. Il existe trois versions de Burpsuite : la version community \(free\), la version professional et la version enterprise \(les deux dernières sont payantes et assez chères\).

Pour ce qui est de l’installation je vous invite à lire la page d’installation du site officiel. Pour lancer l’outil, ouvrez un terminal et entrez cette commande: `burpsuite`

Cette page apparaîtra:

[![1.pgn](https://user-images.githubusercontent.com/38256925/71327305-37d62000-2507-11ea-9312-ac14aa60ef70.png)](https://user-images.githubusercontent.com/38256925/71327305-37d62000-2507-11ea-9312-ac14aa60ef70.png)

Cliquez sur « Next » puis sur « Start Burp ». Vous atterrirez sur la page principale de Burp:

[![pgn2](https://user-images.githubusercontent.com/38256925/71327309-4290b500-2507-11ea-982c-70f7186e3478.png)](https://user-images.githubusercontent.com/38256925/71327309-4290b500-2507-11ea-982c-70f7186e3478.png)

Sur la barre du dessus vous avez les différents outils disponibles. Au cours de cet article nous verrons comment utiliser le proxy, le repeater, le spider, l’intruder et le decoder.

## I/ Proxy

C’est l’outil le plus utilisé de la suite et pour cause, il nous permet d’intercepter toutes les requêtes envoyées depuis notre navigateur, de les modifier et de réceptionner la réponse du serveur. Pour cela il va falloir configurer notre navigateur pour qu’il envoie toutes les requêtes au Burp Proxy. Personnellement je travaille exclusivement sur Firefox donc je vais vous expliquer comment configurer ce navigateur.

Allez dans les «**Paramètres**» puis dans «**Préférences**» &gt; «**Avancés**» et enfin «**Réseau**».

[![pgn3](https://user-images.githubusercontent.com/38256925/71327314-4d4b4a00-2507-11ea-967b-4b48ba11648e.png)](https://user-images.githubusercontent.com/38256925/71327314-4d4b4a00-2507-11ea-967b-4b48ba11648e.png)

Cliquez sur « Paramètres » et complétez le panel de configuration de la façon suivante:

[![pgn.4](https://user-images.githubusercontent.com/38256925/71327317-53d9c180-2507-11ea-8654-a9efeecfdf4f.png)](https://user-images.githubusercontent.com/38256925/71327317-53d9c180-2507-11ea-8654-a9efeecfdf4f.png)

En faisant ça on dit juste à Firefox que nous voulons que toutes les requêtes soient envoyés au proxy qui tourne sur le localhost et qui écoute sur le port **8080**.

**Note** : il existe plusieurs plugins pour Firefox et Chrome qui permettent de switcher la configuration des proxys. Pour ma part j’utilise le plugin Proxy SwitchyOmega disponible [ici.](https://addons.mozilla.org/fr/firefox/addon/switchyomega/)

Retournez sur Burp Suite, allez dans l’onglet « Proxy » et assurez vous que le proxy est configuré pour intercepter les requêtes: [![pgn.5](https://user-images.githubusercontent.com/38256925/71327319-5a683900-2507-11ea-9580-0119552fca1e.png)](https://user-images.githubusercontent.com/38256925/71327319-5a683900-2507-11ea-9580-0119552fca1e.png)

Voilà ! Notre Burp Proxy est configuré. Maintenant si vous lancez une requête sur Firefox vous allez recevoir une notification qui vous dit que la requête à été intercéé…

[![pgn.6](https://user-images.githubusercontent.com/38256925/71327322-618f4700-2507-11ea-845a-89fae0096a48.png)](https://user-images.githubusercontent.com/38256925/71327322-618f4700-2507-11ea-845a-89fae0096a48.png)

Oh wait ! On ne pourra pas se rendre sur des sites en HTTPS à cause des règles HSTS ! Eh oui, nous essayons de contacter le site [http://www.facebook.com](http://www.facebook.com/) mais le certificat que nous recevons est celui de Burpsuite donc notre navigateur bloque la connexion!

Pour régler ce problème on va devoir télécharger le certificat d’autorité de Burp et l’installer dans Firefox. Pour cela rendez vous [ici](http://burp/) en ayant le proxy activé. Puis cliquez sur CA Certificate et téléchargez le certificat.

Maintenant rendez vous dans l’onglet « Avancées » puis « Certificats » dans les paramètres Firefox. Cliquez sur «**Voir les certificats**». Un onglet va s’ouvrir, allez dans «**Autorités**»:

[![pgn.7](https://user-images.githubusercontent.com/38256925/71327326-68b65500-2507-11ea-963e-a499281408cf.png)](https://user-images.githubusercontent.com/38256925/71327326-68b65500-2507-11ea-963e-a499281408cf.png)

Cliquez sur « Importer » puis sélectionnez le certificat que vous avez téléchargé tout à l’heure. Pour finir cochez «**Faire confiance à ce CA pour identifier les sites web**»:

[![pgn.8](https://user-images.githubusercontent.com/38256925/71327328-6fdd6300-2507-11ea-81ad-8268cd549a35.png)](https://user-images.githubusercontent.com/38256925/71327328-6fdd6300-2507-11ea-81ad-8268cd549a35.png)

A partir de là vous ne devriez plus avoir de problèmes. Les requêtes HTTP et HTTPS seront captées par Burp et vous pourrez les modifier.

Problèmes encore, si vous êtes en train de tester les sécurités d’un site en écoutant de la musique sur YouTube vous allez recevoir les requêtes du site que vous testez et celles venant de YouTube \(et YouTube va vous flooooooder au max\). Donc on va voir comment restreindre notre scope à un ou plusieurs sites. Pour cela allez dans l’onglet « **Target** » sur Burp Suite:

[![pgn.9](https://user-images.githubusercontent.com/38256925/71327331-779d0780-2507-11ea-9221-f3617f20c64f.png)](https://user-images.githubusercontent.com/38256925/71327331-779d0780-2507-11ea-9221-f3617f20c64f.png)

Sur la gauche vous avez tous les sites pour lesquels des requêtes ont été réceptionnées par Burp.

**Attention, pour que le site apparaisse il faut que vous vous y soyez rendus avec le proxy actif.**

Si vous voulez que Burp ne se concentre que sur un site il faudra le sélectionner, faire un clic droit et cliquez sur «**Add to scope**». Ensuite retournez dans la section proxy puis « Options » et modifier les règles d’interception comme ceci:

[![pgn.10](https://user-images.githubusercontent.com/38256925/71327332-7cfa5200-2507-11ea-9958-3fe680920578.png)](https://user-images.githubusercontent.com/38256925/71327332-7cfa5200-2507-11ea-9958-3fe680920578.png)

Voilà! Les seuls requêtes que vous recevrez seront celles que vous avez placé dans votre scope.

Une fois que vous avez intercepté une requête, burp vous affichera son contenu:

[![pgn.11](https://user-images.githubusercontent.com/38256925/71327336-85528d00-2507-11ea-9342-302b4561c12c.png)](https://user-images.githubusercontent.com/38256925/71327336-85528d00-2507-11ea-9342-302b4561c12c.png)

Vous pouvez tout modifier. Absolument tout et comment bon vous semble. Ensuite vous n’avez plus qu’à la forwarder \(i.e : l’envoyer au serveur\) et si vous voulez réceptionner la réponse du serveur il vous suffira de cliquer sur «**Action**» puis sur «**Do intercept** &gt; **Response to this request**».

Bon c’est cool, on intercepte, modifie et renvoie des requêtes mais n’y aurait-il pas moyen de rejouer une requête sans avoir à se retaper toutes ces actions ? Eh bien si, la suite burp propose un outil qui permet de rejouer une requête très simplement: Repeater.

## II/ Repeater

Burp Repeater c’est clairement là où vous passerait le plus de temps. Par défaut il n’y a rien dans cet onglet et c'est tout à fait normal puisque c’est à vous de dire quelle requête vous voulez envoyer dans le repeater. Pour cela choisissez une requête dans le proxy, cliquez sur « Action » puis « Send to Repeater » puis rendez vous dans la section Repeater et … Tadaa:

[![pgn.12](https://user-images.githubusercontent.com/38256925/71327343-8e435e80-2507-11ea-83ac-ec6766fbbd76.png)](https://user-images.githubusercontent.com/38256925/71327343-8e435e80-2507-11ea-83ac-ec6766fbbd76.png)

Maintenant vous pouvez modifier votre requête puis l’envoyer. Vous obtiendrez la réponse du serveur automatiquement:

[![pgn.13](https://user-images.githubusercontent.com/38256925/71327345-969b9980-2507-11ea-875e-2f24a26f8bae.png)](https://user-images.githubusercontent.com/38256925/71327345-969b9980-2507-11ea-875e-2f24a26f8bae.png)

Pour ma part je me sers de cet outil quand j’ai déjà repéré une faille et que je chercher à l’exploiter au mieux ou encore lorsque je fuzz des paramètres afin d’analyser le fonctionnement de l’application.

## III/ Spider

Burp Spider est un outil bien sympas puisqu’il permet de mapper un site. C’est en fait un scanner qui va parcourir tous les liens qu’il trouve sur le site web que vous lui avez spécifié.

Pour le lancer c’est très simple, rendez vous dans l’onglet Target, sélectionner le site que vous voulez mapper, clic droit &gt; Spider This Host. Vous trouverez les résultats du scan dans l’onglet Site Map :

[![pgn.14](https://user-images.githubusercontent.com/38256925/71327349-9bf8e400-2507-11ea-97c7-bf386a465941.png)](https://user-images.githubusercontent.com/38256925/71327349-9bf8e400-2507-11ea-97c7-bf386a465941.png)

Ensuite c’est à vous de faire l’analyse des liens. Certains peuvent être intéressants dans le cadre d’une exploitation!

## IV/ Decoder

Le dernier outil que je trouve intéressant c’est le burp decoder. Il permet très rapidement d’encoder/décoder une chaîne de caractères \(on s’en sert beaucoup pour bypass des filtres par exemple\):

[![pgn.15](https://user-images.githubusercontent.com/38256925/71327351-a6b37900-2507-11ea-8ef3-8363bd485348.png)](https://user-images.githubusercontent.com/38256925/71327351-a6b37900-2507-11ea-8ef3-8363bd485348.png)

Je ne vais pas vous expliquer comment il fonctionne, je pense que c’est assez explicite

## BurpSuite-Utilisation

### Comment utiliser Burp Suite - Test de pénétration Web \(Partie 1\)

[Tutoriel original de www.pentestgeek.com](https://www.pentestgeek.com/web-applications/burp-suite-tutorial-1)

[![alt tag](https://user-images.githubusercontent.com/38256925/70809308-d05ef880-1dc1-11ea-9244-3355f0e741d9.jpg)](https://user-images.githubusercontent.com/38256925/70809308-d05ef880-1dc1-11ea-9244-3355f0e741d9.jpg)

[Burp Suite](https://fr.wikipedia.org/wiki/Burp_suite) de [Portswigger](https://portswigger.net/) est l'un de mes outils préférés à utiliser lors de l'exécution d'un test de pénétration Web. Ce qui suit est un tutoriel Burp Suite étape par étape. Je vais vous montrer comment configurer et utiliser correctement de nombreuses fonctionnalités de Burp Suite. Après avoir lu ceci, vous devriez pouvoir effectuer un test de pénétration Web approfondi. Ce sera le premier d'une série d'articles en deux parties.

### Sommaire

* [Comment utiliser Burp Suite - Test de pénétration Web \(Partie 1\)]()
  * [Configuration du proxy SOCKS sortant]()
  * [Configurer le comportement d'interception]()
  * [Procédure d'application]()
  * [Rechercher des mots clés spécifiques]()
  * [Utilisation de Spider et Discover]()
  * [Utilisation de l'onglet Répéteur]()
  * [Utilisation de l'onglet Intruder]()
  * [Utilisation du Scan Automatique]()
* [Comment utiliser Burp Suite - Test de pénétration Web \(Partie 2\)]()
  * [Validation des résultats du scanner]()
  * [Exportation des rapports de scanner]()
  * [Analyse des résultats XML]()
  * [Enregistrement d'une session Burp]()
  * [Burp Extensions]()
* [Fin de la partie 2]()

**Avis de non-responsabilité:** tester des applications Web que vous n'avez pas l'autorisation écrite de tester est illégal et punissable par la loi

### Configuration du proxy SOCKS sortant

Selon l'étendue de votre engagement, il peut être nécessaire de tunneler votre trafic Burp Suite via un proxy SOCKS sortant. Cela garantit que le trafic de test provient de votre environnement de test approuvé. Je préfère utiliser une simple connexion SSH qui fonctionne bien à cet effet. Connectez-vous à votre serveur de test et configurez un proxy SOCKS sur votre hôte local via l'option ‘–D’ comme celle-ci.

```text
ssh –D 9292 –l username servername
```

Accédez à l'onglet Options situé à l'extrême droite du menu supérieur dans Burp Suite. Dans le sous-onglet `Connections`, faites défiler jusqu'à la troisième section intitulée `SOCKS Proxy`. Tapez localhost pour l'option hôte et 9292 pour l'option port.

[![alt tag](https://user-images.githubusercontent.com/38256925/70810526-3a789d00-1dc4-11ea-8ff3-ddb33a571135.png)](https://user-images.githubusercontent.com/38256925/70810526-3a789d00-1dc4-11ea-8ff3-ddb33a571135.png)

Désormais, Burp Suite est configuré pour acheminer le trafic via votre tunnel SSH sortant. Configurez les paramètres proxy de votre navigateur pour utiliser Burp Suite. Accédez à [www.whatismyip.com](https://www.whatismyip.com/) et assurez-vous que votre adresse IP provient de votre environnement de test.

**Conseil:** J'utilise un navigateur séparé pour les tests d'applications Web. Cela garantit que je ne transmets accidentellement aucune donnée personnelle à l'un des sites de mes clients, comme le mot de passe de mon compte gmail par exemple.

Je préfère également utiliser un module complémentaire de commutation de proxy tel que [SwitchySharp](https://addons.mozilla.org/fr/firefox/addon/switchyomega/). Cela me permet de basculer facilement entre les différentes configurations de proxy dont je pourrais avoir besoin lors de différents engagements. Voici à quoi ressemblent mes paramètres de configuration pour Burp Suite.

[![alt tag](https://user-images.githubusercontent.com/38256925/70810546-47958c00-1dc4-11ea-9a85-4e59131b7f6f.png)](https://user-images.githubusercontent.com/38256925/70810546-47958c00-1dc4-11ea-9a85-4e59131b7f6f.png)

### Configurer le comportement d'interception

La prochaine chose que je fais est de configurer la fonction d'interception du proxy. Réglez-le pour ne suspendre que les demandes et réponses en provenance et à destination du site cible. Accédez à l'onglet `Proxy` sous le sous-onglet `Options`. Les deuxième et troisième en-têtes affichent les options configurables pour intercepter les demandes et les réponses. Décochez les paramètres par défaut de Burp Suite et cochez `URL Is in target scope`\(L'URL est dans la portée cible\). Désactivez ensuite l'interception car elle n'est pas nécessaire pour la procédure pas à pas initiale de l'application. Dans le sous-onglet `Intercept`, assurez-vous que le bouton à bascule indique `Intercept is off`

[![alt tag](https://user-images.githubusercontent.com/38256925/70810582-54b27b00-1dc4-11ea-8b5c-6d38a4c7d638.png)](https://user-images.githubusercontent.com/38256925/70810582-54b27b00-1dc4-11ea-8b5c-6d38a4c7d638.png)

### Procédure d'application

Bizarrement , beaucoup de gens aiment sauter cette étape. Je ne le recommande pas. Au cours de la procédure pas à pas initiale de votre application cible, il est important de cliquer manuellement sur autant de sites que possible. Essayez de résister à l'envie de commencer à analyser les choses dans Burp Suite correctement. Au lieu de cela, passez un bon moment et cliquez sur chaque lien et affichez chaque page. Tout comme un utilisateur normal pourrait le faire. Pensez au fonctionnement du site ou à son fonctionnement _supposé_

Vous devriez penser aux questions suivantes:

```text
* Quels types d'actions quelqu'un peut-il faire, tant du point de vue authentifié que non authentifié?
* Des demandes semblent-elles être traitées par un travail côté serveur ou une opération de base de données?
* Y a-t-il des informations affichées que je peux contrôler
```

Si vous tombez sur des formulaires de saisie, assurez-vous de faire quelques tests manuels. Saisie d'une seule coche et appuyez sur soumettre sur n'importe quel formulaire de recherche ou champ de code postal que vous rencontrez. Vous pourriez être surpris de voir à quelle fréquence les failles de sécurité sont découvertes par une exploration curieuse et non par une analyse automatisée.

### Configurez votre portée cible

Maintenant que vous avez une bonne idée du fonctionnement de votre application cible, il est temps de commencer à analyser certains GETs et publications. Cependant, avant de faire des tests avec Burp Suite, il est judicieux de définir correctement votre portée cible. Cela garantira que vous n'envoyez pas de trafic potentiellement malveillant vers des sites Web que vous n'êtes pas autorisé à tester.

**Conseil:** Je suis autorisé à tester [www.pentestgeek.com](https://www.pentestgeek.com/) Tu ne l'es pas.

Rendez-vous sur l'onglet `Target` puis sur le sous-onglet `Site map`. Sélectionnez votre site Web cible dans le volet d'affichage de gauche. Faites un clic droit et choisissez `Add to scope`. Sélectionnez ensuite tous les autres sites dans le volet d'affichage, cliquez avec le bouton droit et sélectionnez Supprimer de la portée. Si vous l'avez fait correctement, votre onglet de portée Burp Suite devrait ressembler à l'image ci-dessous.

[![alt tag](https://user-images.githubusercontent.com/38256925/70810592-5bd98900-1dc4-11ea-9f0e-236762b6eda5.png)](https://user-images.githubusercontent.com/38256925/70810592-5bd98900-1dc4-11ea-9f0e-236762b6eda5.png)

### Vol Initial

Cliquez sur l'onglet `Target` et le sous-onglet `Site Map`. Faites défiler jusqu'à la branche de site appropriée et développez toutes les flèches jusqu'à ce que vous obteniez une image complète de votre site cible. Cela devrait inclure toutes les pages individuelles que vous avez consultées ainsi que tous les fichiers javascript et css. Prenez un moment pour vous imprégner de tout cela, essayez de repérer les fichiers que vous ne reconnaissez pas dans la procédure manuelle. Vous pouvez utiliser Burp Suite pour afficher la réponse de chaque demande dans un certain nombre de formats différents situés sur l'onglet `Resposne` du volet d'affichage en bas à droite. Parcourez chaque réponse en recherchant des données intéressantes. Vous pourriez être surpris de découvrir:

```text
   * Commentaires des développeurs
   * Adresses mail
   * Noms d’utilisateur et mots de passe si vous avez de la chance
   * Divulgation du chemin vers d'autres fichiers / répertoires
   * Etc…
```

### Rechercher des mots clés spécifiques

Vous pouvez également tirer parti de Burp Suite pour faire une grosse partie du travail à votre place. Faites un clic droit sur un nœud, dans le sous-menu `Engagement tools`, sélectionnez `Search`. L'une de mes préférées consiste à rechercher la chaîne `set-cookie`. Cela vous permet de savoir quelles pages sont suffisamment intéressantes pour nécessiter un cookie unique. Les cookies sont couramment utilisés par les développeurs d'applications Web pour différencier les demandes d'utilisateurs de plusieurs sites. Cela garantit que l’utilisateur **A** n’a pas accès aux informations appartenant à l’utilisateur **B**. Pour cette raison, il est judicieux d'identifier ces pages et d'y porter une attention particulière.

[![alt tag](https://user-images.githubusercontent.com/38256925/70810605-609e3d00-1dc4-11ea-8523-f72c6fc4d519.png)](https://user-images.githubusercontent.com/38256925/70810605-609e3d00-1dc4-11ea-8523-f72c6fc4d519.png)

### Utilisation de Spider et Discover

Après un peu de piquer et de pousser manuelle, il est généralement avantageux de permettre à Burp Suite de contrôler l'hôte. Faites un clic droit sur la branche racine de la cible dans le `Target` _\(plan du site\)_ et sélectionnez `Spider this host`.

[![alt tag](https://user-images.githubusercontent.com/38256925/70810615-66941e00-1dc4-11ea-9cc9-e02d90040177.png)](https://user-images.githubusercontent.com/38256925/70810615-66941e00-1dc4-11ea-9cc9-e02d90040177.png)

Une fois `Spider` terminée, revenez à votre plan du site et voyez si vous avez ramassé de nouvelles pages. Si c'est le cas, jetez-y un œil manuel dans votre navigateur et également dans Burp Suite pour voir s'ils produisent quelque chose d'intéressant. Existe-t-il de nouvelles invites de connexion ou des zones de saisie par exemple? Si vous n'êtes toujours pas satisfait de tout ce que vous avez trouvé, vous pouvez essayer le module de découverte de Burp Suite. Cliquez avec le bouton droit sur la branche racine du site cible et dans le sous-menu `Engagement tools`, sélectionnez `Discover Conten`. Sur la plupart des sites, ce module peut fonctionner et fonctionnera longtemps. Il est donc recommandé de le surveiller. Assurez-vous qu'il se termine ou arrêtez-le manuellement avant de l'exécuter trop longtemps.

### Utilisation de l'onglet Répéteur

L'onglet Répéteur est sans doute l'une des fonctionnalités les plus utiles de Burp Suite. Je l'utilise des centaines de fois sur chaque application web que je teste. Il est extrêmement précieux et incroyablement simple à utiliser. Faites un clic droit sur n'importe quelle demande dans l'onglet `Target` ou `Proxy` et sélectionnez `Send to Repeater`. Ensuite, cliquez sur l'onglet `Repeater` et appuyez sur `Go`. Vous verrez quelque chose comme ça:

[![alt tag](https://user-images.githubusercontent.com/38256925/70810628-6bf16880-1dc4-11ea-86a5-113b083ec181.png)](https://user-images.githubusercontent.com/38256925/70810628-6bf16880-1dc4-11ea-86a5-113b083ec181.png)

### Utilisation de l'onglet Intruder

Si vous êtes limité dans le temps et avez trop de demandes et de paramètres individuels pour effectuer un test manuel approfondi. Le Burp Suite Intruder est un moyen vraiment génial et puissant d'effectuer un fuzzing automatisé et semi-ciblé. Vous pouvez l'utiliser contre un ou plusieurs paramètres dans une requête HTTP. Faites un clic droit sur une demande comme nous l'avons fait auparavant et cette fois sélectionnez `Send to Intruder`. Rendez-vous sur l'onglet `Intruder` et cliquez sur le sous-onglet `Positions`. Vous devriez voir quelque chose comme ça:

[![alt tag](https://user-images.githubusercontent.com/38256925/70810640-714eb300-1dc4-11ea-8253-01b08e980c95.png)](https://user-images.githubusercontent.com/38256925/70810640-714eb300-1dc4-11ea-8253-01b08e980c95.png)

Je recommande d'utiliser le bouton `Clear` pour supprimer ce qui est sélectionné au départ. Le comportement par défaut consiste à tout tester avec un signe **=**. Mettez en surbrillance les paramètres que vous ne souhaitez pas utiliser et cliquez sur `Add`. Ensuite, vous devez aller dans le sous-onglet `Payloads` et indiquer à Burp Suite quels cas de test effectuer pendant le fuzzing. Un bon point de départ est `Fuzzing-full`. cela enverra un certain nombre de test de base à chaque paramètre que vous avez mis en évidence dans le sous-onglet `Positions`.

[![alt tag](https://user-images.githubusercontent.com/38256925/70810655-76abfd80-1dc4-11ea-9dfe-102f61f226d0.png)](https://user-images.githubusercontent.com/38256925/70810655-76abfd80-1dc4-11ea-9dfe-102f61f226d0.png)

### Utilisation du Scan Automatique

La dernière chose que je fais lors du test d'une application Web est d'effectuer une analyse automatisée à l'aide de Burp Suite. De retour sur votre sous-onglet `Site Map`, faites un clic droit sur la branche racine de votre site cible et sélectionnez `Passively scan this host`\(Scanner passivement cet hôte\). Cela analysera chaque demande et réponse que vous avez générée pendant votre session Burp Suite. Il produira un conseille de vulnérabilité sur le sous-onglet `Résults` situé sur l'onglet `Scanner`. J'aime faire l'analyse passive en premier car elle n'envoie aucun trafic au serveur cible. Vous pouvez également configurer Burp Suite pour analyser passivement les demandes et les réponses automatiquement dans le sous-onglet `Live scanning` \(Analyse en direct\). Vous pouvez également le faire pour la numérisation active, mais je ne le recommande pas.

Lors d'une analyse active, j'aime utiliser les paramètres suivants:

[![alt tag](https://user-images.githubusercontent.com/38256925/70810666-7c094800-1dc4-11ea-8bf3-c7fa8f3ef5d9.png)](https://user-images.githubusercontent.com/38256925/70810666-7c094800-1dc4-11ea-8bf3-c7fa8f3ef5d9.png)

### Comment utiliser Burp Suite - Test de pénétration Web \(Partie 2\)

[Tutoriel original de www.pentestgeek.com](https://www.pentestgeek.com/web-applications/how-to-use-burp-suite)

[![alt tag](https://user-images.githubusercontent.com/38256925/70846239-b07a1400-1e57-11ea-8b58-e904a456a6a7.jpg)](https://user-images.githubusercontent.com/38256925/70846239-b07a1400-1e57-11ea-8b58-e904a456a6a7.jpg)

Dans la première partie du tutoriel Burp Suite, nous avons présenté certaines des fonctionnalités utiles que Burp Suite a à offrir lors de l'exécution d'un test de pénétration d'application Web. Dans la **partie 2** de cette série, nous continuerons d'explorer comment utiliser Burp Suite, notamment: **la validation des résultats du scanner, l'exportation des rapports du scanner, l'analyse des résultats XML, l'enregistrement d'une session Burp et les extensions Burp**. Allons droit au but!

### Validation des résultats du scanner

C'est toujours une bonne idée de valider en profondeur les résultats de tout outil de numérisation automatisé. Burp Suite fournit tout ce dont vous avez besoin pour ce faire sur l'onglet `Scanner/Results`. Cliquez sur un nœud dans le volet gauche pour voir les vulnérabilités identifiées associées à cette cible. Le volet inférieur droit affiche les informations détaillées de demande/réponse relatives à la vulnérabilité spécifique sélectionnée dans le volet supérieur droit.

L'onglet `Advisory` contient des informations sur la vulnérabilité, y compris un détail de haut niveau, une description et une recommandation proposée. Les onglets `Request` & `Response` afficheront exactement ce que Burp Suite a envoyé à l'application cible afin de vérifier la vulnérabilité ainsi que ce qui a été renvoyé par l'application. Jetez un œil à l'exemple ci-dessous.

[![alt tag](https://user-images.githubusercontent.com/38256925/70846242-bff95d00-1e57-11ea-90f3-cc76b388099d.png)](https://user-images.githubusercontent.com/38256925/70846242-bff95d00-1e57-11ea-90f3-cc76b388099d.png)

L'onglet de demande nous montre quelle page a généré l'alerte.

```text
https://www.pentestgeek.com/wp-content/cache/minify/000000/NYtBDoAgDMA-JFsML5oEYShDYSbwez3goUl7qMV0P76OxU4xmUMl9ZBZlhVdpVHEtCFK3UQO8fxQXzE13Enc2EqfK_wNLKxwkTte.js
```

Juste en demandant cette page dans un navigateur, ou en consultant l'onglet `Réponse`, nous sommes en mesure de valider que l'adresse e-mail prétendument divulguée était bien présente dans la réponse. Nous pouvons considérer cette question comme validée et aller de l'avant.

[![alt tag](https://user-images.githubusercontent.com/38256925/70846247-cf78a600-1e57-11ea-9dfc-88f984e62978.png)](https://user-images.githubusercontent.com/38256925/70846247-cf78a600-1e57-11ea-9dfc-88f984e62978.png)

**Conseil:** Assurez-vous d'effectuer cette étape sur chaque vulnérabilité identifiée par le scanner. Tous les outils d'analyse automatisés produisent des faux positifs en raison de la nature des tests effectués. La plupart des entreprises sont capables d'acheter des outils et de les exécuter sur leurs réseaux. Des pentesters sont embauchés spécifiquement pour identifier et supprimer ces faux positifs.

### Exportation des rapports de scanner

Une fois que vous avez validé les résultats du scanner, vous souhaiterez peut-être générer un certain type de rapport. Il existe deux options de rapport disponibles dans l'onglet `Scanner/Results`, HTML et XML. Pour générer un rapport, cliquez avec le bouton droit sur une cible dans le volet d'affichage de gauche et sélectionnez `Report selected issues`. Cela vous présentera la boîte de dialogue suivante.

[![alt tag](https://user-images.githubusercontent.com/38256925/70846252-d56e8700-1e57-11ea-9094-823078a141d7.png)](https://user-images.githubusercontent.com/38256925/70846252-d56e8700-1e57-11ea-9094-823078a141d7.png)

Cliquez sur l'Assistant et sélectionnez les éléments que vous souhaitez dans votre rapport et quel format. Le rapport HTML peut être ouvert dans un navigateur, puis exporté au format PDF, ce qui peut être utile pour aider à communiquer les résultats à votre client. Le rapport XML vous permet d'analyser des sections spécifiques d'un rapport pour plus de détails. Si vous générez un rapport XML, assurez-vous de décocher l'option d'encodeur Base64 pour voir les Request/Responses HTTP complètes.

### Analyse des résultats XML

J'ai écrit un simple script Ruby pour analyser les données de la sortie XML générée à partir d'un scan automatisé. Le script utilise la gem [Nokogiri](https://nokogiri.org/tutorials/parsing_an_html_xml_document.html) et génère les résultats dans un fichier CSV délimité par colonne qui peut être importé dans Excel pour produire une belle feuille de calcul. Si vous avez une compréhension de base de l'analyse des nœuds XML à l'aide de sélecteurs CSS, vous n'aurez aucun problème à modifier le script pour répondre à vos besoins spécifiques.

```text
def clean_finding(finding)
  output = []
  output << 'Web Application Findings'
  output << ''
  output << finding.css('severity').text
  output << 'Open'
  output << finding.css('host').text
  output << finding.css('path').text
  output << finding.css('issueDetail').text
  output << finding.css('name').text
  output << finding.css('issueBackground').text
  output << finding.css('remediationBackground').text
  response = finding.css('response').text
  if response.include?('Server:')
    output << response.split('Server: ')[1].split("\n")[0]
  end
  output
end
```

Vous pouvez voir qu'en appelant simplement la méthode .css et en passant `[LA VALEUR QUE VOUS VOULEZ].text` en tant que paramètre, vous pourrez retirer tous les éléments spécifiques que vous souhaitez de la soupe XML. Exécutez le script sans argument et vous verrez qu'il prend un fichier XML et crache la sortie à l'écran.

```text
$ ./parse-burp.rb
Parse Burp Suite XML output into Tab delimited results
Example: ./parse-brup.rb > output.csv
```

Vous pouvez filtrer les résultats dans un fichier.csv si vous le souhaitez. Le fichier CSV peut ensuite être importé dans une feuille de calcul Excel qui ressemble à ceci.

[![alttag](https://user-images.githubusercontent.com/38256925/70846256-db646800-1e57-11ea-8500-c409bff1dbcf.png)](https://user-images.githubusercontent.com/38256925/70846256-db646800-1e57-11ea-8500-c409bff1dbcf.png)

### Enregistrement d'une session Burp

Dans certains cas, il peut être nécessaire de suspendre une évaluation et de revenir plus tard. Vous pourriez également vous retrouver à vouloir partager votre session Burp Suite avec un autre consultant. Deux yeux valent souvent mieux qu'un après tout. Dans ces cas, la chose la plus simple à faire est d'enregistrer une copie locale de votre session. Sélectionnez simplement `Save state` dans le menu Burp en haut. Cela va créer un fichier plat que vous ou un autre consultant pouvez importer dans Burp Suite et voir le trafic capturé et les points précis du test. Il s'agit d'une fonctionnalité extrêmement utile.

Si vous avez essayé de le faire dans le passé et que vous avez remarqué que la taille du fichier résultant était inutilement grande \(des centaines de Mo\). Il est possible que vous ayez oublié de cocher la case `Save in-scope items only`

[![alt tag](https://user-images.githubusercontent.com/38256925/70846259-e3240c80-1e57-11ea-9c25-484d3cc7202e.png)](https://user-images.githubusercontent.com/38256925/70846259-e3240c80-1e57-11ea-9c25-484d3cc7202e.png)

Si vous configurez votre portée en suivant les instructions de la partie 1, vous ne devriez pas avoir à vous soucier d'un fichier d'état massif. La page suivante de l'Assistant vous demande de quels outils vous souhaitez stocker la configuration. J'ai trouvé que les avoir toutes cochées ou toutes non cochées ne semble pas affecter la taille du fichier, voire pas du tout, mais n'hésitez pas à jouer avec ces options et à vous faire votre propre opinion.

[![alt tag](https://user-images.githubusercontent.com/38256925/70846262-e919ed80-1e57-11ea-9d67-35d38ee5d191.png)](https://user-images.githubusercontent.com/38256925/70846262-e919ed80-1e57-11ea-9d67-35d38ee5d191.png)

Pour restaurer un état de burp précédemment enregistré, sélectionnez simplement `Restore state` dans le menu Burp en haut. Sélectionnez le fichier dans votre système, cliquez `Open`et suivez les instructions de l'assistant. Selon la taille du fichier d'état, il peut prendre un moment pour tout importer, mais une fois terminé, vous pouvez continuer votre évaluation ou celle de quelqu'un d'autre pour ce sujet comme si vous n'aviez jamais fait de pause en premier lieu. C'est vraiment cool!

### Burp Extensions

Les extensions Burp sont des ajouts après-vente écrits par d'autres pentesters qui peuvent être facilement installés et configurés pour ajouter des fonctionnalités améliorées ou supplémentaires à Burp Suite. Pour illustrer ce processus, nous allons télécharger et installer le `Plugin Shellshock Burp` depuis la page [Accuvant LABS Github](https://github.com/AccuvantLABS). Accédez à l'URL suivante [https://github.com/AccuvantLABS/burp-shellshock](https://github.com/AccuvantLABS/burp-shellshock) et cliquez sur le lien [Download here!](https://github.com/AccuvantLabs-Appsec/burpshellshock/releases/download/1.0/burpshellshock-1.0-SNAPSHOT-jar-with-dependencies.jar)

[![alt tag](https://user-images.githubusercontent.com/38256925/70846271-f0d99200-1e57-11ea-908a-8b4afb42f519.png)](https://user-images.githubusercontent.com/38256925/70846271-f0d99200-1e57-11ea-908a-8b4afb42f519.png)

Cliquez ensuite sur l'onglet `Extender` dans Burp Suite et cliquez sur le bouton `Add` dans le coin supérieur gauche. Lorsque la boîte de dialogue apparaît, sélectionnez le fichier .jar Shell Shock que vous venez de télécharger et cliquez sur `Next`.

[![alt tag](https://user-images.githubusercontent.com/38256925/70846274-f800a000-1e57-11ea-963c-2eabc5b62120.png)](https://user-images.githubusercontent.com/38256925/70846274-f800a000-1e57-11ea-963c-2eabc5b62120.png)

Si tout s'est bien passé, vous devriez voir un message indiquant **The extension loaded successfully** sans message d'erreur ni sortie. L'onglet Extensions montre maintenant que notre extension `Shellshock Scanner` est chargée. Nous pouvons voir dans la section Détails qu'une nouvelle vérification du scanner a été ajoutée.

[![alt tag](https://user-images.githubusercontent.com/38256925/70846276-fd5dea80-1e57-11ea-89ca-56c21935d979.png)](https://user-images.githubusercontent.com/38256925/70846276-fd5dea80-1e57-11ea-89ca-56c21935d979.png)

### Fin de la partie 2

J'espère que ce tutoriel vous a été utile. Après avoir lu les deux articles de cette série, vous devez vous familiariser avec la plupart des fonctionnalités essentielles offertes dans la suite Burp. Veuillez profiter de la section `Post Comment` pour laisser vos [commentaires/questions](https://www.pentestgeek.com/web-applications/how-to-use-burp-suite). Merci d'avoir lu!

Source may 2020 : [https://github.com/TeePee/BurpSuite-Utilisation](https://github.com/TeePee/BurpSuite-Utilisation)

Source 3 mai 2020 : [https://github.com/TeePee/BurpSuite-Utilisation](https://github.com/TeePee/BurpSuite-Utilisation)

