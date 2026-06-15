# Dataset

## `dataset_issue_first.json`

975 samples of GitHub Issue + Pull Request conversations paired with before/after code, collected from 13 open-source repositories across 13 programming languages.

Each sample contains:

- **Issue discussion** — why the change was needed (`source: "issue"`)
- **Transition marker** — bridges problem and solution (`source: "transition"`)
- **PR review discussion** — how the code was changed (`source: "pr"`)
- **`before_code`** — file content before the fix
- **`after_code`** — file content after merge (ground truth)

## Stats

- Total samples: 975
- Avg turns per sample: 26.4 (9.5 issue + 15.9 PR)
- Max turns: 80
- Repositories: 13

### Per-Repository Language Breakdown

| Repository | Total | Language Breakdown |
|---|---|---|
| rust-lang/rust | 212 | Rust 180, Other 28, Python 3, C++ 1 |
| kubernetes/kubernetes | 169 | Go 143, Other 26 |
| numpy/numpy | 93 | Python 43, C 23, Other 22, C++ 5 |
| python/cpython | 75 | Python 36, Other 21, C 18 |
| scikit-learn/scikit-learn | 70 | Python 55, Other 15 |
| elastic/elasticsearch | 65 | Java 55, Other 10 |
| keras-team/keras | 63 | Python 62, Other 1 |
| django/django | 61 | Python 52, Other 5, JavaScript 4 |
| flutter/flutter | 56 | Dart 26, Other 24, Python 2, Java 2, Kotlin 1, Swift 1 |
| rails/rails | 52 | Ruby 48, Other 4 |
| pandas-dev/pandas | 36 | Python 31, Other 4, C 1 |
| microsoft/TypeScript | 19 | TypeScript 8, Other 7, JavaScript 4 |
| pytorch/pytorch | 4 | Python 4 |

### Overall Language Distribution

| Language | Samples |
|---|---|
| Python | 288 |
| Rust | 180 |
| Other | 167 |
| Go | 143 |
| Java | 57 |
| Ruby | 48 |
| C | 42 |
| Dart | 26 |
| JavaScript | 8 |
| TypeScript | 8 |
| C++ | 6 |
| Kotlin | 1 |
| Swift | 1 |

*Language is detected per-sample from the actual files changed in the PR, not the repo's primary language. "Other" includes build configs, shell scripts, and files with unrecognized extensions.*

## Sample Format

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

See main [README](../README.md) for full pipeline details.
