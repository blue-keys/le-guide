---
description: Orion est un langage de programmation inspiré du Schème écrit en Rust.
---

# Orion

![](../../.gitbook/assets/logo-orion.png)

Étant donné qu'il est inspiré de Lisp, tout est fonction, par exemple, voici comment s'utilise la fonction `define` permettant de déclarer une variable:

```scheme
(define a "foo") ; Assigne la valeur "foo" à la variable bar
```

Il possède une petite base de «mots clés» \(ce sont en réalité des fonctions mais nous allons les appeler ainsi pour une meilleure compréhension\), ainsi qu'une petite std \(_id est_ bibliothèque standard, bibliothèque contenant toutes les fonctions mathématiques, sur les collections, sur le système, etc, du langage\) écrite en Rust, rendant facile à partager les programmes écrits en Orion car nul besoin d'ajouter 50 fichiers.

Il est statiquement et fortement typé, ce qui signifie qu'une variable gardera son type originel tout au long du programme et qu'il n'y a aucune conversion implicite \(par exemple, `3 + "5"` retournera `nil` et non pas 8\).

Il permet la méta programmation \(_id est_, du code qui écrit du code\) à travers le mot clé `macro`, utilisable comme suit:

```scheme
macro foo {(print "Hello from a macro !")}
foo ; sera remplacé par `(print "Hello from a macro à l'éxecution")`.
```

Il permet aussi le pattern matching, avec une syntaxe s'approchant du OCaml ou du Rust:

```scheme
(define a 5); variable a valant 5

(match a {
    (=> 3 {
        (print "A vaut 3 !")
    })
    (_ { ; le _ signifie "n'importe quel autre cas"
        (print (+ "A ne vaut pas 3, mais " a))
    })
})
```

Il possède aussi une sorte de debugger intégré, utilisable via la fonction `breakpoint`.

### Installer Orion

Pour l'installer il y a plusieurs manières possibles:

* Via cargo: `cargo install orion-lang`
* Via les releases github \(en téléchargeant un binaire\)
* Depuis la source, en utilisant `cargo` et `git`.

### Apprendre Orion

Pour apprendre Orion, il y a un document nommé `POUR DÉMARRER` apprenant les bases, et la documentation de la bibliothèque standard pour aller plus loin.

{% hint style="info" %}
Vous pouvez me retrouver sur le Discord de Bluekeys sur le salon \#Orion. Merci de m'avoir lu, 
{% endhint %}

## 🔗 Github

{% embed url="https://github.com/wafelack/orion-lang" %}

## 🔗 Site perso

{% embed url="https://wafelack.fr" %}

