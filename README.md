# kaida-darice-de Organization Workflows

This repository contains organization-wide reusable workflows and community health files for all repositories in the **kaida-darice-de** organization.

## 🎯 What This Repository Does

This `.github` repository is a special organization-level repository that provides:

- **Reusable Workflows** - Shared GitHub Actions workflows that any repo can use
- **Community Health Files** - Default files (CODE_OF_CONDUCT, CONTRIBUTING, etc.) for all repos
- **Consistent Standards** - Enforce quality and practices across all 89+ repositories

## 🚀 Available Reusable Workflows

### 1. Codex Code Review

AI-powered code review using OpenAI Codex CLI.

**Location:** `.github/workflows/codex-code-review-reusable.yml`

**Features:**
- Structured JSON output with detailed feedback
- Configurable review depth (quick, standard, thorough)
- Security, performance, and quality analysis
- Read-only sandbox for safety

**How to use in any repository:**

Create `.github/workflows/codex-code-review.yml` in your repo:

```yaml
name: Codex Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write

jobs:
  review:
    uses: kaida-darice-de/.github/.github/workflows/codex-code-review-reusable.yml@main
    with:
      review_depth: 'standard'  # Options: quick, standard, thorough
    secrets:
      OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Customization options:**

```yaml
with:
  review_depth: 'thorough'
  files_pattern: '**/*.py **/*.js'  # Only review Python and JavaScript
  files_ignore: '**/*.test.py'      # Ignore test files
```

---

### 2. Codex CI Autofix

Automatically fixes CI failures using OpenAI Codex.

**Location:** `.github/workflows/codex-ci-autofix-reusable.yml`

**Features:**
- Triggers on CI workflow failures
- Analyzes failure logs
- Creates minimal fix PR
- Supports build, test, lint, and type errors

**How to use in any repository:**

Create `.github/workflows/codex-ci-autofix.yml` in your repo:

```yaml
name: Codex CI Autofix

on:
  workflow_run:
    workflows: ["CI"]  # Replace with your CI workflow name
    types: [completed]

permissions:
  contents: write
  pull-requests: write
  actions: read

jobs:
  autofix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    uses: kaida-darice-de/.github/.github/workflows/codex-ci-autofix-reusable.yml@main
    with:
      failed_workflow_id: ${{ github.event.workflow_run.id }}
      failed_workflow_name: ${{ github.event.workflow_run.name }}
      head_branch: ${{ github.event.workflow_run.head_branch }}
      head_sha: ${{ github.event.workflow_run.head_sha }}
    secrets:
      OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 📋 Setup Requirements

### Organization Secret

These workflows require an organization-level secret:

1. Go to: https://github.com/organizations/kaida-darice-de/settings/secrets/actions
2. Add secret: `OPENAI_API_KEY`
3. Repository access: **All repositories**

### Repository Permissions

Ensure repositories have proper workflow permissions:

1. Settings → Actions → General
2. Workflow permissions: **Read and write permissions**
3. Enable: **Allow GitHub Actions to create and approve pull requests**

---

## 💰 Cost Estimation

### Codex Code Review
- Small PR (1-3 files): $0.005-0.015
- Medium PR (5-10 files): $0.02-0.08
- Large PR (20+ files): $0.08-0.20

### Codex CI Autofix
- Small fix: $0.02-0.05
- Medium fix: $0.05-0.15
- Complex fix: $0.15-0.50

### Organization-wide (89 repos)
- Conservative (500 PRs/month): $25-75/month
- Moderate (1000 PRs/month): $50-150/month
- Heavy (2000 PRs/month): $100-300/month

---

## 🎨 Customization Examples

### Different Review Depths for Different Repos

**Critical production repo:**
```yaml
with:
  review_depth: 'thorough'
```

**Internal tool:**
```yaml
with:
  review_depth: 'quick'
```

### Language-Specific Reviews

**Python-only repo:**
```yaml
with:
  files_pattern: '**/*.py'
  files_ignore: '**/*.pyc **/__pycache__/**'
```

**Frontend repo:**
```yaml
with:
  files_pattern: '**/*.ts **/*.tsx **/*.js **/*.jsx'
  files_ignore: '**/node_modules/** **/*.test.ts'
```

### Selective Branch Reviews

**Only review PRs to main:**
```yaml
on:
  pull_request:
    branches:
      - main
```

---

## ✅ Quick Reference

### Enable Codex in 3 Steps

1. **Copy this to `.github/workflows/codex.yml` in your repo:**
   ```yaml
   name: Codex
   on: [pull_request]
   jobs:
     review:
       uses: kaida-darice-de/.github/.github/workflows/codex-code-review-reusable.yml@main
       secrets: inherit
   ```

2. **Commit and push**

3. **Done!** Your repo now has AI code review

---

## 📚 Resources

- [GitHub Reusable Workflows Documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [OpenAI Codex Documentation](https://developers.openai.com/codex)
- [openai/codex-action](https://github.com/openai/codex-action)
- [Organization Deployment Guide](https://github.com/kaida-darice-de/allen-the-law-advisor/blob/main/.github/org-reusable-workflows/ORG_DEPLOYMENT_GUIDE.md)

---

🤖 Powered by [OpenAI Codex](https://openai.com/index/codex/) | Maintained by kaida-darice-de organization
