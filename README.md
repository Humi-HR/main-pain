# Main Pain

⚠ This repository is public because GitHub actions [must](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace) be in a public repository.

## What is this?

A GitHub action to notify a Slack webhook that the main branch is in a failing state.

We use this at Humi to let us know if failing code has somehow made it in to
the main branch so that we can fix the issue right away.

## Sample Usage

```yaml
# This job will run only if the `needs` jobs have a failure.
main-pain:
  name: Main Pain
  runs-on: ubuntu-20.04
  needs: [test-frontend, test-backend] # These are the jobs about which we want to notify if there is a failure.
  if: always() && github.ref == 'refs/heads/main' && contains(join(needs.*.result, ','), 'failure')
  steps:
    - name: Notify Slack Channel
      uses: Humi-HR/main-pain@main
      with:
        slack-webhook-url: ${{ secrets.MAIN_PAIN_WEBHOOK_URL }}
```

## Requirements

You must provide the `slack-webhook-url`.
