# Security Considerations

This document outlines security best practices and considerations for the Tech Post Auto-Publisher system.

## API Keys and Secrets

### Storage
✅ **DO:**
- Store API keys in GitHub Secrets
- Use environment variables in workflows
- Rotate keys regularly

❌ **DON'T:**
- Commit API keys to the repository
- Share keys in issue descriptions
- Log keys in workflow outputs

### Access Control

**OpenAI API Key:**
- Grants access to GPT-4 API
- Can incur costs based on usage
- Set spending limits in OpenAI dashboard
- Monitor usage regularly
- **Protected by authorized users list** - Only authorized users can trigger API calls

**Medium API Token:**
- Has same permissions as your Medium account
- Can publish posts on your behalf
- Can be revoked at any time
- Create a dedicated token for this use

## Authorization and Token Protection

### Preventing Unauthorized API Usage

**By default, only the repository owner can trigger AI post generation.**

This critical security feature prevents:
- ❌ Unauthorized users from wasting your OpenAI tokens
- ❌ Random issue creators from running up API costs
- ❌ Spam or malicious actors from depleting your quota

**How it works:**
1. When an issue is created, the workflow checks the author's username
2. Compares against authorized users list (defaults to repository owner)
3. **Authorized users**: Issue saved, AI post generated, files committed
4. **Unauthorized users**: Nothing saved, nothing committed, workflow exits cleanly

**Configuration:**
```yaml
# Default: Only repository owner
AUTHORIZED_USERS="${REPO_OWNER}"

# Custom: Add specific users via GitHub Secret
AUTHORIZED_USERS="imaging8896,trusted-colleague,team-member"
```

**Cost Protection:**
- Each AI generation costs ~$0.05-$0.15
- Without authorization, any issue = wasted tokens
- With authorization, only trusted users can trigger AI calls

**To set up:**
1. Go to Settings → Secrets and variables → Actions
2. Add secret: `AUTHORIZED_USERS`
3. Value: Comma-separated usernames
4. If not set, only repository owner is authorized ✅

**Monitoring:**
- Check workflow logs to see authorization decisions
- Look for "✓ Author is authorized" or "✗ Author is NOT authorized"
- Review OpenAI usage dashboard for unexpected spikes

## GitHub Actions Permissions

### Workflow Permissions

The workflows require specific permissions:

```yaml
permissions:
  contents: write  # To commit generated posts
  issues: read     # To read issue content
```

**Why these permissions:**
- `contents: write` - Needed to save issues and posts to the repository
- `issues: read` - Needed to access issue details when they're created

### Repository Settings

Recommended security settings:

1. **Branch Protection:**
   - **CRITICAL:** Configure branch protection for main branch
   - See [Branch Protection Guide](BRANCH_PROTECTION.md) for detailed setup
   - **Recommended:** Restrict pushes to `github-actions[bot]` only
   - Prevents unauthorized direct commits to main
   - Ensures only the workflow can commit when issues are created

2. **Action Permissions:**
   - Set to "Read and write permissions"
   - Only enable for this repository
   - Review workflow runs regularly

3. **Secret Access:**
   - Secrets are only accessible to workflows in the repository
   - Not visible in logs or outputs
   - Audit secret access in Settings → Security

## Code Review

### Workflow Changes

Before modifying workflows:
- Review changes carefully
- Understand permission requirements
- Test in a fork first
- Never add code that logs secrets

### Third-Party Actions

Only use verified actions:
- `actions/checkout@v4` ✓ (Official GitHub action)
- `actions/setup-python@v5` ✓ (Official GitHub action)

If adding new actions:
- Check action source code
- Verify publisher reputation
- Pin to specific versions (not `@main`)
- Review permissions required

## Data Privacy

### Issue Content

**What's Stored:**
- Issue title and description
- Author username
- Creation timestamp
- Generated blog posts

**Where:**
- In your GitHub repository (public or private based on repo settings)
- Sent to OpenAI API for processing
- Published to Medium (public)

**Privacy Considerations:**
- Don't include sensitive information in issues
- Don't include personal identifiable information (PII)
- Don't include company secrets or proprietary code
- Review generated posts before publishing

### API Data Handling

**OpenAI:**
- Issue content is sent to OpenAI API
- Used only for generating blog posts
- Follow OpenAI's data usage policy
- Data retention per OpenAI's terms

**Medium:**
- Generated posts are published publicly
- Associated with your Medium account
- Subject to Medium's terms of service

## Rate Limiting and Abuse Prevention

### OpenAI Rate Limits

Set up safeguards:
```python
# Consider adding rate limiting in workflow
# Process max N issues per run
pending_issues = pending_issues[:5]  # Process max 5 at a time
```

### Cost Controls

Monitor and limit costs:
1. Set spending limits in OpenAI dashboard
2. Use GPT-3.5-turbo instead of GPT-4 for lower costs
3. Limit max_tokens in API calls
4. Process issues in batches

### GitHub Actions Minutes

Free tier limits:
- 2,000 minutes/month for private repos
- Unlimited for public repos

Optimize usage:
- Run daily instead of hourly
- Skip processing if no pending issues
- Combine steps where possible

## Incident Response

### If API Key is Compromised

**OpenAI Key:**
1. Go to https://platform.openai.com/api-keys
2. Revoke the compromised key
3. Create a new key
4. Update GitHub Secret
5. Check OpenAI usage logs for unauthorized activity
6. Contact OpenAI support if needed

**Medium Token:**
1. Go to https://medium.com/me/settings/security
2. Revoke the compromised token
3. Create a new integration token
4. Update GitHub Secret
5. Check Medium posts for unauthorized publications

**GitHub Secrets:**
1. Rotate the compromised secret
2. Review workflow run history
3. Check repository access logs
4. Update security settings if needed

## Monitoring

### Regular Checks

Weekly:
- Review workflow run logs
- Check OpenAI usage and costs
- Verify Medium publications
- Review repository access

Monthly:
- Rotate API keys (recommended)
- Audit workflow permissions
- Review and update dependencies
- Check for GitHub Actions updates

### Alert Setup

Consider setting up:
- OpenAI spending alerts
- GitHub Actions failure notifications
- Medium email notifications for new posts

## Compliance

### Open Source

If repository is public:
- Workflows are visible to everyone
- Anyone can see the workflow logic
- Secrets are hidden but workflow code is public
- Consider this when adding custom logic

### Data Regulations

If handling user data:
- Ensure compliance with GDPR, CCPA, etc.
- Document data processing in privacy policy
- Provide data deletion procedures
- Consider data residency requirements

## Security Checklist

Before going live:

- [ ] API keys stored in GitHub Secrets (not in code)
- [ ] OpenAI spending limit set
- [ ] Workflow permissions are minimal required
- [ ] Branch protection rules configured
- [ ] Repository access restricted appropriately
- [ ] .gitignore excludes sensitive files
- [ ] Workflows don't log secrets
- [ ] Third-party actions are verified
- [ ] Rate limiting implemented
- [ ] Monitoring/alerting configured
- [ ] Incident response plan documented
- [ ] Team trained on security practices

## Reporting Security Issues

If you discover a security vulnerability:

1. **DO NOT** create a public issue
2. Email the repository owner directly
3. Include details about the vulnerability
4. Allow time for patch before disclosure
5. Follow responsible disclosure practices

## References

- [GitHub Actions Security Best Practices](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [OpenAI API Safety Best Practices](https://platform.openai.com/docs/guides/safety-best-practices)
- [Medium API Terms of Service](https://github.com/Medium/medium-api-docs)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)

## Updates

This security documentation should be reviewed and updated:
- When adding new features
- When security best practices change
- After security incidents
- At least quarterly

Last updated: 2024-01-31