# QuantoLens

**Quantum-Coherent Assistive Spectacles for Real-Time Multilingual Speech-to-Text Transcription: A Hybrid Quantum-Classical Architecture with DisCoCat Compositional Semantics and Noise-Resilient Quantum Signal Enhancement**

This repository contains the LaTeX source for the QuantoLens manuscript, along with the figures, bibliography, and glossary files needed to build it. QuantoLens is a proposed hybrid quantum-classical architecture for assistive smart-glasses transcription, combining a DisCoCat-based compositional semantic processor with quantum Fourier-transform-based signal denoising.

## What's in here

The paper is long — over 30 sections including a full appendix — so here's the map:

| Part | What it covers |
|---|---|
| §1–2 | Introduction, and the DisCoCat/QNLP theoretical foundations |
| §3–5 | Quantum signal processing, the hybrid architecture, and the five original quantum operators (with proofs) |
| §6–7 | Multilingual/code-switching handling and edge hardware constraints |
| §8–9 | Experimental setup and quantitative results |
| §10–11 | Discussion and conclusion |
| Appendices | Proofs, circuit specs, training configs, an extended results section, statistical tests, and a large glossary of QNLP terminology |

## Repository structure

```
.
├── main.tex                  # Root document (this is the file you compile)
├── glossary.tex              # Acronym/glossary definitions (\gls{} entries)
├── references.bib            # Bibliography (biblatex/biber, numeric style)
├── figures/                  # All \includegraphics assets referenced below
└── README.md
```

### Figures the manuscript expects

The source references two kinds of figures. Make sure both sets are present in `figures/` (or wherever your `\graphicspath` points) before you build:

- **Architecture/diagram figures:** `fig_architecture`, and `New_Dia_1` through `New_Dia_41` (conceptual diagrams; a few numbers in that range are unused, which is normal).
- **Data/results figures (PDF):** `01_cv_metadata_overview`, `02_mfcc_single_clip`, `03_feature_distributions`, `04_feature_correlation_heatmap`, `06_unrated_transcripts_filesizes`, `07_massive_overview`, `09_cross_dataset_summary`, `10_pairplot_acoustic_features`, `12_best_model_confusion`, `12_massive_confusion_matrix`, `14_quantum_tsne`, `15_quantum_feature_matrix`, `16_comprehensive_acoustic_analysis`, `24_learning_curves`, `25_intent_confusion`, `32_roc_curves`, `35_code_switch`.

The preamble defines a `\safeincludegraphics` fallback that checks for `.png`/`.pdf`/`.jpg`/`.jpeg` variants and drops in a placeholder box if none exist, so a missing figure won't break the build — it'll just show up as an empty box with the filename, which is a handy way to spot anything you forgot to add.

## Building the document

You'll need a full TeX distribution (TeX Live 2023+ or MiKTeX) with `biber` and `latexmk` on your path. This document uses `biblatex`/`biber` (not classic BibTeX) and `glossaries` (which needs `makeglossaries`), so a plain `pdflatex` × 2 pass won't resolve citations or acronyms.

**Recommended — one command via latexmk:**
```bash
latexmk -pdf -shell-escape main.tex
```


### Dependencies (LaTeX packages)

Standard TeX Live/MiKTeX installs should cover all of these, but if you're on a minimal install, you'll specifically need:

- **Quantum circuits:** `quantikz`, `tikz` (with the `shapes.geometric`, `arrows.meta`, `decorations.pathreplacing`, `matrix`, `3d`, and related libraries)
- **Plots:** `pgfplots` (compat 1.18) with `groupplots`, `fillbetween`, `colorbrewer`, `statistics`
- **Bibliography:** `biblatex` + `biber` backend (numeric style), `csquotes`
- **Glossary/acronyms:** `glossaries` (needs a working `makeglossaries` / Perl on your system)
- **Math:** `amsmath`, `amssymb`, `amsthm`, `mathtools`, `bm`, `bbm`, `accents`
- **Tables:** `booktabs`, `multirow`, `makecell`, `tabularx`, `longtable`, `array`
- **Fonts/typography:** `lmodern`, `microtype`, `tipa`
- **Algorithms:** `algorithm2e`
- Standard: `graphicx`, `subcaption`, `float`, `wrapfig`, `hyperref`, `cleveref`, `fancyhdr`, `xcolor`, `geometry`, `enumitem`, `caption`

If `-shell-escape` isn't available in your environment (some shared/CI TeX setups disable it by default), check whether any package in your local build actually requires it — the ones above don't strictly need it unless you're running additional externalized-plot or SVG-conversion tooling on top.

## Datasets referenced

The manuscript evaluates against several public multilingual speech/NLU corpora:

- **[Common Voice 21.0](https://commonvoice.org)** (Mozilla) — 134-language crowd-sourced speech corpus, CC0 licensed. Used as the primary training/eval signal.
- **[Amazon MASSIVE](https://github.com/alexa/massive)** — 51-language intent/slot dataset, used for the cross-lingual transfer (CLCT) experiments.
- **Multilingual LibriSpeech** — clean read-speech, used as an upper-performance reference.
- **MLC Spoken Words** — used for code-switching evaluation.
- **CHiME-6** — cocktail-party noise conditions.

None of these are vendored in this repository — pull them directly from their sources above under their respective licenses.

## Important notes on reported results

This section exists because the manuscript itself is candid about it in the appendix figure captions, and that context is worth surfacing here for anyone building or citing from this repo rather than leaving it buried on page 60+:

- The abstract and conclusion report headline numbers (WER 12.8% quiet / 18.6% at 85 dB, RTFx 418×, 640 mW, a 7.6-point margin over Whisper-small) framed as experimental results.
- The appendix's own data-summary figure states that the ASR evaluation run referenced there produced **no WER/CER figure at all**, because the audio files it needed weren't found on disk during that run.
- Several other appendix figures are explicitly labeled as **classical simulations or synthetic proxies**, not quantum-hardware or fully-measured outputs — e.g., the "gender" classification figure uses a median-F0 split as a stand-in label (not a real demographic field), the quantum feature heatmaps are described as classical kernel-based simulations, and the code-switching WER-vs-percentage curve is generated by the pipeline's simulation fallback in the absence of real code-switched audio.
- The learning-curve figure is fit on only 48 samples, with training accuracy pinned at 1.0 (overfitting), so the ~89% validation figure quoted there shouldn't be read as a stable, generalizable number.

None of this means the theoretical contributions aren't sound — the operator derivations and proofs are a separate matter from the empirical section. But before this goes out for review, it's worth reconciling the abstract/conclusion's framing with what the appendix actually supports, so reviewers don't find the gap for you. The cleanest fix is usually to either (a) qualify the headline numbers as simulation-based projections rather than measured results, or (b) re-run the pipeline against real audio so the numbers in the abstract match what the appendix demonstrates.
