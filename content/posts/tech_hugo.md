---
title: "Hugo + GitHub Pages: Fully Automated Deployment Guide"
date: 2025-03-10T14:00:19+08:00
draft: false
tags:
  ["Hugo", "GitHub Pages", "Automation", "GitHub Actions", "Blog Deployment"]
categories: ["Tech Guide", "Web Development"]
summary: "Learn how to set up and fully automate your Hugo site deployment using GitHub Pages and GitHub Actions."
weight: 2
slug: "hugo-github-pages-auto-deploy"
---

## 🚀 Introduction

Deploying a Hugo site manually every time you update your blog can be time-consuming. What if you could automate the entire process, so that every time you push changes, your site is automatically rebuilt and published?

In this guide, we will learn how to set up Hugo with GitHub Pages and GitHub Actions for fully automated deployment. You will learn:

- How to install and configure Hugo
- How to deploy a Hugo site to GitHub Pages
- How to set up GitHub Actions for automatic deployment
- How to troubleshoot common issues

By the end of this tutorial, your Hugo blog will automatically update every time you push changes to GitHub—no more manual deployment required!

---

## 📌 How to Generate a Hugo Site on macOS

### Step 1: Install Hugo

```shell
brew install hugo
```

Verify the installation:

```shell
hugo version
```

### Step 2: Create a New Hugo Site

```shell
hugo new site myblog
cd myblog
```

### Step 3: Add a Theme (PaperMod)

```shell
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive
```

### Step 4: Configure Hugo

Edit config.toml:

```shell
baseURL = "https://yourusername.github.io/"
languageCode = "en-us"
title = "My Hugo Blog"
theme = "PaperMod"

[params]
  homeInfoParams = "Welcome to my site!"
```

### Step 5: Create a Post

```shell
hugo new posts/my-first-post.md
```

Edit content/posts/my-first-post.md and remove draft: true.

### Step 6: Preview the Site Locally

```shell
hugo server
```

Visit http://localhost:1313/ to see the live preview.

### Step 7: Change Output Folder to docs/

Since GitHub Pages requires files in /docs, modify config.toml:

```shell
publishDir = "docs"
```

### Step 8: Generate Static Files

After making changes, run:

```shell
hugo --minify
```

This builds the site inside the docs/ directory.

---

## 📌 How to Connect GitHub Pages to Hugo

### Step 9: Initialize Git

```shell
git init
git add .
git commit -m "Initial commit"
```

### Step 10: Push to GitHub

1. Create a GitHub repository (e.g., yourusername.github.io)
2. Link the repository:

```shell
git remote add origin git@github.com:yourusername/yourusername.github.io.git
```

3. Push the files:

```shell
git push -u origin main
```

### Step 11: Enable GitHub Pages

1. Go to GitHub Repository → Settings → Pages
2. Under Build and deployment:
   - Source: Select Deploy from a branch
   - Branch: Choose main and /docs directory
   - Click Save ✅

### Step 12: Deploy and Update the Site

After making updates to your content:

```shell
hugo --minify
git add --force docs/
git commit -m "Update site"
git push origin main
```

---

## 🔧 Fix Common Issues

### ❌ Issue 1: “GitHub Pages shows 404 error after deploy”

✅ Solution: Ensure baseURL is correct in config.toml:

```shell
baseURL = "https://yourusername.github.io/"
```

### ❌ Issue 2: “fatal: No URL found for submodule path ‘themes/PaperMod’”

✅ Solution: Remove and re-add the submodule.

```shell
git submodule deinit -f themes/PaperMod
rm -rf themes/PaperMod
rm -rf .git/modules/themes/PaperMod
git rm -rf themes/PaperMod
git commit -m "Remove broken PaperMod submodule"
git push origin main
```

Then reinstall:

```shell
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive
git commit -m "Re-add PaperMod submodule"
git push origin main
```

---

## 📌 Step 13: Automate Deployment with GitHub Actions

Instead of manually running hugo --minify and pushing docs/, you can use GitHub Actions to automatically deploy Hugo to GitHub Pages.

### Step 13.1: Create GitHub Actions Workflow

1. In your Hugo project, create a new folder for GitHub Actions:

```shell
mkdir -p .github/workflows
nano .github/workflows/deploy.yml
```

2. Copy and paste the following YAML script into deploy.yml:

```shell
name: Deploy Hugo site

on:
  push:
    branches:
      - main  # Runs when changes are pushed to the main branch

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes
          fetch-depth: 0    # Get full history

      - name: Install Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'

      - name: Build Hugo site
        run: hugo --minify  # Generates static files

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public  # Hugo generates output in public/
          publish_branch: gh-pages  # Push the output to gh-pages branch
```

3. Save and commit the workflow:

```shell
git add .
git commit -m "Add GitHub Actions for auto deployment"
git push origin main
```

### Step 13.2: Configure GitHub Pages to Use gh-pages

1. Go to GitHub → Repository → Settings → Pages
2. Change the deployment branch:
   - Source: Deploy from a branch
   - Branch: Select gh-pages and root (/)
   - Click Save ✅

### Step 13.3: write permission for gh-pages

1. Go to GitHub → Repository → Settings → Actions
2. Under “Workflow permissions”, select:
   - ✅ Read and write permissions
   - ✅ Allow GitHub Actions to create and approve pull requests
3. Save settings and trigger a new deployment.

### Step 13.4: Test Auto Deployment

1. Modify an article or create a new post

```shell
hugo new posts/test-automation.md
```

2. Edit content/posts/test-automation.md and set draft: false
3. Push the changes to GitHub

```shell
git add .
git commit -m "Test auto deployment"
git push origin main
```

4. Go to GitHub Actions → Check if “Deploy Hugo site” is running
5. After a few minutes, visit your site:

```shell
https://yourusername.github.io/
```

🎉 Your Hugo site should now be updated automatically!

---

## 🎯 Conclusion

### You Have Successfully Set Up GitHub Pages + Hugo with Auto Deployment!

✅ Manual deployment (Step 12) is no longer needed!  
✅ Every time you git push, GitHub Actions will rebuild and deploy your site automatically.
