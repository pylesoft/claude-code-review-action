# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a GitHub Action that integrates Anthropic's Claude AI for automated code reviews on pull requests. It's a configuration-only project that wraps the `anthropics/claude-code-action@beta` action.

## Project Structure

```
claude-code-review-action/
├── action.yml          # Main GitHub Action configuration (needs cleanup - has duplicate jobs)
├── .gitattributes      # Git line ending configuration
└── .claude/            # Claude Code IDE settings
```

## Key Configuration

The action triggers on:
- Pull request events: opened, synchronize, reopened
- Issue comments containing "@claude review"

Required secrets:
- `CLAUDE_CODE_OAUTH_TOKEN` - Authentication token for Claude API

## Common Development Tasks

Since this is a GitHub Action configuration project, there are no build/test/lint commands. Development focuses on:

1. **Editing action.yml**: The main configuration file that defines the GitHub Action workflow
2. **Testing**: Create test PRs in a repository that uses this action
3. **Validation**: Use GitHub's action syntax validation or yamllint

## Important Notes

1. **action.yml has structural issues**: Contains duplicate job definitions that need to be resolved
2. **No local development environment**: This is purely a GitHub Action configuration
3. **External dependency**: All functionality comes from `anthropics/claude-code-action@beta`
4. **Permissions required**: The action needs read access to contents and write access to pull requests and issues

## Architecture

This is a thin wrapper around Anthropic's Claude code review action. The architecture is:

1. GitHub triggers the action on PR events or comments
2. The action checks out the repository with full history
3. It invokes the Claude code review action with the OAuth token
4. Claude analyzes the code and posts review comments on the PR

No custom code or logic exists in this repository - it's purely configuration.