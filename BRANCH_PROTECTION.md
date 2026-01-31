# Branch Protection and PR Approval Setup Guide

This guide explains how to configure branch protection to ensure that:
1. Only the GitHub Actions workflow can commit directly to the main branch
2. All Pull Requests require your approval before merging

## Overview

The `capture-issue.yml` workflow is configured to:
- Checkout the `main` branch explicitly
- Commit issues and generated posts to `main`
- Push changes to `main` using the GitHub Actions bot

To ensure **only this workflow** can push to main (and all PRs require approval), you need to configure branch protection rules.

## Required Branch Protection Settings

### Step 1: Access Branch Protection Settings

1. Go to your repository on GitHub: https://github.com/imaging8896/tech-post
2. Click **Settings** (top menu)
3. Click **Branches** (left sidebar under "Code and automation")
4. Under "Branch protection rules", click **Add rule** (or edit existing rule for `main`)

### Step 2: Configure Protection Rule for Main Branch

**Branch name pattern:** `main`

#### Required Settings for Workflow-Only Commits + PR Approval:

1. **☑ Require a pull request before merging**
   - This prevents direct pushes to main from users
   - Forces all changes (except workflow commits) to go through PRs
   
2. **☑ Require approvals**
   - Set to **1** (or more if you want additional reviewers)
   - This ensures YOU must approve all PRs before they can be merged
   
3. **☑ Dismiss stale pull request approvals when new commits are pushed**
   - Recommended for security
   - Forces re-approval if PR is updated after initial approval
   
4. **☑ Require review from Code Owners** (Optional but recommended)
   - Create a CODEOWNERS file to designate yourself as owner
   - Ensures only you can approve changes
   
5. **☑ Restrict who can dismiss pull request reviews** (Optional)
   - Prevents others from dismissing your reviews
   
6. **☑ Restrict who can push to matching branches**
   - Click "Restrict pushes that create matching branches"
   - Add **ONLY**: `github-actions[bot]`
   - This explicitly allows ONLY the GitHub Actions bot to push directly
   
7. **☑ Do not allow bypassing the above settings**
   - Ensures the rules apply to everyone (including you)
   - Maximum protection

### Step 3: (Optional) Create CODEOWNERS File

To ensure you're always designated as the required reviewer:

1. Create a file named `.github/CODEOWNERS` in your repository
2. Add the following content:
   ```
   # Repository owner must review all changes
   * @imaging8896
   ```
3. Commit this file to your repository
4. In branch protection, enable "Require review from Code Owners"

This ensures that PRs targeting main **must** have your review.

### Step 4: Configure Workflow Permissions

The workflow already has the correct permissions configured:

```yaml
permissions:
  contents: write  # Required to push commits
  issues: read     # Required to read issue content
```

The workflow uses `${{ secrets.GITHUB_TOKEN }}` which is automatically provided by GitHub Actions with the necessary permissions.

## Verification

### Verify Branch Protection is Working:

1. **Test Direct Push (should be rejected):**
   - Try to push directly to main: `git push origin main`
   - Expected result: ❌ **Push rejected** (even for repository owner)
   - Error message: "protected branch hook declined"

2. **Test Workflow (should succeed):**
   - Create a new issue in the repository
   - Check Actions tab for workflow run
   - Expected result: ✅ **Workflow successfully pushes to main**
   - Verify commits appear in main branch

3. **Test PR Approval Requirement:**
   - Create a test branch: `git checkout -b test-branch`
   - Make a small change (e.g., update README)
   - Push the branch: `git push origin test-branch`
   - Create a Pull Request on GitHub
   - Expected result: ⚠️ **Cannot merge without approval**
   - Approve the PR as repository owner
   - Expected result: ✅ **Can now merge after approval**

### Check Current Protection Status:

```bash
# Using GitHub CLI (if installed)
gh api repos/imaging8896/tech-post/branches/main/protection

# Or check in GitHub UI:
# Settings → Branches → Branch protection rules
```

## Pull Request Approval Requirements

### Why Require PR Approvals?

Requiring approval for all PRs ensures:
- ✅ **Code Review**: Every change is reviewed before merging
- ✅ **Quality Control**: Catch bugs and issues before they reach main
- ✅ **Audit Trail**: Clear record of who approved what changes
- ✅ **Intentional Changes**: Prevents accidental or hasty merges
- ✅ **Collaboration**: Even solo developers benefit from reviewing their own work

### Setting Up PR Approval Requirements

**For Solo Developer (imaging8896):**

1. Go to Settings → Branches → Edit rule for `main`
2. Under "Protect matching branches":
   - ☑ **Require a pull request before merging**
   - ☑ **Require approvals**: Set to **1**
3. Under "Require approval from specific reviewers":
   - Option A: Enable **"Require review from Code Owners"** (recommended)
     - Create `.github/CODEOWNERS` file with: `* @imaging8896`
   - Option B: Don't enable Code Owners, but you'll need to approve manually
4. ☑ **Dismiss stale pull request approvals when new commits are pushed**
   - This ensures re-approval if PR is updated after initial review

**Important for Solo Developer:**
- You'll need to approve your own PRs (if not bypassing)
- Or set "Allow specified actors to bypass pull request requirements" with your username
- Recommended: Do NOT bypass - forces you to review your own work!

### PR Approval Workflow

**When YOU Create a PR:**
1. Create feature branch: `git checkout -b feature/my-change`
2. Make changes and commit
3. Push branch: `git push origin feature/my-change`
4. Create PR on GitHub
5. Review your own changes carefully
6. If everything looks good, approve your own PR
7. Merge the PR

**When Others Create PRs (if collaborators):**
1. They create a PR
2. GitHub notifies you as reviewer
3. Review the changes
4. Approve or Request Changes
5. Once approved, they (or you) can merge

### Best Practices for PR Approvals

1. **Always Review Your Own PRs**
   - Even if you can bypass, review the diff on GitHub
   - Check for debugging code, console.logs, TODO comments
   - Verify documentation is updated

2. **Use PR Templates** (Optional)
   - Create `.github/pull_request_template.md`
   - Include checklist: [ ] Tests pass, [ ] Docs updated, etc.

3. **Require Status Checks** (Optional but recommended)
   - If you add CI/CD, require tests to pass before merge
   - Settings → Branches → Require status checks

4. **Use Meaningful PR Titles**
   - Clear description of what changed
   - Reference issue numbers if applicable

5. **Review the GitHub Diff**
   - Don't just click approve
   - Actually read through the changes
   - Look for security issues, API keys, etc.

## Configuration Options

### Option 1: Maximum Protection (Recommended - Workflow + PR Approval)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Require approvals: **1** (you must approve)
- ☑ Dismiss stale pull request approvals when new commits are pushed
- ☑ Restrict who can push to matching branches
  - Allowed: **ONLY** `github-actions[bot]`
- ☑ Do not allow bypassing the above settings

**Result:**
- ✅ Only the workflow can push directly to main
- ✅ All manual changes require a PR
- ✅ All PRs require YOUR approval before merging
- ✅ You cannot bypass your own approval requirement
- 🔒 Maximum security and control

**Use Case:** Solo developer who wants full control and approval over all changes

### Option 2: Strict Protection with Self-Bypass (Single User)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Require approvals: 1
- ☑ Allow specified actors to bypass pull request requirements
  - Add: `imaging8896` (repository owner)
- ☑ Restrict who can push to matching branches
  - Allowed: `github-actions[bot]`

**Result:**
- ✅ Only the workflow can push directly to main
- ✅ You can merge your own PRs without approval (bypass)
- ⚠️ Less strict than Option 1
- Workflow pushes directly for issue-based commits

**Use Case:** Solo developer who wants flexibility to merge own PRs quickly

### Option 3: Relaxed Protection (For Teams)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Require approvals: 1 (or more for multiple reviewers)
- ☑ Allow specified actors to bypass pull request requirements
  - Add specific trusted users
- ☑ Restrict who can push to matching branches
  - Allowed: `github-actions[bot]`, trusted collaborators

**Result:**
- Workflow and trusted users can push to main
- PRs require approval from designated reviewers
- Team members can approve each other's PRs

**Use Case:** Team repository with multiple developers

### Option 4: Workflow-Only (Most Restrictive for Direct Pushes)

**Settings:**
- ☑ Require a pull request before merging
- ☑ Require approvals: 1+
- ☑ Restrict who can push to matching branches
  - Allowed: `github-actions[bot]` ONLY
- ☑ Do not allow bypassing the above settings
- ☑ Require review from Code Owners

**Result:**
- **ONLY** the workflow can push to main
- All users (including owner) must use PRs
- All PRs require Code Owner approval (you)
- Maximum protection for all changes

**Use Case:** High-security repositories where every change needs review

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

**Problem: PRs can be merged without approval**

Solutions:
1. Ensure "Require approvals" is set to 1 or more
2. Check "Require review from Code Owners" is enabled
3. Verify `.github/CODEOWNERS` file exists and contains your username
4. Ensure you don't have "Allow specified actors to bypass" enabled for yourself

**Problem: I can't approve my own PRs**

Solutions:
1. This is by design for "Require review from Code Owners"
2. Either:
   - Add another collaborator who can approve
   - Or enable "Allow specified actors to bypass pull request requirements" for yourself
   - Or use Option 2 (Self-Bypass) configuration

## Security Recommendations

1. **Use Option 1 (Maximum Protection)** for best security with PR approvals
2. **Require PR approval for ALL changes** - even your own
3. **Enable Code Owners** to ensure you're always the designated reviewer
4. **Enable audit logging** to track all pushes and approvals
5. **Review workflow runs** regularly in the Actions tab
6. **Rotate GitHub tokens** if you suspect unauthorized access
7. **Use signed commits** for additional verification (optional)
8. **Enable "Dismiss stale approvals"** to force re-review after updates

## Additional Resources

- [GitHub Docs: Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Actions: Permissions](https://docs.github.com/en/actions/security-guides/automatic-token-authentication)
- [Repository Settings](https://github.com/imaging8896/tech-post/settings)

## Summary

To ensure **only the workflow can push directly to main AND all PRs require your approval**:

### Workflow Configuration (Already Done ✅)
1. ✅ Workflow explicitly targets `main` branch
2. ✅ Workflow has `contents: write` permission
3. ✅ Workflow pushes as `github-actions[bot]`

### Your Required Configuration (In GitHub Settings ⚠️)

**For Maximum Security (Recommended):**
1. Go to Settings → Branches → Add/Edit rule for `main`
2. Branch pattern: `main`
3. Enable these settings:
   - ☑ **Require a pull request before merging**
   - ☑ **Require approvals: 1**
   - ☑ **Dismiss stale pull request approvals when new commits are pushed**
   - ☑ **Restrict who can push to matching branches**
     - Add ONLY: `github-actions[bot]`
   - ☑ **Do not allow bypassing the above settings**
4. (Optional) Create `.github/CODEOWNERS` file:
   ```
   * @imaging8896
   ```
5. (If using CODEOWNERS) Enable:
   - ☑ **Require review from Code Owners**

**Result:**
- ✅ Only workflow can push to main directly
- ✅ All PRs require your approval before merge
- ✅ You must review even your own PRs
- 🔒 Maximum protection achieved

**Quick Setup (1 minute):**
1. Go to Settings → Branches → Add rule
2. Branch pattern: `main`
3. Check "Require a pull request before merging"
4. Set "Require approvals" to 1
5. Check "Restrict who can push to matching branches"
6. Add: `github-actions[bot]`
7. Check "Do not allow bypassing the above settings"
8. Click "Create" or "Save changes"

✅ **Done!** Only the workflow can push to main, and all PRs need your approval!
