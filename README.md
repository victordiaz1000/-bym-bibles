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
| Ancien Testament Interlinéaire (mot à mot hébreu-français) | [`ati/`](ati/) | 39 (AT) | © Biblia Universalis · texte hébreu : Biblia Hebraica Stuttgartensia |
| Septuaginta grec + Strong (mot à mot grec, sans français) | [`ses/`](ses/) | 39 (AT + Psaume 151) | © Biblia Universalis |
| Bible Darby + Strong (1890, numéros Strong) | [`dbs/`](dbs/) | 66 | Domaine public · taggage Strong : © Biblia Universalis 3 |

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
traduction française, affichée sous la première quand elle existe), `notes`
(les pieds de page de la source, `•`-listés et sans ancrage mot : conservés
en données, jamais affichés par l'application) et `section` (titres de
section). `text` reste le français affiché (Giguet) ; un verset où seul le
grec subsiste garde `text` vide plutôt que de disparaître. Au niveau fichier,
`copyright` répète les droits du texte (voir plus bas).

`ati/` change de schéma, et c'est le seul dossier qui ne porte pas de texte à
afficher tel quel : un verset y est une **liste de mots**, chacun avec sept
champs — numéro Strong (`s`), translittération (`t`), hébreu vocalisé (`h`),
découpage morphologique (`d`), glose française (`f`), analyse grammaticale
(`g`, `a`, index dans les tables `ca` / `cg` portées par le livre) et renvoi de
glossaire (`n`). Le fichier est nommé par le **numéro standard** du livre
(`1.json` = Genèse, `39.json` = Malachie) et porte aussi `bym_index` (l'ordre
du canon hébreu, celui de la navigation de l'application). Un `notes.json`
unique reçoit les 37 pages de glossaire que les `n` viennent y chercher.
L'application n'affiche pas l'interlinéaire : elle joint les gloses françaises
en une ligne de texte, la donnée complète restant en réserve pour le rendu en
colonnes.

`ses/` applique le même schéma au grec de la Septante : un mot y porte sa
forme imprimée (`m`), son numéro Strong (`s`) et son analyse morphologique
(`g`, `a`, mêmes tables `cg` / `ca`) — sans lemme, sans koinè et surtout sans
glose française, que la source ne porte pas. La ligne aplatie est donc vide,
et ce sont les colonnes que le lecteur pose ; les numéros s'y cliquent vers
les fiches du lexique comme sur les deux autres interlinéaires. Ancien
Testament seul, Psaume 151 compris (151 chapitres aux Psaumes).

## Conversion

Six scripts du dépôt BYM produisent ce dossier ; tous retirent le balisage
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
- `ATI/ati_to_json.py` — extraction de `ATI.xml` (Biblia Universalis 3,
  `ATI/extrait/`) → `ati/`. Colonnes de mots hébreux parsées champ par champ,
  analyses grammaticales mises en table de codes (les mêmes libellés reviennent
  des milliers de fois), glossaire sorti dans `notes.json`. Numérotation des
  chapitres déjà celle de l'application : aucun renumérotage ; une seule
  correction déclarée (Job 40, un verset étiqueté « 4 » pour « 28 »).
- `SES/ses_to_json.py` — extraction de `SES.xml` (Biblia Universalis 3,
  Septante grecque + Strong) → `ses/`. Tables à trois rangées (mot imprimé,
  Strong, analyse) lues par rôle ; tables mal fermées découpées sur leurs
  ouvertures ; Strong 0 abandonné ; sous-versets (`46-a`) fusionnés sur leur
  numéro de tête comme la SEF ; doublon du Psaume 151 en fin de PSA/150.html
  écarté (le psaume a son chapitre 151). Sans français dans la source, sans
  glose dans les fichiers.
- `DBS/dbs_to_json.py` — extraction de `DBS.xml` (Biblia Universalis 3, Darby +
  Strong) → `dbs/`. Même moteur que `extraction/extract_lgs_lss.py` : le
  premier marqueur de chaque chapitre est toujours `<span class="cn">1</span>`
  (il ouvre le verset 1, il ne porte pas le n° de chapitre), les suivants sont
  des `vn` numérotés 1..N sans écart. Notes Darby (`<span class="note">`,
  `<span class="nn">`) et astérisques de tête retirés. Les codes Strong sont
  gardés **inline** dans `text` (« Dieu H0430 », rembourrés à quatre chiffres),
  comme la LSGS embarquée : l'application les rend cliquables sans nouveau
  format. Un mot français portant deux codes hébreux/grecs (nombres composés
  « 23 », « d'où ») donne deux codes à la suite (« 23 H6242 H7969 »).

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
  champ `copyright`. Les traducteurs, eux, sont ceux que le corpus nomme :
  **Pierre Giguet** (1794-1883) et l'équipe de la Bible d'Alexandrie
  (Éditions du Cerf, sous la direction de Marguerite Harl, Gilles Dorival,
  Olivier Munnich, avec Cécile Dogniez) — ces noms accompagnent la carte de
  la version dans l'application.
- **Ancien Testament Interlinéaire** : © Biblia Universalis — mention relevée
  sur la fiche du corpus source (« Biblia Universalis », 2024), même logiciel
  que la Septuaginta. La fiche nomme aussi le texte hébreu sous-jacent, la
  **Biblia Hebraica Stuttgartensia**. La glose française mot à mot vient du
  même corpus ; elle est jointe en une ligne dans l'application, les sept
  champs par mot restant en données dans le fichier.
- **Septuaginta grec + Strong** : © Biblia Universalis — même logiciel que
  l'interlinéaire. Le grec de la Septante mot à mot, chaque mot avec son
  numéro Strong et son analyse morphologique, sans traduction française.
- **Bible Darby + Strong** : texte biblique dans le **domaine public** (Darby
  1890). Le taggage Strong mot à mot vient de **Biblia Universalis 3**
  (Laurent SOUFFLET) — même logiciel que la Septuaginta et l'interlinéaire.
  Les codes sont gardés inline dans `text`, au format qu'attend le lecteur
  (`H0430`, quatre chiffres).
