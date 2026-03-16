# Enforce Semi-Linear History

A reusable GitHub Action that ensures pull request branches maintain a
semi-linear (rebase) history — no merge commits, and always rebased on top of
the base branch.

## Usage

```yaml
name: Enforce Semi-Linear History

on:
  pull_request:
    branches: [main]

jobs:
  check-linear-branch:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          ref: ${{ github.event.pull_request.head.sha }}

      - uses: your-org/enforce-semi-linear@v1
```

### Inputs

| Name       | Description                  | Default           |
|------------|------------------------------|--------------------|
| `base_ref` | Base branch to check against | `github.base_ref` |

## Requirements

The checkout step **must** use:

- `fetch-depth: 0` — full history is needed to detect merge commits
- `ref: ${{ github.event.pull_request.head.sha }}` — checks out the actual
  branch head, not GitHub's synthetic merge commit
