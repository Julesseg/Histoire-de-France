# Histoire de France

Un épisode par soir sur l'histoire de France. Site statique : **une coquille
fixe qui lit une base JSON**.

## Structure du dépôt

| Fichier | Rôle | Modifié |
|---|---|---|
| `index.html` | La coquille : tout le CSS, la mise en page, le routage et le rendu. Se construit une fois. | Presque jamais |
| `episodes.json` | La base de données. Le tableau `episodes` contient tous les épisodes. | **Chaque soir** |
| `README.md` | Ce fichier. | — |

Au chargement, `index.html` fait un seul `fetch('episodes.json')` et rend
l'index puis une vue par épisode. Le routage se fait par ancre (`#slug`),
sans rechargement.

## Le contrat de la base

Chaque entrée du tableau `episodes` :

```json
{
  "numero": 12,
  "date": "2026-08-19",
  "slug": "mot-simple-sans-accent",
  "titre": "Le titre",
  "chapo": "Une phrase qui situe.",
  "periode": "Contexte · année",
  "recit": ["par. 1", "par. 2", "par. 3", "par. 4"],
  "chute": "La phrase de fin.",
  "ressources": [
    {"titre": "Titre", "url": "https://…", "note": "Une ligne."},
    {"titre": "Auteur, <i>Titre</i>, éditeur, année", "url": null, "note": "Une ligne."}
  ]
}
```

- `slug` unique, minuscules, sans accent ni espace.
- Balises HTML légères admises (`<i>`, `<sup>`, `<span class="glose">`) ;
  aucun guillemet non échappé qui casserait le JSON.
- Le tri d'affichage se fait par `date`, pas par `numero`.
- `manquants` (facultatif) : mêmes champs `numero`/`date`/`slug`/`titre`/`periode`
  pour signaler un épisode non archivé.

## Mise à jour du soir

Le seul geste : ajouter un objet au tableau `episodes` de `episodes.json`,
puis `commit` + `push`. Aucune étape de build — la coquille rend la base
telle quelle.

## Déploiement (GitHub Pages)

Servir le dépôt à la racine (`main`, dossier `/`). Les deux fichiers sont
livrés tels quels ; aucune action CI n'est nécessaire.
