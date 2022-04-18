---
description: Quark est un langage de programmation interprété en Typescript.
---

# Quark

Quark est un langage de programmation interprété en **Typescript** dont la syntaxe se base sur **Lisp**. Le langage fonctionne dans sa globalité de **manière récursive** se basant sur un **système de registre et CallStac**k.

## Exemple

Comme indiqué, il s'agit d'un Lisp like, en voici quelques démonstrations à travers de simples exemples :

{% tabs %}
{% tab title="Hello world" %}
```scheme
(print "Hello world")
```
{% endtab %}

{% tab title="Affichage de variable" %}
```scheme
(let username "Thomas")
(print "Hello" username)
```
{% endtab %}

{% tab title="Fonction de bienvenue" %}
```scheme
(let welcome (fn (username) {
  (print "Hello" username)
}))
```
{% endtab %}
{% endtabs %}

En Lisp, tout réside dans l'habitude et l'organisation. Bien que le langage puisse paraître assez désordonné, ça n'est simplement qu'une **question d'habitude**. Pratiquer vous permettra de développer une certaine **aptitude dans l'organisation** de votre code en contrepartie. En plus de cela, le Lisp vous permettra d'accroître votre productivité de par sa liberté et sa simplicité à écrire.

## Installation

L'installation de Quark est assez simple, il faut cependant plusieurs dépendances au préalable :

* Deno
* Git

```bash
# On clone le repôt et on se déplace dedans
$ git clone https://github.com/quark-lang/quark
$ cd quark

# On installe ensuite au path le projet
$ deno install --no-check -n quark --allow-all src/main.ts

$ quark
```

## Fonctionnement

### Formatage

Dans un premier temps, le code va être formaté afin que tous les commentaires soient supprimés de ce dernier : le fait que le langage soit simplement interprété supprime toute utilité aux commentaires **pour le moment**.&#x20;

Ce code ne retourne donc qu'une simple copie du code brut modifié.

### Tokenisation

Ensuite, le code, une fois formaté, va être analysé de manière à retourner une liste de ce qu'on appelle **Token** : La définition est assez simple :

```typescript
export enum Tokens {
  Node = 'Node',
  String = 'String',
  Word = 'Word',
}

export type Node = 
  | '(' 
  | ')' 
  | '{' 
  | '}';

export interface Token {
  token: Tokens,
  value: Node | string,
}

```

Voici le résultat de l'analyse du code ci-dessous :

```scheme
(print "Hello world")
```

```typescript
[
  { token: "Node", value: "(" },
  { token: "Word", value: "print" },
  { token: "String", value: '"Hello world"' },
  { token: "Node", value: ")" }
]
```

### Analyse syntaxique

Par la suite, vient l'étape d'analyse syntaxique qui consiste à **créer une représentation du code** pouvant être plus **simplement ensuite manipulée** par l'interpréteur. C'est sans-doute l'un des étapes les plus importantes au bon fonctionnement du langage. Dans le cas de Quark, il s'agit d'une **array dite multidepth**, voici sa définition :

```typescript
export type Block = (Element | Block)[];

export type ElementTypes =
  | 'String'
  | 'Number'
  | 'Word'
  | 'Node';

export interface Element {
  type: ElementTypes,
  value: string | number,
}
```

Pour le code suivant :

```scheme
(print "4 + 2 =" (+ 4 2))
```

La représentation sera :

```typescript
[
  [
    { type: "Word", value: "print" },
    { type: "String", value: "4 + 2 =" },
    [
      { type: "Word", value: "+" },
      { type: "Number", value: 4 },
      { type: "Number", value: 2 }
    ]
  ]
]
```

### Interprétation

L'interprétation est la dernière étape du processus du langage de programmation. Elle consiste en l'exécution de notre arbre syntaxique (ou AST) généré à l'étape précédente. Cette étape bien que plus complexe demeure toujours réalisable et assez simple avec un peu de motivation.\
\
_Nous ne survolerons cependant que la partie simple de l'interpréteur._\
\
Avant de partir dans les profondeurs du fonctionnement d'un langage interprété, il est déjà important de définir plusieurs termes :

* La stack : l'intégralité de votre code est régis par la Stack. Pour faire simple, c'est un conteneur qui contient lui-même des conteneurs appelés **Function Frame**. Ce dernier vous permet donc de maintenir la structure des différentes variables du code. C'est cette organisation en conteneurs qui permet de délimiter le **scoping** de ces dernières.
* La Function Frame : il s'agit simplement d'un conteneur, cette fois-ci encore, permettant contrairement à la Stack, d'organiser bien plus précisément les variables et leur utilisation. En effet, la function frame contient ce qu'on appelle des **Local Frame**. La function frame est créée dès lors l'appel d'une fonction. Les function frames ne peuvent pas intéragir entre-elles.
* La Local Frame : c'est le conteneur le plus profond, celui qui contient quant à lui, l'intégralité de vos variables, du scope global au scope local, il est omniprésent. Chaque local frame représente un niveau de scope bien précis et propre à la function frame en question. Les local frames peuvent intéragir entre-elles.
* Le scoping : Le scoping n'est ni plus ni moins qu'un terme pour désigner la délimitation de code via des blocs. Cette dite délimitation permet notamment la création de nouvelles Local Frames, ce pouvant être utile à la création de variables "privées".&#x20;

#### Définition d'une variable

Dès à présent, commençons par la définition d'une variable et sa modification.\
Lors de la création d'une variable, cette dernière est stockée sur la **dernière frame** contenue dans la local frame en cours d'utilisation, ce qui fait qu'elle est constamment créée de manière locale à moins d'être dans la racine du programme, où elle sera alors considérée comme faisant parti intégrante de la frame globale.\
\
La modification de variable, quant à elle, récupère modifie la variable en question dans le scope dans lequel elle est trouvée en dernier. De ce fait, une variable dans un scope antérieur peut être modifiée. Mais ne peut pas être modifié, les variables d'une function frame.

#### Appel d'une fonction

L'appel d'une fonction déclenche la création d'une nouvelle frame : Les arguments sont définis dans cette nouvelle frame afin qu'ils puissent être accessibles dans la fonction, et le corps de la fonction est ensuite exécuté. La frame est enfin retirée et le résultat de la fonction, retourné.

#### Valeur de retour

Le système de retour peut paraître assez innocent mais est en réalité une réelle peine si attention n'est pas prise : dans un système où toute fonction retourne constamment une valeur, il est compliqué de différencier une valeur de retour d'une fonction de notre code Quark que d'une simple valeur de retour de notre interpréteur.\
\
C'est pour cela, qu'il a été mis en place dans Quark un système assez simple en définition et en application permettant de gérer simplement et à un seul endroit le retour : ce système consiste en retourner une liste dont le premier élément est la valeur de retour et le second élément est un booléen indiquant s'il s'agit d'une valeur de retour d'une fonction dans Quark ou pas. \
\
A partir de là, s'il s'agit bel et bien d'une valeur de retour, nous pouvons retourner la valeur en question de la fonction et donc arrêter la boucle de noeud, sinon on continue simplement la boucle.



## :link: Liens

{% embed url="https://github.com/quark-lang" %}

{% embed url="https://github.com/quark-lang/quark" %}

{% embed url="https://github.com/thomasvergne" %}

