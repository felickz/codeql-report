# felickz/codeql-report GitHub Action

This action allows you to generate a CodeQL Actions workflow success/failure report.  It walks all repos looking for Actions runs against non pr branches and reports on the 
- Conclusion (success/failure/null)
- Workflow_Url
- Is_Default (if CodeQL is enabled via Code Scanning default setup)
- Org
- Repo
- Workflow_Path

<img width="978" alt="image" src="https://github.com/felickz/codeql-report/assets/1760475/ccc5a2b4-252e-40e1-8eba-b9c30c12bd8b">


## Usage

To use the `felickz/codeql-report` action, you need to set it up in a workflow file (`.github/workflows/codeql-report.yml`).

Here's a basic example:

```yaml
name: CodeQL Report

on:
  push:
    paths:
      - '.github/workflows/codeql-report.yml'
  workflow_dispatch:
  #every 6 hours
  schedule:
    - cron: '0 */6 * * *'

jobs:
  run-report:
    runs-on: ubuntu-latest

    steps:
    - name: Use felickz/codeql-report action
      uses: felickz/codeql-report@main
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
    - name: Upload CodeQL Report CSV as Artifact
      uses: actions/upload-artifact@v4
      with:
        name: "CodeQLReport-${{ github.run_id }}"
        path: ./*.csv
```

In this example, the felickz/codeql-report action is used from the `main` branch directly.  The report is run every 6 hours via cron schedule.

The github-token input is required for the felickz/codeql-report action. It uses the GITHUB_TOKEN secret, which would need to have `metadata:read` and `actions:read` permissions for your organization for any private repos.

The `upload-artfact` action is used to create the CSV attached to the action workflow summary.

## Inputs
### github-token
Required The GitHub token to authenticate and pull CodeQL Action workflow status with.  Needs to have `metadata:read` and `actions:read` permissions for your organization's private repos.
### organization
Optional The GitHub Organization. Defaults to the current Organization.
