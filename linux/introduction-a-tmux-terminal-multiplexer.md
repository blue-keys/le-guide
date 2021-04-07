---
description: Pouvoir exécuter des terminaux en ssh en session et multi fenêtres.
---

# Introduction à tmux \(terminal multiplexer\)

## Qu’est-ce que tmux?

Tmux est un multiplexeur de terminaux, cela permet de créer des sessions dans lesquels vous pouvez créer un ou plusieurs terminal virtuel.

L’interet de tmux est le fait qu’une session peut être détaché mais quelle continue à exister et à fonctionner en arrière plan et que l’on puisse s’y rattacher plus tard.

Une autre fonction pratique est le fait que plusieurs utilisateurs puissent se connecter simultanément à une même session et pouvoir voir et effectuer des actions sur les mêmes terminaux.

![Tmux avec trois terminaux splitt&#xE9;s](http://denisrosenkranz.com/wp-content/uploads/2012/09/Tmux1-1024x562.png)

Tmux avec trois terminaux splittés

## Installation de tmux sur une distribution à base de Debian

```text
apt-get install tmux
```

## Utilisation de tmux

Dans un terminal tapez « tmux » pour lancer une session tmux

La gestion de tmux se fait via des raccourcis clavier qui commencent tous par **Control+B**, voici la liste des commandes utiles sous tmux.

### Commandes de bases \(tapez Control + b avant\)

**c** : Créer un nouveau terminal dans la session tmux active  
 **n** : Switcher entre les différents terminaux de la session  
 **X** : Choisir un terminal spécifique \(ou X est le numéro du terminal\)  
 **d** : Se détacher de la session tmux  
 **,** : Permet de renommer un terminal  
 **w** : Affiche la liste des terminaux disponibles  
 **t** : Afficher l’heure dans un terminal

### Commandes dans un Split \(tapez Control + b avant\)

 **»** : Split vertical du terminal courant en deux + ouverture d’un terminal dans le nouveau panel  
 **%** : Split horizontal du terminal courant en deux + ouverture d’un terminal dans le nouveau panel  
 **o** : Switcher entre les terminaux splittés  
 **espace** : Changer l’organisation visuelle des terminaux splittés  
 **Alt + \(flèches directionnelles\)** : Reduire, agrandir fenêtre du split  
 **!** : Convertir un split en terminal seul  
 **q** : Afficher les numéros des terminaux splittés  
 **:join** : permet de joindre un terminal seul sans un split  
 **Exemple pour rajouter le terminal numéro 3 verticalement et pour qu’il prenne 50% de l’espace total:  
 :joinp -v -s 3.0 -p 50**  
 **-h ou -v** : horizontalement ou verticalement  
 **-s 0.0** : terminal 0 et volet 0 \(volet si écran splitté\)  
 **-p 50** : occupation à 50% de la fenêtre

### Commandes à taper dans un terminal classique

**tmux** : Créer une session  
 **tmux attach** : Se rattacher à la dernière session utilisé  
 **tmux ls** : Voir la liste des sessions tmux active  
 **tmux attach -t X** : S’attacher à une sessions tmux ou X est le numéro de la session

Voici par exemple une connection SSH à une session tmux \[0\] sur un serveur Asterisk avec d’un coté un terminal avec la console Asterisk\(0:Console Asterisk\*\) et de l’autre un terminal pour éditer les fichiers de configurations\(1:Fichiers de conf-\)  
 Le \* montre la console active.

![Connection SSH &#xE0; une session tmux](http://denisrosenkranz.com/wp-content/uploads/2012/09/Tmux.png)

Connection SSH à une session tmux

Je vous conseil fortement ce logiciel très pratique !

N’hésitez pas à taper **man tmux** pour découvrir toutes les autres fonctionnalités de tmux !

#### Denis

Etudiant en informatique, se passionne pour Linux et des technologies Open Sources.

Source : [http://denisrosenkranz.com/tuto-introduction-a-tmux-terminal-multiplexer/](http://denisrosenkranz.com/tuto-introduction-a-tmux-terminal-multiplexer/)

