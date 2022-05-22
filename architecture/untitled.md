# L'Art De Créer Des Diagrammes d'Architecture

### Points Clés

* La conception de diagrammes architecturaux n'est pas une tâche facile ; elle peut être délicate ou source d'erreurs, même pour les plus simples. La création de diagrammes cohérents et significatifs apporte clarté et consensus entre les différentes parties prenantes.
* Dans la plupart des cas, les véritables problèmes ne sont pas strictement liés à l'utilisation d'un langage de description architecturale moins efficace (par exemple UML), mais à la mauvaise compréhension de l'importance des diagrammes, au recours à des directives inappropriées ou incohérentes ou même au manque de formation en architecture.
* Dans le processus de création des diagrammes, essayez de combiner les diagrammes générés automatiquement avec ceux créés manuellement afin de minimiser le travail, d'illustrer différents ensembles de préoccupations et de couvrir plusieurs niveaux d'abstraction du système.
* Comme le système évolue, maintenir les diagrammes à jour demande un effort supplémentaire. Nous devons savoir comment procéder efficacement dans de tels cas en conservant la cohérence et la robustesse des diagrammes d'architecture.
* Les architectures modernes apportent des complexités supplémentaires qui se reflètent dans les diagrammes. Des préoccupations supplémentaires peuvent apparaître.

À un moment donné, dans chaque projet logiciel auquel nous participons, il peut être nécessaire de créer des diagrammes d'architecture. Que nous suivions un modèle architectural formel (e.g. Kruchten 4+1, Rozanski & ; Woods, etc.) ou non, il est nécessaire de documenter certaines parties de l'application en créant des diagrammes. Dans l'architecture logicielle, de tels diagrammes sont créés conformément à des vues qui sont liées à un point de vue spécifique qui pourrait faire partie d'un modèle, mais dans cet article, je préfère m'en tenir au terme de diagramme d'architecture et ne pas être très formel ; tous les autres aspects ne sont pas destinés à être couverts ici.

D'après mon expérience d'architecte logiciel et de formateur technique, il y a beaucoup de divergences entre les projets et au sein de l'équipe de projet, de développeur à développeur, dans la manière dont les diagrammes d'architecture sont créés. J'ai constaté beaucoup de problèmes concernant l'incohérence, la fragmentation et la granularité des informations rendues et l'aspect des diagrammes. Par rapport à un modèle architectural qui doit être formel et standardisé, les diagrammes ne sont pas nécessairement formalisés ou ne suivent pas nécessairement une norme spécifique.

Néanmoins, les diagrammes doivent être autodescriptifs, cohérents, suffisamment précis et liés au code. C'est pourquoi il est important que chaque architecte ou ingénieur logiciel s'appuie sur plusieurs lignes directrices lors de la création de diagrammes d'architecture, car ils constituent le terrain d'entente pour communiquer l'architecture de l'application au fil du temps (e.g. structure, éléments, relations, propriétés, principes) et entre différentes parties prenantes ayant des expériences et des préoccupations techniques diverses.

## Les pièges actuels lors de la conception de diagrammes d'architecture

Avant d'approfondir les problèmes possibles, j'aimerais faire une analogie avec un idiome anglais qui dit "une image vaut mille mots". Selon cette explication [wiki](https://en.wikipedia.org/wiki/A\_picture\_is\_worth\_a\_thousand\_words), "il s'agit de la notion selon laquelle une idée complexe peut être véhiculée par une seule image fixe ou qu'une image d'un sujet transmet sa signification ou son essence de manière plus efficace qu'une description". Le même concept s'applique à un diagramme d'architecture : s'il soulève plus de questions qu'il n'apporte de réponses, le diagramme n'est pas bien réalisé. Ne laissez pas un diagramme d'architecture nécessiter des milliers de mots ou de précisions !

![](https://res.infoq.com/articles/crafting-architectural-diagrams/fr/resources/picture.jpg)

**Exemple d'un diagramme d'architecture incorrect. Il souffre de la plupart des problèmes décrits ci-dessous**

Passons maintenant à une liste de pièges qui pourraient entraver le processus de création correcte de diagrammes d'architecture.

### Que désigne une boîte ou une forme ?

* L'utilisation de toute sorte de boîte ou de forme qui n'est pas correctement documentée peut entraîner de multiples interprétations. Elle peut être associée soit à une donnée, soit à un ensemble de code, soit à un processus. Une simple boîte dans un diagramme peut soulever de multiples doutes et il est très important de les éviter en ajoutant explicitement des détails sur la signification de la boîte ou de la forme dans la légende du diagramme.

### Que représentent les différents bords d'une forme ?

* Chaque bord d'une forme (e.g. pointillé, tiret, etc.) peut être mal compris dans le cas d'un mauvais diagramme. Une bordure spécifique fait-elle référence à un type de composant spécifique (e.g. une ligne pointillée fait référence à un conteneur, un microservice, une couche, etc.), ou est-ce simplement la préférence du concepteur d'avoir un visuel élaboré ? Évitez une telle confusion en fournissant des détails précis dans la légende du diagramme lorsque vous choisissez des bordures multiples ou non standard.

### Que signifie une ligne ou une flèche?

* Une ligne ou une flèche peut être interprétée soit comme un flux de données (par exemple, les flux de données du système A vers le système B), soit comme une relation entre les éléments (par exemple, le composant A dépend du composant B). Dans la plupart des cas, les relations ou les flux de données représentés par des flèches ne convergent pas dans les mêmes directions et il est important de l'écrire explicitement dans la légende du diagramme.

### Quel est le type de communication/association indiqué par une ligne ou une flèche ?

* Même si la ligne fait référence à un flux de données ou à une relation entre des composants, le type de communication (par exemple en cas de flux de données) ou le type d'association (par exemple en cas de relation) indiqué par cette ligne ou cette flèche doit être détaillé. Par exemple, si la ligne représente un flux de données, la communication peut être synchrone ou asynchrone, mais si la ligne fait référence à une relation, elle peut être représentée par une dépendance, un héritage, une implémentation, etc. Tous ces détails doivent être présents dans la légende du diagramme.

### Que signifie cette couleur?

* Avoir un diagramme bariolé (e.g. plusieurs couleurs pour les boîtes, les lignes) sans aucune intention correctement documentée pourrait soulever plusieurs questions (par exemple, pourquoi certaines boîtes sont-elles vertes et d'autres rouges ? Pourquoi certaines lignes sont-elles noires et d'autres bleues ?) Le jeu de couleurs est moins important dans un diagramme, et l'utilisation d'un grand nombre de couleurs n'apporte pas tant de contenu supplémentaire ou d'informations précieuses. Un diagramme peut également être explicatif et bien conçu en utilisant uniquement des couleurs noires et blanches, à moins qu'il n'y ait une contrainte stricte pour insister sur certaines parties du diagramme en utilisant des couleurs distinctes. Dans tous les cas, il est toujours préférable de s'en tenir à la simplicité en termes de couleurs utilisées, mais si ce n'est pas le cas, n'oubliez pas de détailler le choix.

### Les relations manquantes entre des éléments de diagramme ou des entités isolées

* L'absence de relations entre des éléments ou des entités isolées dans un diagramme peut être un indice d'incomplétude. Du point de vue structurel et comportemental, chaque élément ou entité doit s'appuyer sur / avoir une relation (représentée par une ligne ou une flèche) avec une autre partie du système représentée par un élément différent.

### Acronymes trompeurs/non documentés ou termes trop vagues/génériques

* Lorsque l'on utilise un libellé pour un élément d'un diagramme, il est recommandé de ne pas utiliser d'acronyme trompeur ou non documenté qui pourrait prêter à confusion. Une simple séquence de lettres (par exemple TFH, RBPM) ne signifie rien sans une explication appropriée sur l'élément du diagramme ou, mieux encore, dans la légende du diagramme (e.g. TFH - ticket feed handler, RBPM - rates business process manager).
* Une autre caractéristique du nommage des éléments du diagramme concerne les termes extrêmement vagues ou génériques (e.g. logique métier, logique d'intégration) qui n'apportent pas beaucoup d'informations utiles car leurs noms ne sont pas correctement autodescriptifs. Ce problème peut également se poser au niveau du code, et il est suggéré de toujours utiliser des noms explicites et suggestifs en suivant les principes du code propre ("clean code").

### Mettre l'accent sur les technologies, les frameworks, les langages de programmation ou de script, l'IDE ou la méthodologie de développement dans les diagrammes

* La conception architecturale n'est pas liée ou fondamentalement basée sur une technologie, un framework, un langage de programmation ou de script, un IDE ou une méthodologie de développement. Tous ces éléments viennent plus tard dans le processus afin d'aider à construire l'architecture, mais ils ne sont pas le point central. Ils ne doivent pas être inclus dans les diagrammes, mais être mentionnés dans la description de l'architecture, y compris la justification de leur choix.

### Mélanger des éléments d'exécution et des éléments statiques dans le même diagramme

* Les éléments d'exécution (e.g. threads, processus, machines virtuelles, conteneurs, services, pare-feu, dépôts de données, etc.) ne sont pas présents au moment de la compilation et il est recommandé d'éviter de mélanger ces éléments avec les éléments statiques (e.g. composants, packages, classes) dans le même diagramme. Il existe des types de diagrammes dédiés (e.g. diagramme de concurrence, diagramme de déploiement) qui sont principalement axés sur les éléments d'exécution et il est important de faire la distinction entre ces deux catégories d'éléments et d'éviter de les mélanger autant que possible.

### Faire des hypothèses comme "je vais décrire ça verbalement", et "je l'expliquerai plus tard"

* Tout ce qui n'est pas décrit par le diagramme lui-même n'existe pas, et il n'y a pas de place pour fournir des détails verbaux pour compléter un diagramme. Pourquoi ? Parce que toutes les explications mentionnées oralement mais non capturées dans le diagramme sont perdues, et plus tard, lorsque d'autres parties prenantes (e.g. le développeur, l'architecte) liront le diagramme, elles ne seront pas au courant de ces explications. Essayez d'inclure tous les détails nécessaires dans un diagramme afin d'éviter tout besoin de clarification supplémentaire.

### Des niveaux de détails contradictoires ou des abstractions mixtes

* L'ajout d'éléments liés à différents niveaux d'abstraction dans un même diagramme peut être source de conflit, car ils sont vus sous des angles différents. Par exemple, l'ajout de composants à un diagramme de contexte ou de classes à un diagramme de déploiement peut faire diverger l'objectif du diagramme lui-même. Lors de la création un diagramme, essayez de vous en tenir au même niveau d'abstraction.

### Diagrammes encombrés ou trop vagues essayant de montrer un niveau de détail trop élevé ou insuffisant

* "Rendez les choses aussi simples que possible, mais pas plus simple" ; est une citation bien connue d'Albert Einstein. Ceci est également valable pour les diagrammes d'architecture ; le niveau et la granularité des informations saisies doivent être choisis de manière significative. Ce n'est pas une chose facile ; cela dépend du modèle architectural utilisé, de l'expérience de l'architecte et de la complexité du système.

## Lignes directrices à suivre lors de la création de diagrammes d'architecture

Outre les écueils ci-dessus, qui doivent faire partie d'une liste de contrôle préalable afin de les éviter, il existe également des lignes directrices générales sur la façon de créer correctement des diagrammes :

### Choisissez le nombre optimal de diagrammes

* Comme l'a dit Philippe Kruchten, "l'architecture est une bête complexe. L'utilisation d'un seul plan pour représenter l'architecture aboutit à un désordre sémantique inintelligible." Pour documenter les systèmes modernes, nous ne pouvons pas nous retrouver avec un seul type de diagramme, mais lors de la création de diagrammes d'architecture, il n'est pas toujours facile de savoir quels diagrammes choisir et combien en créer. Il y a de multiples facteurs à prendre en considération avant de prendre une décision ; par exemple, la nature et la complexité de l'architecture, les compétences et l'expérience de l'architecte logiciel, le temps disponible, la quantité de travail nécessaire pour les maintenir, et ce qui a du sens ou est utile pour répondre aux préoccupations des parties prenantes. Par exemple, un ingénieur réseau voudra probablement voir un modèle de réseau explicite comprenant les hôtes, les ports de communication et les protocoles ; un administrateur de base de données se préoccupe de la manière dont le système manipule, gère et distribue les données, etc. Sur la base de tous ces aspects, il est recommandé de choisir le nombre optimal de diagrammes, quel que soit ce nombre.
* S'il n'y a pas assez de diagrammes (e.g. sous-documentation), des parties de l'architecture peuvent être cachées ou non documentées ; en revanche, s'il y en a trop (e.g. sur-documentation), l'effort nécessaire pour les maintenir cohérents, à jour et non fragmentés peut augmenter considérablement.

### Conserver une cohérence structurelle et sémantique entre les diagrammes

* Chaque diagramme doit être cohérent avec les autres en termes de boîtes, de formes, de bordures, de lignes, de couleurs, etc. L'aspect structurel doit être le même et chaque partie prenante ne doit avoir aucune difficulté à comprendre les diagrammes créés par différents développeurs au sein d'une équipe. L'idéal est de s'en tenir à un outil de création de diagrammes commun et de le réutiliser pour tous les projets.
* Du point de vue sémantique, tous ces diagrammes doivent être périodiquement synchronisés avec les dernières modifications du code et entre eux, car une modification d'un diagramme peut avoir un impact sur les autres. Ce processus peut être déclenché manuellement ou automatiquement à l'aide d'un outil de modélisation. Ce dernier est le mécanisme préféré mais cela dépend du projet, dans tous les cas l'idée est de maintenir la cohérence entre les diagrammes et le code, indépendamment de la méthode ou de l'outil. Simon Brown a déclaré "les diagrammes ne sont pas utiles pour l'amélioration de l'architecture s'ils ne sont pas liés au code", ce qui met l'accent sur l'idée de cohérence sémantique.

### Empêcher la fragmentation des diagrammes

* Le fait d'avoir plusieurs diagrammes peut rendre la description architecturale difficile à comprendre, mais aussi représenter un effort important pour les maintenir. Par ailleurs, une fragmentation peut apparaître (e.g. deux ou plusieurs diagrammes illustrent le même attribut de qualité - performance, évolutivité, etc. - mais chacun d'entre eux est individuellement incomplet). Dans de tels cas, il est recommandé soit de supprimer les diagrammes qui ne reflètent pas les attributs de qualité pertinents (liés à des exigences architecturales importantes), soit, mieux encore, de fusionner les diagrammes (e.g. concurrence et déploiement).

### Maintenir la traçabilité des diagrammes

* Pour pouvoir vérifier l'historique, il est également important de faire des comparaisons entre les différentes versions du diagramme et de pouvoir revenir facilement à une version précédente. L'utilisation d'un outil de modélisation qui ne permet pas cela peut être un obstacle. Les dernières tendances dans le secteur reposent sur l'utilisation d'un langage simple et intuitif en texte clair pour générer les diagrammes à partir de celui-ci, ce qui semble résoudre le problème de la traçabilité. Un autre avantage d'une telle approche est qu'elle assure implicitement une cohérence structurelle homogène entre les diagrammes.

### Ajouter des légendes à côté des diagrammes architecturaux

* Si vous ne suivez pas un langage de description architecturale standard (e.g. UML, ArchiMate), détaillez chaque élément du diagramme dans la légende (e.g. boîtes, formes, bordures, lignes, couleurs, acronymes, etc.)
* Si ce n'est pas le cas, il suffit d'ajouter dans la légende le langage de description architecturale comme clé et il n'est pas nécessaire de donner des explications supplémentaires, puisque chaque lecteur suivra les spécificités de ce langage pour comprendre le diagramme.

## Le langage de description architecturale (e.g. UML, ArchiMate, etc.) fait-il une différence ?

Il y a beaucoup d'avis sur la question de savoir quel est le bon langage de description à adopter dans le projet. Certaines personnes pourraient faire valoir que l'UML est rigide et pas assez flexible pour modéliser la conception architecturale, un point de vue auquel je souscris. Néanmoins, dans certains cas, il peut être plus que suffisant pour documenter les principes fondamentaux d'une architecture sans s'appuyer sur des caractéristiques d'extensibilité de l'UML comme les profils et les stéréotypes. En examinant d'autres langages de description, nous pouvons constater que [ArchiMate](http://www.opengroup.org/subjectareas/enterprise/archimate) est plus puissant et plus adapté à la modélisation des systèmes d'entreprise par rapport à UML ; il existe également [BPMN](http://www.bpmn.org/) qui est particulièrement ciblé sur les processus d'entreprise, etc. Les comparaisons pourraient se poursuivre, mais je n'ai pas l'intention de les passer en revue de manière approfondie, car ce n'est pas le but de cet article.

Disposer d'un langage de description architecturale suffisamment complet et flexible est un grand pas en avant et cela devrait être un critère solide lors du choix de ce langage. Mais de mon point de vue, la véritable cause réside ailleurs et est liée au fait que la documentation architecturale n'est pas du tout créée. Les gens trouvent souvent que sa création est ennuyeuse, inutile ou sans intérêt. Le nombre de projets logiciels sans documentation ou avec une documentation incorrecte est énorme. Je ne pense pas que les gens créent ou participent intensivement à la création de diagrammes d'architecture en utilisant un langage de description inapproprié, et s'ils devaient les remplacer par un meilleur, les résultats seraient très différents. Non, les gens ne créent pas de documentation architecturale (y compris des diagrammes d'architecture), et pire encore, la plupart d'entre eux n'ont aucune idée de la façon de la créer correctement. Ce sont les points que nous devons aborder en premier : comprendre pourquoi la documentation est importante et comment la créer correctement (en formant des ingénieurs logiciel) ; ensuite, le choix des outils appropriés vient naturellement.

## Comment maintenir des diagrammes à jour au fur et à mesure du développement du système et que les modifications de l'architecture se matérialisent

Il existe peu d'approches pour tenir les diagrammes à jour ; j'en présenterai trois ci-dessous. La première option, et la plus simple, serait de générer automatiquement des diagrammes à partir du code source, ce qui est la vérité du terrain. Cela garantit qu'ils sont tous cohérents avec le code. Malheureusement, avec les outils existants, ce n'est pas encore tout à fait possible (du moins à ma connaissance), car les outils actuels ne peuvent créer aucun type de diagramme précis et significatif uniquement à partir du code source, sans intervention manuelle importante. Len Bass a dit "l'environnement de développement idéal est celui pour lequel la documentation est disponible essentiellement gratuitement en appuyant sur un bouton", implicitement en générant automatiquement les diagrammes, mais nous n'en sommes pas encore là.

La deuxième approche consisterait à concevoir d'abord les diagrammes à l'aide d'un outil dédié qui génère ensuite les squelettes du code source (e.g. composants/packages avec des frontières, API) utilisés plus tard par les développeurs pour les remplir avec du code. De cette façon, chaque changement dans l'architecture doit être déclenché à partir du diagramme lui-même, qui pourrait automatiquement régénérer ou mettre à jour le squelette du code.

Le dernier cas implique la mise à jour manuelle des diagrammes chaque fois qu'une nouvelle fonctionnalité - qui a un impact sur la conception architecturale - est mise en œuvre. Pour être sûr que toutes les modifications de code sont reflétées dans les diagrammes, il est recommandé que la mise à jour des diagrammes fasse partie de la définition du réalisé dans le processus de développement. Ce scénario est moins recommandé car il pourrait facilement entraîner des diagrammes obsolètes ou incohérents (e.g. les développeurs oublient souvent ou ne sont pas d'humeur à mettre à jour les diagrammes) et malheureusement cela se produit encore dans une majorité de projets.

En tenant compte des outils existants, ma recommandation est d'avoir un mixte ; pour mélanger création automatique et manuelle des diagrammes. Par exemple, essayez de générer automatiquement des diagrammes, qui peuvent être raisonnablement rendus par des outils basés sur le code source sans trop de bruit (e.g. des informations trop encombrées ou dénuées de sens). Dans cette catégorie, nous pouvons inclure soit des diagrammes avec un degré élevé de volatilité (e.g. plus enclins à des changements fréquents de développement, ayant généralement une abstraction plus faible) ou, au contraire, des diagrammes statiques. Certains de ces diagrammes peuvent faire référence à des diagrammes de contexte, d'architecture de référence, de package, de classe, d'entité, etc. Néanmoins, dans certains cas, il n'est pas évident, en se basant uniquement sur le code source, de savoir comment le système répond à certains attributs de qualité (e.g. disponibilité, extensibilité, performance), c'est pourquoi la création automatique de diagrammes n'est pas une option suffisante. Elle doit être complétée par des diagrammes modélisés manuellement. Parmi les exemples de tels diagrammes, on peut citer les diagrammes de séquence, d'état, de concurrence, de déploiement, d'opération, etc.

## Quelles complications (ou simplifications) émergent pour les diagrammes d'architecture lorsqu'il s'agit d'architectures modernes (par exemple, les microservices) ?

Les microservices ou tout autre style d'architecture moderne (e.g. sans serveur ("serverless"), orienté événements) ne font que piloter la structure du système, la façon dont les composants communiquent entre eux (par exemple les relations entre eux) et les principes qui les régissent. Personnellement, je ne pense pas que le style architectural devrait changer la logique ou les concepts autour de la création des diagrammes (et implicitement la description architecturale), ni ce qu'ils devraient capturer. Néanmoins, lorsque nous parlons d'architectures de systèmes modernes, ayant généralement des niveaux de complexité plus élevés par rapport aux systèmes anciens et classiques (e.g. monolithes), elles ont certainement un impact sur la description architecturale et implicitement sur les diagrammes, dans le sens où il y a de multiples considérations à prendre en compte. Ces considérations peuvent concerner la compréhension du nombre de composants distribués (e.g. les microservices distribués), le type de chaque composant, la façon dont les composants communiquent entre eux (e.g. frontières, API, messages), leur cycle de vie et le propriétaire de chaque composant.

En tenant compte de tous ces éléments, les vues capturant la décomposition, le développement, le déploiement et l'opérabilité du système doivent être considérées par défaut. Imaginez un système avec un nombre impressionnant de microservices, par exemple ; dans un tel cas, le nombre de diagrammes pourrait augmenter de manière significative car chaque microservice pourrait finir par avoir son propre ensemble de diagrammes. Les questions relatives à la cohérence (e.g. la modification de l'API d'un service a une incidence sur les autres services X, et tous les diagrammes concernés doivent donc être mis à jour), à la fragmentation (e.g. la haute disponibilité ou les performances entre les services distribués ne sont pas consolidées dans un seul diagramme) ou aux préoccupations transversales (e.g. qui est chargé d'illustrer, de manière consolidée, des aspects tels que la surveillance ou la sécurité de l'ensemble des éléments du système) pourraient ne pas être facilement traitées. En outre, il peut y avoir des défis liés à la coexistence et la collaboration des équipes pendant le développement du projet, et même après, afin de le maintenir.

Pour résumer, les systèmes modernes avec des architectures complexes pourraient apporter des préoccupations supplémentaires qui pourraient entraîner des complications même au niveau des diagrammes.&#x20;

## About the Author

<img src="https://res.infoq.com/articles/crafting-architectural-diagrams/fr/resources/IMG_9271-2.JPG" alt="" data-size="original">**Ionut Balosin** est architecte logiciel et formateur technique indépendant. Il est un conférencier régulier lors de conférences et de rencontres sur le développement de logiciels à travers le monde, en donnant des présentations, des cours de formation et des ateliers. Pour plus de détails, veuillez consulter [son site web](http://www.ionutbalosin.com/).

Source : [https://www.infoq.com/fr/articles/crafting-architectural-diagrams/](https://www.infoq.com/fr/articles/crafting-architectural-diagrams/)
