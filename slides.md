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
- Problèmes à résoudre
- Solutions
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
layout: two-cols 
---

# Baleinev Festival

Association Baleinev

* À la HEIG-VD
* Festival de musique depuis près de 30 ans
* Nouveau concept depuis 2014 : Pimp My Wall
  * Application de dessin collaboratif

<img src="/pmw-app.png" class="w-55 pl-6"/>


::right::

<img src="/pmw.jpg" class="mt-24"/>
<p class="text-sm text-gray-700 dark:text-gray-300 italic">Crédits: Antoine Kaelin</p>

<!-- 
Permet aux festivaliers de dessiner en temps réel sur les murs de l'école.

Collaboration

Site donc accessible depuis leur smartphone

Utilisation d'écrans / de projecteurs.

 -->


---
layout: two-cols 
---

# BeeScreens

Depuis 2018

<br>
<br>

* Nouvelle version open source
* Collection d'applications interactives
  * Faciliter l'ajout de nouvelles applications
* Cadre pouvant sortir du festival

::right::

<img src="/gitlab.png" class="w-30 pl-12 mb-10"/>

```plantuml {scale: 0.7}
@startuml

package "BeeScreens" {
  [Pimp My Wall]
  [Media Player]
  [BeePlace]
  [Any other app ...]
}
@enduml
```

<!--
Volonté d'utiliser des technologies modernes

Ajout d'une nouvelle app le plus simple possible -> idée de ce TB
-->

---
layout: two-cols
---

# r/place

reddit.com

<br>
<br>

* Concept proposé par Reddit
* Toile partagée
* 1 pixel par personne toutes les 5 minutes
* Encourage la collaboration

::right::

<img src="/rplace.png" class=""/>
<p class="text-sm">Résultat du r/place de 2022</p>

<!--
Reddit: plus grand forum au monde

r/place en 2017 puis 2022 et récemment 2023 (mois passé)

Utilisateurs peuvent choisir la couleur
-->

---
layout: cover
background: baleinev-2023.png
---

# Problèmes à résoudre

---
layout: two-cols
---

# 1. Liberté de Pimp My Wall

<br>

* Débordements en fin de soirée (oeuvres inappropriées)
* Modération difficile et chronophage

<br>

**Solutions:**

* Concept du r/place
* Temps d'attente entre chaque pixel
  * Besoin d'identifier les utilisateurs
* Modération plus efficace

::right::

<img src="/pmw-debordement.png" class="w-80 absolute right-0"/>

<!--
Même si l'utilisateur veut dessiner qqch d'inapproprié, cela lui prend du temps.

Si on modère, il perd son temps et doit tout recommencer.
-->

---
layout: two-cols
---

# 2. Application web

Contraintes externes

<br>

* Utilisée principalement sur smartphone
* Optimisation pour ce médium
* (Taille écran, utilisation tactile, etc.)

<br>
<br>

* Diffusion sur les murs de l'école
* Mode affichage nécessaire
* Séparer l'affichage entre plusieurs écrans

::right::

<img src="/heig-screens.png" class=""/>
<p class="text-sm text-gray-700 dark:text-gray-300 italic">Crédits: Kevin Pradervand</p>

---
layout: two-cols
---

# 3. Montée en charge

<br>
<br>

* Assurer la scalabilité
* Pouvoir gérer un nombre élevé d'utilisateurs simultanés:
  * Pics de fréquentation lors du festival
* Garder une latence faible (bonne UX)


::right::

<img src="/undraw_stepping_up.png" class="mt-10"/>

<!--
Latence faible: assurer une bonne UX
-->

---
layout: cover
background: baleinev-2023.png
---

# Solutions

---

# Technologies

<img src="/technos.png" class="w-[94%] -mt-3"/>

---
layout: two-cols 
---

# Identification

Pour limiter la fréquence d'ajout de pixels

* Plus simple possible pour les festivaliers
* Sans nuire à la fluidité de l'expérience

<br>

**Solution:**

* Authentification par empreinte digitale (fingerprint)
* Librairie FingerprintJS (open source)

<br>

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
---

# Frontend

<br>

**Next.js**

* Canvas HTML5
* Connexion WebSockets avec Socket.IO
* Stockage de l'état dans un state global
* PinchZoom du canvas
* Mode affichage

<br>

**Design**
* Utilisation de TailwindCSS



::right::

<img src="/screenshot-app.png" class="w-50 absolute right-22 -top-8"/>
<img src="/appendix/display-mode-config.png" class="w-90 absolute right-0 mt-65"/>

<!--
State global: Zustand

TailwindCSS: utilitaire de classes CSS (design system)
Pas bcp de design à faire donc tout custom
-->

---
layout: two-cols
class: backend
---

# Backend

**Nest.js**

* Architecture DDD
* ORM Prisma (PostgreSQL)
* Configuration par variables d'environnement


**Administration**

* Endpoints HTTP protégés
* Stratégie d'API Key
* Actions:
  * Read only
  * Remise à zéro de la toile
  * Recouvrir une zone de pixels

::right::

<img src="/backend-architecture.png" class="w-64 absolute right-10 mt-20"/>

<!---
DDD: Domain Driven Design, découpage par domaine
ORM: Object Relational Mapping, permet de manipuler la base de données comme des objets

Config: env var avant le futur dashboard
Exs: taille du canvas, couleurs, nombre de pixels que l'utilisateur peut poser, etc.
-->

---
layout: two-cols 
---

# Stockage

<br>
<br>

**3 niveaux de stockage:**

1. Dans la mémoire de l'application
2. Bitfield Redis
3. Base de données PostgreSQL

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

<!-- TODO: voir si un slide sur le package est nécessaire ici -->

<!-- ---

# Déploiement

TODO -->

---
layout: two-cols 
---

# Montée en charge

<div class="flex items-baseline space-x-8 mb-4">
  <strong>Outils utilisés</strong>
  <div class="flex items-baseline space-x-3">
    <img src="/logos/k6.png" class="w-12"/>
    <img src="/logos/clinicjs.png" class="w-12"/>
  </div>
</div>

* k6 pour les tests de montée en charge
* Clinic.js pour le profiling
  * Graphiques en flammes

<br>

**Résultats initiaux des tests**

* Utilisateurs virtuels : **558**
* Pixels dessinés : **1017**

::right::

<img src="/appendix/flame3-getBoard.png" class="mt-20"/>

<!--
Déployée sur une seconde VM de l'école pour les tests

k6: permet de créer des tests en JS/TS
Pas possible d'utiliser Socket.IO de base => module créé

Breakpoint Test: crée des users virtuels qui se connectent et dessinent sur la toile (3 pixels chacun)
Jusqu'à ce que la latence soit supérieure à 1.2s


Clinic.js: plusieurs outils dispos mais utilisé le flame pour le profiling

Faut lancer clinic puis lancer l'app, lancer les tests et ensuite stopper l'app pour avoir le graph
-->

---
layout: two-cols
---

# Optimisations

<br>

* Broadcast des pixels avec un intervalle de temps
* Format plus léger pour l'envoi des pixels
* Cache du Bitfield Redis en mémoire
* Configuration de l'OS Linux

<br>

**Résultats finaux**

* Utilisateurs virtuels : **1348.5** (<span class="text-green-600 font-semibold">+ 141.67%</span>)
* Pixels dessinés : **3078.5** (<span class="text-green-600 font-semibold">+ 202.70%</span>)

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
---

# Conclusion technique

<br>

**Objectifs**

✅ Fonctionnalités _required_ et _essential_

✅ Moitié des fonctionnalités _nice to have_

✅ Fonctionnalités non prévues:  
  <ul class="ml-5">
    <li>Mode affichage plus poussé</li>
    <li>Package pour partager le code</li>
    <li>Historique dans la base de données SQL</li>
  </ul>

::right::

<br>
<br>
<br>

**Retour d'expérience**

* Application fonctionnelle et déployée
* Technologies bien choisies, aucun réel blocage
* Optimisations concluantes, permet de tenir tous les festivaliers 🕺💃

**Améliorations possibles**

* Accessibilité de l'app:
  * Tutoriel, textes informatifs
  * Internationalisation
* Tests unitaires et d'intégration

---
layout: two-cols
---

# Conclusion personnelle

* Projet qui me tient à coeur
* Bénéfices de réaliser un projet plus long et conséquent
* Retours lors du Baleinev Festival 2023 encourageants

**Organisationnel**

* Travail seul sur un projet mais au sein d'une équipe 
* Review du code bénéfique, permet d'améliorer la qualité
* Bonnes pratiques du monde professionnel (daily meeting, sprint review, ...)

::right::

<img src="/beescreens-team.png" class="mt-20"/>

<!--
Chance d'avoir travaillé sur un projet qui me passionne, c'était un plaisir d'ajouter petits à petits des fonctionnalités.

-->


---
layout: two-cols
---

# Perspectives futures

<br>

**Organisation**

* Tests lors du Baleinev 2024
* Lier le monde physique
  * Résoudre les problèmes d'authentification 

<br>

**Développement**

* Dashboard d'administration
* Statistiques
* Toile rectangulaire

::right::

<img src="/ticket-baleinev.png" class="absolute right-0 mt-20 w-[90%]"/>

<!--
Baleinev 2024 -> BeePlace sur les tours et pas que sur des petits écrans du rez
Plus grande échelle grâce au mode affichage

Lier monde physique: QRCode sur billet ? Ou numéro unique ?

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

Venez placer vos pixels !

[place.beescreens.ch](https://place.beescreens.ch)

<img src="/qrcode.png" class="w-[250px] -ml-4"/>

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
