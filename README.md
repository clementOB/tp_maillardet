# Search

**Version : 1.0**

Un script Bash `search` pour effectuer des recherches simplifiées avec `find`, `grep` et `locate`.

---

## Prérequis

- **Bash ≥ 4**
- **findutils** (commande `find`)
- **plocate** ou **mlocate** (commandes `locate`, `updatedb`)
- **GNU getopt** (inclus dans `util-linux`)

---

## Installation

1. Copiez le script `search` dans un répertoire de votre `$PATH`, par exemple :
   ```bash
   mv search ~/bin/
   chmod +x ~/bin/search
   ```
2. Vérifiez :
   ```bash
   which search
   # → ~/bin/search
   ```

---

## Préparation des données

1. Téléchargez et décompressez les données du TP :
   ```bash
   wget https://admx.welibre.org/files/data-search.tar.gz
   tar xzf data-search.tar.gz
   ```
   Vous obtiendrez un dossier `data/` contenant `music/`, `picture/`, etc.
2. Créez le dossier `bla/` non lisible par l’utilisateur :
   ```bash
   cd data
   mkdir bla && touch bla/bla
   sudo chown root: bla
   sudo chmod o-rwx bla
   ```

---

## Usage

```bash
search [options] STRING [STRING…]
search [options] STRING [PATH] [STRING…]
search [options] -p PATH STRING [STRING…]
search -u
```

### Options

- `-h, --help`  
  Affiche cette aide et quitte.

- `-u, --update`  
  Met à jour la base `locate` (`sudo updatedb`) et quitte.

- `-d, --debug`  
  Active le mode debug (`set -x`).

- `-e, --error`  
  Affiche en temps réel les erreurs (permission, fichier non trouvé).  
  Sinon, un warning final s’affiche si des erreurs ont eu lieu.

- `-l, --locate`  
  Utilise `locate` (option `-i`).

- `-i, --inside`  
  Recherche également à l’intérieur des fichiers (grep).

- `-o, --only`  
  (à utiliser avec `-i`) Recherche **uniquement** dans le contenu.

- `-p, --path PATH`  
  Force le dossier de recherche (relatif ou absolu).

- `--`  
  Tout ce qui suit est traité comme terme, même s’il ressemble à un chemin ou une option.

---

## Exemples

```bash
# Aide
search -h

# Recherche par nom
search ac-dc high
# → ./data/music/AC-DC - Highway to Hell.m4a

# Recherche locate
search -l high ac-dc
# → /…/data/music/AC-DC - Highway to Hell.m4a

# Recherche dans le contenu
search -i Cocoon

# Recherche exclusivement contenu
search -i -o MACHINE

# Forcer le dossier
search -p data ac-dc

# Gérer les erreurs
search ghts
search -e ghts

---