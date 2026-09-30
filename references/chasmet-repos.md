---
description: "Repos Android Chasmet déjà connus avec CI APK. Charger si l'utilisateur ne donne pas owner/repo."
connections: [github-tools]
---

# Repos Chasmet (sept. 2026)

Owner GitHub : `Chasmet`.

| App | Repo | Workflow | Notes |
|---|---|---|---|
| CHK Crypto | `Binance-bybyt-` | `build-apk.yml` | Release signée + MAJ. Signing OIDC Render. Ne pas changer le certificat. |
| Remix Studio | `Montage-vid-o-` | `build-apk.yml` | Release `RemixStudio.apk`. CI déjà complète. |
| ViralVoice | `ViralVoice` | `android-apk.yml` | Releases v4.x |
| Rush Studio | `rush-studio` | `android-apk.yml` | Releases v2.1.x |
| Fond vert | `Vid-o-fond-vert-` | `android-apk.yml` | |
| Anglais | `Application-anglais-` | `build-apk.yml` | |
| Cut vidéo | `Cut-vid-o-` | `android.yml` | |
| Blue Lock | `Trot-et-classe-les-personnages-de-blue-lock` | `android.yml` | |
| D-complex | `D-complex-gpt` | à vérifier | Java, **pas de Release** vue le 30 sept. 2026 |
| GLB viewer | `glb-viewer-apk` | PWA | Pas Gradle natif. APK via PWABuilder, pas assembleRelease |
| Installer | `APK-Installer-Web-CHK` | Java | Outil d'install, pas une app métier |

Si l'utilisateur dit "l'app montage" → `Montage-vid-o-`.
Si "crypto / binance" → `Binance-bybyt-`.
Si "rush" → `rush-studio`.
