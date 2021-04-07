# Google Admin Toolbox HAR Analyzer

 [HAR \(HTTP Archive\)](https://www.google.com/url?q=http://www.softwareishard.com/har/viewer/) est un format de fichier utilisé par plusieurs outils de session HTTP pour exporter les données capturées. Ce format correspond à un objet JSON avec une distribution de champs particulière. Notez que les champs ne sont pas tous obligatoires et qu'il arrive souvent que des informations ne soient pas enregistrées dans le fichier.

Les fichiers HAR contiennent des données sensibles :

* Le contenu des pages téléchargées lors de l'enregistrement
* Vos cookies, qui permettraient à toute personne ayant accès à ce fichier HAR d'usurper votre compte
* Toutes les informations que vous avez envoyées lors de  l'enregistrement : détails personnels, mots de passe, numéros de carte de crédit...

 Vous pouvez obtenir une capture d'une session HTTP dans l'un des trois navigateurs principaux \(IE, Firefox et Chrome\), même si nous vous conseillons Chrome ou Firefox.

Un navigateur web peut enregistrer une session HAR, ça permet dans le développement de pouvoir comprendre ce que vous voyez et faire de l'analyse post mortum, on peut rejouer une sessions HAR depuis un outil en ligne d'HAR analyser : [https://toolbox.googleapps.com/apps/har\_analyzer/](https://toolbox.googleapps.com/apps/har_analyzer/)

## Internet Explorer/Edge

 Edge crée des fichiers HAR en natif. Reportez-vous aux instructions détaillées disponibles sur le [site Microsoft.](https://www.google.com/url?q=https://docs.microsoft.com/microsoft-edge/f12-devtools-guide/network)

* Ouvrez l'outil Réseau dans les outils pour les développeurs \(F12\).
* Reproduisez le problème.
* Exportez le trafic capturé dans un fichier HAR \(CTRL+S\).

Pour Internet Explorer, vous devez utiliser l'outil suivant : [HttpWatch ](https://www.google.com/url?q=http://www.httpwatch.com).

* Téléchargez et installez HttpWatch.
* Lancez la capture HttpWatch juste avant de reproduire le comportement que vous étudiez.
* Arrêtez la capture HttpWatch juste après avoir reproduit le comportement qui vous intéresse.
* Exportez la capture au format HAR.



## Firefox

* Lancez les outils de développement Web en mode Réseau dans Firefox \(menu en haut à droite &gt; Développement Web &gt; Réseau, ou Ctrl+Maj+E\).
* Reproduisez le problème.
* Pour enregistrer la capture, effectuez un clic droit sur la grille, puis sélectionnez Tout enregistrer en tant que HAR.
* Exportez la capture dans un fichier HAR.

## Chrome

 

Vous pouvez enregistrer votre session HTTP depuis l'onglet "Network" \(Réseau\) des outils de développement dans Chrome.

* Sélectionnez "Outils de développement" dans le menu \(Menu &gt; Plus d'outils &gt; Outils de développement\), ou appuyez sur les touches Ctrl+Maj+C de votre clavier.
* Cliquez sur l'onglet "Network" \(Réseau\).
* Cherchez un bouton rond dans la partie supérieure gauche de l'onglet "Network" \(Réseau\). Vérifiez qu'il est rouge. S'il est gris, cliquez dessus pour commencer l'enregistrement.
* Cochez la case "Preserve log" \(Conserver le journal\).
* Vous pouvez utiliser le bouton d'effacement \(un cercle barré d'une ligne diagonale\) juste avant d'essayer de reproduire le problème pour supprimer les informations d'en-tête superflues.
* Reproduisez le problème.
* Pour enregistrer la capture, effectuez un clic droit sur la grille et sélectionnez Save as HAR with Content \(Enregistrer au format HAR avec le contenu\).

Filter by HTTP status codes. [Learn More](https://www.w3schools.com/tags/ref_httpmessages.asp)

 0 1xx 2xx 3xx 4xx 5xx

