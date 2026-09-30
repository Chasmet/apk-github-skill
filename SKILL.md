---
name: apk-github
description: "Compiler, signer et publier une APK Android via GitHub Actions (Gradle, artifact, Release, MAJ in-app). Use when the user asks for APK, assembleRelease, GitHub Release APK, workflow Android, mise à jour APK, or to rebuild an existing Chasmet Android app. NOT for PWA-only GitHub Pages, Play Store listing copy, or generating an APK inside the chat sandbox."
type: workflow
lifecycle: active
---

# APK GitHub — Build et Release Android

Grok ne fabrique pas l'APK dans le chat. Il pilote le dépôt GitHub. Le runner Actions compile avec Gradle.

Compte par défaut : `Chasmet`. Confirmer `github___get_me` si le login n'est pas évident.

## Règles

1. **Ne jamais inventer une APK.** Preuve = run Actions `success` + fichier `.apk` (artifact ou Release).
2. **Ne jamais coller un mot de passe keystore, un JKS, ni un secret dans le chat, le workflow, ou ce skill.** Utiliser GitHub Secrets / OIDC.
3. **Ne pas casser une signature existante.** Une nouvelle clé empêche la MAJ par-dessus l'ancienne app.
4. **Ne pas recréer un workflow** si `.github/workflows/*apk*.yml` ou `android.yml` existe déjà. L'auditer, puis le relancer.
5. Répondre en français, format court : résumé / sûr / bloqué / lien APK.

Lire `references/github-tools.md` avant le premier appel d'outil.
Lire `references/workflow-template.md` seulement pour un nouveau projet sans CI.
Lire `references/troubleshooting.md` si Actions échoue.
Lire `references/chasmet-repos.md` si le repo n'est pas nommé.

## Workflow

### 1. Identifier le dépôt

- Si l'utilisateur donne `owner/repo` : l'utiliser.
- Sinon : chercher `user:Chasmet` puis matcher le nom d'app.
- Vérifier les droits : `push` requis pour commit + trigger.

### 2. Auditer avant de toucher

Lister :

- `app/build.gradle` ou `app/build.gradle.kts`
- `settings.gradle*`
- `gradlew` (présent ou pas)
- `.github/workflows/`
- Releases existantes
- Derniers workflow runs

Décision :

| Situation | Action |
|---|---|
| Workflow APK existe et passe | Trigger `workflow_dispatch` ou push version |
| Workflow existe mais rouge | Lire logs, corriger le minimum, re-run |
| Projet Android sans workflow | Ajouter `assets/android-apk.yml` adapté |
| PWA HTML seulement | Capacité WebView/Cordova/PWABuilder — ne pas prétendre Gradle |
| Pas de projet Android | Le dire. Ne pas fake une APK |

### 3. Préparer la build

- Garder `on.workflow_dispatch` pour relancer sans nouveau commit.
- `permissions.contents: write` si publication Release.
- Java 17 + Gradle 8.x sauf si le repo impose autre chose.
- Debug si pas de signing. Release signée seulement si secrets/keystore déjà en place.
- Bump `versionName` / `versionCode` avant une vraie Release.

### 4. Pousser et lancer

1. `github___push_files` ou `create_or_update_file` (SHA obligatoire si le fichier existe).
2. `github___actions_run_trigger` method `run_workflow` avec `workflow_id` = nom du fichier (`build-apk.yml`) et `ref` = branche.
3. Attendre. Relire `list_workflow_runs` puis jobs/logs.
4. Si `success` : lister artifacts. Si le workflow publie une Release : `list_releases` / `get_latest_release`.

### 5. Livrer à l'utilisateur

Toujours donner :

- repo + branche
- run Actions (succès/échec)
- type d'APK : debug / release signée
- lien artifact ou Release
- si la MAJ in-app marchera (même package + même certificat)

## Sortie obligatoire

```
1. Résumé
2. Repo / workflow / run
3. APK : oui/non + type
4. Lien
5. Bloquant restant
```

Si échec : citer l'étape + 1 cause probable + 1 correctif. Voir `references/troubleshooting.md`.
