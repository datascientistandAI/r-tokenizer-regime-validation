# R-Tokenizer (Rphi / Witness Representation)

A learned representation that reads a market context window and produces a compact, discrete "witness" code meant to capture the market's hidden state directly from data — rather than relying on hand-specified indicators or a hand-picked number of regime buckets.

To test whether this representation actually captures something real, it was validated the way you'd validate a measurement instrument: build synthetic worlds where the true hidden regime is planted and known exactly, generate them from two different statistical model families so the result isn't an artifact of one generator's quirks, hold out entire paths the representation never saw during fitting, and check whether it can still tell "the regime stayed the same" apart from "the regime switched" — using pre-registered pass/fail gates, not judgment calls made after seeing the results.

On that held-out synthetic test, the representation recovered the planted regime switch perfectly across both generator families, clearing every one of 18 pre-declared statistical gates (bootstrap confidence intervals, permutation-null tests, cross-generator generalization). Two independent, legitimate regime-detection approaches — a Gaussian Hidden Markov Model and a realized-volatility-based classifier — were run on comparable synthetic recovery tasks as reference points, and both fell meaningfully short of it.

![R-Tokenizer vs. two regime-detection baselines](comparison.png)

**Important boundary, stated plainly:** this is a synthetic positive-control result, not a live-market performance claim. It shows the representation preserves planted ground-truth structure better than two credible baselines under controlled conditions — the next step (external point-in-time regime definition, independent held-out real-market evaluation) is what would be needed to turn this into a real-market claim, and that step hasn't been published here.

### Scope of the synthetic test — and why 100% isn't overfitting

The two generator families used here define a regime shift narrowly: a change in mean and variance (volatility-state switching). Real market regime shifts can show up through other channels this test doesn't touch — changes in correlation/dependency structure, tail and jump behavior, autocorrelation memory, liquidity and microstructure regime. A clean result on this synthetic test is evidence the representation recovers *this* kind of shift well; it is not evidence it recovers every kind of regime shift a real market can produce.

A perfect score on a single synthetic recipe would be a legitimate overfitting concern on its own. What argues against that here: the representation was trained on one generator family and tested, unseen, on the *other* — an architecturally different data-generating process it never saw during fitting — and performance held (100% balanced accuracy / AUROC training on `ms_garch_2state` and testing on `msm_3component`; 98.5% / 99.9% the reverse direction). That cross-family generalization, not the in-family number alone, is the stronger piece of evidence.

Beyond this formal test, informal behavior observed on the broader model is consistent with the representation picking up more than pure mean/variance shifts — but that broader claim hasn't yet been run through the same rigorous, pre-registered synthetic-validation process as the result above, so it's stated here as an open observation, not a validated result.

No architecture details, training procedure, or code are included — this is IP-sensitive work.

---

*This repo is a research showcase extracted from a larger private research workspace.*
