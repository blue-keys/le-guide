---
description: Ressources en cours d'écriture à titre informatif
---

# Développement web sur windows

Pour nos amies qui sont sur Windows et qu’ils veulent faire du web je vous recommande de supprimer WAMP ! Et d’installer un outil que j’utiliser il y a trois ans : 

Laragon [https://www.grafikart.fr/tutoriels/laragon-1010](https://www.grafikart.fr/tutoriels/laragon-1010) 

Par contre l’extension pour MongoDb est pour PHP... donc à voir. \][https://forum.laragon.org/topic/172/tutorial-how-to-install-mongodb-extension/2](https://forum.laragon.org/topic/172/tutorial-how-to-install-mongodb-extension/2)

Il existe une extension pour PHP pour utiliser MongoDB [https://itanea.fr/apprendre-le-developpement-web/laravel-et-lincontournable-laragon/](https://itanea.fr/apprendre-le-developpement-web/laravel-et-lincontournable-laragon/)



Pour tes pages web tu peux utiliser différentes choses. 

- utiliser Laragon ^^ - avoir un ordinateur physique dédié \(type rack ou serveur ou autre\) que tu va utiliser en tant que serveur chez un herbergeur comme par exemple chez Scalway 

- meme chose mais en machine virtuel cette fois donc pas les mêmes avantages et inconvénient de façon général 

- tu peux aussi le faire avec une Freebox delta utilisant des VM pour ensuite avoir un serveur dispo et un serveur web exposant tes sites depuis l’ip chez toi sur ta box, tu peux même mettre un domaine. 

- utiliser des hébergeurs près à l’emploi avec une interface pour gérer la partie technique de mise en ligne de ton site, la on appel ça de l’hébergement mutualisé, c’est à dire plusieurs personnes utilise le même serveur que toi avec les ressources; le plus souvent pour de l'hebergement WEB avec par exemple du PHP. 

- lorsque tu développe ton site ou un bout de code, sans avoir besoin d’un hébergeur il existe les pages des plateformes tel que gitlab et je crois github a confirmer.  \(En gros tu gére ton code et les versions de celui-ci chez eux et tu peux afficher ta page web static directement depuis eux\) mais pas de chose comme PHP, ou Java ou autre. 

- tu peux utiliser aussi des services en ligne comme codepen qui permette d’afficher ton code source et le résultat. Tu as d’autres choses très chouette aussi avec d’autre comme lui! 

-Tu peux aussi le faire sur un ordinateur ou objet connecté ou NAS moderne qui auto héberge un site chez toi que tu laisse allumé derrière ta box, ou même installé un serveur web sur un ordinateur qui est ou pas installé comme un serveur. Il existe encore plein d’autre façon... En tout cas, lorsque tu veux héberger du code source pour un site et l’utiliser tu vas avoir besoin de ce qu’on appel un serveur web : Apache et Nginx \(et d'autres\) sont deux serveurs web utilisable dans ce contexte la et bien plus tu peux même les additionner pour faire ce qu’on appel un reverse proxy.

