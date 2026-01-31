# Tech Post Auto-Publisher

This repository automatically captures technical issues and publishes AI-generated tech posts to Medium.

## 📚 Documentation

- **[Quick Start Guide](QUICKSTART.md)** - Get up and running in 5 minutes
- **[Configuration Guide](CONFIGURATION.md)** - Detailed setup and customization
- **[Issue Templates](TEMPLATES.md)** - Examples and best practices for writing issues
- **[Security Guide](SECURITY.md)** - Security best practices and considerations

## Overview

This system enables you to:
1. **Automatically capture issues** - Create GitHub issues about problems you've solved or interesting findings, and they're automatically saved to the repository
2. **AI-generated tech posts** - Issues are transformed into comprehensive technical blog posts using OpenAI's GPT-4
3. **Daily Medium publishing** - Generated posts are automatically published to Medium each day
4. **Secure access control** - Only authorized accounts can push commits (configured via GitHub repository settings)

## How It Works

### 1. Issue Capture Workflow

When you create a new issue in this repository:
- The issue is automatically saved to the `issues/` directory as a markdown file
- The file includes metadata (issue number, title, author, timestamp)
- Status is set to "pending" for processing

### 2. Daily Publication Workflow

Every day at 9 AM UTC (configurable):
- The workflow scans for pending issues in the `issues/` directory
- Each pending issue is processed:
  - OpenAI GPT-4 generates a comprehensive technical blog post
  - The generated post is saved to the `posts/` directory
  - The issue status is updated to "generated"
- Generated posts are published to Medium via the Medium API
- Post status is updated to "published" with the Medium URL

## Setup Instructions

### Prerequisites

1. A GitHub account with this repository
2. An OpenAI API key (for GPT-4 access)
3. A Medium API token (for publishing posts)

### Configuration

1. **Add GitHub Secrets** - Go to your repository Settings → Secrets and variables → Actions, and add:
   - `OPENAI_API_KEY`: Your OpenAI API key
   - `MEDIUM_API_TOKEN`: Your Medium integration token

2. **Get OpenAI API Key**:
   - Visit https://platform.openai.com/api-keys
   - Create a new API key
   - Ensure you have GPT-4 API access

3. **Get Medium API Token**:
   - Visit https://medium.com/me/settings/security
   - Scroll to "Integration tokens"
   - Create a new integration token
   - Copy the token (you won't be able to see it again)

4. **Configure Repository Permissions**:
   - Go to Settings → Actions → General
   - Under "Workflow permissions", select "Read and write permissions"
   - Check "Allow GitHub Actions to create and approve pull requests"

### Repository Access Control

To ensure only your account can push commits:

1. Go to Settings → Branches
2. Add a branch protection rule for `main` (or your default branch):
   - Enable "Require a pull request before merging" (optional)
   - Enable "Restrict who can push to matching branches"
   - Add your username to the allowed list

Alternatively, manage collaborators in Settings → Collaborators to control who has write access.

## Usage

### Creating Issues

Simply create a new issue in this repository:

1. Click on the "Issues" tab
2. Click "New issue"
3. Enter a title and description of the technical problem/solution
4. Click "Submit new issue"

The workflow will automatically:
- Capture the issue to the repository
- Process it the next day at 9 AM UTC
- Generate a tech post
- Publish to Medium

### Manual Trigger

You can manually trigger the publication workflow:

1. Go to the "Actions" tab
2. Select "Generate and Publish Tech Posts"
3. Click "Run workflow"
4. Select the branch and click "Run workflow"

## Directory Structure

```
tech-post/
├── .github/
│   └── workflows/
│       ├── capture-issue.yml      # Captures issues automatically
│       └── publish-posts.yml      # Generates and publishes posts daily
├── issues/                         # Stored issues (auto-generated)
│   └── issue-{number}.md
├── posts/                          # Generated tech posts (auto-generated)
│   └── post-{number}-{date}.md
├── LICENSE
└── README.md
```

## Workflows

### Capture Issue (`capture-issue.yml`)

- **Trigger**: When an issue is opened or edited
- **Actions**:
  - Saves issue content to `issues/issue-{number}.md`
  - Commits and pushes to the repository
  - Sets status to "pending"

### Generate and Publish Tech Posts (`publish-posts.yml`)

- **Trigger**: Daily at 9 AM UTC (or manual)
- **Actions**:
  - Finds pending issues
  - Uses OpenAI GPT-4 to generate technical blog posts
  - Saves generated posts to `posts/` directory
  - Publishes posts to Medium
  - Updates issue and post statuses

## Customization

### Change Publication Schedule

Edit `.github/workflows/publish-posts.yml`:

```yaml
on:
  schedule:
    - cron: '0 9 * * *'  # Change this cron expression
```

Common cron patterns:
- `0 9 * * *` - Daily at 9 AM UTC
- `0 9 * * 1` - Every Monday at 9 AM UTC
- `0 9,21 * * *` - Twice daily at 9 AM and 9 PM UTC

### Customize AI Prompts

Edit the prompt in `.github/workflows/publish-posts.yml` in the "Generate tech posts" step to customize how the AI generates posts.

### Change Medium Tags

Edit the `tags` array in `.github/workflows/publish-posts.yml`:

```python
'tags': ['technology', 'programming', 'software-development']
```

## Troubleshooting

### Posts Not Publishing

1. Check that secrets are configured correctly in repository settings
2. Verify your OpenAI API key has GPT-4 access
3. Verify your Medium API token is valid
4. Check the Actions tab for workflow execution logs

### Permission Errors

1. Ensure workflow permissions are set to "Read and write"
2. Check that the repository has Actions enabled
3. Verify GitHub Actions bot has necessary permissions

### API Rate Limits

- OpenAI has rate limits - avoid processing too many issues at once
- Medium has publishing limits - the daily schedule helps stay within limits

## Security Notes

- Never commit API keys directly to the repository
- Always use GitHub Secrets for sensitive tokens
- Regularly rotate your API keys
- Monitor your OpenAI and Medium usage

## License

See LICENSE file for details.
