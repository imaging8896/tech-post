# Quick Start Guide

Get your automated tech blog up and running in 5 minutes!

## ⚡ Quick Setup

### Step 1: Get Your API Keys (5 minutes)

#### OpenAI API Key
1. Go to https://platform.openai.com/api-keys
2. Sign in or create an account
3. Click **"Create new secret key"**
4. Copy the key (starts with `sk-`)
5. Save it somewhere safe

#### Medium API Token
1. Go to https://medium.com/me/settings/security
2. Scroll to **"Integration tokens"**
3. Enter description: `Tech Post Publisher`
4. Click **"Get integration token"**
5. **Copy immediately** (you won't see it again!)

### Step 2: Add Secrets to GitHub (1 minute)

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **"New repository secret"**
4. Add first secret:
   - Name: `OPENAI_API_KEY`
   - Value: [paste your OpenAI key]
   - Click **"Add secret"**
5. Add second secret:
   - Name: `MEDIUM_API_TOKEN`
   - Value: [paste your Medium token]
   - Click **"Add secret"**

### Step 3: Enable Workflow Permissions (30 seconds)

1. Stay in **Settings** → **Actions** → **General**
2. Scroll to **"Workflow permissions"**
3. Select **"Read and write permissions"** ✓
4. Check **"Allow GitHub Actions to create and approve pull requests"** ✓
5. Click **"Save"**

### Step 4: Create Your First Issue (1 minute)

1. Go to **Issues** tab in your repository
2. Click **"New issue"**
3. Title: `Testing Automated Tech Post Publishing`
4. Description:
   ```markdown
   ## Testing the System
   
   This is my first automated tech post. I'm testing the GitHub Actions workflow
   that captures issues and transforms them into Medium blog posts using AI.
   
   **What this system does:**
   - Automatically saves issues to the repository
   - Uses GPT-4 to generate comprehensive blog posts
   - Publishes to Medium daily
   
   **How it works:**
   1. I create an issue (like this one)
   2. GitHub Actions captures it automatically
   3. Daily workflow processes pending issues
   4. AI generates a blog post
   5. Post is published to Medium
   
   Pretty cool automation! 🚀
   ```
5. Click **"Submit new issue"**

## ✅ Verify Setup

### Check Issue Capture and AI Post Generation (Immediate)

1. Go to **Actions** tab
2. Look for "Capture Issue and Generate Post" workflow
3. It should be running or completed ✓
4. Check workflow logs to see:
   - ✓ Issue saved
   - ✓ AI post generated with OpenAI
   - ✓ Both committed to repository
5. Go to your repository and check:
   - `issues/` directory has your issue file
   - `posts/` directory has the generated post

### Test Medium Publishing (Manual)

1. Go to **Actions** tab
2. Click **"Generate and Publish Tech Posts"** workflow
3. Click **"Run workflow"** button
4. Select your branch (usually `main`)
5. Click **"Run workflow"**
6. Wait 1-2 minutes for completion
7. Check workflow logs to see:
   - ✓ Post published to Medium

### Check Your Medium Account

1. Go to https://medium.com/me/stories
2. You should see your new published post! 🎉

## 🎯 Daily Usage

Now that you're set up, here's your workflow:

### When You Learn Something New

1. Open GitHub (on any device)
2. Create an issue in this repository
3. Write about what you learned
4. Submit the issue
5. Done! ✓

The system automatically:
- Saves your issue
- **Immediately generates an AI-powered blog post**
- Commits both to the repository
- Publishes to Medium daily at 9 AM UTC
- Updates you with the published URL

## 📱 Pro Tips

### Use GitHub Mobile App

Install the GitHub mobile app to create issues on the go:
- Learn something at work? Create an issue immediately
- Quick notes become full blog posts
- No need to write the full post yourself

### Issue Templates

Use the templates in `TEMPLATES.md` for better posts:
- Problem-Solution
- Tutorial/How-To
- Technology Comparison
- Architecture/Design

### Customize Your Schedule

Don't want daily posts? Edit `.github/workflows/publish-posts.yml`:

```yaml
schedule:
  - cron: '0 9 * * 1'  # Only Mondays
  # or
  - cron: '0 9 1 * *'  # First day of each month
```

### Manual Publishing

Need to publish immediately?
1. Go to **Actions** tab
2. Select **"Generate and Publish Tech Posts"**
3. Click **"Run workflow"**

## 🔧 Troubleshooting

### "Workflow didn't run"
- ✅ Check Actions are enabled in Settings
- ✅ Verify workflow permissions are set

### "OpenAI error"
- ✅ Check API key is correct in Secrets
- ✅ Verify you have GPT-4 access
- ✅ Check OpenAI account has credits

### "Medium publishing failed"
- ✅ Check token is correct in Secrets
- ✅ Verify token hasn't been revoked
- ✅ Check Medium account is active

### "Permission denied"
- ✅ Enable "Read and write permissions"
- ✅ Check GitHub Actions has access

## 📚 Next Steps

1. ✅ Read `README.md` for full documentation
2. ✅ Check `CONFIGURATION.md` for advanced settings
3. ✅ Review `TEMPLATES.md` for issue writing tips
4. ✅ Create your first real issue!

## 💰 Cost Estimate

**OpenAI GPT-4:**
- ~$0.05 - $0.15 per post
- Daily post = ~$1.50 - $4.50/month
- Use GPT-3.5-turbo for 90% cost reduction

**Medium:**
- Free! No API costs
- Consider Medium Partner Program for earnings

## 🎉 You're Ready!

Your automated tech blog is now running. Every issue you create will become a published Medium post. 

**Start creating issues and watch your blog grow automatically!** 🚀

---

### Questions?

Create an issue in this repository if you need help with setup!