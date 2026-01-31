# Issues Directory

This directory automatically stores issues captured from GitHub.

## How It Works

When you create a new issue in this repository:
1. The "Capture Issue" GitHub Actions workflow triggers
2. The issue content is saved here as `issue-{number}.md`
3. The file includes metadata and the full issue content

## File Format

Each file follows this structure:

```markdown
---
issue_number: 123
title: "Issue Title"
author: username
created_at: 2024-01-31T10:00:00Z
status: pending
---

# Issue Title

**Issue #123**
**Author:** username
**Created:** 2024-01-31T10:00:00Z

## Description

[Issue content here]
```

## Status Values

- `pending` - Issue has been captured, waiting to be processed
- `generated` - AI has generated a blog post from this issue
- `published` - Blog post has been published to Medium

## Manual Management

You generally don't need to modify these files manually. They're automatically:
- Created when issues are opened/edited
- Updated when posts are generated
- Updated when posts are published

However, you can:
- Delete files to prevent processing
- Edit content before processing
- Change status to skip or retry processing

## Notes

- Files are named `issue-{number}.md` where `{number}` is the GitHub issue number
- The same issue file is updated if the GitHub issue is edited
- This directory is committed to git for persistence