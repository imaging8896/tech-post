# Tech Post Auto-Publisher

This repository automatically captures technical issues and publishes AI-generated tech posts to LinkedIn.

## 📚 Documentation

- **[Quick Start Guide](QUICKSTART.md)** - Get up and running in 5 minutes
- **[Configuration Guide](CONFIGURATION.md)** - Detailed setup and customization
- **[Issue Templates](TEMPLATES.md)** - Examples and best practices for writing issues
- **[Security Guide](SECURITY.md)** - Security best practices and considerations
- **[Branch Protection Guide](BRANCH_PROTECTION.md)** - Ensure only workflows can commit to main

## Overview

This system enables you to:
1. **Automatically capture issues** - Create GitHub issues about problems you've solved or interesting findings, and they're automatically saved to the repository
2. **AI-generated tech posts** - Issues are transformed into comprehensive technical blog posts using OpenAI's GPT-4
3. **Daily LinkedIn publishing** - Generated posts are automatically published to LinkedIn each day
4. **Optional image support** - Attach images to your issues to include them in your LinkedIn posts
5. **Secure access control** - Only authorized accounts can push commits (configured via GitHub repository settings)
6. **Token-saving authorization** - AI generation only runs for authorized users to prevent wasting OpenAI tokens

## How It Works

### 1. Issue Capture and Immediate AI Post Generation

When you create a new issue in this repository:
- The issue is automatically saved to the `issues/` directory as a markdown file
- The file includes metadata (issue number, title, author, timestamp)
- **Authorization check**: The workflow verifies if the issue author is authorized
  - ✅ **Authorized users**: Issue is saved and AI-generated blog post is created immediately using OpenAI GPT-4
  - ❌ **Unauthorized users**: Issue is NOT saved to the repository - no files committed, no AI tokens consumed
- For authorized users, both the issue and generated post are committed to the repository together
- The issue status is updated to "generated"
- Generated posts are saved to the `posts/` directory

### 2. Daily LinkedIn Publication Workflow

Every day at 9 AM Taiwan Time (1 AM UTC, configurable):
- The workflow scans for generated posts in the `posts/` directory
- Generated posts are published to LinkedIn via the LinkedIn API
- Post status is updated to "published" with the LinkedIn URL

## Setup Instructions

### Prerequisites

1. A GitHub account with this repository
2. An OpenAI API key (for GPT-4 access)
3. A LinkedIn API token (for publishing posts)

### Configuration

1. **Add GitHub Secrets** - Go to your repository Settings → Secrets and variables → Actions, and add:
   - `OPENAI_API_KEY`: Your OpenAI API key
   - `LINKEDIN_ACCESS_TOKEN`: Your LinkedIn integration token
   - `AUTHORIZED_USERS` (Optional): Comma-separated list of GitHub usernames authorized for AI generation
   - `LINKEDIN_TOKEN_CREATED_AT` (Optional): Date when LinkedIn token was created (format: YYYY-MM-DD) for expiration monitoring
   - `DISCORD_WEBHOOK_URL` (Optional): Discord webhook URL for token expiration notifications

2. **Get OpenAI API Key**:
   - Visit https://platform.openai.com/api-keys
   - Create a new API key
   - Ensure you have GPT-4 API access

3. **Get LinkedIn API Token**:
   - Visit https://www.linkedin.com/developers/
   - Create an app or use existing app
   - Request access to "Share on LinkedIn" product
   - Generate OAuth 2.0 access token with **only** these scopes: `r_liteprofile`, `w_member_social`
     > ⚠️ Do **not** include the `openid` scope — it targets a different API endpoint (`/v2/userinfo`) that returns a 403 with a standard posting token
   - Copy the token

4. **Configure Repository Permissions**:
   - Go to Settings → Actions → General
   - Under "Workflow permissions", select "Read and write permissions"
   - Check "Allow GitHub Actions to create and approve pull requests"

### AI Token Protection - Authorized Users

**By default, only the repository owner can trigger AI post generation.**

This prevents waste of OpenAI API tokens from unauthorized issue creators.

**How it works:**
- When an issue is created, the workflow checks if the author is authorized
- ✅ **Authorized**: Issue saved and AI post generated immediately (costs ~$0.05-$0.15 in tokens)
- ❌ **Unauthorized**: Issue NOT saved to repository, no files committed, no tokens used

**To authorize additional users:**
1. Go to Settings → Secrets and variables → Actions
2. Add a new secret named `AUTHORIZED_USERS`
3. Set value to comma-separated usernames: `imaging8896,user2,user3`
4. Save the secret

**Example:**
```
AUTHORIZED_USERS=imaging8896,colleague1,colleague2
```

If you don't set `AUTHORIZED_USERS`, only the repository owner (`imaging8896`) can trigger AI generation.

### Repository Access Control and PR Approvals

**Important:** To ensure only the GitHub Actions workflow can push to the main branch AND all Pull Requests require your approval before merging, you must configure branch protection rules.

See **[Branch Protection Guide](BRANCH_PROTECTION.md)** for detailed instructions on:
- Restricting direct pushes to main
- Allowing only `github-actions[bot]` to commit
- **Requiring PR approval before merge**
- Setting up Code Owners for automatic review requests
- Preventing unauthorized changes

**Quick Setup for Workflow + PR Approval:**
1. Go to Settings → Branches → Add rule
2. Branch pattern: `main`
3. Check "Require a pull request before merging"
4. Set "Require approvals" to 1
5. Check "Restrict who can push to matching branches"
6. Add only: `github-actions[bot]`
7. Check "Do not allow bypassing the above settings"
8. Save changes

This ensures:
- ✅ Only the workflow can commit/push to main when issues are created
- ✅ All Pull Requests require your approval before merging
- ✅ Code review process enforced for all manual changes
- ❌ Regular users cannot push directly to main (including repository owner)
- 🔒 Maximum protection and quality control

**For additional access control options**, see the full [Branch Protection Guide](BRANCH_PROTECTION.md).

## Usage

### Creating Issues

Simply create a new issue in this repository:

1. Click on the "Issues" tab
2. Click "New issue"
3. Enter a title and description of the technical problem/solution
4. **Optional: Attach images** by dragging & dropping or pasting them into the issue
5. Click "Submit new issue"

The workflow will automatically:
- Capture the issue to the repository
- **Immediately generate an AI-powered tech post in Traditional Chinese** (專有名詞可使用英文)
- If the content exceeds LinkedIn's 3000 character limit, it will be split into multiple posts
- **Upload attached images to LinkedIn** (if any images are included in the issue)
- Commit both the issue and generated post(s)
- (Post will be published to LinkedIn the next day at 9 AM UTC)

### Adding Images to Posts (Optional)

You can include images in your LinkedIn posts:

1. **Drag & drop** images directly into the issue text area, or
2. **Paste** images from clipboard (Ctrl+V / Cmd+V)

GitHub will automatically create an image link (either Markdown or HTML format). When the post is published:
- Images are uploaded to LinkedIn via the Assets API
- **Images are displayed expanded (full-width)** in your LinkedIn post for maximum visual impact
- If image upload fails, images are automatically added as comments on the post as a fallback
- **It's optional**: Posts without images are published as text-only

### Manual Trigger

You can manually trigger the publication workflow:

1. Go to the "Actions" tab
2. Select "Publish Tech Posts to LinkedIn"
3. Click "Run workflow"
4. Select the branch and click "Run workflow"

## Directory Structure

```
tech-post/
├── .github/
│   └── workflows/
│       ├── capture-issue.yml        # Captures issues and generates AI posts immediately
│       ├── check-linkedin-token.yml # Checks LinkedIn token expiration daily
│       └── publish-posts.yml        # Publishes generated posts to LinkedIn daily
├── issues/                           # Stored issues (auto-generated)
│   └── issue-{number}.md
├── posts/                            # Generated tech posts (auto-generated, in Traditional Chinese)
│   └── post-{number}-{date}.md       # Single post, or post-{number}-{date}-part{n}.md for split posts
├── LICENSE
└── README.md
```

## Workflows

### Capture Issue and Generate Post (`capture-issue.yml`)

- **Trigger**: When an issue is opened or edited
- **Actions**:
  1. **Check author authorization** - Verifies if issue author is authorized
  2. **If authorized**: 
     - Saves issue content to `issues/issue-{number}.md`
     - Immediately generates AI blog post in **Traditional Chinese** using OpenAI GPT-4 (專有名詞可使用英文)
     - If content exceeds 3000 characters (LinkedIn limit), splits into multiple posts
     - Saves generated post(s) to `posts/post-{number}-{date}.md` or `posts/post-{number}-{date}-part{n}.md`
     - Updates issue status to "generated"
     - Commits and pushes all files
  3. **If unauthorized**:
     - Does NOT save issue to repository
     - Does NOT generate AI post (saves tokens!)
     - Does NOT commit anything
     - Workflow completes without creating files

**Authorization:** By default, only repository owner. Customize with `AUTHORIZED_USERS` secret.

### Publish Tech Posts to LinkedIn (`publish-posts.yml`)

- **Trigger**: Daily at 9 AM Taiwan Time / 1 AM UTC (or manual)
- **Actions**:
  - Finds generated posts (status: "generated")
  - Publishes posts to LinkedIn via API
  - Updates post status to "published"
  - Adds published URL to post metadata

### Check LinkedIn Token Expiration (`check-linkedin-token.yml`)

- **Trigger**: Daily at 8 AM UTC (or manual)
- **Purpose**: Monitors LinkedIn access token expiration and sends Discord notifications
- **Requirements**: 
  - `LINKEDIN_TOKEN_CREATED_AT` secret (format: YYYY-MM-DD)
  - `DISCORD_WEBHOOK_URL` secret
- **Behavior**:
  - Calculates days remaining until token expires (60-day lifetime)
  - Sends Discord notification when 10 days or less remain
  - Notification urgency levels: NOTICE → WARNING → CRITICAL → EXPIRED

## Customization

### Change Publication Schedule

Edit `.github/workflows/publish-posts.yml`:

```yaml
on:
  schedule:
    - cron: '0 1 * * *'  # Change this cron expression
```

Common cron patterns:
- `0 1 * * *` - Daily at 9 AM Taiwan Time (1 AM UTC) — current default
- `0 9 * * *` - Daily at 9 AM UTC
- `0 9 * * 1` - Every Monday at 9 AM UTC
- `0 9,21 * * *` - Twice daily at 9 AM and 9 PM UTC

## Troubleshooting

### Posts Not Publishing

1. Check that LINKEDIN_ACCESS_TOKEN secret is configured correctly in repository settings
2. Verify your LinkedIn API token is valid
3. Check the Actions tab for workflow execution logs
4. Ensure there are posts with status "generated" in the posts/ directory

### LinkedIn 403 Error (userinfo endpoint)

If you see `403 Forbidden` for `https://api.linkedin.com/v2/userinfo`, your token doesn't have the `openid` scope. The workflow uses `/v2/me` instead, which only requires `r_liteprofile`. Ensure your token was generated with at least the `r_liteprofile` and `w_member_social` scopes.

### Permission Errors

1. Ensure workflow permissions are set to "Read and write"
2. Check that the repository has Actions enabled
3. Verify GitHub Actions bot has necessary permissions

### API Rate Limits

- OpenAI has rate limits - avoid processing too many issues at once
- LinkedIn has publishing limits - the daily schedule helps stay within limits

## Security Notes

- Never commit API keys directly to the repository
- Always use GitHub Secrets for sensitive tokens
- Regularly rotate your API keys
- Monitor your OpenAI and LinkedIn usage

## License

See LICENSE file for details.
