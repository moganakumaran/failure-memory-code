# Paper #1 — Code

Code for: **"Failure Memory Does Not Help Text-to-SQL Agents: A Compute-Fair,
Corrected-Label Evaluation"** (Mogana Kumaran Sivaraman, Draft v2, 2026-11-23).
This repository contains only the code. The manuscript, protocol, traces,
results, and datasets live in the companion project repository.

## Files

**Experiment harness**
- `pilot_lib.py` — model backends (OpenAI / OpenRouter / Ollama), budget
  ledger with hard cap, read-only SQL execution, embeddings, and the
  `EpisodeStore` that enforces the paper's no-gold memory-acquisition policy
  (§3.4) in code.
- `pilot.py` — the base agent and arms A0–A4 (§3.3): no memory, fingerprinted
  memory, generic memory, generate-and-select, stale memory; drift and
  undrifted-control modes; `--no-verifier` ablation mode (§5.6).

**Substrate construction**
- `build_manifest.py` — task manifest with label status (L1–L4, §3.1).
- `build_splits.py` — the repeated-failure split (§3.2).
- `build_drift.py`, `build_drift_lib.py`, `fix_drift_silent.py` — the drift
  split: 60 execution-verified drifted database variants, 55% silent (§3.2).

**Dry runs (taxonomy + base rates)**
- `dry_run.py` (local Qwen2.5-Coder-3B), `dry_run_frontier.py` (GPT-5),
  `dry_run_mini.py` (GPT-5 mini), `dry_run_or.py` (Qwen2.5-Coder-32B via
  OpenRouter) — the four dry runs behind the frozen failure taxonomy (§3.5).

**Analysis (every number in the paper)**
- `power_analysis.py` — simulation-based power analysis (§4).
- `analysis_stage1.py` — Stage 1: A0 vs A1, repeated split (§5.1).
- `analysis_stage2.py` — Stage 2: A3 (§5.2), A2 (§5.3), drift (§5.4).
- `analysis_rep.py` — open-model replication (§5.5).
- `analysis_a14.py` — Amendment A14: paired drift control (§5.4) and verifier
  ablation (§5.6).
- `analysis_fixes.py` — offline analyses: pass^k (§5.7), fingerprint retrieval
  rates (§5.3), drift-arm acquisition counts.
- `make_figure1.py`, `make_figure2.py` — the paper's figures.

All paired contrasts use per-task run-mean pass@1 with task-level bootstrap
95% CIs (10,000 resamples, seed 20261122).
