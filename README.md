# Challenge - Inspection et Analyse d'un Repository GIT

## Consignes générales

Ce challenge a pour but d'évaluer votre capacité à **explorer, comprendre et analyser l'historique d'un projet GIT**.

### Règles

- **Aucune interface graphique n'est autorisée**, vous devez travailler **exclusivement en ligne de commande** (sauf pour le FORK depuis Github)
- **L'utilisation d'outils d'intelligence artificielle est strictement interdite.**
- Vous pouvez utiliser la documentation à l'adresse suivante: https://git-scm.com/book/fr/v2
- **Objectif : comprendre l'évolution du code et reconstituer les décisions prises.**

## Travail à effectuer

Le dépôt d'origine à utiliser est disponible à l'adresse suivante :
```bash
https://github.com/ETML-RRY/324_inspection_git.git
```

### Partie 1 - Préparation

1. Faites un *FORK* du dépôt sur votre compte GitHub (Attention il faut copier toutes les branches donc il faut **décocher** la case "Copy the main branch only" sur l'interface de Github)
2. Ajoutez votre enseignant comme collaborateur à votre dépôt forké.
3. Vous trouverez une réplique de ces instructions dans le fichier README.md de votre dépôt.
4. Répondez directement aux questions dans le fichier README.md qui est au format **Markdown**
5. Pour chaque points, veuillez noter la ou les commandes `git` utilisées vous permettant de répondre à la question.
6. Pour chaque partie, effectuez au minimum un commit et un push lorsque vous avez complété les réponses de la partie correspondante.

> Le format Markdown: [https://www.markdownguide.org/basic-syntax/](https://www.markdownguide.org/basic-syntax/)


### Partie 2 — Exploration de base

1. Combien de branches existent dans le dépôt ? Citez-les.  
```bash
 git branch -a
``` 
résultat:

>"* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/experiment/dark-mode
  remotes/origin/feature/header
  remotes/origin/feature/login
  remotes/origin/hotfix/typo
  remotes/origin/main"

2. Quels sont les **tags** disponibles ? A quoi correspondent-ils ?  
```bash
 git tag
```
résultat:

>"v0.1 v0.2"

Ensuite:

```bash
git log
```

ça correspond au commits :

>commit 4d28bc59eee6c8f52277bf505642a0e8e3595674 **(tag: v0.1)**
Author: Romain Rosay <romain.rosay@eduvaud.ch>
Date:   Wed Nov 5 22:15:09 2025 +0100

>Initial commit: structure HTML/CSS/JS + README + docs

et

>commit 5d66c29ef71dcd9f8e91794bf4c8591a9a56823d **(tag: v0.2)**
Merge: 1b682c9 6b6d950
Author: Romain Rosay <romain.rosay@eduvaud.ch>
Date:   Wed Nov 5 22:15:18 2025 +0100

>merge(feature/header): intègre la navigation

3. Quelle est la **branche principale** du projet ?
```bash
git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'
```
résultat:

> main

### Partie 3 — Historique et commits

4. Quel est le message du **premier commit** du projet ?  
```bash
git log --reverse
```
résultat: 

> Initial commit: structure HTML/CSS/JS + README + docs

5. Trouvez le commit où une **clé API** a été ajoutée par erreur. Quel est son identifiant (hash court) ?  

```bash
git log --grep="API"
```

résultat: 

> commit bea2d1aeaecd11e9c0af36cf6f052f65e82d36c5
Author: Romain Rosay <romain.rosay@eduvaud.ch>
Date:   Wed Nov 5 22:15:16 2025 +0100

    chore(config): AJOUT TEMPORAIRE d'une clé API (à retirer)

6. Quel commit a ensuite corrigé cette erreur ?  

```bash
git log --grep="API"
```

résultat: 

>commit 1b682c91ef14cda333419e2e387a53033ae575a1
Author: Romain Rosay <romain.rosay@eduvaud.ch>
Date:   Wed Nov 5 22:15:17 2025 +0100

    chore(config): retire la clé API et documente la bonne pratique

7. Trouvez le commit où le **titre de la page d'accueil** a été corrigé.  

```bash
git log --grep="titre"
```

résultat: 

>commit 6317c073f7514d580522c90fa1f0f0402066a48f (origin/hotfix/typo)
Author: Romain Rosay <romain.rosay@eduvaud.ch>
Date:   Wed Nov 5 22:15:21 2025 +0100

    hotfix: corrige la typo 'Wolrd' dans le titre

8. Quel est le message du commit qui a **ajouté le fichier `CHANGELOG.md`** et quelle commande avez-vous utilisé ?

```bash
git log --oneline */CHANGELOG.md
```

résultat:

> ed62890 docs: ajoute un changelog de base


### Partie 4 — Branches et fusions

9. Quelles branches ont été fusionnées dans `main` ?  

```bash
git branch -a --merged main
```

> * main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/header
  remotes/origin/feature/login
  remotes/origin/hotfix/typo
  remotes/origin/main


10. Quelle branche **n'a pas été fusionnée** ? Pourquoi, selon vous ? 

```bash
git branch -a --no-merged main
```
résultat:

>  remotes/origin/experiment/dark-mode

Je suppose que vu que cette fonctionnalité est dans une branche qui s'appelle "experiment", cette fonctionnalité n'a pas été accomplit, et n'a pas été mergé dans la main.

### Partie 5 — Analyse du contenu

11. Quelle est la **différence principale** entre les fichiers `index.html` 
dans les versions `v0.1` et `v0.2` et quelle commande permet de le voir rapidement ?

```bash
git diff v0.1 v0.2 -- index.html
```

résultat:

```html diff --git a/index.html b/index.html
index 2019876..60ce2f7 100644
--- a/index.html
+++ b/index.html
@@ -6,6 +6,7 @@
   <link rel="stylesheet" href="style.css">
 </head>
 <body>
+  <header><nav><a href="/">Accueil</a> | <a href="#">Contact</a></nav></header>
   <!-- NOTE HISTORIQUE: ancien slogan modifié plusieurs fois -->
   <h1>Bienvenue sur notre site Wolrd!</h1>
   <p id="tagline">Un site tout simple pour apprendre Git.</p>
```

12. Que contient la branche `feature/login` ?  

```bash
git show origin/feature/login
```

résultat: 

```bash
commit 74cc148bf664c82b26a8bf5643c49f7bea1d58a9 (origin/feature/login)
Author: Romain Rosay <romain.rosay@eduvaud.ch>
Date:   Wed Nov 5 22:15:14 2025 +0100

    feature(login): ajoute page de connexion et pseudo-fonction JS

diff --git a/login.html b/login.html
new file mode 100644
index 0000000..ff96acb
--- /dev/null
+++ b/login.html
@@ -0,0 +1,13 @@
+<!doctype html>
+<html lang="fr">
+<head><meta charset="utf-8"><title>Connexion</title></head>
+<body>
+  <h1>Connexion</h1>
+  <form>
+    <label>Email <input type="email"></label>
+    <label>Mot de passe <input type="password"></label>
+    <button type="submit">Se connecter</button>
+  </form>
+  <!-- TODO: validation côté client -->
+</body>
+</html>
diff --git a/script.js b/script.js
index d6ab491..0234cd0 100644
--- a/script.js
+++ b/script.js
@@ -4,3 +4,8 @@ document.addEventListener('DOMContentLoaded', () => {
   if (p) p.textContent += " (v0)";
   // TODO: extraire ce message depuis config.js plus tard
 });
+
+// TODO: branch login - gérer la soumission
+function fakeLogin(email) {
+  return email && email.includes('@');
+}
```

13. Dans quelle branche a été ajouté le code pour le **mode sombre** ? 

On regarde les branches existantes afin de réperer une qui contient le mode sombre -> 

```bash
git branch -a
```

résultat :

> remotes/origin/experiment/dark-mode

14. Quelle bonne pratique de sécurité est évoquée dans les commits du fichier `config.js` ?

```bash
git log -p -- config.js
```

-// Configuration (ne doit PAS contenir de secrets en prod)
-export const API_KEY = 'sk_live_NE_PAS_UTILISER_abc123xyz';
+// (clé API retirée; utiliser des variables d'environnement)

### Partie 6 — Réflexion

15. Pourquoi est-il important de **taguer** des versions dans un projet ?  

Pour marquer une version stable d'un projet avec un nom compréhensible au lieu d'un simple numéro de commit.

16. Que peut-on déduire du style de travail de l'équipe à partir de cet historique GIT ?  

Les branches sont bien séparées, etc

Bonne chance, et surtout... **ne vous perdez pas dans le log !** 😉