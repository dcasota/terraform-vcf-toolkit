
**Markdown**
```markdown
[![GitHub Clones](https://img.shields.io/badge/dynamic/json?color=success&label=Clone&query=count&url=https://gist.githubusercontent.com/nathanthaler/a70d34a404527b23caff392674e753c4/raw/clone.json&logo=github)](https://gist.githubusercontent.com/nathanthaler/a70d34a404527b23caff392674e753c4/raw/clone.json)
```

## Setup Instructions for Clone Count Workflow

The `.github/workflows/clone.yml` workflow tracks GitHub clone statistics for this repository. To use it properly, you need to configure a Personal Access Token (PAT).

### Why is CLONE_SECRET_TOKEN needed?

The GitHub traffic/clones API endpoint requires **Administration (read)** permissions to access clone statistics. The default `GITHUB_TOKEN` provided by GitHub Actions typically doesn't have these permissions. Therefore, you need to create a Personal Access Token with the appropriate scope.

### How to set up CLONE_SECRET_TOKEN

1. **Create a Personal Access Token (PAT):**
   - Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Or directly visit: https://github.com/settings/tokens
   - Click "Generate new token" → "Generate new token (classic)"
   - Give it a descriptive name (e.g., "Clone Count Workflow")
   - Select the following scopes:
     - ✅ `repo` (Full control of private repositories) - This includes access to traffic data
     - ✅ `gist` (Create gists) - Required to store historical clone data
   - Set an appropriate expiration date
   - Click "Generate token"
   - **Important:** Copy the token immediately - you won't be able to see it again!

2. **Add the token as a repository secret:**
   - Go to your repository's Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `CLONE_SECRET_TOKEN`
   - Value: Paste the Personal Access Token you just created
   - Click "Add secret"

3. **Test the workflow:**
   - Go to the Actions tab in your repository
   - Select "GitHub Clone Count Update Everyday" workflow
   - Click "Run workflow" → "Run workflow"
   - Wait for the workflow to complete
   - Check if it succeeds

### Fallback Behavior

If `CLONE_SECRET_TOKEN` is not configured, the workflow will attempt to use the default `GITHUB_TOKEN`. However, this will likely fail when trying to access clone statistics with an error message explaining that the token doesn't have sufficient permissions.

### Troubleshooting

- **Error: "Input required and not supplied: token"** - This was fixed in the latest version. Update your workflow file.
- **Error: "Failed to fetch clone statistics"** - Make sure your PAT has the `repo` scope enabled.
- **Error: "Failed to create gist"** - Make sure your PAT has the `gist` scope enabled.
- **Token expired** - Generate a new PAT and update the `CLONE_SECRET_TOKEN` secret.
