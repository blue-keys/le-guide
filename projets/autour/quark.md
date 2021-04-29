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

En Lisp, tout réside dans l'habitude et l'organisation. Bien que le langage puisse paraître assez désordonné, ça n'est simplement qu'une **question d'habitude**. Pratiquer vous permettra de développer une certaine **aptitude dans l'organisation** de votre code en contrepartie.

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

Dans un premier temps, le code va être formaté afin que tous les commentaires soient supprimés de ce dernier : le fait que le langage soit simplement interprété supprime toute utilité aux commentaires **pour le moment**. 

Ce code ne retourne donc qu'une simple copie du code brut modifié.

### Tokenisation

Ensuite, le code, une fois formaté, va être analysé de manière à retourner une liste de ce qu'on appelle **Token** : La définition est assez simple :

```typescript
export enum Tokens {
  Node = 'Node',
  String = 'String',
  Word = 'Word',
}

export type Node = '(' | ')' | '{' | '}';

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

Par la suite, vient l'étape d'analyse syntaxique qui consiste à **créer une représentation du code** pouvant être plus **simplement ensuite manipulée** par l'interpréteur. Dans le cas de Quark, il s'agit d'une **array dite multidepth**, voici sa définition :

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

L'interprétation est la dernière étape du processus du langage de programmation. Elle consiste en l'exécution de notre arbre syntaxique \(ou AST\) généré à l'étape précédente. Cette étape bien que plus complexe demeure toujours réalisable et assez simple avec un peu de motivation.  
  
_Nous ne survolerons cependant que la partie simple de l'interpréteur._  
  
Avant de partir dans les profondeurs du fonctionnement d'un langage interprété, il est déjà important de définir plusieurs termes :

* Stack : La stack n'est ni plus ni moins qu'une simple liste contenant des **frames**, elle est nécessaire au bon fonctionnement du langage puisqu'elle permet de stocker d'une part les variables et d'autre part de gérer le scope.
* Frame : La frame est l'élément trouvé dans la **stack**, dans notre cas, la frame est surtout utilisée afin de stocker les variables d'un scope précis. Cette dernière n'est ni plus ni moins qu'un objet où la propriété représente le nom de la variable ou de la fonction et la valeur, la valeur de la variable ou fonction.
* Scope : Le scope permet de séparer la définition de chaque variable suivant son lieu de définition. Se différencient alors deux types de variables : les variables locales et globales. De manière simplifiée, un nouveau scope est créé lorsqu'un nouveau bloc l'est ou qu'une fonction est appelée. Donc autrement dit, lorsque l'une de ses actions a lieu, une nouvelle frame est créée.

#### Définition d'une variable

Dès à présent, commençons par la définition d'une variable et sa modification.  
Lors de la création d'une variable, cette dernière est stockée sur la **dernière frame** contenue dans la stack, ce qui fait qu'elle est constamment créée de manière locale à moins d'être dans la racine du programme.  
  
La modification de variable, quant à elle, récupère modifie la variable en question dans le scope dans lequel elle est trouvée en dernier. De ce fait, une variable dans un scope antérieur peut être modifiée.

#### Appel d'une fonction

L'appel d'une fonction déclenche la création d'une nouvelle frame : Les arguments sont définis dans cette nouvelle frame afin qu'ils puissent être accessibles dans la fonction, et le corps de la fonction est ensuite exécuté. La frame est enfin retirée et le résultat de la fonction, retourné.

#### Valeur de retour

Le système de retour peut paraître assez innocent mais est en réalité une réelle peine si attention n'est pas prise : dans un système où toute fonction retourne constamment une valeur, il est compliqué de différencier une valeur de retour d'une fonction de notre code Quark que d'une simple valeur de retour de notre interpréteur.  
  
C'est pour cela, qu'il a été mis en place dans Quark un système assez simple en définition et en application permettant de gérer simplement et à un seul endroit le retour : ce système consiste en retourner une liste dont le premier élément est la valeur de retour et le second élément est un booléen indiquant s'il s'agit d'une valeur de retour d'une fonction dans Quark ou pas.   
  
A partir de là, s'il s'agit bel et bien d'une valeur de retour, nous pouvons retourner la valeur en question de la fonction et donc arrêter la boucle de noeud, sinon on continue simplement la boucle.



## 🔗 Liens

{% embed url="https://github.com/quark-lang" %}

{% embed url="https://github.com/quark-lang/quark" %}

{% embed url="https://github.com/thomasvergne" %}



