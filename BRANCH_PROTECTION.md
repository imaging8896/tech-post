# Branch Protection Setup Guide

This guide explains how to configure branch protection to ensure that only the GitHub Actions workflow can commit and push to the main branch.

## Overview

The `capture-issue.yml` workflow is configured to:
- Checkout the `main` branch explicitly
- Commit issues and generated posts to `main`
- Push changes to `main` using the GitHub Actions bot

To ensure **only this workflow** can push to main (and prevent direct pushes from users), you need to configure branch protection rules.

## Required Branch Protection Settings

### Step 1: Access Branch Protection Settings

1. Go to your repository on GitHub: https://github.com/imaging8896/tech-post
2. Click **Settings** (top menu)
3. Click **Branches** (left sidebar under "Code and automation")
4. Under "Branch protection rules", click **Add rule** (or edit existing rule for `main`)

### Step 2: Configure Protection Rule for Main Branch

**Branch name pattern:** `main`

#### Required Settings for Workflow-Only Commits:

1. **☑ Require a pull request before merging**
   - This prevents direct pushes to main from users
   - **IMPORTANT:** Uncheck "Allow specified actors to bypass required pull requests"
   
2. **☑ Do not allow bypassing the above settings**
   - Ensures the rules apply to everyone
   
3. **☑ Allow force pushes** → Enable only for: "Specify who can force push"
   - Add: `github-actions[bot]` or your GitHub App
   - This allows the workflow to push
   
4. **Alternative Approach (Recommended):**
   - **☑ Restrict who can push to matching branches**
   - Click "Restrict pushes that create matching branches"
   - Add `github-actions[bot]` to the list
   - This explicitly allows ONLY the GitHub Actions bot to push

### Step 3: Configure Workflow Permissions

The workflow already has the correct permissions configured:

```yaml
permissions:
  contents: write  # Required to push commits
  issues: read     # Required to read issue content
```

The workflow uses `${{ secrets.GITHUB_TOKEN }}` which is automatically provided by GitHub Actions with the necessary permissions.

## Verification

### Verify Branch Protection is Working:

1. **Test as a User:**
   - Try to push directly to main: `git push origin main`
   - Expected result: ❌ **Push rejected** (even for repository owner)
   - Error message: "protected branch hook declined"

2. **Test Workflow:**
   - Create a new issue in the repository
   - Check Actions tab for workflow run
   - Expected result: ✅ **Workflow successfully pushes to main**
   - Verify commits appear in main branch

### Check Current Protection Status:

```bash
# Using GitHub CLI (if installed)
gh api repos/imaging8896/tech-post/branches/main/protection

# Or check in GitHub UI:
# Settings → Branches → Branch protection rules
```

## Configuration Options

### Option 1: Strict Protection (Recommended for Single User)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Require approvals: 0 (since it's just you)
- ☑ Restrict who can push to matching branches
  - Allowed: `github-actions[bot]`

**Result:**
- Only the workflow can push to main
- You must create PRs for any manual changes
- Workflow pushes directly for issue-based commits

### Option 2: Relaxed Protection (For Teams)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Allow specified actors to bypass pull request requirements
  - Add specific trusted users
- ☑ Restrict who can push to matching branches
  - Allowed: `github-actions[bot]`, `imaging8896`, other trusted users

**Result:**
- Workflow and trusted users can push to main
- Other users must create PRs

### Option 3: Workflow-Only (Most Restrictive)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Require approvals: 1
- ☑ Restrict who can push to matching branches
  - Allowed: `github-actions[bot]` ONLY
- ☑ Do not allow bypassing the above settings

**Result:**
- **ONLY** the workflow can push to main
- All users (including owner) must use PRs
- Maximum protection

## Important Notes

### GitHub Actions Bot Identity

The workflow pushes as `github-actions[bot]` with email `github-actions[bot]@users.noreply.github.com`.

This is configured in the workflow:
```yaml
git config user.name "github-actions[bot]"
git config user.email "github-actions[bot]@users.noreply.github.com"
```

### GITHUB_TOKEN Permissions

The `GITHUB_TOKEN` automatically provided to workflows has special privileges:
- Can bypass some branch protections when "Allow GitHub Actions to create and approve pull requests" is enabled
- Has write access when workflow has `contents: write` permission
- Automatically authenticates the workflow

### Troubleshooting

**Problem: Workflow can't push to main**

Solutions:
1. Check workflow permissions: `contents: write` is set
2. Check Settings → Actions → General → Workflow permissions:
   - Select "Read and write permissions" ✓
3. Ensure "Allow GitHub Actions to create and approve pull requests" is checked
4. Verify branch protection allows `github-actions[bot]`

**Problem: Users can still push to main**

Solutions:
1. Ensure "Restrict who can push to matching branches" is enabled
2. Remove users from the allowed list
3. Ensure "Do not allow bypassing the above settings" is checked
4. Check that rule is active (not in "Preview" mode)

## Security Recommendations

1. **Use Option 3 (Workflow-Only)** for maximum security
2. **Enable audit logging** to track all pushes to main
3. **Review workflow runs** regularly in the Actions tab
4. **Rotate GitHub tokens** if you suspect unauthorized access
5. **Use signed commits** for additional verification (optional)

## Additional Resources

- [GitHub Docs: Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Actions: Permissions](https://docs.github.com/en/actions/security-guides/automatic-token-authentication)
- [Repository Settings](https://github.com/imaging8896/tech-post/settings)

## Summary

To ensure **only the workflow can commit and push to main**:

1. ✅ Workflow explicitly targets `main` branch (configured)
2. ✅ Workflow has `contents: write` permission (configured)
3. ⚠️ **YOU MUST CONFIGURE:** Branch protection rules in GitHub Settings
4. ⚠️ **YOU MUST CONFIGURE:** Restrict pushes to `github-actions[bot]` only

**Quick Setup (30 seconds):**
1. Go to Settings → Branches → Add rule
2. Branch pattern: `main`
3. Check "Restrict who can push to matching branches"
4. Add: `github-actions[bot]`
5. Click "Create" or "Save changes"

✅ **Done!** Only the workflow can now push to main.
