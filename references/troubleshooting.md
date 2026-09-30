---
description: "Erreurs Gradle/Actions fréquentes et correctif minimum."
connections: [github-tools, workflow-template]
---

# Dépannage APK Actions

| Symptôme | Cause probable | Correctif |
|---|---|---|
| `gradlew: Permission denied` | bit d'exécution | `chmod +x gradlew` puis commit |
| `SDK location not found` | ANDROID_HOME | setup-java + cmdline-tools / sdkmanager platforms+build-tools |
| `compileSdk` / platform manquant | SDK trop vieux | installer `platforms;android-XX` demandé par Gradle |
| `assembleRelease` échoue, debug OK | signingConfig | passer en debug ou brancher les secrets |
| `Keystore was tampered with` | mauvais mot de passe / JKS | ne pas brute-force ; demander le secret existant |
| Release créée sans `.apk` | path artifact faux | vérifier `app/build/outputs/apk/release/*.apk` |
| MAJ in-app refuse l'install | autre signature ou autre package | restaurer le certificat d'origine |
| Minutes Actions épuisées | quota free | le dire ; ne pas relancer en boucle |
| Workflow existe mais trigger 404 | mauvais `workflow_id` | `list_workflows` puis utiliser le filename exact |
| Push rejeté | protections de branche | commit sur une branche + PR |
| APK trop grosse / OOM | heap Gradle | `ORG_GRADLE_JVM_ARGS=-Xmx2g` dans le job |

## Lecture des logs

1. `list_workflow_jobs` sur le `run_id`
2. `get_job_logs` `failed_only: true`
3. Prendre la **première** tâche `FAILURE`, pas le bruit final

Ne pas réécrire tout le projet pour un échec SDK.
