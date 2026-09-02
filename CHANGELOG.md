# Changelog

## [2.1] — 2026-09-02

Période : 26 août – 2 septembre 2026  
Base : `99a03cb` → `73e7b87` (18 commits)

### Version et identité

- Version passée de **1.157-ALPHA2** à **2.1**
- Copyright mis à jour : **© 2025-2030 Funregis**
- **Funregis** ajouté aux crédits développeurs du menu principal
- README mis à jour pour indiquer que ce fork est basé sur [NUSspli de V10lator](https://github.com/V10lator/NUSspli)

### Réseau et téléchargements

#### Réinitialisation réseau (`resetNetwork`)

- Suppression du retour anticipé qui empêchait parfois la réinitialisation
- Correction de l'utilisation de `ACGetCloseStatus()` (codes 0 = succès, 1 = en cours)
- Ajout d'un **timeout** avec attente active (polling toutes les 10 ms)
- Cycle complet de déconnexion/reconnexion : `ACFinalize` → `socket_lib_finish` → `socket_lib_init` → `ACInitialize` → `ACConnect`
- En cas d'erreur **« Error closing network! »**, réinitialisation forcée de la connexion au lieu de rappeler `ACClose()` en boucle
- Nettoyage correct en cas d'échec de reconnexion

#### libCURL / sockets

- Les options socket (WinScale, TCP SACK, etc.) ne font plus échouer le transfert si non supportées (`ENOPROTOOPT`)
- Ajout de `CURLOPT_NOSIGNAL`, `CURLOPT_CONNECTTIMEOUT` (30 s) et `CURLOPT_MAXREDIRS`
- Refactorisation de `initDownloader()` avec gestion d'erreurs simplifiée
- **Nouvelle tentative sur connexion fermée** : `curlReuseConnection = false` après toute erreur pour forcer une nouvelle connexion
- Ajout d'un bundle de certificats CA (`data/ca-certs.pem`)

#### Messages d'erreur réseau

- Messages d'erreur **détaillés et contextualisés** selon le code curl (DNS, timeout, SSL, etc.)
- Traduction des erreurs curl via `localise()`
- Correction du compte à rebours de reprise automatique (affichage des secondes restantes)
- Nouvelles traductions : `Error closing network!`, `Error connecting to network!`, et messages réseau associés

### Notifications

- Rumble Wiimote et LED GamePad gérés dans le même thread de notification
- La LED reste allumée **4 secondes** avant de s'éteindre
- Le rumble Wiimote n'est activé que si la méthode rumble est sélectionnée
- Le rumble GamePad reste géré séparément via `VPADControlMotor`
- Extinction de la LED à la fermeture des notifications

### Stabilité et mémoire

- **Suppression des buffers statiques partagés** (`getStaticScreenBuffer`, `getStaticLineBuffer`, `getStaticPathBuffer`)
- Remplacement par des buffers sur la pile dans tout le code (downloader, menus, renderer, etc.)
- Corrections de troncature de texte dans le renderer (débordement de buffer, ellipsis)
- Mise à jour importante de **SDL_FontCache**

### Localisation

- Ajout de nombreuses chaînes manquantes dans les **8 langues** (français, allemand, espagnol, italien, portugais, portugais brésilien, turc, gallois)
- Erreurs réseau, SSL et sauvegarde de `title.tmd` / `title.tik` désormais traduites

### Build

- **Makefile** : passage à `-O3`, `--gc-sections`, suppression du LTO/agressif `-Ofast`
- **Dockerfile** : flags de compilation ajustés, bibliothèques statiques sans PIC
- **build.py** : exécution de `SDL2/setup.sh` via `sh`, téléchargement des certificats sans vérification SSL

### Commits

| Date   | Commit    | Description                                      |
|--------|-----------|--------------------------------------------------|
| 26/08  | `4ca67c5` | Optimisations, corrections, fiabilisations       |
| 26/08  | `a721f40` | Optimisations, corrections, fiabilisations       |
| 26/08  | `e7d7d81` | Optimisations, corrections, fiabilisations       |
| 26/08  | `1ca4957` | Optimisations, corrections, fiabilisations       |
| 26/08  | `7876223` | Optimisations, corrections, fiabilisations       |
| 26/08  | `3be7668` | Optimisations, corrections, fiabilisations       |
| 26/08  | `c9ea831` | Optimisations, corrections, fiabilisations       |
| 26/08  | `6e5b7cf` | Optimisations, corrections, fiabilisations       |
| 26/08  | `80c8025` | Mise à jour README                               |
| 26/08  | `c571ca0` | Amélioration des notifications                   |
| 26/08  | `f7d41e1` | Mise à jour build                                |
| 26/08  | `586d87f` | Mise à jour version → 2.1                        |
| 26/08  | `cc2be0b` | Localisation, optimisations, corrections         |
| 27/08  | `7019cd8` | Meilleure gestion réseau (sockets, reset)        |
| 27/08  | `c3e04fa` | Meilleure gestion réseau                         |
| 01/09  | `3a5e822` | Meilleure gestion réseau (AC API, timeout)       |
| 01/09  | `701d7ab` | Retry sur connexion fermée + certificats CA      |
| 02/09  | `73e7b87` | Ajout Funregis aux crédits                       |
