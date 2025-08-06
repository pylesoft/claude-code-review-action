# Claude PR Review Action

Automated code reviews powered by Claude AI for GitHub pull requests. A reusable action for Pylesoft repositories.

## Features

-   **AI-powered code reviews** on every pull request
-   **On-demand reviews** via `@claude review` comments
-   **Full context analysis** with complete git history
-   **Zero configuration** - just add your token

## Quick Start

### 1. Get your Claude token

Visit [claude.ai/code](https://claude.ai/code) to get your OAuth token.

### 2. Add token to repository secrets

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Add new secret: `CLAUDE_CODE_OAUTH_TOKEN`

### 3. Add workflow

Create `.github/workflows/claude-review.yml`:

```yaml
name: Claude PR Review

on:
    pull_request:
        types: [opened, synchronize, reopened]
    issue_comment:
        types: [created]

jobs:
    review:
        if: github.event_name == 'pull_request' || (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude review'))
        runs-on: ubuntu-latest
        permissions:
            contents: read
            pull-requests: write
            issues: write
            id-token: write

        steps:
            - uses: actions/checkout@v4
              with:
                  fetch-depth: 0

            - uses: pylesoft/claude-code-review-action@master
              with:
                  api_key: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
              timeout-minutes: 30
```

### 4. Use it

-   **Automatic**: Claude reviews every new PR and update
-   **Manual**: Comment `@claude review` on any PR

## How It Works

1. **PR opened/updated** → Claude analyzes the changes
2. **Reviews code** for bugs, style, performance, security
3. **Posts comments** with suggestions and improvements
4. **Responds to mentions** for follow-up questions

## Configuration

| Setting                   | Description                              | Required |
| ------------------------- | ---------------------------------------- | -------- |
| `CLAUDE_CODE_OAUTH_TOKEN` | Authentication token from claude.ai/code | ✅ Yes   |

## Permissions Required

-   `contents: read` - Read repository code
-   `pull-requests: write` - Post review comments
-   `issues: write` - Respond to issue comments
-   `id-token: write` - GitHub OIDC token

## Examples

### Basic Review

```
@claude review
```

### Specific Focus

```
@claude review - focus on security implications
```

### Ask Questions

```
@claude is this SQL query vulnerable to injection?
```

## Requirements

-   GitHub repository with pull requests enabled
-   Claude OAuth token from [claude.ai/code](https://claude.ai/code)
-   Workflow permissions configured

## License

MIT
