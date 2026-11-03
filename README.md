# Job Tracker

Suivi de recherche d'emploi en une seule page web : pipeline kanban, relances, réseau, objectifs hebdomadaires.

Aucune installation, aucun serveur, aucun compte. Les données ne quittent pas ton ordinateur (ou ton dossier cloud).

## Lancer l'appli

Le code vit sur GitHub, les données vivent dans ton dossier OneDrive : ce sont deux choses séparées.

- **Sur n'importe quel ordinateur** : `git clone https://github.com/awyplosz/Job_tracker.git`, puis double-clic sur `index.html`.
- **Ou sans rien cloner** : active GitHub Pages (dépôt → Settings → Pages → Branch `main` / `(root)`), puis ouvre `https://awyplosz.github.io/Job_tracker/`.
- Mettre le code à jour sur un autre ordinateur : `git pull`.

## Synchroniser les données entre ordinateurs

1. Premier ordinateur : **Données → Créer un fichier de données…**, enregistre `jobtracker-data.json` dans un dossier OneDrive **hors du dépôt Git** (par exemple `Documents/Job/data/`).
2. Autre ordinateur : attends que OneDrive ait synchronisé, ouvre l'appli, **Données → Ouvrir un fichier de données…** et choisis le même fichier.
3. Ne modifie pas le même fichier sur deux ordinateurs en même temps : OneDrive créerait une copie « en conflit » et il faudrait choisir laquelle garder.

Les données ne passent jamais par GitHub (`*.json` est ignoré par `.gitignore`).

## Utilisation avec Chrome ou Edge (sauvegarde automatique)

1. Ouvre `index.html` (double-clic) ou la page GitHub Pages.
2. Menu **Données → Créer un fichier de données…** et enregistre `jobtracker-data.json` dans ton dossier OneDrive / Google Drive / Dropbox.
3. Tout est ensuite sauvegardé automatiquement dans ce fichier.

Sur un autre ordinateur : **Données → Ouvrir un fichier existant…** et choisis le même fichier.

## Utilisation avec Firefox (sauvegarde en un geste)

Firefox ne permet pas à une page d'écrire directement dans un fichier. L'appli travaille donc dans le navigateur et tu sauvegardes avec **Ctrl+S**.

1. Une seule fois : Paramètres Firefox → Général → Téléchargements → coche **« Toujours vous demander où enregistrer les fichiers »**.
2. **Ctrl+S** (ou clic sur la pastille orange) télécharge `jobtracker-data.json` : enregistre-le dans ton dossier cloud en **remplaçant** l'ancien.
3. Sur un autre ordinateur, ou après avoir vidé le navigateur : **Données → Ouvrir un fichier de données…**

La pastille est orange tant que des modifications ne sont pas sauvegardées, et Firefox te prévient si tu fermes l'onglet dans cet état.
Conseil : ouvre l'appli via GitHub Pages plutôt qu'en double-clic, l'adresse stable rend le stockage du navigateur plus fiable.

Raccourcis clavier : `N` pour ajouter une opportunité, `Ctrl+S` pour sauvegarder.

## Sauvegardes

- La pastille en haut à droite indique où sont tes données (vert = fichier lié).
- **Exporter une sauvegarde (JSON)** chaque semaine, par sécurité.
- **Exporter en CSV** pour ouvrir dans Excel.

## Confidentialité

Le `.gitignore` exclut tous les fichiers `.json` et `.csv` : le code est public, les données ne le sont jamais.
Vérifie avec `git status` avant chaque commit.

## Structure des données

```json
{
  "settings": { "weeklyApps": 5, "weeklyNetwork": 3 },
  "opportunities": [{ "id", "company", "role", "url", "sector", "source", "stage",
                      "appliedAt", "nextAction", "nextActionDate", "notes", "history": [] }],
  "contacts": [{ "id", "name", "company", "role", "email", "linkedin",
                 "opportunityId", "interactions": ["2026-11-02"], "nextFollowUp", "notes" }]
}
```
