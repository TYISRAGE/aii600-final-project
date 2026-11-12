# Data

Source: UCI Machine Learning Repository, "Spambase" dataset (https://archive.ics.uci.edu/dataset/94/spambase), DOI 10.24432/C53G6X. Released under CC BY 4.0, which permits republishing, so the file is attached to this fork's `data` GitHub release rather than committed to git.

- File: `data/spambase.csv` (renamed from the source's `spambase.data`; comma-separated, no header row — column order follows `spambase.names`)
- SHA-256: `b1ef93de71f97714d3d7d4f58fc9f718da7bbc8ac8a150eff2778616a8097b12`
- Unit of observation: one email (57 numeric features + binary spam label)
- Sample size: 4,601
- Time span: collected circa 1999 at Hewlett-Packard Labs (exact dates not published by the source)

Download from the source directly:

```bash
curl -sL https://archive.ics.uci.edu/static/public/94/spambase.zip -o data/raw/spambase.zip
unzip -o data/raw/spambase.zip -d data/raw
cp data/raw/spambase.data data/spambase.csv
```

Or, once the release exists:

```bash
gh release download data --dir data --repo TYISRAGE/aii600-final-project
```
