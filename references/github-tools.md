---
description: "Outils GitHub à utiliser pour auditer, pousser, lancer Actions et récupérer une APK."
connections: [workflow-template, troubleshooting, chasmet-repos]
---

# Outils GitHub (connecteur)

Appeler `search_connected_tools` seulement si un nom d'outil manque. Noms stables :

| Besoin | Outil | Notes |
|---|---|
| Login | `github___get_me` | Confirmer `Chasmet` |
| Trouver un repo | `github___search_repositories` | `user:Chasmet <nom>` |
| Arborescence | `github___get_repository_tree` | `recursive: true`, filtrer `android`, `gradle`, `.github` |
| Lire un fichier | `github___get_file_contents` | Récupérer le SHA avant update |
| Chercher un workflow | `github___search_code` | `user:Chasmet path:.github/workflows assembleRelease` |
| Lister workflows | `github___actions_list` | method `list_workflows` |
| Lister runs | `github___actions_list` | method `list_workflow_runs` + `resource_id` = fichier yml |
| Détail run | `github___actions_get` | method `get_workflow_run` |
| Artifacts | `github___actions_list` | method `list_workflow_run_artifacts` |
| Télécharger artifact | `github___actions_get` | method `download_workflow_run_artifact` |
| Logs | `github___get_job_logs` | `failed_only: true` + `run_id` |
| Lancer | `github___actions_run_trigger` | method `run_workflow`, `ref` = branche, `workflow_id` = `build-apk.yml` |
| Re-run | `github___actions_run_trigger` | `rerun_workflow_run` ou `rerun_failed_jobs` |
| Commit multi-fichiers | `github___push_files` | Branche explicite |
| Update 1 fichier | `github___create_or_update_file` | `sha` obligatoire si le fichier existe |
| Releases | `github___list_releases` / `get_latest_release` | Pas d'outil create-release dédié : la Release se fait dans le workflow via `gh release` |

## Ordre minimal

1. `get_me`
2. tree + workflows
3. `list_workflow_runs` (status récente)
4. trigger ou push
5. logs si rouge
6. artifacts + latest release si vert

## Interdit

- Promettre une Release si le workflow n'a pas `gh release create/upload`
- Dire "APK prête" sur un run `in_progress` ou `failure`
- Modifier le keystore ou le `applicationId` sans demande explicite
