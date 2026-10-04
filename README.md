# corpus-first-epss

A plain-text mirror of the **FIRST Exploit Prediction Scoring System (EPSS)**
current bulk scores, one file per CVE, for use as a retrieval corpus.

- **Source file:** https://epss.cyentia.com/epss_scores-current.csv.gz (columns: `cve,epss,percentile`)
- **Project page:** https://www.first.org/epss/
- **Fetched:** 2026-10-04
- **Rows:** 382,621 (unique CVEs; the downloaded CSV had 382,623 lines = 1 header comment + 1 column header + 382,621 data rows; zero malformed rows, zero duplicate CVEs)
- **Model version:** **v2026.06.15**, taken verbatim from the CSV's own header comment line — `#model_version:v2026.06.15,score_date:2026-10-04T12:00:21Z` — not guessed. Cross-reference: FIRST's "Get the Data" page (https://www.first.org/epss/data) lists the model lineage, with "EPSS v5 (v2026.06.15): started publishing 2026-06-15" as the current generation. The same page now names https://epss.empiricalsecurity.com/epss_scores-current.csv.gz as the stable daily-CSV URL; this mirror was fetched from the cyentia host, which served the byte-identical current file (same content, model version, and score date).

## Layout

- `epss/CVE-YYYY-NNNNN.txt` — one file per CVE (382,621 files)
- `source/epss_scores-current.csv.gz` — the exact file the text files were generated from, kept for verification
- `MANIFEST.tsv` — TSV mapping every path to the URL it was derived from

## File format

Every file is exactly four labelled lines, UTF-8:

```
cve: CVE-1999-0001
epss: 0.03351
percentile: 0.88305
data: EPSS scores model v2026.06.15 generated 2026-10-04T12:00:21Z
```

`epss` is the probability of observed exploitation in the next 30 days
(0–1), `percentile` is the proportion of all scored vulnerabilities with the
same or lower score. The `data:` line's model version and generation timestamp
come from the CSV header comment quoted above.

## Licence — NOT VERIFIED, read this before redistributing

**We could not verify any named licence for the EPSS score data, and we are
not guessing one.** The operator's task brief expected "CC BY 4.0"; as of
2026-10-04 no page we could check states that or any other named licence for
the data. What the sources actually say, verbatim:

- FAQ, https://www.first.org/epss/faq — under "Is EPSS free to use?":
  "Yes. EPSS scores are published freely via CSV download and API with no
  registration required. Attribution is requested when EPSS data is used in
  publications or products. See Get the Data for access details and
  attribution guidance."
- Get the Data, https://www.first.org/epss/data — "EPSS scores are published
  daily, free of charge, with no registration required." (No licence or
  attribution paragraph is present on the page body as fetched.)
- Site-wide footer on first.org EPSS pages: "Copyright © 2015—2026 by Forum of
  Incident Response and Security Teams, Inc. All Rights Reserved."
- The official scores archive repository, https://github.com/empiricalsec/epss_scores
  ("This is the official repository for current and historical EPSS Scores"),
  has **no LICENSE file** (GitHub API reports `license: null).

Working summary: scores are published freely with no registration, attribution
is requested, and the site footer asserts copyright — i.e. treat as
"free to use, attribution requested", **not** as public domain or CC-licensed
unless FIRST states so. Re-check https://www.first.org/epss/data and the FAQ
before any downstream redistribution that assumes an open licence.

If EPSS data from this snapshot is used, the requested attribution is to the
EPSS SIG at FIRST / the EPSS project (per the FAQ guidance above).

## Regeneration

```
curl -sL -o epss_scores-current.csv.gz https://epss.cyentia.com/epss_scores-current.csv.gz
gunzip epss_scores-current.csv.gz
# expand each data row to epss/<cve>.txt; take model version and score date
# from the '#' comment line, never from memory
```

This mirror is a snapshot; upstream scores are regenerated daily.
