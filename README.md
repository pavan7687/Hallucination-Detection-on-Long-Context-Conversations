# Hallucination Detection on Long-Context Multi-Turn Conversations

A novel dataset and pipeline for studying hallucinations in code generation from long-context, multi-turn developer conversations.

## Overview

This project addresses a gap in existing hallucination detection research: no dataset combines **multi-turn natural-language conversations**, **code review discussions**, and **verified ground-truth code changes** in one place. We built our own dataset by mining GitHub repositories directly.

## What This Project Does

Given a GitHub issue and its linked, merged Pull Request, we extract:

- The **issue discussion** — why a code change is needed (problem analysis, design discussion, decisions)
- The **PR review discussion** — how the change was implemented (code review comments, requested fixes, approvals)
- The **code before and after** the change (verified ground truth from the merged commit)

These are merged into a single chronological, multi-turn conversation with a transition marker separating the "problem" phase from the "solution" phase.

## Dataset

- **975 samples** across **13 major open-source repositories**
- **13 programming languages** (Python, Rust, Go, Java, Ruby, TypeScript, Dart, and more)
- **Average 26.4 turns per sample** (9.5 issue turns + 15.9 PR turns), max 80 turns
- Every sample includes `before_code` and `after_code` as verified ground truth

| Repository | Language | Samples |
|---|---|---|
| rust-lang/rust | Rust | 212 |
| kubernetes/kubernetes | Go | 169 |
| numpy/numpy | Python | 93 |
| python/cpython | Python | 75 |
| scikit-learn/scikit-learn | Python | 70 |
| elastic/elasticsearch | Java | 65 |
| keras-team/keras | Python | 63 |
| django/django | Python | 61 |
| flutter/flutter | Dart | 56 |
| rails/rails | Ruby | 52 |
| pandas-dev/pandas | Python | 36 |
| microsoft/TypeScript | TypeScript | 19 |
| pytorch/pytorch | Python | 4 |

## Pipeline

1. Scan closed issues in each repository
2. Keep only issues with a linked, merged Pull Request
3. Fetch the issue conversation (problem discussion)
4. Fetch the PR review conversation (code review discussion)
5. Fetch the code file before and after the merge
6. Merge both conversations in chronological order
7. Insert a transition marker between problem and solution phases
8. Remove bot comments and low-information noise (LGTM, +1, templates)
9. Save as a structured JSON record

## Sample Structure

```json
{
  "sample_id": "django/django#issue16012",
  "repository": "django/django",
  "language": "Python",
  "num_turns": 80,
  "num_issue_turns": 38,
  "num_pr_turns": 41,
  "focus_file": "django/contrib/admin/options.py",
  "conversation": [
    {"turn": 1, "source": "issue", "author": "...", "text": "...", "date": "..."},
    {"turn": 39, "source": "transition", "author": "system", "text": "PR opened..."},
    {"turn": 40, "source": "pr", "author": "...", "text": "...", "date": "..."}
  ],
  "before_code": "...",
  "after_code": "..."
}
```

## Why This Matters

Existing hallucination benchmarks are either single-turn with no real conversation (e.g., CodeHaluEval) or multi-turn with no ground-truth code (e.g., HalluDetect, DiaHalu). This dataset provides both — enabling research into how LLMs hallucinate when generating code from long, realistic multi-turn discussions.

| Dataset | Samples | Avg Turns | Multi-turn | Ground Truth Code |
|---|---|---|---|---|
| HalluDetect (2025) | 115 | 7.39 | ✅ | ❌ |
| DiaHalu | 200 | 8–10 | ✅ | ❌ |
| CodeHaluEval | 8,883 | Single | ❌ | ✅ |
| **This dataset** | **975** | **26.4** | ✅ | ✅ |

## Status

- ✅ Dataset collection complete
- 🔄 Hallucination detection pipeline (in progress) — feeds conversations to an LLM, generates code, and compares against ground truth using a three-tier detector (ecosystem, historical, structural hallucinations)

