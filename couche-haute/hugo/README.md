---
description: Un générateur de site statique rapide et flexible écrit en Go
---

# HUGO

![](../../.gitbook/assets/hugo-logo-wide.svg)

A Fast and Flexible Static Site Generator built with love by [bep](https://github.com/bep), [spf13](http://spf13.com/) and [friends](https://github.com/gohugoio/hugo/graphs/contributors) in [Go](https://golang.org/).

[Website](https://gohugo.io/) \| [Forum](https://discourse.gohugo.io/) \| [Documentation](https://gohugo.io/getting-started/) \| [Installation Guide](https://gohugo.io/getting-started/installing/) \| [Contribution Guide](https://github.com/gohugoio/hugo/blob/master/CONTRIBUTING.md) \| [Twitter](https://twitter.com/gohugoio)

[![GoDoc](https://camo.githubusercontent.com/573a3afe84e9fb5c11f4583c2ded6a7d69397833965c8cfc85fd73c42f300acb/68747470733a2f2f676f646f632e6f72672f6769746875622e636f6d2f676f6875676f696f2f6875676f3f7374617475732e737667)](https://translate.google.com/website?sl=auto&tl=fr&u=https://godoc.org/github.com/gohugoio/hugo) [![&#xC9;tat de compilation Linux et macOS](https://camo.githubusercontent.com/11a7b3f29a0763583bcbff4c60954cb2ccff38be58eb0af87f10d264e9e5a2ec/68747470733a2f2f6170692e7472617669732d63692e6f72672f676f6875676f696f2f6875676f2e7376673f6272616e63683d6d6173746572266c6162656c3d57696e646f77732b616e642b4c696e75782b616e642b6d61634f532b6275696c64)](https://translate.google.com/website?sl=auto&tl=fr&u=https://travis-ci.org/gohugoio/hugo) [![Go Report Card](https://camo.githubusercontent.com/21eb268546002b418099bd562a34efa2c47645623bfa502efdc5a65c46585319/68747470733a2f2f676f7265706f7274636172642e636f6d2f62616467652f6769746875622e636f6d2f676f6875676f696f2f6875676f)](https://translate.google.com/website?sl=auto&tl=fr&u=https://goreportcard.com/report/github.com/gohugoio/hugo)

### Aperçu

Hugo est un générateur de site Web HTML et CSS statique écrit en [Go](https://translate.google.com/website?sl=auto&tl=fr&u=https://golang.org/) . Il est optimisé pour la vitesse, la facilité d'utilisation et la configurabilité. Hugo prend un répertoire avec du contenu et des modèles et les rend dans un site Web HTML complet.

Hugo s'appuie sur des fichiers Markdown avec une interface pour les métadonnées, et vous pouvez exécuter Hugo à partir de n'importe quel répertoire. Cela fonctionne bien pour les hôtes partagés et autres systèmes sur lesquels vous n'avez pas de compte privilégié.

Hugo rend un site Web typique de taille moyenne en une fraction de seconde. Une bonne règle de base est que chaque élément de contenu est rendu en environ 1 milliseconde.

Hugo est conçu pour fonctionner correctement pour tout type de site Web, y compris les blogs, les tumbles et les documents.

**Architectures prises en charge**

Actuellement, nous fournissons des binaires Hugo pré-construits pour Windows, Linux, FreeBSD, NetBSD, DragonFly BSD, Open BSD, macOS \(Darwin\) et [Android](https://translate.google.com/website?sl=auto&tl=fr&u=https://gist.github.com/bep/a0d8a26cf6b4f8bc992729b8e50b480b) pour les architectures x64, i386 et ARM.

Hugo peut également être compilé à partir des sources partout où la chaîne d'outils du compilateur Go peut s'exécuter, par exemple pour d'autres systèmes d'exploitation, y compris Plan 9 et Solaris.

**Une documentation complète est disponible sur** [**Hugo Documentation**](https://gohugo.io/getting-started/) **.**

### Choisissez comment installer

Si vous souhaitez utiliser Hugo comme générateur de site, installez simplement les binaires Hugo. Les binaires Hugo n'ont pas de dépendances externes.

Pour contribuer au code source ou à la documentation de Hugo, vous devez créer un [fork du projet Hugo GitHub](https://5llmeqnjnqmli35ydv5lwdyeai--github-com.translate.goog/gohugoio/hugo#fork-destination-box) et le cloner sur votre machine locale.

Enfin, vous pouvez installer le code source de Hugo avec `go`, créer les binaires vous-même et exécuter Hugo de cette façon. Construire les binaires est une tâche facile pour un `go`getter expérimenté .

#### Installez Hugo comme générateur de site \(installation binaire\)

Utilisez les [instructions d'installation de la documentation Hugo](https://gohugo.io/getting-started/installing/)



### Contribuer à Hugo

Pour un guide complet sur la contribution à Hugo, consultez le [Guide de contribution](https://5llmeqnjnqmli35ydv5lwdyeai--github-com.translate.goog/gohugoio/hugo/blob/master/CONTRIBUTING.md) .

Nous acceptons les contributions à Hugo de tout type, y compris la documentation, les thèmes, l'organisation, les didacticiels, les articles de blog, les rapports de bogues, les problèmes, les demandes de fonctionnalités, les implémentations de fonctionnalités, les demandes d'extraction, les réponses aux questions sur le forum, l'aide à la gestion des problèmes, etc.

La communauté Hugo et les mainteneurs sont [très actifs](https://5llmeqnjnqmli35ydv5lwdyeai--github-com.translate.goog/gohugoio/hugo/pulse/monthly) et serviables, et le projet profite grandement de cette activité.

#### Poser des questions d'assistance

Nous avons un [forum de discussion](https://translate.google.com/website?sl=auto&tl=fr&u=https://discourse.gohugo.io) actif où les utilisateurs et les développeurs peuvent poser des questions. Veuillez ne pas utiliser l'outil de suivi des problèmes GitHub pour poser des questions.

#### Signaler des problèmes

Si vous pensez avoir trouvé un défaut dans Hugo ou dans sa documentation, utilisez le suivi des problèmes GitHub pour signaler le problème aux responsables de Hugo. Si vous n'êtes pas sûr qu'il s'agisse d'un bogue ou non, commencez par demander dans le [forum de discussion](https://translate.google.com/website?sl=auto&tl=fr&u=https://discourse.gohugo.io) . Lorsque vous signalez le problème, veuillez fournir la version d'Hugo utilisée \( `hugo version`\).

#### Soumettre des correctifs

Le projet Hugo accueille tous les contributeurs et contributions quel que soit leur niveau de compétence ou d'expérience. Si vous souhaitez contribuer au projet, nous vous aiderons avec votre contribution. Hugo est un projet très actif avec de nombreuses contributions quotidiennes.

Nous voulons créer le meilleur produit possible pour nos utilisateurs et la meilleure expérience de contribution pour nos développeurs, nous avons un ensemble de directives qui garantissent que toutes les contributions sont acceptables. Les lignes directrices ne sont pas conçues comme un filtre ou un obstacle à la participation. Si vous n'êtes pas familier avec le processus de contribution, l'équipe Hugo vous aidera et vous apprendra comment apporter votre contribution conformément aux directives.

Pour un guide complet sur la contribution du code à Hugo, consultez le [Guide de contribution](https://5llmeqnjnqmli35ydv5lwdyeai--github-com.translate.goog/gohugoio/hugo/blob/master/CONTRIBUTING.md) .

{% embed url="https://github.com/gohugoio/hugo/blob/master/README.md" %}

Thème que vous pouvez utiliser : [https://github.com/halogenica/beautifulhugo](https://github.com/halogenica/beautifulhugo)

