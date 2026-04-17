# All-Campus .github

Org-level reusable GitHub Actions workflows and composite actions for community health and deployment notifications.

## Composite Actions

### `slack-deploy-notify` — Standardized Slack deploy notifications

Wraps `act10ns/slack` with a consistent interface for deployment notifications. Sends a message to a Slack channel on deploy start, success, or failure.

**Usage:**

```yaml
- name: Notify Slack
  if: always()  # required — ensures the step runs even when the job fails
  uses: All-Campus/.github/actions/slack-deploy-notify@main
  with:
    status: ${{ job.status }}   # starting | success | failure | cancelled | warning
    message: 'Deployed my-app to production'
    steps: ${{ toJson(steps) }} # optional — enables per-step status icons
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_DEPLOY_WEBHOOK }}
```

**Inputs:**

| Input | Required | Description |
|-------|----------|-------------|
| `status` | yes | `starting`, `success`, `failure`, `cancelled`, or `warning` |
| `message` | no | Additional message text (supports Slack mrkdwn and `act10ns/slack` template vars like `{{ workflowRunUrl }}`) |
| `steps` | no | Pass `toJson(steps)` to enable per-step status icons in the notification |

**Setup required per repo:**
1. Create a Slack Incoming Webhook URL for your channel
2. Add it as a repo secret named `SLACK_DEPLOY_WEBHOOK`

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
