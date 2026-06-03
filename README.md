# Mon Studio — application de pilotage pour prof / tuteur

Application web tout-en-un pour un professeur particulier : suivi des élèves, routine
hebdomadaire, tracking du temps (style Toggl) et agenda. Le tout dans **un seul fichier
HTML autonome**, sans étape de build.

> Ce dépôt est destiné à être repris par **Claude Code** pour le mettre en ligne sur un
> vrai site internet. Lis la section « Tâches pour Claude Code » plus bas.

---

## Contenu

- **`index.html`** — l'application complète (HTML + CSS + JavaScript, tout en un seul fichier).
  C'est la version **web-ready** : elle sauvegarde les données dans le `localStorage` du
  navigateur, donc elle persiste une fois hébergée.

## Fonctionnalités

L'appli a 5 onglets (navigation en bas sur mobile, à gauche sur ordinateur) :

1. **Aujourd'hui** — vue du jour : chrono en cours, RDV du jour, tâches de la routine du
   jour, et une section **Sauvegarde / Restauration** des données (export/import en texte JSON).
2. **Semaine** — routine hebdomadaire : chaque jour a ses tâches récurrentes, on les coche,
   et les coches se remettent à zéro chaque lundi (le contenu de la routine, lui, reste).
   Une routine par défaut est pré-remplie au premier lancement.
3. **Élèves** — pour chaque élève : prénom, « Objectifs de la semaine » et une « Note ».
   Ajout, édition, suppression.
4. **Temps** — timeline hebdomadaire façon Toggl (24 h, une colonne par jour). Deux façons
   d'enregistrer : chrono start/stop en direct, ou ajout manuel (début, fin, description).
   Affiche aussi les RDV en blocs.
5. **Agenda** — calendrier mensuel (grille type Google Agenda) avec les RDV.

## Stack technique

- HTML / CSS / JavaScript **vanilla**, aucun framework, aucune dépendance npm.
- Polices via Google Fonts (Fraunces + Spline Sans), chargées par `<link>`.
- **Aucune étape de build** : `index.html` se déploie tel quel comme site statique.
- Persistance : `localStorage` (clés préfixées `studio-`). Voir l'objet `store` et la
  constante `KEYS` dans le `<script>`.

---

## Mise en ligne (site statique)

C'est un site statique : il suffit d'héberger `index.html`. Options simples :

- **Netlify** — glisser-déposer le dossier sur https://app.netlify.com/drop, ou connecter le dépôt Git.
- **Vercel** — `vercel` depuis le dossier, ou import du dépôt.
- **GitHub Pages** — pousser le dépôt, activer Pages sur la branche `main` (racine).
- **Cloudflare Pages** — connecter le dépôt, pas de commande de build, dossier de sortie = racine.

Aucune commande de build n'est nécessaire (« build command » vide, « output directory » = racine).

---

## Points d'attention / personnalisation

- **Données Google Agenda** : actuellement, les RDV Google sont un **instantané statique**
  codé en dur dans la constante `GCAL_IMPORT` (dans le `<script>`). Il n'y a pas de
  synchronisation Google en direct dans cette version (une vraie synchro nécessiterait un
  backend OAuth + l'API Google Calendar — voir tâches optionnelles).
- **Routine par défaut** : modifiable via la constante `DEFAULT_ROUTINE`.
- **Couleurs / typo** : tout est dans les variables CSS `:root` en haut du fichier
  (`--accent`, `--bg`, etc.).
- **Sauvegarde** : l'utilisateur peut exporter/importer ses données en texte JSON depuis
  l'onglet « Aujourd'hui ». Format : `{ app:"mon-studio", v:1, routine, students, time, appts }`.

---

## Tâches pour Claude Code

Objectif : publier cette appli comme site web exploitable au quotidien.

1. **Initialiser le dépôt** et vérifier que `index.html` s'ouvre et fonctionne en local
   (les données doivent persister après un rechargement — c'est le `localStorage`).
2. **Déployer** sur l'un des hébergeurs ci-dessus (site statique, pas de build).
3. *(Optionnel, recommandé pour mobile)* Ajouter un **manifest PWA** + service worker pour
   pouvoir « ajouter à l'écran d'accueil » et utiliser l'appli comme une vraie app, hors-ligne.
4. *(Optionnel)* Pour une **vraie synchronisation Google Agenda** : mettre en place un petit
   backend (ex. fonction serverless) gérant l'OAuth Google et l'API Google Calendar, puis
   remplacer la lecture statique `GCAL_IMPORT` par un appel à cette API. Prévoir lecture
   (afficher les événements) et écriture (créer un RDV).
5. *(Optionnel)* Découper le fichier unique en `index.html` + `styles.css` + `app.js` si on
   préfère une structure de projet plus classique (purement cosmétique, non requis).

> Important : ne pas casser la couche `store` (elle gère à la fois `localStorage` pour le web
> et `window.storage` pour l'environnement Claude). Garder le préfixe de clés `studio-`.
