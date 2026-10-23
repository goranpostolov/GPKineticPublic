# GPKinetic public data

Open data packages for the GPKinetic Android app: packaged foods with their barcodes and nutrition values, and ski areas
with their runs and lifts. Everything here is derived from open databases and is published under the **Open Database
License (ODbL) 1.0** — see [LICENSE](LICENSE).

The app downloads these files only when the user asks for them, after agreeing to the "Downloadable data (GitHub)"
privacy setting. Nothing is sent to this repository except the request for a file: GitHub receives the device's IP
address, which file is requested and the app's `User-Agent`.

## What is here

| file | what it is |
|---|---|
| `README.md` | this page |
| `LICENSE` | the license of the data (ODbL 1.0) and the attributions |
| `catalogo.json` | the list of the packages of the current release: id, kind, country, date of the data, file name, size compressed and uncompressed, SHA-256 of the compressed file, number of entries, source |

The packages themselves are **attached to the releases** (one release per data date, currently
[`dati-2026-10-23`](../../releases/tag/dati-2026-10-23)), not stored in the git history. A file's address is
`https://github.com/goranpostolov/GPKineticPublic/releases/download/<release>/<file>`; the app reads
`https://raw.githubusercontent.com/goranpostolov/GPKineticPublic/main/catalogo.json` and compares the SHA-256 of each package to know
whether an update is available.

Current release: **dati-2026-10-23**, 98 packages, 342.0 MB compressed in total.

## Foods — from Open Food Facts

Source: the public CSV export of [Open Food Facts](https://world.openfoodfacts.org) of 2026-10-23.
A product is included when it has an 8–14 digit barcode, a name and kcal, carbohydrates, proteins and fats per 100 g
within plausible limits (≤ 900 kcal, ≤ 100 g each); one product per barcode (the most scanned). Countries come from the
product's `countries_tags`: a product sold in several countries appears in each country's file. A country has its own
file when it has at least 10,000 complete products.

Each file is a gzip-compressed **SQLite** database:

- table `prodotti`, ordered by number of scans (most scanned first): `codice` (barcode), `nome` (name, as written on the
  package), `marca` (first brand), `kcal`, `carboidrati`, `proteine`, `grassi` (per 100 g), `scansioni` (unique scans),
  `cerca` (name and brand lowercased without accents, for `LIKE` search), `bevanda` (1 for a beverage: `en:beverages`,
  but not powders or capsules), `tipo` (kind of beverage: water, juice, soda, tea, coffee, beer, wine, …), `alcolica`
  (1 if alcoholic), `acqua` (water per 100 g, when Open Food Facts has it), `ml` (package size in ml, read from the
  quantity); unique index on `codice`;
- table `info`: country, date of the data, version, source, number of products.

| country | file | products | download | on the phone |
|---|---|---|---|---|
| FR | `cibi-fr.sqlite.gz` | 882,987 | 43.2 MB | 116.4 MB |
| US | `cibi-us.sqlite.gz` | 777,243 | 37.8 MB | 111.1 MB |
| ES | `cibi-es.sqlite.gz` | 246,278 | 11.3 MB | 31.3 MB |
| DE | `cibi-de.sqlite.gz` | 234,756 | 12.7 MB | 30.9 MB |
| IT | `cibi-it.sqlite.gz` | 216,628 | 9.8 MB | 27.3 MB |
| GB | `cibi-gb.sqlite.gz` | 150,038 | 8.2 MB | 20.7 MB |
| CA | `cibi-ca.sqlite.gz` | 111,235 | 5.6 MB | 14.6 MB |
| CH | `cibi-ch.sqlite.gz` | 86,242 | 4.4 MB | 10.7 MB |
| BE | `cibi-be.sqlite.gz` | 80,688 | 4.1 MB | 10.2 MB |
| AU | `cibi-au.sqlite.gz` | 65,688 | 3.5 MB | 9.2 MB |
| IE | `cibi-ie.sqlite.gz` | 59,281 | 3.2 MB | 7.7 MB |
| NL | `cibi-nl.sqlite.gz` | 59,012 | 3.1 MB | 7.8 MB |
| JP | `cibi-jp.sqlite.gz` | 40,671 | 2.4 MB | 6.6 MB |
| BR | `cibi-br.sqlite.gz` | 25,821 | 1.4 MB | 3.7 MB |
| SE | `cibi-se.sqlite.gz` | 24,679 | 1.2 MB | 3.1 MB |
| PL | `cibi-pl.sqlite.gz` | 23,937 | 1.3 MB | 3.2 MB |
| DK | `cibi-dk.sqlite.gz` | 19,081 | 947 KB | 2.4 MB |
| NO | `cibi-no.sqlite.gz` | 19,043 | 974 KB | 2.5 MB |
| FI | `cibi-fi.sqlite.gz` | 19,020 | 947 KB | 2.5 MB |
| AT | `cibi-at.sqlite.gz` | 18,883 | 1.0 MB | 2.5 MB |
| PT | `cibi-pt.sqlite.gz` | 15,009 | 792 KB | 2.0 MB |
| CZ | `cibi-cz.sqlite.gz` | 14,850 | 817 KB | 2.0 MB |
| RU | `cibi-ru.sqlite.gz` | 14,246 | 812 KB | 2.6 MB |
| MX | `cibi-mx.sqlite.gz` | 12,489 | 666 KB | 1.7 MB |
| NZ | `cibi-nz.sqlite.gz` | 12,412 | 680 KB | 1.8 MB |
| RO | `cibi-ro.sqlite.gz` | 12,372 | 697 KB | 1.6 MB |
| BG | `cibi-bg.sqlite.gz` | 10,704 | 631 KB | 1.7 MB |
| World (`en:world`) | `cibi-mondo.sqlite.gz` | 40,318 | 2.2 MB | 6.3 MB |
| **All complete products** | `cibi-tutti.sqlite.gz` | 3,305,108 | 160.3 MB | 446.0 MB |

`cibi-tutti` contains every complete product of every country: with it, the country files are not needed.

## Ski areas — from OpenSkiData

Source: the GeoPackage of [OpenSkiData](https://openskidata.org) of 2026-10-21, built from
[OpenStreetMap](https://www.openstreetmap.org) data. Included: operating ski areas with downhill skiing and at least one
operating downhill run; their operating runs and lifts. Some connections that the data lacks (a train, a walk through a
village) are added by hand for a few areas.

- `piste-indice.json.gz`: the index of all 4,796 ski areas (334 KB compressed): id, name,
  countries, region, locality, bounding box, number of runs and lifts, and the share of named runs reachable from a
  start and back;
- `piste-<country>.json.gz`: `{"comprensori": {"<id>": <ski area>}}`, the areas whose **first** country is that
  country (an area across a border is in the file of its first country); `piste-altri` holds the areas without a
  country. A ski area has its runs (`piste`: id, name, difficulty, coordinates) and lifts (`impianti`: id, name, type,
  duration, coordinates).

| country | file | ski areas | download | on the phone |
|---|---|---|---|---|
| US | `piste-us.json.gz` | 503 | 3.0 MB | 10.0 MB |
| JP | `piste-jp.json.gz` | 453 | 864 KB | 2.9 MB |
| DE | `piste-de.json.gz` | 445 | 357 KB | 1.2 MB |
| AT | `piste-at.json.gz` | 359 | 2.6 MB | 8.4 MB |
| CH | `piste-ch.json.gz` | 330 | 1.6 MB | 5.2 MB |
| CZ | `piste-cz.json.gz` | 324 | 248 KB | 833 KB |
| IT | `piste-it.json.gz` | 259 | 1.6 MB | 5.1 MB |
| CA | `piste-ca.json.gz` | 250 | 1.2 MB | 4.0 MB |
| CN | `piste-cn.json.gz` | 243 | 205 KB | 671 KB |
| FR | `piste-fr.json.gz` | 243 | 2.9 MB | 9.0 MB |
| RU | `piste-ru.json.gz` | 190 | 253 KB | 812 KB |
| NO | `piste-no.json.gz` | 182 | 295 KB | 945 KB |
| PL | `piste-pl.json.gz` | 161 | 137 KB | 457 KB |
| SE | `piste-se.json.gz` | 122 | 300 KB | 967 KB |
| SK | `piste-sk.json.gz` | 115 | 122 KB | 418 KB |
| FI | `piste-fi.json.gz` | 68 | 126 KB | 436 KB |
| RO | `piste-ro.json.gz` | 60 | 68 KB | 223 KB |
| UA | `piste-ua.json.gz` | 59 | 59 KB | 196 KB |
| ES | `piste-es.json.gz` | 33 | 261 KB | 827 KB |
| GB | `piste-gb.json.gz` | 33 | 29 KB | 93 KB |
| BG | `piste-bg.json.gz` | 22 | 44 KB | 140 KB |
| HU | `piste-hu.json.gz` | 22 | 12 KB | 41 KB |
| SI | `piste-si.json.gz` | 22 | 49 KB | 157 KB |
| NZ | `piste-nz.json.gz` | 21 | 77 KB | 260 KB |
| CL | `piste-cl.json.gz` | 16 | 69 KB | 229 KB |
| IR | `piste-ir.json.gz` | 16 | 16 KB | 50 KB |
| TR | `piste-tr.json.gz` | 16 | 42 KB | 132 KB |
| BA | `piste-ba.json.gz` | 15 | 36 KB | 119 KB |
| GR | `piste-gr.json.gz` | 15 | 34 KB | 108 KB |
| KR | `piste-kr.json.gz` | 15 | 36 KB | 114 KB |
| AR | `piste-ar.json.gz` | 14 | 74 KB | 260 KB |
| RS | `piste-rs.json.gz` | 14 | 37 KB | 118 KB |
| HR | `piste-hr.json.gz` | 13 | 9 KB | 32 KB |
| IS | `piste-is.json.gz` | 12 | 17 KB | 54 KB |
| AU | `piste-au.json.gz` | 10 | 117 KB | 426 KB |
| KG | `piste-kg.json.gz` | 10 | 19 KB | 60 KB |
| EE | `piste-ee.json.gz` | 9 | 5 KB | 15 KB |
| GE | `piste-ge.json.gz` | 8 | 31 KB | 99 KB |
| MK | `piste-mk.json.gz` | 8 | 19 KB | 60 KB |
| NL | `piste-nl.json.gz` | 8 | 4 KB | 13 KB |
| BE | `piste-be.json.gz` | 7 | 4 KB | 12 KB |
| KZ | `piste-kz.json.gz` | 7 | 12 KB | 34 KB |
| ME | `piste-me.json.gz` | 6 | 10 KB | 36 KB |
| LB | `piste-lb.json.gz` | 5 | 12 KB | 36 KB |
| LT | `piste-lt.json.gz` | 5 | 3 KB | 9 KB |
| AD | `piste-ad.json.gz` | 4 | 66 KB | 201 KB |
| LV | `piste-lv.json.gz` | 4 | 7 KB | 21 KB |
| UZ | `piste-uz.json.gz` | 4 | 8 KB | 24 KB |
| other | `piste-altri.json.gz` | 3 | 1 KB | 4 KB |
| AM | `piste-am.json.gz` | 3 | 6 KB | 19 KB |
| BY | `piste-by.json.gz` | 3 | 3 KB | 10 KB |
| DK | `piste-dk.json.gz` | 3 | 2 KB | 6 KB |
| GL | `piste-gl.json.gz` | 3 | 3 KB | 8 KB |
| KP | `piste-kp.json.gz` | 3 | 4 KB | 12 KB |
| MA | `piste-ma.json.gz` | 3 | 4 KB | 14 KB |
| AZ | `piste-az.json.gz` | 2 | 11 KB | 34 KB |
| IN | `piste-in.json.gz` | 2 | 4 KB | 12 KB |
| AL | `piste-al.json.gz` | 1 | 1 KB | 2 KB |
| BR | `piste-br.json.gz` | 1 | 1 KB | 1 KB |
| CY | `piste-cy.json.gz` | 1 | 3 KB | 9 KB |
| IE | `piste-ie.json.gz` | 1 | 1 KB | 2 KB |
| IL | `piste-il.json.gz` | 1 | 5 KB | 17 KB |
| LI | `piste-li.json.gz` | 1 | 7 KB | 22 KB |
| LS | `piste-ls.json.gz` | 1 | 1 KB | 1 KB |
| MN | `piste-mn.json.gz` | 1 | 1 KB | 3 KB |
| MX | `piste-mx.json.gz` | 1 | 1 KB | 1 KB |
| PT | `piste-pt.json.gz` | 1 | 2 KB | 6 KB |
| ZA | `piste-za.json.gz` | 1 | 1 KB | 4 KB |

## 3D model of the muscles — from Z-Anatomy

The 3D model the app uses to show the muscles, published here because its license asks that derived material stay
available under the same terms. It is **not** under the ODbL: it is licensed under **CC BY-SA 4.0**
(https://creativecommons.org/licenses/by-sa/4.0/), like its source.

Source: [Z-Anatomy](https://github.com/LluisV/Z-Anatomy) (branch `PC-Version`), CC BY-SA 4.0. Changes: meshes selected,
cropped and simplified (decimation), regrouped by muscle zone, coloured, and exported as glTF/GLB with Draco compression;
`zones.json` maps the meshes to the muscle zones. The attribution and the license are in this README and in `LICENSE`.

| file | size |
|---|---|
| `modello-3d/systems.glb` | 2.5 MB |
| `modello-3d/bodymap.glb` | 1.8 MB |
| `modello-3d/bones.glb` | 1.1 MB |
| `modello-3d/extras.glb` | 545 KB |
| `modello-3d/mannequin.glb` | 139 KB |
| `modello-3d/zones.json` | 64 KB |

## How the files are made

From the two public sources above, downloaded by hand (OpenSkiData asks for at most one automated download a day), with
the rules written in each section. `catalogo.json` lists the files of the release, with sizes and SHA-256 measured on the
files themselves.

## License and attribution

- Foods: © Open Food Facts contributors — https://world.openfoodfacts.org — Open Database License (ODbL) 1.0; the
  individual contents of the database under the Database Contents License (DbCL) 1.0.
- Ski areas: © OpenStreetMap contributors, via OpenSkiData — https://openskidata.org — Open Database License (ODbL) 1.0.
- 3D model of the muscles (`modello-3d/`): adapted from Z-Anatomy — https://github.com/LluisV/Z-Anatomy — CC BY-SA 4.0.

These packages are derived databases: they are published here under the same license, and anyone may use them under
its terms (attribution, share-alike, keep open). The data comes from volunteers and may be incomplete or wrong: check
the label of a product, and the ski area's own map before skiing.
