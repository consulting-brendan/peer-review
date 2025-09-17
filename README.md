# Gemini AI PR Review Action

A reusable GitHub Action that provides automated code reviews using Google's Gemini AI. This action analyzes pull request changes and provides detailed feedback on code quality, security, performance, and best practices.

## Features

- 🤖 **AI-Powered Reviews**: Uses Gemini 1.5 Flash for intelligent code analysis
- 🔒 **Security Scanning**: Integrated Semgrep security analysis
- 📝 **Detailed Feedback**: Structured reviews covering multiple aspects
- 🏷️ **Auto-Labeling**: Automatically labels PRs with review status
- ⚙️ **Customizable**: Configurable prompts and diff limits
- 🔄 **Reusable**: Can be used across multiple repositories

## Quick Start

### 1. Add to Your Repository

Create `.github/workflows/gemini-pr-review.yml` in your repository:

```yaml
name: Gemini AI PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write
  issues: write

jobs:
  gemini-review:
    runs-on: ubuntu-latest
    name: Gemini AI Code Review
    steps:
      - name: Run Gemini PR Review Action
        uses: consulting-brendan/peer-review@v1.0.0
        with:
          gemini-api-key: ${{ secrets.GEMINI_API_KEY }}
```

### 2. Configure Secrets

Add your Gemini API key as a repository secret:

1. Go to your repository Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `GEMINI_API_KEY`
4. Value: Your Gemini API key from [Google AI Studio](https://aistudio.google.com/app/apikey)

### 3. Create a Pull Request

The action will automatically trigger on new PRs and provide:
- Detailed code review comments
- Security analysis via Semgrep
- Automatic labeling (`ai-reviewed`, `security-review-needed`)

## Configuration Options

### Inputs

| Input | Description | Default | Required |
|-------|-------------|---------|----------|
| `gemini-api-key` | Gemini API key for authentication | - | ✅ |
| `review-diff-limit` | Max characters for diff content | `8000` | ❌ |
| `review-prompt` | Custom prompt for the AI reviewer | [Default prompt] | ❌ |

### Example with Custom Configuration

```yaml
- name: Run Gemini PR Review Action
  uses: consulting-brendan/peer-review@v1.0.0
  with:
    gemini-api-key: ${{ secrets.GEMINI_API_KEY }}
    review-diff-limit: '10000'
    review-prompt: |
      You are a senior developer reviewing this pull request.
      Focus on:
      1. Code quality and maintainability
      2. Security vulnerabilities
      3. Performance implications
      4. Testing coverage
      
      {{ pr_title }}
      {{ pr_body }}
      {{ changed_files }}
      {{ diff_content }}
```

## Review Format

The AI review includes:

1. **Code Quality Assessment** - Overall quality rating and explanation
2. **Security Analysis** - Potential vulnerabilities and concerns  
3. **Performance Review** - Performance implications of changes
4. **Best Practices** - Adherence to coding standards
5. **Suggestions** - Specific, actionable improvements
6. **Resources** - Relevant documentation and learning materials

## Security Features

- **Semgrep Integration**: Runs static analysis for common vulnerabilities
- **Automatic Labeling**: Flags PRs needing security review
- **Safe API Usage**: API keys are securely handled via GitHub secrets
- **Permission Scoping**: Minimal required permissions

## Branch Protection

To require AI review before merging:

1. Go to Settings → Branches
2. Add/edit branch protection rule for your main branch
3. Enable "Require status checks to pass before merging"
4. Select: "AI Review Status Check" and "semgrep"

## Supported Languages

Optimized for:
- TypeScript/JavaScript
- React/Next.js
- Python
- Go
- And more...

The AI adapts its review style based on the detected languages in your PR.

## Limitations

- **Diff Size**: Large diffs are truncated (configurable via `review-diff-limit`)
- **API Limits**: Subject to Gemini API rate limits and quotas
- **Binary Files**: Only reviews text-based files
- **Context**: Reviews individual PRs, not entire codebase context

## Contributing

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Test with a sample repository
5. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) for details.

## Support

For issues and questions:
- 🐛 [Create an issue](https://github.com/consulting-brendan/peer-review/issues)
- 📧 Email: [your-email@example.com]
- 💬 [Discussions](https://github.com/consulting-brendan/peer-review/discussions)

---

Built with ❤️ for better code reviews