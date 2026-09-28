# Bibles françaises en JSON

Versions bibliques françaises servies en JSON compatible avec l'API getbible.net —
un fichier par livre, numéroté dans l'**ordre standard** (`1.json` = Genèse,
`66.json` = Apocalypse).

Chaque version y porte sa **licence complète** : domaine public, licence libre,
ou copyright. Un texte sous droits n'est pas « libre » parce qu'il se
télécharge : la mention d'auteur accompagne le fichier ici, et le nom de la
version dans l'application.

| Version | Dossier | Livres | Licence |
|---|---|---|---|
| Bible Ostervald (1744) | [`ostervald/`](ostervald/) | 66 | Domaine public |
| Sainte Bible néo-Crampon Libre | [`neocrampon/`](neocrampon/) | 66 | CC BY-SA 4.0 |
| Bible Chouraqui (André Chouraqui, 1987) | [`chouraqui/`](chouraqui/) | 66 | © Desclée de Brouwer |
| King James Française (2006) | [`kjf/`](kjf/) | 66 | © Nadine L. Stratford |

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

Les versions catholiques (néo-Crampon Libre, Chouraqui) sont ramenées au canon
de 66 livres : les livres et ajouts deutérocanoniques (Tobie, Judith, Sagesse,
Siracide, Baruch, 1-2 Maccabées, les suppléments d'Esther et de Daniel) sont
retirés, afin que la numérotation des chapitres corresponde au canon
protestant / hébraïque de l'application. Le Chouraqui, dont Daniel compte
14 chapitres, est ramené à 12 : le cantique des trois jeunes gens (3:24-90) est
retiré, la reprise du récit conservée.

## Conversion

Deux scripts du dépôt BYM produisent ce dossier ; les deux retirent le
balisage pour ne laisser que du texte nu, renumérotent les versets **par
position** (les sources impriment des numéros faux : `222`, `74` pour `174`, un
« 55 » hébreu en tête de Genèse 32) et comparent les comptes chapitre par
chapitre au corpus BYM avec `--bym`.

- `appCodebar/ostervald_to_json.py` — fichiers USFM source (eBible.org,
  `fra_fob` / `francl`) → `ostervald/` et `neocrampon/`. Notes de bas de page,
  renvois et balises Strong retirés.
- `appCodebar/html_verses_to_json.py` — archives HTML `<livre>/<chapitre>.html`
  (`.zip` fournis par la source) → `chouraqui/` et `kjf/`. Titres de section,
  entêtes de livre et liste de dérogations retirés.

Une conversion ne devient une version de l'application qu'une fois le dépôt
poussé **et** l'entrée correspondante de `bible_app/lib/data/version_catalog.dart`
(`urlTemplate`) mise à jour.

## Licence

- **Ostervald** : texte biblique dans le **domaine public** (Ostervald 1744, révision eBible.org).
- **néo-Crampon Libre** : © 2022 Fraternité de Tibériade, sous licence
  **Creative Commons Attribution - Partage dans les Mêmes Conditions 4.0 (CC BY-SA 4.0)**.
  Voir [`neocrampon/README.md`](neocrampon/README.md) pour l'attribution et la source.
- **Chouraqui** : © 1987 Éditions Desclée de Brouwer, traduction d'André Chouraqui —
  copyright relevé sur l'index du corpus source.
- **King James Française** : © 2006 Nadine L. Stratford — copyright relevé sur
  l'index du corpus source.
