# Claude Code Review Action

A GitHub Action that provides automated code reviews using Anthropic's Claude AI on pull requests.

## Features

- 🤖 **AI-powered code reviews** on every pull request
- 💬 **On-demand reviews** via `@claude review` comments
- 🔍 **Comprehensive analysis** of code quality, security, and best practices
- ⚡ **Easy integration** with existing workflows
- 🎯 **Customizable instructions** for focused reviews

## Quick Start

### 1. Get Claude Code OAuth Token

1. Visit [claude.ai/code](https://claude.ai/code)
2. Generate an OAuth token for API access
3. Save it for the next step

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
  code-review:
    # Run on PR events, or when someone comments "@claude review" on a PR
    if: |
      github.event_name == 'pull_request' ||
      (github.event_name == 'issue_comment' && 
        github.event.issue.pull_request && 
        contains(github.event.comment.body, '@claude review'))
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      issues: write
      id-token: write
    steps:
      - name: Run Claude Code Review
        uses: pylesoft/claude-code-review-action@master
        with:
          claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
```

## Usage

Once configured, Claude will automatically review:

1. **New pull requests** - When opened
2. **Updated pull requests** - When new commits are pushed
3. **On-demand** - Comment `@claude review` on any PR

## Configuration Options

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `claude_code_oauth_token` | OAuth token for Claude Code API | Yes | - |
| `mode` | Review mode (`experimental-review` or `review`) | No | `experimental-review` |
| `timeout_minutes` | Timeout for review process | No | `30` |
| `custom_instructions` | Custom review instructions | No | See below |

### Custom Instructions

You can customize Claude's review focus:

```yaml
- name: Run Claude Code Review
  uses: pylesoft/claude-code-review-action@master
  with:
    claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
    custom_instructions: |
      Focus on React best practices and hooks usage.
      Check for accessibility issues.
      Ensure proper error handling.
```

### Default Review Focus

By default, Claude reviews for:

- Code quality and maintainability
- Security vulnerabilities
- Performance issues
- Best practices and design patterns
- Test coverage gaps

## Advanced Examples

### Basic Review

```yaml
- uses: pylesoft/claude-code-review-action@master
  with:
    claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
```

### With Custom Instructions

```yaml
- uses: pylesoft/claude-code-review-action@master
  with:
    claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
    custom_instructions: |
      Pay special attention to:
      - SQL injection vulnerabilities
      - API endpoint security
      - Input validation
```

### Extended Timeout

```yaml
- uses: pylesoft/claude-code-review-action@master
  with:
    claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
    timeout_minutes: "45"
```

## Permissions

The action requires these GitHub permissions:

- `contents: read` - To access repository code
- `pull-requests: write` - To post review comments
- `issues: write` - To respond to issue comments
- `id-token: write` - For secure authentication

## Troubleshooting

### Common Issues

1. **"Error: Credentials not found"**
   - Ensure `CLAUDE_CODE_OAUTH_TOKEN` is set in repository secrets
   - Check the secret name matches exactly

2. **Reviews not triggering**
   - Verify workflow file is in `.github/workflows/`
   - Check workflow permissions are set correctly
   - Ensure PR events are enabled in repository settings

3. **Timeout errors**
   - Increase `timeout_minutes` for large PRs
   - Consider breaking large PRs into smaller ones

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License.

## Acknowledgments

Built on top of [Anthropic's Claude Code Action](https://github.com/anthropics/claude-code-action).
