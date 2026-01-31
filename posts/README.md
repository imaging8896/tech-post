# Posts Directory

This directory stores AI-generated blog posts ready for or already published to Medium.

## How It Works

1. The daily "Generate and Publish Tech Posts" workflow runs
2. Finds pending issues in the `issues/` directory
3. Uses OpenAI GPT-4 to generate blog posts
4. Saves generated posts here as `post-{issue-number}-{date}.md`
5. Publishes posts to Medium
6. Updates post files with publication status

## File Format

Each file follows this structure:

```markdown
---
issue_number: 123
original_title: Original Issue Title
generated_at: 2024-01-31T10:00:00Z
status: generated
---

# Generated Blog Post Title

[AI-generated content here in Markdown format]

## Introduction
...

## Main Content
...

## Conclusion
...
```

After publication, the metadata is updated:

```markdown
---
issue_number: 123
original_title: Original Issue Title
generated_at: 2024-01-31T10:00:00Z
status: published
published_url: https://medium.com/@username/post-slug-12345
---
```

## Status Values

- `generated` - Post has been created by AI, ready to publish
- `published` - Post has been published to Medium

## Manual Operations

### Review Before Publishing

You can review and edit generated posts before they're published:

1. Wait for the workflow to generate posts (or run it manually)
2. Check this directory for new `post-*.md` files
3. Review and edit the content if needed
4. Keep status as `generated` for auto-publishing
5. Change status to `published` to skip (won't publish to Medium)

### Manually Trigger Publishing

1. Go to Actions tab in GitHub
2. Select "Generate and Publish Tech Posts"
3. Click "Run workflow"

### Republish or Skip

- To skip publishing: Change status from `generated` to `published`
- To retry publishing: Change status from `published` to `generated`

## File Naming

- Format: `post-{issue-number}-{YYYYMMDD}.md`
- Example: `post-42-20240131.md`
- Issue number links back to source issue
- Date shows when post was generated

## Notes

- Multiple posts can be generated from the same issue (on different days)
- Edit posts before publishing by modifying the Markdown
- Published posts include the Medium URL in metadata
- This directory is committed to git for record-keeping