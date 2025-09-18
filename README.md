# Gemini AI PR Review Action

A reusable GitHub Action that provides automated code reviews using Google's Gemini AI. This action analyzes pull request changes and provides detailed feedback on code quality, security, performance, and best practices.

---

## Quick Start

### 1. Add Workflow File

Create `.github/workflows/gemini-pr-review.yml` in your repository:

```yaml
name: Gemini AI PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write

jobs:
  gemini-review:
    runs-on: ubuntu-latest
    name: Gemini AI Code Review
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0   # IMPORTANT: Needed for git diff between base and head

      - name: Run Gemini PR Review
        uses: consulting-brendan/peer-review@v1.0.4
        with:
          gemini-api-key: ${{ secrets.GEMINI_API_KEY }}
          base-sha: ${{ github.event.pull_request.base.sha }}
          head-sha: ${{ github.event.pull_request.head.sha }}
          pr-title: ${{ github.event.pull_request.title }}
          pr-body: ${{ github.event.pull_request.body }}
          review-prompt: |
            You are an expert code reviewer specializing in TypeScript, React, Next.js, and Convex database applications.
            Please review the following pull request changes for the Responsely.ai customer support platform.

            **Focus areas:**
            - TypeScript best practices
            - React component patterns and performance
            - Next.js App Router implementation
            - Convex database queries and schema
            - API security and validation
            - Accessibility and performance for customer UI

            **Pull Request Details:**
            - Title: {{ pr_title }}
            - Description: {{ pr_body }}
            - Changed Files: {{ changed_files }}

            **Code Changes:**
            ```diff
            {{ diff_content }}
            ```
