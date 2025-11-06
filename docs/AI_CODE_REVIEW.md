# AI Code Review Guide

This guide explains how to use the AI-powered code review system integrated into the template.

## Overview

The template includes automated AI code review that runs on every pull request. The system uses AI to analyze code changes and provide actionable feedback.

## Features

✅ **Automatic Reviews** - Runs on every PR automatically  
✅ **Multi-Language Support** - Python, TypeScript, JavaScript, Go, Rust, and more  
✅ **Comprehensive Analysis** - Checks for:
   - Code style violations
   - Security vulnerabilities
   - Performance issues
   - Potential bugs
   - Documentation gaps

✅ **PR Comments** - Posts feedback directly on PRs  
✅ **Configurable** - Adaptable to your project's needs  

## Setup

### 1. Add API Key (Optional but Recommended)

The workflow works without API keys (basic analysis), but for full AI-powered reviews, add one of:

- **Gemini** (Recommended) - Get key from [Google AI Studio](https://makersuite.google.com/app/apikey)
- **OpenAI** - Get key from [OpenAI Platform](https://platform.openai.com/api-keys)
- **Anthropic** - Get key from [Anthropic Console](https://console.anthropic.com/)

Add to repository secrets:
- Settings → Secrets and variables → Actions
- Add `GEMINI_API_KEY` (or other provider key)

### 2. Workflow is Already Configured

The template includes `.github/workflows/code-review.yml` which is already set up!

### 3. Test It

Create a PR and the workflow will automatically run!

## How It Works

1. **PR Created/Updated** → Workflow triggers automatically
2. **Code Analysis** → AI analyzes changed files
3. **Review Generation** → AI generates review comments
4. **PR Comment** → Review posted on the PR

## Review Comment Format

The AI posts comments like this:

```markdown
## 🤖 AI Code Review

**Review Status:** ✅ Analysis Complete

### 📊 Change Summary
- Files Changed: 5
- Languages: Python, TypeScript

### 💡 Suggestions

1. **Security**: Consider using parameterized queries
2. **Performance**: Cache this expensive computation
3. **Style**: Follow PEP 8 naming conventions

---
*This is an automated review. Please also have a human reviewer check this PR.*
```

## Customization

### Adjust Review Focus

Edit `.github/workflows/code-review.yml`:

```yaml
jobs:
  review:
    uses: stevei101/cursor-agent-pr-review/.github/workflows/code-review-reusable.yml@main
    with:
      review_focus: 'security'  # Options: all, security, performance, style, bugs
      min_severity: 'high'      # Options: low, medium, high, critical
      max_comments: 5            # Limit number of comments
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
```

### Repository-Specific Configuration

Create `.cursor-review-config.json` in your repository root:

```json
{
  "focus_areas": ["security", "performance"],
  "languages": {
    "python": {
      "style_guide": "pep8",
      "min_test_coverage": 0.7
    }
  },
  "ignore_patterns": [
    "**/node_modules/**",
    "**/__pycache__/**"
  ]
}
```

## Best Practices

1. **Use as Augmentation** - AI reviews complement human reviews
2. **Review Suggestions** - Always validate AI suggestions
3. **Configure Appropriately** - Adjust focus based on project needs
4. **Monitor Feedback** - Update config based on team feedback

## Troubleshooting

### Workflow Not Running

- ✅ Check workflow file is in `.github/workflows/`
- ✅ Verify PR trigger is configured
- ✅ Ensure workflow is committed to default branch

### No Comments Posted

- ✅ Check workflow logs for errors
- ✅ Verify API keys are configured (if using AI services)
- ✅ Ensure PR has code changes

### Too Many Comments

- ✅ Adjust `max_comments` input
- ✅ Increase `min_severity` threshold
- ✅ Customize `review_focus` areas

## Resources

- [Cursor-Agent PR Review Repository](https://github.com/stevei101/cursor-agent-pr-review)
- [Integration Guide](https://github.com/stevei101/cursor-agent-pr-review/blob/main/docs/INTEGRATION_GUIDE.md)
- [Main README](https://github.com/stevei101/cursor-agent-pr-review#readme)

---

**Note**: AI reviews are automated and should always be supplemented with human code review for architectural decisions and business logic.

