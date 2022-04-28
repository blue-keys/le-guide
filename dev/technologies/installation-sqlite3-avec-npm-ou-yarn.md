---
description: Petit tutoriel suite à des erreurs d'installation sur Windows 10
---

# Installation Sqlite3 avec NPM ou Yarn

![](../../.gitbook/assets/SQLite370.svg.png)

Le but de cet article est de résoudre les erreurs rencontrés sur Windows 10 à l'installation de Sqlite3 le 10 janvier 2021

Contrairement aux serveurs de bases de données traditionnels, comme MySQL ou PostgreSQL, sa particularité est de ne pas reproduire le schéma habituel client-serveur mais d'être directement intégrée aux programmes. L'intégralité de la base de données (déclarations, tables, index et données) est stockée dans un fichier indépendant de la plateforme.

**SQLite** est le moteur de base de données le plus distribué au monde, grâce à son utilisation dans de nombreux logiciels grand public comme Firefox, Skype, Google Gears, dans certains produits d'Apple, d'Adobe et de McAfee et dans les bibliothèques standards de nombreux langages comme PHP ou Python. De par son extrême légèreté (moins de 300 Kio), il est également très populaire sur les systèmes embarqués, notamment sur la plupart des smartphones modernes : l'iPhone ainsi que les systèmes d'exploitation mobiles Symbian et Android l'utilisent comme base de données embarquée.

## **Sur Ubuntu 20.04 LTS**

Aucun soucis ( 9 secondes ).

```bash
user@Leolios:~$ cd git/
user@Leolios:~/git$ mkdir monbot
user@Leolios:~/git$ cd monbot/
user@Leolios:~/git$ npm init
user@Leolios:~/git/monbot$ npm install sqlite3
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated har-validator@5.1.5: this library is no longer supported

> sqlite3@5.0.1 install /home/user/git/monbot/node_modules/sqlite3
> node-pre-gyp install --fallback-to-build

node-pre-gyp WARN Using request for node-pre-gyp https download 
[sqlite3] Success: "/home/user/git/monbot/node_modules/sqlite3/lib/binding/napi-v3-linux-x64/node_sqlite3.node" is installed via remote
npm WARN saveError ENOENT: no such file or directory, open '/home/user/git/monbot/package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open '/home/user/git/monbot/package.json'
npm WARN monbot No description
npm WARN monbot No repository field.
npm WARN monbot No README data
npm WARN monbot No license field.

+ sqlite3@5.0.1
added 120 packages from 167 contributors and audited 120 packages in 9.572s

2 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities



   ╭─────────────────────────────────────────────────────────────────╮
   │                                                                 │
   │      New patch version of npm available! 6.14.4 → 6.14.10       │
   │   Changelog: https://github.com/npm/cli/releases/tag/v6.14.10   │
   │                Run npm install -g npm to update!                │
   │                                                                 │
   ╰─────────────────────────────────────────────────────────────────╯

user@Leolios:~/git/monbot$ npm -v
6.14.4
```

## Sur Windows 10 janvier 2021(Shadow)

j'ai installé depuis l'interface la dernière version de de NodeJS 14.15.4 LTS (j'ai coché l'option ou chocolat) [https://nodejs.org/en/](https://nodejs.org/en/)

```bash
C:\Users\Shadow>cd git
C:\Users\Shadow>cd monbot
C:\Users\Shadow>npm init                                                                                                     
C:\Users\Shadow\git\monbot>npm install sqlite3 

# J'ai ajouter les logs d'erreur de fin
2956 verbose stack Error: sqlite3@5.0.1 install: `node-pre-gyp install --fallback-to-build`
2957 verbose pkgid sqlite3@5.0.1
2958 verbose cwd C:\Users\Shadow\git\monbot
2959 verbose Windows_NT 10.0.19041
2960 verbose argv "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "install" "sqlite3"
2961 verbose node v14.15.4
2962 verbose npm  v6.14.10
2963 error code ELIFECYCLE
2964 error errno 1
2965 error sqlite3@5.0.1 install: `node-pre-gyp install --fallback-to-build`
2965 error Exit status 1
2966 error Failed at the sqlite3@5.0.1 install script.
2966 error This is probably not a problem with npm. There is likely additional logging output above.
2967 verbose exit [ 1, true ]
```

Effectivement y a bien un soucis, je vient d'ouvrir le fichier de debug log, j'ai collé un bout ici

Le problème est clairement Python, j'ai pas installé Python pour voir si c'était volontairement le soucis en installant uniquement les mêmes versions de NPM et NodeJS que celle sur Ubuntu 20.04 LTS sachant que Python est déjà bien gérer de base par Ubuntu.

![Première trace d'erreur](<../../.gitbook/assets/image (2).png>)

Il explique ici qu'il te faut Python mais qu'il n'a pas trouvé de dossier d'installation pour l'utiliser, sur une version spécifique pour installe **node-gyp**

Cette fois j'ai installé Python et l'erreur à disparu [https://www.python.org/downloads/](https://www.python.org/downloads/) :

![](<../../.gitbook/assets/image (3).png>)

Maintenant l'erreur est corrigé, il me dit que je n'ai pas d'installation trouvé de visual studio pour qu'il puisse faire des traitements, go installer\*\*.\*\*

**VS 2015 et pour nodeJS 8 => VS2013** [https://visualstudio.microsoft.com/fr/vs/older-downloads/](https://visualstudio.microsoft.com/fr/vs/older-downloads/) Je vais tenter la 2019 pour voir. (_malgré un ordi très puissant avec un super processeur, 12Go de mémoire vive l'installe est très longue_)

![](<../../.gitbook/assets/image (4).png>)

![](<../../.gitbook/assets/image (5).png>)

ça ne lui convient toujours pas mais il trouve cette fois VS2019 et il demande le VS C++ core en plus. De mémoire NodeJs utilise du C++ avec le V8 à coté du coup, je ne suis pas du tout étonné Je vais revoir l'installation pour check si je peux pas l'inclure en retournant voir visual studio installer et cette fois je coche développement web ASP.NET peut être qu'il va m'inclure le VS C++ core cette fois.

Ce n'étais finalement pas nécessaire il me dit que je peux trouvé ce qu'il me faut en allant ici :[https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) pour node gyp\[05:47]Il me dit d'ouvrir powershell en admin puis ensuite de demander à mon ordi d'installer les outil de dev :

```
npm install --global windows-build-tools
```

\[05:48]Parfait ça fonctionne il à ajouté à NPM un supplément de façon global à la conf du Binaire pour l'avoir en dépendance partout. Mais j'ai toujours le soucis qu'il n'arrive pas à trouver l'installation de VS à utiliser :

![](<../../.gitbook/assets/image (6).png>)

D'après un forum il faut que j'ajoute autre chose :

![](<../../.gitbook/assets/image (7).png>)

![](<../../.gitbook/assets/image (8).png>)

Résultat pour installer Sqlite3 sur Windows 10, il m'aura fallut 1h, quelque recherches et aussi attendre le téléchargement et l'installation sur l'installer VS 2019.

{% hint style="info" %}
Il faut installer :

**Node**, **Npm** ou **Yarn**, **Python** latest, **Visual Studio 2019** latest,\*\*`npm install --global windows-build-tools`\*\*et **cocher** dans l'installer de VS2019 le developpement c++
{% endhint %}

{% hint style="success" %}
**En gros**, lorsque tu fais ton installation de Sqlite3 il n'arrive pas a télécharger son archive et npm reçoit une **403** résultat, il propose ce qu'on appel une solution de repli (fallback) qui est de build Sqlite3 sauf que pour ça faut les outils de dev C++. Sous Ubuntu je n'ai pas eu de 403 parce-qu'il fait autrement.
{% endhint %}

{% hint style="danger" %}
Pense à chaque début de projet à faire un : `npm init` pour créer ton fichier package.json à la racine de ton projet dans le dossier de celui-ci.
{% endhint %}

Une fois l'installation terminé mon fichier de dépendances package.json ressemble à ça :

```yaml
{
  "name": "monbot",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "minipass": "^2.7.0",
    "sqlite3": "^5.0.1"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

:two\_hearts: Si on me demande **pourquoi je préféré Unix** pour le développement maintenant vous savez <img src="https://discord.com/assets/2e41bfdeba797283ee9da9bb439c3ece.svg" alt=":wink:" data-size="line">
