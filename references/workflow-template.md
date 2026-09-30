---
description: "Quand et comment ajouter un workflow APK. Copier assets/android-apk.yml, pas un workflow propriétaire existant."
connections: [github-tools, troubleshooting]
---

# Template workflow

Utiliser `assets/android-apk.yml` seulement si le repo n'a pas déjà une CI APK.

## Adapter avant push

- Chemin module : `:app` sauf settings.gradle différent
- Java 17 par défaut
- `assembleDebug` toujours
- `assembleRelease` signé seulement si secrets présents
- `workflow_dispatch` obligatoire
- `permissions.contents: write` si Release

## Secrets GitHub recommandés (release signée)

| Secret | Usage |
|---|---|
| `KEYSTORE_BASE64` | JKS/BKS encodé base64 |
| `KEYSTORE_PASSWORD` | store password |
| `KEY_ALIAS` | alias |
| `KEY_PASSWORD` | key password |

Ne jamais committer le JKS ni le mot de passe en clair dans le YAML.

## Deux modes valides chez Chasmet

1. **Secrets / OIDC** (ex. CHK Crypto) : clé hors repo, empreinte certificat vérifiée dans la CI.
2. **Keystore déjà dans le repo + workflow existant** : ne pas remplacer la clé. Relancer le workflow tel quel.

Si une app a déjà des utilisateurs : **même `applicationId` + même certificat**. Sinon la MAJ in-app casse.

## Publication Release (dans le workflow, pas dans le chat)

```bash
TAG="v${VERSION}"
gh release create "$TAG" app-release.apk --latest --title "App $VERSION"
# ou upload --clobber si le tag existe
```

Sans `gh release`, l'APK reste un artifact Actions (téléchargeable 30-90 jours).
