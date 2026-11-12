# Decisions

This file is the history of the project. Append an entry when a choice changes the work: the quantity, the data, the split, the model, the loss, or the action. Leave old entries in place. A later entry can reverse an earlier one. If a result depends on a random draw, record the seed under Why. The split entry records the cutoff date, or the seed and the fraction. The pull request that merges the code names this entry.

## 2026-11-11: dataset — UCI Spambase

- Decision. Use the UCI Spambase dataset (4,601 emails, 57 content features, CC BY 4.0) to predict P(spam) for an email provider's block/deliver decision.
- Why. Pre-extracted numeric features (no NLP pipeline needed), small enough to iterate on quickly before the 18 November checkpoint, and license permits republishing on the `data` release.
- Alternative. Raw Nazario phishing corpus + SpamAssassin ham, which is closer to "phishing" specifically but requires building a feature pipeline from scratch.
- Revisit. If time allows after the baseline, extend with a modern phishing-specific feature set (see Limits in `analysis.ipynb`).

## 2026-11-11: split — stratified random, seed 0

- Decision. 80/20 train/test split, stratified on the spam label, `random_state=0`.
- Why. The data carries no per-email timestamp finer than "circa 1999," so there is no later time period to hold out by date; a random split is the only option. Stratifying keeps the ~39.4% spam rate roughly equal in both halves.
- Alternative. A plain (non-stratified) random split; rejected since it risks an unlucky draw shifting the class balance on the smaller test set.
- Revisit. If a timestamped or multi-source version of the data turns up, switch to a time-based or source-based split instead.

## 2026-11-11: loss — asymmetric cost, false negative = 5x false positive

- Decision. Score the decision (block vs. deliver) with cost(false negative) = 5, cost(false positive) = 1, giving a cost-minimizing threshold t = 1/6 ≈ 0.167 on the fitted P(spam).
- Why. A missed phishing/spam email (delivered) is assumed worse than a blocked legitimate email, matching the hypothesis in `PROPOSAL.md`. The ratio itself is illustrative, not sourced from a real provider's incident data.
- Alternative. The default 0.5 threshold (equivalent to assuming the two mistakes cost the same); rejected because the proposal's whole premise is that they don't.
- Revisit. If a real cost ratio is found (e.g. from a published phishing-incident cost study), replace 5:1 with that number — `analysis.ipynb`'s Decision section already shows how much the threshold and cost move as the ratio changes.
