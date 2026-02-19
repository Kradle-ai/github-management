# Kradle GitHub Management

Shared GitHub Actions workflows for the Kradle org.

## Reusable Workflows

### Claude Code Review

Automated PR review using Claude. Reviews code quality, bugs, security, performance, and breaking changes. Posts a single comment per review and auto-collapses previous reviews.

**Setup for a new repo:**

1. Add `CLAUDE_CODE_OAUTH_TOKEN` as an org-level secret (Settings → Secrets and variables → Actions), or per-repo if preferred.

2. Create `.github/workflows/claude-code-review.yml` in your repo:

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, ready_for_review, reopened]

jobs:
  review:
    uses: Kradle-ai/github-management/.github/workflows/claude-code-review.yml@main
    secrets: inherit
```

That's it. Every PR will get an automated Claude review.

**Optional: Filter by file type:**

```yaml
on:
  pull_request:
    types: [opened, synchronize, ready_for_review, reopened]
    paths:
      - "src/**/*.ts"
      - "src/**/*.tsx"
      - "*.py"
```

**Optional: Filter by PR author:**

```yaml
jobs:
  review:
    if: github.event.pull_request.user.login != 'dependabot[bot]'
    uses: Kradle-ai/github-management/.github/workflows/claude-code-review.yml@main
    secrets: inherit
```
