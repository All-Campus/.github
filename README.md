# All-Campus .github

Org-level reusable GitHub Actions workflows and community health files.

## Reusable Workflows

### `stale-prs.yml` — Auto-close stale PRs

Labels PRs as `stale` after a period of inactivity and auto-closes them if no activity resumes.

**Usage** — add this to `.github/workflows/stale.yml` in any repo:

```yaml
name: 'Close stale PRs'
on:
  schedule:
    - cron: '30 9 * * 1-5' # Weekdays 9:30 AM UTC / 5:30 AM ET
  workflow_dispatch: {}

jobs:
  stale:
    uses: All-Campus/.github/.github/workflows/stale-prs.yml@main
```

**Inputs** (all optional):

| Input | Default | Description |
|-------|---------|-------------|
| `days-before-stale` | `14` | Days of inactivity before labeling a PR stale |
| `days-before-close` | `7` | Days after stale label before auto-closing |
| `exempt-pr-labels` | `do-not-close,wip` | Labels that exempt a PR |
| `exempt-draft-pr` | `true` | Whether draft PRs are exempt |

**Required labels** in the calling repo: `stale`, `do-not-close` (optional), `wip` (optional).
