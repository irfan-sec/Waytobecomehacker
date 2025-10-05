# GitHub Pages Setup Instructions

This repository is now configured to automatically publish content to GitHub Pages using GitHub Actions.

## 🚀 How to Enable GitHub Pages

To activate GitHub Pages for this repository, follow these steps:

### 1. Enable GitHub Actions Workflow Permissions

1. Go to your repository on GitHub: `https://github.com/irfan-sec/Waytobecomehacker`
2. Click on **Settings** (top menu)
3. In the left sidebar, click **Actions** → **General**
4. Scroll down to **Workflow permissions**
5. Select **"Read and write permissions"**
6. Check **"Allow GitHub Actions to create and approve pull requests"**
7. Click **Save**

### 2. Configure GitHub Pages Settings

1. Still in **Settings**, click **Pages** in the left sidebar
2. Under **Source**, select **"GitHub Actions"**
3. Click **Save**

### 3. Trigger the Deployment

The workflow will automatically run when you:
- Push to the `main` branch
- Manually trigger it from the **Actions** tab

To manually trigger the first deployment:
1. Go to the **Actions** tab
2. Click on **"Deploy Jekyll site to Pages"** workflow
3. Click **"Run workflow"** button
4. Select the `main` branch
5. Click **"Run workflow"**

### 4. Access Your Published Site

Once the workflow completes successfully (usually takes 1-2 minutes):
- Your site will be available at: **https://irfan-sec.github.io/Waytobecomehacker**

## 📋 What Gets Published

The workflow will automatically publish all content including:
- ✅ Homepage (`index.md`)
- ✅ Career path pages (PenetrationTester.md, SecurityAnalyst.md, etc.)
- ✅ Blog posts from `_posts/` directory
- ✅ Additional pages from `_pages/` directory (about, blog index)
- ✅ Web Hacking Tools documentation from `Web-Hacking-Tools/`
- ✅ Networking documentation from `Networking/`
- ✅ All other markdown content in the repository

## 🔄 Automatic Updates

After initial setup:
- Every push to the `main` branch will automatically rebuild and deploy the site
- Changes typically appear live within 1-2 minutes
- You can monitor deployments in the **Actions** tab

## 🎨 Customization

The site uses the **Minimal Mistakes** Jekyll theme. To customize:
- Edit `_config.yml` for site-wide settings
- Modify `index.md` for homepage content
- Add new blog posts in `_posts/` with format: `YYYY-MM-DD-title.md`
- Update pages in `_pages/` directory

## 🐛 Troubleshooting

If the workflow fails:
1. Check the **Actions** tab for error messages
2. Ensure all markdown files have valid front matter
3. Verify Gemfile and _config.yml are properly formatted
4. Check that GitHub Pages is set to "GitHub Actions" as source

## 📚 Additional Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Minimal Mistakes Theme Docs](https://mmistakes.github.io/minimal-mistakes/)

---

**Note:** If you see a 404 error after setup, wait a few minutes for DNS propagation and cache clearing. The first deployment may take slightly longer than subsequent ones.
