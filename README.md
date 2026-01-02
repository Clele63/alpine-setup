# Configuration et Installation de YASH Shell

Ce dépôt contient les fichiers de configuration pour le shell **Yash (Yet Another Shell)**, adaptés pour un environnement **Alpine Linux** (avec adaptations présentes dans le profil).

---

## 1. Installation de Yash

Avant de copier les configurations, assurez-vous que `yash` est installé sur votre système.

### Alpine Linux

Pré-requis :

```bash
doas apk add vim git curl doas man-pages lsd fzf bfs nnn translate-shell gnupg rxvt-unicode runit
```

```bash
doas apk add yash yash-completion
```

---

## 2. Déploiement des fichiers

L'installation se divise en deux parties :

* **Fichiers système** (dans `/usr/share`, nécessite les droits root)
* **Fichiers utilisateur** (dans le `$HOME` ou `~/.config`)

---

### A. Fichiers Système (root requis)

Ces fichiers rendent les configurations et complétions disponibles globalement pour Yash.

#### 1. Initialisation par défaut

Le fichier `initialization/default` contient :

* Alias
* Prompt
* Raccourcis clavier

```bash
# Créer le dossier s'il n'existe pas
doas mkdir -p /usr/share/yash/initialization/

# Copier le fichier
doas cp yash/initialization/default /usr/share/yash/initialization/default
```

#### 2. Complétions automatiques

Scripts de complétion pour : `xbps`, `doas`, `sv`, `passage`.

```bash
# Créer le dossier s'il n'existe pas
doas mkdir -p /usr/share/yash/completion/

# Copier les scripts de complétion
doas cp yash/completion/* /usr/share/yash/completion/
```

---

### B. Fichiers Utilisateur

Ces fichiers configurent l'environnement de l'utilisateur courant.

#### 1. Profil Yash (`.yash_profile`)

Charge les variables d'environnement, configure `nnn`, `fzf` et source le profil global.

```bash
cp .yash_profile ~/.yash_profile
```

#### 2. Configuration Readline (`inputrc`)

Yash recherche ce fichier dans `$XDG_CONFIG_HOME/inputrc`.

```bash
mkdir -p ~/.config
cp inputrc ~/.config/inputrc
```

---

## 3. Détail des fichiers de configuration

### `/usr/share/yash/initialization/default`

Chargé au démarrage du shell interactif.

* **Alias** : `ls` (lsd), `grep`, `vim` (lecture), `git`
* **Prompt (PS1)** : Prompt coloré (utilisateur, hôte, dossier courant)
* **Keybinds** :

  * Touches Home / End
  * Ctrl + flèches
  * Mode *emacs* activé par défaut
  * Mode *vi* explicitement désactivé ici

---

### `/usr/share/yash/completion/`

Règles de complétion (Tab) pour :

* **xbps** : `xbps-install`, `xbps-query`, etc.
* **sv** : `status`, `up`, `down`, `restart`
* **doas** : incluant l'option `-u`
* **passage** : `otp`, `show`, `insert`

---

### `~/.yash_profile`

Script exécuté au login.

Fonctionnalités clés :

* **Gestion XDG** :

  * Définit `XDG_CONFIG_HOME`, `XDG_DATA_HOME`, etc.
  * Adaptation spécifique pour Alpine Linux

* **Variables d'environnement** :

  * `JAVA_HOME` (OpenJDK 17)
  * `EDITOR=vim`
  * Configuration du `PATH`

* **Intégrations** :

  * **FZF** : recherche rapide avec `bfs`
  * **NNN** : couleurs, plugins, options d'archivage

---

### `~/.config/inputrc`

Configuration de la bibliothèque **Readline** utilisée par Yash.

* Mode **Vi** pour l'édition de ligne
* Complétion par menu (Tab / Maj+Tab)
* Mappages avancés façon Vim (ex: `ci"` pour modifier le texte entre guillemets)

---

## 4. Finalisation

Pour appliquer les changements :

* Déconnectez-vous et relancez votre session
* Ou lancez une nouvelle instance de Yash :

```bash
exec yash -l
```
