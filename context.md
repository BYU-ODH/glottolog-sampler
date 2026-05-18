# Sample Instrument for Linguistic Typology (SILT) — Project Context

## What it is

A single-file vanilla HTML/CSS/JS web app (`docs/index.html`) that fetches language data from the [Glottolog CLDF dataset](https://github.com/glottolog/glottolog-cldf) and lets the user draw a representative sample of languages across six typological/geographic axes.

No build step, no server required — open the file in a browser.

---

## Data source

Fetches two CSVs from the `glottolog/glottolog-cldf` GitHub repo at runtime (tries `master` then `main`):

- `cldf/languages.csv` — core language metadata (name, glottocode, macroarea, family, level, AES status, country codes)
- `cldf/values.csv` — parameter values; used to extract AES endangerment and MED documentation levels if not present in `languages.csv`

Rows where `Level = language` are kept; `Level = family` rows are used to resolve family names. Family size (count of language-level members) is computed from the data itself.

---

## The six axes

| Axis | Field | Notes |
|---|---|---|
| Macroarea | `macroarea` | Africa, Australia, Eurasia, North America, Papunesia, South America |
| Family | `familyName` | All top-level families; searchable; "Top 20 families" quick-select |
| Family Size | `famSizeBin` | Isolate / Small 2–5 / Medium 6–25 / Large 26–100 / Major 101–500 / Giant 500+ |
| Country | `countries` | ISO country codes from `Country_IDs`; sorted by language count; searchable |
| Endangerment | `aes` | AES levels, color-coded green→purple; normalized from several possible column names |
| Documentation | `med` | MED levels from Long Grammar down to No Description |

Each axis has three modes:
- **Off** — ignored
- **Filter** — restricts the candidate pool to checked categories
- **Balance** — contributes to stratified sampling (see below)

---

## Sampling algorithm

1. Apply all Filter-mode axes to get the candidate pool.
2. Collect all Balance-mode axes.
3. If no Balance axes: random shuffle, take N.
4. If Balance axes: group the pool into cells by the Cartesian combination of Balance-axis values. Allocate `⌊N/cells⌋` per cell, distributing the remainder to the largest cells. Sample from each cell, then fill any shortfall randomly from unused pool members.

---

## UI features

- **Pool size note** — live count of languages matching current filters, updated as axes are toggled.
- **Results table** — language name, glottocode (linked to glottolog.org), macroarea, family, family size bin, AES pill (color-coded), MED, countries.
- **Distribution cards** — bar charts showing the sample's breakdown across Macroarea, Family Size, Endangerment, and Documentation.
- **Export CSV** — downloads all sample metadata (including ISO code, raw family size, all country codes).
- **Copy link (⛓ Link)** — encodes current axis configuration and N into a GET query string and copies the full URL to clipboard. Restores on page load.
- **How to cite** — appears after sampling; shows LSA / APA / MLA citation for the sampler with the settings GET string embedded in the URL, so the exact configuration is citable and reproducible. `[Author]` is a placeholder.

---

## URL state format

```
?n=30&macroarea=balance&aes=filter:not%20endangered,threatened&familyName=filter:Indo-European
```

- `n=` — sample count
- `axisId=balance` — axis set to Balance mode
- `axisId=filter:val1,val2` — axis set to Filter mode; values are individually `encodeURIComponent`'d, joined with literal `,`

Parsed by `parseUrlState()` before data loads; applied by `applyUrlState()` after `buildUI()`.
