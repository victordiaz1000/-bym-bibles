# Bibles françaises en JSON

Versions bibliques françaises **libres de droit** ou **librement redistribuables**,
servies en JSON compatible avec l'API getbible.net — un fichier par livre,
numéroté dans l'**ordre standard** (`1.json` = Genèse, `66.json` = Apocalypse).

| Version | Dossier | Livres | Licence |
|---|---|---|---|
| Bible Ostervald (1744) | [`ostervald/`](ostervald/) | 66 | Domaine public |
| Sainte Bible néo-Crampon Libre | [`neocrampon/`](neocrampon/) | 66 | CC BY-SA 4.0 |

## Format

Chaque fichier `{n}.json` suit le schéma getbible.net :

```json
{
  "translation": "…",
  "abbreviation": "…",
  "lang": "fr",
  "nr": 1,
  "name": "Genèse",
  "chapters": [
    {
      "chapter": 1,
      "name": "Genèse 1",
      "verses": [
        { "chapter": "1", "verse": "1", "name": "Genèse 1:1", "text": "…" }
      ]
    }
  ]
}
```

Les versions catholiques (néo-Crampon Libre) sont ramenées au canon de 66 livres :
les livres et ajouts deutérocanoniques (Tobie, Judith, Sagesse, Siracide, Baruch,
1-2 Maccabées, les suppléments d'Esther et de Daniel) sont retirés, afin que la
numérotation des chapitres corresponde au canon protestant / hébraïque de l'application.

## Conversion

Le script `appCodebar/ostervald_to_json.py` du dépôt BYM convertit les fichiers
USFM source (eBible.org) vers ce JSON. Les notes de bas de page, renvois et
balises Strong USFM sont retirés pour produire du texte nu.

## Licence

- **Ostervald** : texte biblique dans le **domaine public** (Ostervald 1744, révision eBible.org).
- **néo-Crampon Libre** : © 2022 Fraternité de Tibériade, sous licence
  **Creative Commons Attribution - Partage dans les Mêmes Conditions 4.0 (CC BY-SA 4.0)**.
  Voir [`neocrampon/README.md`](neocrampon/README.md) pour l'attribution et la source.
