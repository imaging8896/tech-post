# Configuration Guide

## Required GitHub Secrets

Add these secrets to your repository (Settings → Secrets and variables → Actions):

### OPENAI_API_KEY
Your OpenAI API key for GPT-4 access.

**How to get it:**
1. Go to https://platform.openai.com/api-keys
2. Sign in to your OpenAI account
3. Click "Create new secret key"
4. Copy the key (it starts with 'sk-')
5. Add it as a GitHub secret

**Important:**
- Ensure you have GPT-4 API access enabled
- Monitor your usage to avoid unexpected charges
- Set spending limits in your OpenAI dashboard

### LINKEDIN_ACCESS_TOKEN
Your LinkedIn access token for publishing posts.

**How to get it:**
1. Go to https://www.linkedin.com/developers/
2. Create an app or use existing app
3. Request access to "Share on LinkedIn" product
4. Configure OAuth 2.0 settings
5. Generate access token with required scopes (r_liteprofile, w_member_social)
6. Copy the token
7. Add it as a GitHub secret

**Important:**
- The token has the same permissions as your LinkedIn account
- Keep it secure and never share it
- Tokens typically expire after 60 days and need to be refreshed

## Workflow Permissions

Ensure GitHub Actions has the correct permissions:

1. Go to Settings → Actions → General
2. Under "Workflow permissions":
   - Select "Read and write permissions"
   - Check "Allow GitHub Actions to create and approve pull requests"
3. Click "Save"

## Testing Your Setup

### Test Issue Capture

1. Create a test issue in your repository
2. Go to Actions tab
3. Check the "Capture Issue" workflow ran successfully
4. Verify the issue file was created in the `issues/` directory

### Test Post Generation (Manual)

1. Go to Actions tab
2. Select "Publish Tech Posts to LinkedIn"
3. Click "Run workflow"
4. Wait for completion
5. Check your LinkedIn account for the published post

Note: This workflow only publishes already-generated posts. To generate posts, create an issue first (AI post generation happens immediately when issues are created by authorized users).

## Customization Options

### Publication Schedule
Default: Daily at 9 AM UTC

To change, edit the cron expression in `.github/workflows/publish-posts.yml`:
```yaml
schedule:
  - cron: '0 9 * * *'
```

### LinkedIn Post Settings

In `.github/workflows/publish-posts.yml`, you can customize:

```python
post_data = {
    'title': title,
    'contentFormat': 'markdown',
    'content': post_content,
    'publishStatus': 'public',  # or 'draft' or 'unlisted'
    'tags': ['technology', 'programming', 'software-development']  # customize tags
}
```

### AI Prompt Customization

Edit the prompt in the "Generate tech posts" step to control how the AI writes:
- Add specific formatting requirements
- Request certain sections or structure
- Specify tone or style
- Include examples or templates

## Security Best Practices

1. **Never commit secrets** - Always use GitHub Secrets
2. **Rotate tokens regularly** - Update API keys periodically
3. **Monitor usage** - Check OpenAI and LinkedIn dashboards for unusual activity
4. **Limit repository access** - Only grant push access to trusted users
5. **Review generated content** - Consider using 'draft' mode initially to review before publishing

## Cost Considerations

### OpenAI Costs
- GPT-4 pricing: ~$0.03 per 1K input tokens, ~$0.06 per 1K output tokens
- Average post generation: ~500-2000 tokens total
- Estimated cost per post: $0.03 - $0.15
- Daily cost (1 post/day): ~$1-5/month

**Tips to reduce costs:**
- Use GPT-3.5-turbo instead of GPT-4 (much cheaper)
- Limit the number of issues processed per run
- Reduce max_tokens in the API call

### LinkedIn
- LinkedIn publishing is free
- No API usage fees
- Consider LinkedIn Partner Program for monetization

## Troubleshooting

### Issue: Workflow doesn't run
- Check that Actions are enabled in repository settings
- Verify workflow YAML syntax is correct
- Check for any GitHub service disruptions

### Issue: OpenAI errors
- Verify API key is correct
- Check you have GPT-4 access
- Ensure you have available credits
- Check rate limits haven't been exceeded

### Issue: LinkedIn publishing fails
- Verify token is valid
- Check token hasn't been revoked
- Ensure token has write permissions
- Verify LinkedIn account is in good standing

### Issue: Permission denied
- Check workflow permissions are "Read and write"
- Verify GitHub Actions has necessary repository access
- Check branch protection rules

## Support

For issues or questions:
1. Check the Actions logs for detailed error messages
2. Review this configuration guide
3. Check OpenAI and LinkedIn API documentation
4. Create an issue in this repository for help