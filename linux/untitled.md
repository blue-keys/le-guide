# Bases de linux

##  Introduction

##  Architecture d'un ordinateur et de linux

A quoi sert un OS?  
L'OS sert d'intermédiaire entre les applications et le hardware, il gère les tâches de bases telles que l'attribution de la mémoire ou le stockage d'information dans le disque, la communication entre les réseaux.

Un OS est composé d'un Noyau \(le kernel\), le système de fichier, l'interpréteur de commandes \(le shell / CLI\), le GUI.

On verra le GUI avant le CLI, c'est plus fun comme ca.

Linux est réputé pour sa sécurité et ses patchs réguliers.  
 Linux est séparé en plusieurs "distributions" ayant chacune des fonctionnalités différentes.

##  Terminal

`user@hostname:chemin$`

*  **user :** Votre username
*  **hostname :** Le nom du serveur
*  **chemin :** le chemin de votre dossier courant, simplifié
*  `$` = simple utilisateur
*  `#` = root

**Quelques commandes de base :**

*  `pwd` = affiche le chemin absolu du répertoire courant
*  `date` = affiche la date et l'heure
*  `uptime` = affiche l'uptime, le nombre d'utilisateur connectés et la charge du serveur
*  `ls` = affiche le contenu du dossier courant \(ou celui donné en arguments\)
*  `sudo` = execute une commande en tant que superutilisateur
*  `su` = switch user, change de compte
*  `cd` = pour se déplacer dans un dossier
*  `clear` = nettoie le terminal
*  `mkdir` = crée un dossier
*  `touch` = crée un fichier vide
*  `rm` = supprime un fichier \(argument `-r` pour un dossier\)
*  `cat` = affiche le contenu d'un fichier
*  `more` et `less` = deux commandes permettant d'afficher et parcourir des fichiers.
*  `find` = permet de chercher dans l'arborescence de fichiers
*  `apt install` = **a executer avec sudo** permet d'installer des paquets \(**yum** sur red hat et **pacman** sur Arch linux\) 
*  `grep` = filtre une sortie d'une commande
*  `which` \(ou `whereis`\) = affiche le chemin absolu d'un fichier

!! UTILISEZ `man <commande>` pour afficher le manuel de la commande ! Cela va lister tous les arguments de la commande et comment s'en servir !  
 Vous pouvez rechercher dans la page de manuel en tapant `/<pattern>` et naviguer entre les résultats avec les touches `N`\(next\) et `P`\(previous\)

Il est possible de manipuler les sorties des commandes et de les passer dans d'autres commandes et fichiers :

*  `>` = Envoie vers un fichier et **remplace le contenu**
*  `>>` = Envoie vers un fichier et **rajoute a la fin**
*  `|` = Envoie vers l'entrée d'une autre commande

**ctrl+C a tout moment pour arreter l'execution d'une commande**

**Pensez a utiliser TAB pour autocompléter une commande**

##  Le système de fichier

Tout est un fichier, même les répertoire, les lecteurs, les sockets réseau et même votre processeur !

Dans `ls` chaque fichier est indiqué par une lettre correspondant au type de fichier. Par exemple:

*  **l** pour un lien symbolique
*  **b** pour un bloc comme un lecteur de dvd ou une partition
*  **d** pour un répértoire etc...

A la racine il y a plusieurs dossiers :

*  **/root** : répertoire personnels de root
*  **/bin** : Une partie des binaires
*  **/boot** : programme d'amorçage de l'OS
*  **/var** : logs entre autres
*  **/home** : répertoire personnels des users
*  **/lib** : librairies
*  **/etc** : configurations des applications
*  **/dev** : fichiers contenant les périphériques
*  **/tmp** : fichiers temporaires

il y a des raccourcis pour naviguer le système de fichiers :

*  `/` = la racine
*  `~` = votre répertoire personnel
*  `.` = le répertoire courant
*  `..` = le répertoire parent

##  La gestion des droits

Root est le super utilisateur, il a tous les droits. Il doit être utilisé le moins possible car les fichier qu'il crée ne peuvent être lus que par lui \(par défaut\) et tous les programmes qu'il lance héritent de ses droits.

O peut modifier les droits d'un fichier avec la commande `chmod`, on peut changer le propriétaire avec `chown` et le groupe avec `chgrp`.

Chaque fichier a des droits, ils sont séparés en 3 catégories:

* Les droits du propriétaire du fichier
* Les droits du groupe du propriétaire
* Les droits de tous les autres

ils se présentent sous la forme d'un groupe de lettres :

*  **R** pour Read
*  **W** pour Write
*  **X** pour eXecuter

Toujours dans l'ordre `<propriétaire><groupe><autres>`.

Par exemple, un fichier ayant les droits `<RWXR-XR-->` permettra au propriétaire de lire, modifier et exécuter le fichier, a son groupe de le lire et l'exécuter et aux autres utilisateurs seulement de le lire.

S'il s'agit d'un dossier, read correspond a en lister le contenu, write corresponds a en ajouter du contenu, et execute corresponds a se déplacer dans le dossier.

Les droits peuvent aussi être notés en **octal**, c'est a dire en **base 8** \(des nombre de 0 a 7\). On le calcule en additionnant les permissions :

* Read vaut 4
* Write vaut 2
* eXecute vaut 1

Par exemple en calculant notre exemple précédent on obtient :

* Le propriétaire peut lire, modifier et exécuter le fichier, donc 1 + 2 + 4 = **7**
* Le groupe peut lire et exécuter le fichier, donc 1 + 4 = **5**
* Les autres peuvent seulement lire, donc 4

Le fichier a donc les droits **754**. On les attribue donc avec la commande `chmod 754 <chemin du fichier>`

il est possible d'ajouter\(+\), enlever\(-\) ou modifier\(=\) des droits plus simplement, pour l'user \(u\), le groupe\(g\) ou les autres\(o\) par exemple : `chmod o+x <fichier>` va ajouter les droits d'execution aux autres, `chmod u+r <fichier>` va ajouter les droits de lectures a l'utilisateur propriétaire du fichier.

