# BTC Whale Tracker

A quantitative Bitcoin trading signal pipeline that tracks institutional whale flows from corporates, ETF funds, and governments — using Hidden Markov Models, Topological Data Analysis, and Convergent Cross Mapping to build a regime-aware signal with live monitoring.

## Overview

This project implements a full end-to-end research and live-signal pipeline:

1. **Data Ingestion** — BTC OHLCV, macro covariates (DXY, SPX, VIX), hash rate, and institutional whale transaction flows via the Arkham Intelligence API
2. **Signal Enrichment** — Dormancy factor, wash-trade filtering, and net flow velocity per entity class (Corporate, ETF Fund, Government)
3. **CCM Causality Validation** — Convergent Cross Mapping (Sugihara et al. 2012) confirms that whale flows causally precede BTC price movements before any model is built
4. **Graph Features** — Rolling 30-day directed transaction graph with Gini, WCC, SCC, and inflow concentration features
5. **TDA (Persistent Homology)** — Vietoris-Rips persistence on rolling graph state trajectories; Betti numbers and persistence entropy fed to HMM
6. **HMM Regime Detection** — 4-state Gaussian HMM (Bull, Bear-Trend, High-Vol, Neutral) trained on 19 features
7. **Factor Decomposition** — Per-regime OLS with HC3 robust standard errors and dual-spec beta stability analysis
8. **Three-Pillar Signal Architecture**:
   - **P1**: Corp–ETF Divergence (primary, regime-agnostic)
   - **P2**: ETF Contrarian in Bull (secondary)
   - **P3**: Hash Ribbon Bear multiplier (tertiary)
9. **Walk-Forward Backtest** — Monthly rebalancing, vol-targeting, out-of-sample 2024–2025
10. **Sensitivity Analysis** — 8-sweep parameter optimisation
11. **Live Monitor** — Daily Arkham API fetch → regime inference → signal → position log

## Pipeline Architecture
Cell 2  → Whale Transaction Load (Arkham Excel)
Cell 3  → BTC Hash Rate (CoinMetrics + Blockchain.com)
Cell 5  → OHLCV (yfinance)
Cell 6  → Macro Covariates (yfinance)
Cell 7  → Signal Enrichment (dormancy, wash-trade flag)
Cell 8  → Net Flow Velocity (7d / 30d / 90d windows)
Cell 9  → Hash Ribbons (30d/60d MA crossover)
Cell 10 → CCM Causality Validation ← GATE
Cell 11 → Transaction Graph Features
Cell 12 → TDA Persistent Homology
Cell 13 → HMM Feature Matrix Assembly
Cell 14 → HMM Regime Labeling
Cell 15 → Factor Decomposition (OLS per regime)
Cell 16 → Master Dataset Assembly
Cell 17 → Visual QA Gate
Cell 18 → Walk-Forward Backtest (v2)
Cell 19 → Performance Metrics & Attribution
Cell 20 → Sensitivity Analysis (8 sweeps)
Cell 21 → Live Monitor Extraction (Arkham API)

