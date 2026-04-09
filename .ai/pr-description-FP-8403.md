## Summary

- Adds `actions/slack-deploy-notify/action.yml` — a reusable composite action that wraps `act10ns/slack@v2.1.0` with a standardized deploy notification interface
- Inputs: `status` (required), `message` (optional), `steps` (optional, pass `toJson(steps)`)
- Caller sets `SLACK_WEBHOOK_URL` as an env var; the action inherits it automatically
- The notify step runs with `if: always()` so notifications fire even when earlier steps fail

## Usage example

```yaml
- name: Notify Slack
  uses: All-Campus/.github/actions/slack-deploy-notify@main
  with:
    status: ${{ job.status }}
    message: 'Deployed partner-hub to production'
    steps: ${{ toJson(steps) }}
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_DEPLOY_WEBHOOK }}
```

## Out of scope (manual steps)
- Set up Slack Incoming Webhook URLs for the two new channels
- Add `SLACK_DEPLOY_WEBHOOK` repo secrets to `partner-hub` and `snowflake`

## Test plan
- [ ] Verify action YAML parses correctly (no syntax errors)
- [ ] Trigger a workflow in a consumer repo using this action with `SLACK_DEPLOY_WEBHOOK` secret set
- [ ] Confirm notification fires on both success and failure paths
