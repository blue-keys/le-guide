---
description: Vous voulez débuter en sécurité informatique et ne savez pas où commencer ?
---

# Apprendre la cybersécurité

{% embed url="https://giphy.com/gifs/pbsdigitalstudios-education-xT0xeNwHemJw4fM5ag" %}

{% hint style="info" %}
Pas de panique, cette page est là pour vous orienter avec toutes les informations ! Mais avant ça, même si c'est chiant, on doit parler un peu de lois, parce-que nulle n'est l'ignorer.
{% endhint %}

| **Termes** | **Signification** |
| :--- | :--- |
| Hacking | Piratage |
| CTF | Capture The Flag |
| Pentest | Test d'intrusion |
| Mac Lobster | Hamburger vendu au Canada 😂  |
| Forensinc | Analyse du système d'information après une attaque informatique |
| Journalisation | Action de relever, inscrire tous les événements produit par le système informatique |
| PowerShell | Interface en ligne de commande sur Windows, ainsi qu'un kit de développement |

{% hint style="danger" %}
## Hacking et risques juridiques

Il va sans dire que si le piratage peut être fait de manière éthique, il existe de nombreuses personnes qui jugent superflu le fait de rester dans un cadre légal... dans l'éventualité où vous souhaitiez vous abstenir de suivre la loi, laissez-moi vous rappeler les risques auxquels vous seriez exposés.

La France dispose de lois contre le piratage depuis la [loi Godfrain](https://www.securiteinfo.com/legal/loi88-19.shtml) \(en 1988 !\). Déjà à l'époque, la loi prévoyait des peines de prison jusqu'à 2 ans et des amendes jusqu'à 25300€ en comptant l'inflation.

Depuis, les textes de lois ont été modifiés et amendés à plusieurs reprises, prévoyant maintenant des peines beaucoup plus sévères, allant de deux à cinq ans d'emprisonnement et jusqu'à 150000€ d'amende [**Article 323 du code pénal**](https://www.legifrance.gouv.fr/codes/id/LEGISCTA000006149839/2013-06-05/)\*\*\*\*

**Bluekeys se dégage de toute responsabilité quant à la mise en pratique de connaissances acquises grâce à l'association.**
{% endhint %}

{% hint style="info" %}
### Ce qui est interdit et ce qui ne l'est pas

La loi reste vague à ce sujet, en effet [**l’article 323 du code pénal**](https://www.legifrance.gouv.fr/codes/id/LEGISCTA000006149839/2013-06-05/) punit "tout accès non autorisé à un système de traitement d'informations". Les outils de piratage, leurs partages et utilisations restent autorisés. Il est donc légal de pratiquer le piratage tant que vous avez l'autorisation du propriétaire du système, par exemple le vôtre, un autre mis à disposition ou bien une plateforme dédiée comme celle pour les _CTF_.

Attention, si la possession et l'utilisation d'outil n'est pas illégale, la possession et le partage de données obtenues illégalement sont techniquement du recel.

En conclusion, voyez le **hacking** comme une arme à feu, vous pouvez utiliser cette arme sur un champs de tir, de manière tout a fait légale. Exactement de la même manière, vous pouvez très bien utiliser vos outils légalement afin de pirater un site conçu pour ça, ou dans le cas ou vous en avez l'autorisation explicite.

Maintenant que vous êtes au courant de ce qui est interdit et de ce qui ne l'est pas, il est temps de rentrer dans le vif du sujet.
{% endhint %}

## 1. Veille technologique

Suivez les experts influents de la cybersécurité sur Twitter. Ceux qui ont le plus de succès ne se font probablement pas appeler experts de la cybersécurité. Ils se contentent de lâcher constamment des bombes de connaissances, des conseils et des astuces qui peuvent aider votre carrière. Voici une courte liste pour vous aider :

```text
@bammv, @cyb3rops, @InfoSecSherpa, @InfoSystir, @JohnLaTwC, @armitagehacker, @danielhbohannon, @_devonkerr_, @enigma0x3, @gentilkiwi, @hacks4pancakes, @hasherezade, @indi303, @jackcr, @jenrweedon, @jepayneMSFT, @jessysaurusrex, @k8em0, @lnxdork, @mattifestation, @mubix, @pwnallthethings, @pyrrhl, @RobertMLee, @ryankaz42, @_sn0ww, @sroberts, @spacerog, @subtee, @taosecurity
```

## 2. Combinez lecture et pratique

Il existe plusieurs entreprises qui produisent régulièrement du contenu de qualité sur la sécurité. En tête de liste, nous trouvons **CrowdStrike**, **Endgame**, **FireEye**, **Kaspersky**, **Palo Alto's Unit 42** et **TrendLabs**. Au fil de votre lecture, essayez de comprendre comment vous détecteriez l'activité décrite, puis comment vous enquêteriez.

Il existe aussi des _CTF_, il s'agit de compétitions en solo ou en équipe, généralement des challenges lié à la sécurité où il est nécessaire de retrouver un mot de passe qu'on appel le flag, plus d'info sur les CTF: 

{% embed url="https://medium.com/prohacktive/quest-ce-qu-un-ctf-capture-the-flag-26af33219c90" %}

Ce qui est génial, c'est qu'après, les compétiteurs font ce qu'on appelle des write up. Il s'agit de documents qui expliquent pas à pas comment un challenge a été réussi. C'est une vraie mine d'or d'informations ! Nous recommandons vivement que vous en lisiez ou regardiez, car, oui, il y en a même sur YouTube!

Cherchez-vous à développer vos connaissances techniques en vue d'un poste d'analyste ? L'étendue de ce que vous devez savoir peut paraître décourageant. Les connaissances les plus fondamentales à acquérir concernent peut-être la suite de protocoles TCP/IP. Préparez-vous à répondre en toute confiance à la question "que se passe-t-il quand...".

Pour l'apprentissage du F**orensic**, vous ne pouvez probablement pas obtenir de meilleures bases que la 3e édition de "Incident Response and Computer **Forensics**". Le chapitre sur l'investigation de Windows est un véritable trésor. Plongez dans Powershell, les cadres d'attaque associés, et apprenez à accroître la visibilité de l'activité PowerShell grâce à la journalisation. Associez ces connaissances à l'une des meilleures formations gratuites disponibles sur Cobalt Strike et d'autres vidéos, regardez-les et appliquez les concepts que vous avez appris dans le cadre de l'essai de 21 jours de Cobalt Strike. 

#### Vous n'avez pas assez de temps ? 

Envisagez d'investir. Le "Blue Team Field Manual" et le "Red Team Field Manual" complètent nos recommandations à ce sujet. En parallèle, mettez en place un laboratoire avec des postes de travail Windows 7 \(ou ultérieur\) reliés à un domaine. Compromettez le poste de travail en utilisant certaines des techniques les plus simples, puis explorez l'activité post-exploitation. Votre objectif est de vous faire une idée aussi bien du côté de l'attaque que de la défense.

En ce qui concerne le réseau, pensez à :

* "The Practice of Network Security Monitoring" ;
* "Practical Packet Analysis" ;
* "Applied Network Security Monitoring". 

Lorsque le moment est venu de mettre en pratique ce que vous avez appris dans les livres, utilisez des ressources telles que le blog sur l'analyse du trafic des logiciels malveillants et la consultation de PacketTotal pour vous faire une idée de ce qui est "normal" et de ce qui ne l'est pas. Votre objectif ici devrait être de comprendre les sources de données \(preuves réseau\) qui peuvent être utilisées pour détecter et expliquer l'activité. Pour affiner vos processus d'investigation sur le réseau, pensez à Security Onion. Installez quelques capteurs réseau, surveillez le trafic et créez des signatures Snort/Suricata pour signaler le trafic illicite. Votre objectif est d'établir un processus d'enquête de base et, comme pour les points d'accès, de comprendre les côtés attaque et défense de l'équation.

## 3. Cherchez à pratique plus que la lecture

Avez-vous déjà suivi un cours et, des mois plus tard, essayé d'utiliser les connaissances que vous aviez prétendument acquises pour découvrir que vous aviez oublié tous les éléments importants ? Oui, si vous déconnectez l'apprentissage de l'utilisation des connaissances, vous allez être dans une situation difficile. C'est peut-être l'un des plus grands défis à relever lorsqu'on se lance dans un rôle de sécurité plus technique.

Pour y remédier, en plus de combiner lecture et pratique, pensez à la technique de Feynman. Vous n'en avez jamais entendu parler ? Eh bien, il est facile de survoler les éléments que l'on ne comprend pas... mais si vous parvenez à les traduire dans un langage simple de façon à ce que les autres puissent les comprendre, vous les aurez mieux compris par la même occasion. Rien ne vous aide à apprendre comme l'enseignement.

## 4. Développer un état d'esprit malveillant

Il y a quelques années, un spécialiste de la sécurité expliquait comment devenir un meilleur défenseur en pensant comme un adversaire. L'histoire s'accompagnait d'échanges maladroits \(et humoristiques\). Il est entré dans une chambre d'hôtel avec sa famille pendant ses vacances, a vu le distributeur de savon non sécurisé installé dans le mur de la douche et a dit à haute voix : "Wow, ce serait si facile de remplacer le shampoing par du Nair ! \(produit d'épilation\)" Sa famille était horrifiée. 

Soyons clairs : nous ne préconisons pas que vous remplaciez le shampoing par du Nair, ou tout autre produit anti-capillaire. Et le concept de penser comme un agresseur n'est pas nouveau. Il y a huit ans, lorsqu'on a demandé à Lance Cottrell ce qui faisait un bon professionnel de la cybersécurité, il a répondu qu'il se mettait "à la place de l'attaquant et regardait le réseau comme l'ennemi le regarderait, puis réfléchissait à la manière de le protéger".

La meilleure façon d'y parvenir aujourd'hui est de se familiariser avec le cadre **ATTACK de MITRE**. Il devient rapidement le modèle de référence pour structurer le développement d'un processus d'investigation et comprendre où \(et comment\) vous pouvez appliquer la détection et l'investigation. Vous pouvez vous familiariser avec ce cadre avant d'entreprendre des lectures approfondies, puis y revenir de temps en temps si nécessaire.

Avez-vous déjà eu l'idée de vouloir détourner une machine à café juste comme un simple défis de faire couler une tasse sans payer 1€ ?

## 5. Soyez intrépide

Ne laissez pas votre manque de connaissances vous arrêter. Il existe des organisations prêtes à investir dans des personnes ayant les bonnes caractéristiques et le désir d'apprendre. Postulez pour le poste, même si vous ne pensez pas être qualifié. Vous recevrez peut-être un refus. Et alors ? Essayez à nouveau dans une autre entreprise. Ou essayez à nouveau dans cette même entreprise plus tard. La lecture ne vous mènera pas loin... c'est en appliquant vos connaissances que vous passerez au niveau supérieur. Et devinez quoi, vous vous souvenez de la technique de Feynman ? Oui, en enseignant aux autres les connaissances que vous avez acquises, vous passerez au niveau supérieur.

Bonne chance, bonne chasse ! Enfin... à ceux qui disent "une formation en informatique et des compétences techniques approfondies vous aideront à trouver un emploi dans la sécurité", nous répondons : "Nous sommes d'accord !" Et à ceux qui disent que "les rôles dans la sécurité peuvent être larges et que vous pouvez les utiliser pour développer une expertise technique au fil du temps", nous répondons : "Nous sommes également d'accord !"

Ce que nous ne croyons pas, c'est de dire à des gens que nous ne connaissons pas qu'ils ne peuvent pas faire quelque chose sans comprendre leur situation unique. Il peut y avoir des chemins qui sont généralement plus faciles, ou généralement plus difficiles. Mais supposer que vous ne pouvez pas faire quelque chose est un vent contraire dont vous n'avez pas besoin. Nous espérons que vous avez trouvé ici des conseils qui vous donneront l'impulsion nécessaire pour envisager un emploi de sécurité de premier échelon \(ou plus\) et que vous postulerez. À cette fin, nous vous souhaitons... bonne chance !

## 6. Un diplôme universitaire est-il nécessaire pour faire carrière dans la cybersécurité ?

La réponse est courte : pas nécessairement. "Notre industrie a été inaugurée par des personnes sans diplôme universitaire", affirme Josh Feinblum. "Travaillez dur pour vous impliquer dans la communauté, contribuez à des projets open source, essayez de parler de recherches cool lors de conférences - ce sont toutes des choses que les pionniers originaux ont faites et qui peuvent offrir des opportunités aux personnes intelligentes et travailleuses d'entrer dans l'industrie."

## 7. Prérequis en matière de cybersécurité

Comme dans tout domaine technologique, il est utile de commencer par acquérir les bases de la programmation. "Être capable de comprendre un langage de programmation vous donnera un bon départ dans la cybersécurité", déclare Kristen Kozinski. "Vous n'avez pas besoin d'être un expert, mais être capable de lire et de comprendre un langage est une bonne compétence à avoir." Il ne s'agit pas d'un prérequis indispensable en matière de cybersécurité, mais c'est définitivement agréable à avoir.

## 8. Déterminez ce que vous voulez apprendre.

Le domaine de la cybersécurité est très vaste et comporte de nombreuses spécialités, qui changent et évoluent en permanence. La première étape de votre apprentissage autonome de la cybersécurité consiste à définir les domaines sur lesquels vous souhaitez vous concentrer et de vous faire une bonne image mentale de l'ensemble. Vous avez la possibilité de changer d'avis plus tard ou de pivoter,  il est essentiel de savoir quel domaine vous intéresse. Demandez-vous si vous voulez vous concentrer sur la programmation, les tests de pénétration, la sécurité des réseaux, la criminalistique ou un autre domaine. En décidant maintenant, vous pourrez déterminer la meilleure voie à suivre.

## 9. Puis-je apprendre gratuitement ?

Il existe de nombreuses ressources gratuites disponibles en ligne et dans les bibliothèques locales, qui peuvent fournir une grande quantité d'informations sur la cybersécurité. À un moment donné dans tout parcours d'apprentissage, il sera probablement nécessaire d'investir dans des connaissances supplémentaires.

## 10. Parler anglais

Bien qu'il existe de nombreuses ressources en français, il en existe encore plus en anglais. Quand vous rencontrerez un problème, vous trouverez d'avantage de résultats et de réponses en anglais. De plus, vous aurez bien plus d'opportunité d'emploi si vous parlez anglais. En clair, pour faire de la sécurité, il est plus que recommandé de parler anglais, ou d'apprendre l'anglais.

## 11. Linux et les VM

Une machine virtuelle ou VM est un environnement entièrement virtualisé qui fonctionne sur une machine physique. Elle exécute son propre système d’exploitation \(OS\) et bénéficie des mêmes équipement qu’une machine physique : CPU, mémoire RAM, disque dur et carte réseau. Plusieurs machines virtuelles avec des OS différents peuvent coexister sur le même serveur physique : Linux, MacOS, Windows… Ce qui est important, c'est Linux! Si vous ne connaissez pas, nous vous conseillons vivement de rechercher comment créer une VM Linux et comment utiliser Linux. Il s'agit d'un puissant outil, mais il peut paraître compliqué quand vous débutez. Pas besoin de s'inquiéter : vous y arriverez vite !

## 12. Sources

Pour débuter :

[https://expel.io/blog/a-beginners-guide-to-getting-started-in-cybersecurity/](https://expel.io/blog/a-beginners-guide-to-getting-started-in-cybersecurity/)  
 [https://startacybercareer.com/how-to-learn-cyber-security-on-your-own/](https://startacybercareer.com/how-to-learn-cyber-security-on-your-own/)  
 [https://learntocodewith.me/posts/cybersecurity/\#how-to-get-started-in-cybersecurity](https://learntocodewith.me/posts/cybersecurity/#how-to-get-started-in-cybersecurity) 

| Sources | Définition |
| :--- | :--- |
| [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings) | Compilation d'outils et de commandes intéressantes, vous trouverez des informations sur toutes les principales failles. |
| [Rustscan](https://github.com/RustScan/RustScan) | Outil de scan de machine, comme nmap |
| [GTFObins](https://gtfobins.github.io/) | Collection d'exploits visant à escalader ses privilèges |
| [Nishang](https://github.com/samratashok/nishang)  | Collection d'exploits visant à escalader ses privilèges |
| [Nishang](https://github.com/samratashok/nishang)  | Collection de scripts powershell afin d'exploiter les machines windows |
| [Security idiots](http://www.securityidiots.com/) | Très bon blog plein d'information sur l'infosec |

## 13. Une suite par Bluekeys ?

{% hint style="info" %}
Suivez nous sur [Discord](https://discord.gg/2XGRFKDWeC) ainsi que notre future chaîne twitch pour plus d'informations, la bienveillance amène la bienveillance !
{% endhint %}

