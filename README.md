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
2. Autre ordinateur : attends que OneDrive ait synchronisé, ouvre l'appli, **Données → Ouvrir un fichier de données (fusion)…** et choisis le même fichier.
3. Si OneDrive crée une copie « en conflit » (fichier modifié sur deux ordinateurs en même temps), ouvre-la avec **Données → Fusionner un fichier JSON…** : rien n'est perdu.

### Fusion : comment les données se combinent

Ouvrir ou lier un fichier **ne remplace plus** les données du navigateur : les deux sont fusionnées.

- Chaque opportunité et chaque contact porte sa date de dernière modification. Pour une fiche présente des deux côtés, **la version la plus récente gagne** (fiche entière, pas champ par champ).
- Une fiche présente d'un seul côté est ajoutée à l'autre.
- Une suppression laisse une trace (`deleted`) : la fiche reste supprimée partout, sauf si elle a été modifiée ailleurs **après** la suppression (la modification gagne alors).
- Rouvrir un vieux fichier ne fait donc pas revenir les fiches supprimées depuis.
- Les dates viennent de l'horloge de chaque ordinateur : si l'une est très décalée, l'ordre des modifications peut être faux.

Exemple : tu ajoutes des contacts sur l'ordinateur A sans sauvegarder, puis tu modifies et sauvegardes sur B. De retour sur A, ouvre le fichier de B : A garde ses nouveaux contacts **et** reçoit les modifications de B. Sauvegarde ensuite sur A pour que le fichier contienne tout.

**Restaurer une sauvegarde (remplace tout)** fait l'inverse : la sauvegarde devient la référence, ici et sur les autres ordinateurs à leur prochaine synchronisation. Une copie des données actuelles est téléchargée juste avant.

Les données ne passent jamais par GitHub (`*.json` est ignoré par `.gitignore`).

## Utilisation avec Chrome ou Edge (sauvegarde automatique)

1. Ouvre `index.html` (double-clic) ou la page GitHub Pages.
2. Menu **Données → Créer un fichier de données…** et enregistre `jobtracker-data.json` dans ton dossier OneDrive / Google Drive / Dropbox.
3. Tout est ensuite sauvegardé automatiquement dans ce fichier.

Sur un autre ordinateur : **Données → Lier un fichier existant (fusion)…** et choisis le même fichier.
Quand tu reviens sur l'onglet, et avant chaque écriture, l'appli relit le fichier s'il a été modifié ailleurs et fusionne : un ordinateur n'écrase jamais les ajouts d'un autre.

## Utilisation avec Firefox (sauvegarde en un geste)

Firefox ne permet pas à une page d'écrire directement dans un fichier. L'appli travaille donc dans le navigateur et tu sauvegardes avec **Ctrl+S**.

1. Une seule fois : Paramètres Firefox → Général → Téléchargements → coche **« Toujours vous demander où enregistrer les fichiers »**.
2. **Ctrl+S** (ou clic sur la pastille orange) télécharge `jobtracker-data.json` : enregistre-le dans ton dossier cloud en **remplaçant** l'ancien.
3. Sur un autre ordinateur, ou après avoir vidé le navigateur : **Données → Ouvrir un fichier de données (fusion)…**

La pastille est orange tant que des modifications ne sont pas sauvegardées, et Firefox te prévient si tu fermes l'onglet dans cet état.
Conseil : ouvre l'appli via GitHub Pages plutôt qu'en double-clic, l'adresse stable rend le stockage du navigateur plus fiable.

Raccourcis clavier : `N` pour ajouter une opportunité, `Ctrl+S` pour sauvegarder.

## Fonctions V2

- **Version du CV envoyée** : champ dans chaque opportunité (suggestions des versions déjà utilisées). Visible sur les cartes et dans la liste.
- **Onglet Stats** : taux de réponse, entretiens, offres et délai moyen de réponse, par secteur, par version de CV et par source. En dessous de 5 candidatures, le taux est marqué `*` (trop peu pour conclure).
- **Fiche d'entretien** : bouton « 📝 Fiche d'entretien » dans l'opportunité (date, format, pitch, recherche entreprise, points forts, questions probables, questions à poser, checklist, bilan). Un entretien dans les 3 jours apparaît dans « À faire ».

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
  "updatedAt": "2026-11-27T10:00:00.000Z",
  "settings": { "weeklyApps": 5, "weeklyNetwork": 3 }, "settingsUpdatedAt",
  "opportunities": [{ "id", "company", "role", "url", "sector", "track", "country", "source", "stage",
                      "cvVersion", "appliedAt", "nextAction", "nextActionDate", "notes", "history": [], "updatedAt",
                      "prep": { "date", "format", "who", "pitch", "research", "strengths",
                                "qa", "ask", "debrief", "checklist": [{ "t", "done" }] } }],
  "contacts": [{ "id", "name", "company", "role", "email", "phone", "linkedin",
                 "opportunityId", "interactions": ["2026-11-02"], "nextFollowUp", "notes", "updatedAt" }],
  "deleted": { "<id d'une fiche supprimée>": "2026-11-20T09:15:00.000Z" }
}
```
