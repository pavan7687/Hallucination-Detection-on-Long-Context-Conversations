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
- Languages: 13
- Repositories: 13

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
