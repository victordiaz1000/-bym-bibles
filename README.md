# Bibles françaises en JSON

Versions bibliques françaises **libres de droit**, servies en JSON compatible
avec l'API getbible.net — un fichier par livre, numéroté dans l'**ordre standard**
(`1.json` = Genèse, `66.json` = Apocalypse).

## Ostervald (1744)

- **Traduction** : Bible d'Ostervald (révision de la Bible de Genève), domaine public.
- **Source** : [eBible.org](https://ebible.org/) — module `fra_fob`
  (`https://ebible.org/Scriptures/fra_fob_usfm.zip`).
- **Contenu** : 66 livres, 1 189 chapitres, 31 107 versets.
- **Format** : schéma getbible.net (texte nu), un fichier par livre :
  `https://raw.githubusercontent.com/victordiaz1000/-bym-bibles/main/{n}.json`

### Schéma d'un livre

```json
{
  "translation": "Ostervald",
  "abbreviation": "OST",
  "lang": "fr",
  "language": "français",
  "direction": "LTR",
  "encoding": "UTF-8",
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

### Conversion

Le script `appCodebar/ostervald_to_json.py` du dépôt BYM convertit les fichiers
USFM source (`fra_fob_usfm.zip`) vers ce JSON. Les notes de bas de page, renvois
et balises Strong USFM sont retirés pour produire du texte nu.

## Licence

Texte biblique : **domaine public** (Ostervald 1744, révision eBible.org).
