Sample Instrument for Linguistic Typology (SILT)
==========================

A single-file web app for drawing representative language samples from the
Glottolog database, distributed across six typological and geographic axes.

No installation or build step required — open docs/index.html in any
modern browser. Data is fetched live from the Glottolog CLDF dataset on GitHub.


USAGE
-----

1. Open docs/index.html in a browser.
2. Wait for Glottolog data to load (~3–5 seconds).
3. For each axis, choose a mode:
     Off     — axis is ignored
     Filter  — restrict the candidate pool to selected categories
     Balance — distribute the sample evenly across all categories
4. Set N (number of languages to sample).
5. Click Sample.

Results include a language table (linked to glottolog.org), distribution charts,
a CSV export, and a citable URL encoding the exact configuration used.


AXES
----

  Macroarea     — 6 geographic zones (Africa, Australia, Eurasia,
                  North America, Papunesia, South America)
  Family        — all top-level genetic families; searchable; Top 20 shortcut
  Family Size   — Isolate / Small / Medium / Large / Major / Giant
  Country       — primary speaker country (~200 options); searchable
  Endangerment  — AES vitality levels, color-coded green → purple
  Documentation — MED levels from Long Grammar to No Description


SHAREABLE LINKS
---------------

The Link button (⛓) encodes the current configuration into a GET query string
and copies the full URL to the clipboard. Opening that URL restores all axis
modes and filter selections automatically.

The How to Cite section (visible after sampling) provides LSA, APA, and MLA
citations with the settings URL embedded, making the exact sample reproducible.


DATA SOURCE
-----------

Language data: Glottolog CLDF dataset
  https://github.com/glottolog/glottolog-cldf

For Glottolog citation, see: https://glottolog.org


LICENSE
-------

MIT
