# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

This is not a software project — it's a data analysis assignment for the course **Datacamp II** (Master 2 GI, 2025-2026). There is no existing code yet; the deliverable is a Python script to be written from scratch.

- `Travaux_datacamp2_M2_GI.pdf` — the assignment brief (in French): a PCA (ACP) exercise on server audit indicators, 16 questions (9 core + 7 "approfondissement").
- `serveurs_audit_acp_m2.csv` — the dataset: 160 servers, 11 quantitative indicators, 1 identifier, 1 synthetic segment label.

The data is explicitly synthetic/artificial ("aucune mesure opérationnelle réelle ni diagnostic de sécurité ne peut être déduit des résultats") — do not treat findings as real security conclusions.

## Dataset schema (`serveurs_audit_acp_m2.csv`)

| Column | Role |
|---|---|
| `serveur_id` | identifier — exclude from PCA |
| `cpu_pct`, `ram_pct`, `trafic_mbps`, `latence_ms`, `tentatives_intrusion`, `ports_exposes`, `vulnerabilites_ouvertes`, `jours_depuis_correctif`, `disponibilite_pct`, `temps_retablissement_min`, `sauvegardes_reussies_pct` | 11 active quantitative variables for the PCA |
| `segment_synthetique` | external categorical label (`peu_expose` / `intermediaire` / `expose`) — exclude from PCA, used only afterward for coloring/interpretation |

**Never modify the original CSV** (explicitly required by question 12, which involves temporarily removing an outlier server — do this on a copy/subset in memory, not by editing the file).

## Expected deliverable

A single commented Python file (pandas/numpy/scikit-learn/matplotlib or seaborn) that:
- Standardizes the 11 active variables (`StandardScaler`) and runs PCA on the correlation matrix (variables have different units/scales, so correlation-based PCA, not covariance-based).
- Produces the required plots: scree plot (éboulis), cumulative variance, correlation circle (CP1–CP2), heatmap (CP1–CP3), individuals projection colored by `segment_synthetique`. The answer key (`corrige_serveurs_acp_m2.py`, not present in this repo) is said to produce six graphs.
- Computes variable–axis correlations, contributions, and cos² for both variables and individuals.
- Covers reconstruction error vs. explained variance (question 8), bootstrap subspace stability (150 resamples, question 13), parallel analysis via permutation (150 permutations, question 14), and out-of-sample projection of a new fictive profile using the scaler/PCA fitted only on the original 160 servers (question 15) — no refitting.
- Ends with a 15–20 line written interpretation distinguishing data structure, visual exploration, and operational scope (i.e., staying epistemically cautious — PCA distance ≠ security incident).

## Working conventions

- Keep the analysis reproducible: fix random seeds for bootstrap/permutation steps.
- Any outlier-removal or robustness check (question 12) must operate on an in-memory copy, never on the CSV file itself.
- Since PCA axis signs are arbitrary, note this explicitly wherever CP1/CP2/CP3 are interpreted, and account for it when comparing subspaces across runs (e.g., question 12's projector-distance comparison).
