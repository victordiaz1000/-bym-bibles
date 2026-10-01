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
| Septuaginta (Rahlfs) — grec et deux traductions françaises | [`sef/`](sef/) | 39 (AT) | © 1935, 1979 Deutsche Bibelgesellschaft (grec) · © Biblia Universalis 3 (traductions) |

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

`sef/` applique la même règle au corpus de la Septante, et s'arrête aux **39
livres d'Ancien Testament** : Tobie, Judith, Sagesse, Siracide, Baruch, la
Lettre de Jérémie, 1-2 Maccabées, « Esdras A » (1 Esdras), Daniel 13-14
(Suzanne, Bel) et le Psaume 151 sont retirés. La Septante n'ayant pas de
Nouveau Testament, il n'y a rien d'autre à servir : l'application affiche
« absent de la version » pour un livre du NT plutôt qu'un téléchargement qui
ne pourrait pas aboutir.

Le schéma de `sef/` enrichit le précédent — un verset y porte en plus `grec`
(la ligne grecque, affichée au-dessus du français), `alexandrie` (la seconde
traduction française quand elle existe), `notes` (les pieds de page de la
source, `•`-listés et sans ancrage mot) et `section` (titres de section).
`text` reste le français affiché (Giguet) ; un verset où seul le grec subsiste
garde `text` vide plutôt que de disparaître. Au niveau fichier, `copyright`
répète les droits du texte (voir plus bas).

## Conversion

Trois scripts du dépôt BYM produisent ce dossier ; tous retirent le balisage
pour ne laisser que du texte nu, et comparent les comptes chapitre par
chapitre au corpus BYM.

- `appCodebar/ostervald_to_json.py` — fichiers USFM source (eBible.org,
  `fra_fob` / `francl`) → `ostervald/` et `neocrampon/`. Notes de bas de page,
  renvois et balises Strong retirés. Les versets sont renumérotés **par
  position** (les sources impriment des numéros faux : `222`, `74` pour `174`,
  un « 55 » hébreu en tête de Genèse 32).
- `appCodebar/html_verses_to_json.py` — archives HTML `<livre>/<chapitre>.html`
  (`.zip` fournis par la source) → `chouraqui/` et `kjf/`. Titres de section,
  entêtes de livre et liste de dérogations retirés, renumérotation par position.
- `sef/sef_to_json.py` — extraction de `SEF.xml` (Biblia Universalis 3,
  `sef/extrait/`) → `sef/`. Grec, Giguet et Alexandrie séparés par champ,
  pieds de page extraits des balises `<span class="note">`, sous-versets de la
  source (« 46a », « 11-13 », « 1 = 2.35c ») **fusionnés sur leur numéro de
  tête** pour rester aligné sur le comparateur.

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
- **Septuaginta** : © 1935 Württembergische Bibelanstalt ; © 1979 Deutsche
  Bibelgesellschaft, Stuttgart — grec d'Alfred Rahlfs, copyright relevé sur
  l'index du corpus source. Les deux traductions françaises, Pierre Giguet
  (affichée en bleu) et la Bible d'Alexandrie (partiellement, en vert),
  portent pour copyright le nom du logiciel, **Biblia Universalis 3**
  (Laurent SOUFFLET) — relevé sur la page « Septante traduite en langue
  française » du corpus source. Chaque fichier de `sef/` le répète dans son
  champ `copyright`.
