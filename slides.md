---
theme: seriph
background: baleinev-2023.png
class: homepage
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: BeePlace
fonts:
  sans: 'Roboto'
---

# Soutenance du TB

BeePlace - Recréer l'expérience collaborative du r/place de Reddit

<footer class="flex items-center justify-between w-full absolute left-0 bottom-0">
  <span>Valentin Kaelin</span>
  <span>24.08.2023</span>
</footer>

---
layout: two-cols
---

# Sommaire

<br>
<br>

- Contexte
- Problématique
- Solution et contraintes
- Conception et réalisation
- Conclusion
- Démonstration

::right::

<img src="/undraw_Random_thoughts.png" class="mt-16"/>

---
layout: cover
background: baleinev-2023.png
---

# Contexte

---
layout: image-right
image: pmw.jpg
class: col-padding no-subtitle
---

# Baleinev Festival

**Association Baleinev**

* À la HEIG-VD
* Festival de musique depuis près de 30 ans

**Pimp My Wall**
* Nouveau concept depuis 2014 : Pimp My Wall
* Application de dessin collaboratif

**Depuis 2018**
* BeeScreens: nouvelle version open source
* Collection d'applications interactives
* Cadre pouvant sortir du festival

<!-- 
Permet aux festivaliers de dessiner en temps réel sur les murs de l'école.

Collaboration

Site donc accessible depuis leur smartphone

Utilisation d'écrans / de projecteurs.

BeeScreens: 
- Volonté d'utiliser des technologies modernes
- Ajout d'une nouvelle app le plus simple possible -> idée de ce TB
-->

---
layout: cover
background: baleinev-2023.png
---

# Problématique

---
layout: two-cols
---

# Liberté de Pimp My Wall

<br>

Débordements en fin de soirée (oeuvres inappropriées)

<div class="spacer"/>

Modération difficile et chronophage

<div class="spacer"/>

Affecte les autres utilisateurs: gâche les dessins

<br>
<br>

**↳ Expérience frustrante pour nous et les festivaliers**

::right::

<img src="/pmw-debordement.png" class="w-full absolute -right-10 -mt-10"/>

<!--
Même si l'utilisateur veut dessiner qqch d'inapproprié, cela lui prend du temps.

Si on modère, il perd son temps et doit tout recommencer.
-->

---
layout: cover
background: baleinev-2023.png
---

# Solution et contraintes

---
layout: two-cols
class: no-subtitle col-padding
---

# Solution

**r/place de Reddit**

<div class="spacer"/>

Toile partagée par des millions d'utilisateurs

1 pixel par personne toutes les 5 minutes

Encourage la collaboration

Occasionnel pendant une courte période (2017, 2022 et 2023) ⇒ forte rivalité

<br>

**↳ S'inspirer de r/place pour créer sa variante open source : BeePlace**

::right::

<img src="/rplace.png" class=""/>
<p class="text-sm">Résultat du r/place de 2022</p>

<!--
Solution: s'inspirer du concept de r/place de Reddit

Reddit: plus grand forum au monde

r/place en 2017 puis 2022 et récemment 2023 (mois passé)

Utilisateurs peuvent choisir la couleur parmis une palette définie
-->

---
layout: two-cols
class: no-subtitle
---

# Contexte physique

**Les festivaliers**

* Festivaliers utilisent leur propre matériel 
* ⇒ Utilisée principalement sur smartphone
* Optimisation pour ce médium
* (Taille écran, utilisation tactile, etc.)

<br>

**La HEIG-VD**

* Diffusion sur les murs de l'école
* Mode affichage nécessaire
* Séparer l'affichage entre plusieurs écrans

::right::

<img src="/heig-screens.png" class=""/>
<p class="text-sm text-gray-700 dark:text-gray-300 italic">Crédits: Kevin Pradervand</p>

---
layout: two-cols
class: no-subtitle
---

<h1 class="whitespace-nowrap">
  Garantir le fonctionnement de l’application <br>lors du festival
</h1>

Assurer la scalabilité

Garder une latence faible (bonne UX)

Pouvoir gérer un nombre élevé d'utilisateurs simultanés

Pics de fréquentation lors du festival

<div class="spacer"/>

**Ordre de grandeur**

| Reddit                        | Baleinev Festival |
|-------------------------------|-------------------|
| 10.5 millions de participants | 1500 festivaliers |

::right::

<img src="/undraw_stepping_up.png" class="mt-20 absolute -right-12"/>

<!--
Latence faible: assurer une bonne UX

1500 festivaliers max => prévoire au minimum quelques centaines d'utilisateurs simultanés
-->

---
layout: cover
background: baleinev-2023.png
---

# Conception et réalisation

---
layout: two-cols
---

# Besoins de l'application

<br>
<br>

**Fonctionnalités**

* Temps réel
* Stocker les pixels
* Interface pour naviguer dans le canvas
* Identifier les utilisateurs

::right::

<img src="/undraw_Scrum_board.png" class="mt-18"/>

<!--
Temps réel: pour la pose de pixels et les pixels des autres utilisateurs

Stockage des pixels: pour l'état de la toile

Interface: pour naviguer dans le canvas: application web optimisée pour le mobile

Identifier les users: pour limiter la fréquence de pose de pixels (vérifier le temps d'attente)

Pour répondre à ces besoins, je suis arrivé à la stack suivante (slide suivante)
-->
---

# Technologies

<img src="/technos.png" class="-mt-4"/>

---
layout: two-cols 
---

# Identification

<br>

**Pour limiter la fréquence d'ajout de pixels**

* Plus simple possible pour les festivaliers
* Sans nuire à la fluidité de l'expérience

**Solution**

* Authentification par empreinte digitale (fingerprint)
* Librairie FingerprintJS (open source)

**Problèmes**

* Risque de collisions
* Possibilité de bots (générée côté client)

::right::

<img src="/fingerprint.png" class="mt-20 absolute -right-15"/>

<!--
Pas possible d'utiliser des comptes classiques: trop long

Même si connexion via Google ou autre réseau

Problème de collisions car librairire free (env 60% d'accuracy), sur un petit dataset aucune collision trouvée (10-20 personnes).
Problème si collision: temps d'attente partagé
Bot: envoyer une fingerprint différente / random à chaque requête

Solution: on verra plus tard dans la conclusions
-->

---
layout: two-cols
class: no-subtitle
---

# Canvas

<br>

**Canvas HTML5**

Permet d'afficher une image matricielle

Représentation des pixels de la toile

Possible de dessiner les pixels individuellement ou plusieurs d'un bloc

<br>
<br>

**Mais comment stocker les pixels ? 🤔**

::right::

<img src="/load-test-result.png" class="rounded-md mt-20"/>

<!-- 
Canvas élément déjà existant en HTML5, afficher du contenu 2D ou 3D

Bloc de pixels: tableaux Javascript

Structure de données, comment faire pour que ce soit performant -> slide suivant 
-->

---
layout: two-cols 
class: no-subtitle
---

# Stockage

**3 niveaux de stockage**

1. Dans la mémoire de l'application
2. Bitfield Redis
3. Base de données PostgreSQL

<div class="spacer"/>

**Quoi stocker ?**

La position et la couleur de chaque pixel

<div class="spacer"/>

**Bitfield Redis**

Permet de ne stocker que la couleur

La position est implicite

::right::

Structure du Bitfield Redis:

<img src="/bitfield-redis.png"/>

<p class="text-sm text-gray-700 dark:text-gray-300 italic !mt-0">Crédits: Daniel Ellis (Reddit)</p>

Structure SQL:
<img src="/sql-pixels-table.png" class="w-55"/>

<!--

Bitfield Redis: parler du schéma

Pourquoi aussi en mémoire ? Profling: bitfield (string) -> tableau JS couteux

=> Redis utilisé lorsque l'app redémarre ou que l'on change la config

Base de données SQL pas encore utilisée, que pour l'historique de tous les pixels.
Pour de futures statistiques, j'en parlerai dans les perspectives futures
-->

---
layout: two-cols
class: no-subtitle
---

# Backend

**Nest.js**

* Architecture DDD
* ORM Prisma (PostgreSQL)
* Configuration par variables d'environnement
* Communication avec les clients via Socket.IO


**Administration**

* Endpoints HTTP protégés (stratégie d'API Key)
* Actions:
  * Read only
  * Remise à zéro de la toile
  * Recouvrir une zone de pixels

::right::

<br>
<br>
<br>

**Événements Socket.IO**

<div class="">
  <img src="/backend-animation/1.png" class="absolute"/>
  <v-clicks>
    <img src="/backend-animation/2.png" class="absolute"/>
    <img src="/backend-animation/3.png" class="absolute"/>
    <img src="/backend-animation/4.png" class="absolute"/>
  </v-clicks>
</div>

<!---
DDD: Domain Driven Design, découpage par domaine
ORM: Object Relational Mapping, permet de manipuler la base de données comme des objets

Config: env var avant le futur dashboard
Exs: taille du canvas, couleurs, nombre de pixels que l'utilisateur peut poser, etc.

Un des domaines, justement la gestion avec la communication temps réel (Socket.IO)
Montrer l'animation du schéma
-->

---
layout: two-cols
---

# Frontend

<br>

**Next.js**

* Communication: se greffe aux WebSockets du Backend avec Socket.IO
* Stockage de l'état dans un state global
* PinchZoom du canvas
* Mode affichage

<br>

**Design**
* Utilisation de TailwindCSS

::right::

<img src="/screenshot-app.png" class="w-90 absolute right-0 -top-8"/>

<!--
State global: Zustand

TailwindCSS: utilitaire de classes CSS (design system)
Pas bcp de design à faire donc tout custom

Parler de l'interface avec le screen
-->

---
layout: two-cols 
---

# Montée en charge

<div class="flex items-baseline space-x-8 mb-2 -mt-3">
  <strong>Outils utilisés</strong>
  <div class="flex items-baseline space-x-3">
    <img src="/logos/k6.png" class="w-12"/>
    <img src="/logos/clinicjs.png" class="w-12"/>
  </div>
</div>

* k6 pour les tests de montée en charge
* Clinic.js pour le profiling (flame graph)
* Métriques (avant que la latence soit `> 1.2s`):
  * Nombre d'utilisateurs virtuels
  * Nombre de pixels dessinés

<div class="mt-4 mb-2">
  <strong>Méthodologie</strong>
</div>

  1. Récupérer les résultats avant optimisations
  2. Optimiser avec Clinic.js localement
  3. Déployer sur la machine virtuelle de l’école
  4. Lancer les tests avec k6
  5. Comparer les résultats des tests avec les initiaux

<br>

::right::

<img src="/k6-report.png" class="-mt-6 rounded-lg"/>

<img src="/appendix/flame3-getBoard.png" class="mt-4 rounded-lg"/>

<!--
Déployée sur une seconde VM de l'école pour les tests

k6: permet de créer des tests en JS/TS
Test créé: test websocket qui simule les événements d'un client et écoute les réponses
Pas possible d'utiliser Socket.IO de base => module créé

Breakpoint Test: crée des users virtuels qui se connectent et dessinent sur la toile (3 pixels chacun)
Jusqu'à ce que la latence soit supérieure à 1.2s
Latence: temps de la connexion WebSockets (temps avant de recevoir le board avec les pixels etc)

Clinic.js: plusieurs outils dispos mais utilisé le flame pour le profiling
Faut lancer clinic puis lancer l'app, lancer les tests et ensuite stopper l'app pour avoir le graph
-->

---
layout: two-cols
class: no-subtitle
---

# Résultats

**Résultats initiaux**
* Utilisateurs virtuels : **558**
* Pixels dessinés : **1017**

**Résultats finaux**
* Utilisateurs virtuels : **1348.5** (<span class="text-green-600 font-semibold">+ 141.67%</span>)
* Pixels dessinés : **3078.5** (<span class="text-green-600 font-semibold">+ 202.70%</span>)


**Optimisations**
* Broadcast des pixels avec un intervalle de temps
* Format plus léger pour l'envoi des pixels
* Cache du Bitfield Redis en mémoire
* Configuration de l'OS Linux

::right::

<img src="/opti-users.png" class="w-80"/>
<img src="/opti-pixels.png" class="w-80 mt-10"/>

<!--
— Optimisation du broadcast des pixels avec un intervalle de temps ;
— Utilisation d’un format sous forme de chaîne de caractères plus léger que le JSON pour
envoyer les pixels ;
— Mise en cache du Bitfield Redis dans la mémoire de l’application ;
— Installation de dépendances optionnelles pour Socket.IO permettant d’accélérer certaines
opérations ;
— Optimisation de la configuration du système d’exploitation Linux (nombre maximal de
fichiers ouverts en simultané).
-->


---
layout: cover
background: baleinev-2023.png
---

# Conclusion


---
layout: two-cols
class: no-subtitle
---

# Conclusion technique

**Cahier des charges**

✅ Fonctionnalités _required_ et _essential_

✅ Moitié des fonctionnalités _nice to have_

✅ Fonctionnalités non prévues

<br>

**Objectifs accomplis (Baleinev 2023)**

* Moins de débordements
* Plus de collaboration
* Expérience plus positive que Pimp My Wall

::right::

<div class="h-[48px]"/>

**Retour d'expérience**

* Application fonctionnelle et déployée
* Technologies bien choisies, aucun réel blocage
* Optimisations concluantes, permet de tenir tous les festivaliers 🕺💃
* Apprentissages utiles pour le futur

**Améliorations possibles**

* Accessibilité de l'app:
  * Tutoriel, textes informatifs
  * Internationalisation
* Tests unitaires et d'intégration

<!--
Objectifs vérifiés: grâce au test réalisé lors du Baleinev 2023

Fonctionnalités non prévues:
  - Mode affichage plus poussé
  - Package pour partager le code
  - Historique dans la base de données SQL

Apprentissages:
  - Surtout aspect tests de montée en charge, profiling, et optimisation
  - k6 bon outil à connaître, bcp utilisé
-->
---
layout: two-cols
class: no-subtitle
---

# Conclusion personnelle

**Technique**

* Projet qui me tient à coeur
* Bénéfices de réaliser un projet plus long et conséquent
* Retours lors du Baleinev Festival 2023 encourageants

**Organisationnel**

* Travail seul sur un projet mais au sein d'une équipe 
* Review du code bénéfique, permet d'améliorer la qualité
* Bonnes pratiques du monde professionnel (daily meeting, sprint review, ...)

::right::

<img src="/beescreens-team.png" class="mt-29"/>

<!--
Chance d'avoir travaillé sur un projet qui me passionne, c'était un plaisir d'ajouter petits à petits des fonctionnalités.

-->


---
layout: two-cols
class: col-padding
---

# Perspectives futures

<br>

**Organisation**

* Tests lors du Baleinev 2024
* Lier le monde physique pour résoudre les problèmes d'authentification

<br>

**Développement**

* Dashboard d'administration
* Statistiques
* Toile rectangulaire

::right::

<div class="absolute w-full h-full bg-black transform scale-[1.2] -right-10">
</div>

<img src="/ticket-baleinev.png" class="relative mt-29"/>

<!--
Baleinev 2024 -> BeePlace sur les tours et pas que sur des petits écrans du rez
Plus grande échelle grâce au mode affichage

Lier monde physique: QRCode sur billet ? Ou numéro unique ?
Autre idée d’auth: borne de pixels, QR code à scanner qui te file une session de X minutes

Dashboard admin: faciliter la modération
Ex: sélectionner la zone à modérer

Stats, ex: 
- nb de pixels placés par jour, heure, minute avec des graphiques de l’évolution
— nb de pixels placés par utilisateur
— nb de pixels placés en fonction de sa couleur
— nb de pixels placés par zone, en réalisant une sorte d’"heat map" de la toile
— Analyse des actions de modération, affichage par exemple des oeuvres qui ont dû être
supprimées.
-->

---
layout: iframe-right
url: https://place.beescreens.ch/
---

# Démonstration

<br>

À vos pixels !

[place.beescreens.ch](https://place.beescreens.ch)

<img src="/qrcode.png" class="w-[250px] mx-auto mt-8"/>

---
layout: cover
background: baleinev-2023.png
---

# Merci de votre attention !
<br>
Avez-vous des questions ?

---
layout: two-cols
---

# Sources

<br>
<br>

* Photos: Antoine Kaelin & Kevin Pradervand
* Illustrations: unDraw

::right::

<img src="/undraw_Landscape_photographer.png" class="mt-10"/>

---
layout: cover
background: baleinev-2023.png
---

# Annexes

---
layout: image
image: appendix/display-mode-config.png
---

---
layout: image
image: appendix/display-mode-example.png
---

---
layout: two-cols
---

<img src="/appendix/sequence-websockets-connection.svg" class="mt-32 absolute -left-10"/>

::right::

<img src="/appendix/sequence-websockets-pixels.svg" class="mt-28 absolute -right-10"/>
