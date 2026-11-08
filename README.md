# 🧭 Left Behind, Not Averaged Out: A Poset-Based Identification of Youth Deprivation in Europe

This repository contains a group project developed for the **Data Science Lab** course as part of the MSc in Data Science at the **University of Milan-Bicocca (UNIMIB)**.

The project asks which European countries are *unambiguously* leaving their young people behind. It applies **partial order set (poset) theory**, a non-compensatory alternative to composite indices, to **five youth-deprivation indicators** from OECD, Eurostat and PISA data across **24 European countries**, and compares the result with a conventional equal-weight composite index.

📄 **Report:** [`DSLab_project-Left_Behind__Not_Averaged_Out.pdf`](./DSLab_project-Left_Behind__Not_Averaged_Out.pdf)

💻 **Full pipeline (R Markdown):** [`Left_Behind__Not_Averaged_Out.Rmd`](./Left_Behind__Not_Averaged_Out.Rmd)

---

## 🎯 Project Objectives & Research Questions

Composite indices average several indicators, so a good score on one dimension can compensate for a severe weakness on another. A poset only ranks a country as more deprived than another if it is at least as deprived on **every** dimension, and leaves the other profiles **incomparable** instead of forcing them into a single number. The analysis is built around three questions:

* **Who is unambiguously left behind?** Which countries are certainly deprived, and which are certainly not, regardless of how the five dimensions are combined?
* **Where do the two methods disagree?** Which countries are classified substantially differently by a composite index and by the poset, and why?
* **How robust are the results?** How sensitive is the classification to methodological choices (indicators, thresholds, number of grade levels), and which single dimension weighs most for the hard-to-classify countries?

---

## 🛠️ Tech Stack & Workflow

* **R & R Markdown:** Complete, executable pipeline, from data download to final results.
* **`httr` & `eurostat`:** Automatic data download from the OECD SDMX API, with Eurostat as a fallback for permanent gaps.
* **`jsonlite`:** Export of the final summary of results to JSON.
* **`tidyverse` (readr, dplyr, tidyr, purrr):** Data cleaning, orientation and reshaping.
* **`parsec`:** Reference poset implementation, used to cross-validate a from-scratch implementation.
* **`ggplot2`, `corrplot`, `pheatmap`:** Correlation matrices, Hasse diagram and result visualisations.

---

## 📈 Key Insights

* **Five countries are certainly deprived** (identification degree = 1): Spain, Italy, Austria, Lithuania and the United Kingdom.
* **Nine are certainly not deprived** (identification degree = 0): Belgium, Czechia, France, Ireland, Latvia, the Netherlands, Poland, Slovenia and Switzerland.
* **Ten remain ambiguous**, from Estonia (0.474) to Norway (0.947): a definitive classification is possible for only **14 of 24** countries.
* **Severity separates the certain group:** Spain (0.858) and Italy (0.771) are far more severely deprived than Austria, Lithuania and the UK (0.385).
* **Poset vs composite index:** rank correlation of **ρ = 0.85**, but **9 countries shift by at least 3 positions**. Austria and the UK are penalised by the poset (one severe NEET weakness averaged away), while France is mid-table on the composite index but certainly not deprived under the poset.
* **Two implementations agree closely:** Spearman **ρ = 0.9861** between the from-scratch and `parsec` results.
* **NEET is the main blocking dimension:** it keeps **13 of 19** non-certain countries below certain classification.
* **The data are more ordered than chance:** 44.2% of country pairs are incomparable, against a null mean of 63.5% (permutation test, p ≈ 0.003).

---

## 📡 Data Sources

| Indicator | Source | Notes |
|---|---|---|
| Youth monetary poverty (18–25) | OECD SDG database (Goal 1); Eurostat `ilc_li02` as fallback | |
| NEET rate | OECD SDG database (Goal 8); Eurostat `edat_lfse_20` as fallback | Ages 15–24 |
| Youth unemployment | OECD SDG database (Goal 8); Eurostat `une_rt_a` as fallback | |
| Low math skills | PISA 2022 (via OECD SDG database, Goal 4) | |
| Low upper-secondary completion | OECD SDG database (Goal 4); Eurostat `edat_lfse_14` as fallback | Proxy for early leaving |
| ~~Low reading skills (PISA)~~ | OECD SDG database | Dropped: ρ = 0.67 with math skills |
| ~~Lack of a bank account~~ | OECD SDG database (Goal 8), based on World Bank Global Findex 2021 | Dropped: nearly saturated across Europe |

* **Sample:** 24 European OECD countries.
* **Access:** data are downloaded automatically by the pipeline, so no local data files are needed. Only 4 cells required the Eurostat fallback, all on the completion indicator (Czechia, Ireland, Netherlands, Slovakia). No value is estimated or interpolated.
* **Orientation:** all indicators are coded so that higher values always mean more deprivation.

---

## 🔬 Methodology

* **Cleaning:** three-step missing-value search (OECD 2019–2024, OECD 2015–2024, Eurostat), with every indicator removal clearly labelled as pre-registered or post-hoc.
* **Ordinal discretisation:** three grades (0 = low, 1 = medium, 2 = high deprivation), with every cut-point labelled as an official EU target, an empirical anchor or a normative convention. Schema A (policy-anchored) is the main analysis, compared with terciles (B) and quintiles (C).
* **Poset construction:** country profiles ordered by component-wise dominance, with a threshold profile τ = (1, 1, 1, 1, 1) added as an extra node.
* **Identification degree & severity:** estimated by uniformly sampling **20,000 linear extensions** (Bubley–Dyer chain).
* **Cross-validation:** a from-scratch R implementation checked against `parsec`, both validated first on a toy poset with hand-computable results.
* **Composite-index benchmark:** equal-weight average of the five min–max normalised indicators, compared via Spearman correlation.
* **Robustness checks:** indicator jackknife, country jackknife, alternative discretisation schemes, permutation test (1,000 replications) and a blocking-dimension analysis.

---

## ⚠️ Limitations & Notes

* **Small sample:** only 24 countries, so correlations and robustness checks can be sensitive to a few extreme cases.
* **Misaligned reference years:** labour data 2023–24, PISA 2022 and poverty 2021–23.
* **Proxy indicator:** low upper-secondary completion approximates the EU "early leavers" measure.
* **Live data:** the OECD API is rate-limited (20 requests per minute) and both sources may revise their series, so later runs can differ slightly from the reported figures. Sampling uses fixed seeds.
* **AI use:** AI tools were used to support code development; all methodological choices, data checks and final content were reviewed by the authors.

---

## 👥 Contributors

- [@ilaria-cesa](https://github.com/ilaria-cesa)
- [@mezza04](https://github.com/mezza04)

---
*Developed by Andrea Porro, Ilaria Anna Cesaratto, Chiara Mezzanzanica and Nicolò Nucci.*
