# Hugo Website Usage Guide

Complete guide for managing your personal website built with Hugo and the PaperMod theme.

## Table of Contents

1. [Hugo Basics](#hugo-basics)
2. [Site Structure](#site-structure)
3. [Common Tasks](#common-tasks)
4. [Configuration](#configuration)
5. [Theme Customization](#theme-customization)
6. [Deployment](#deployment)
7. [Troubleshooting](#troubleshooting)

---

## Hugo Basics

### What is Hugo?

Hugo is a **static site generator** written in Go. It takes your content (written in Markdown) and templates, then generates a complete static HTML website that can be hosted anywhere.

**Benefits:**
- ⚡ **Fast**: Builds sites in milliseconds
- 🔒 **Secure**: No database, no server-side code to hack
- 📦 **Simple**: Just HTML, CSS, and JavaScript files
- 💰 **Free hosting**: Can be hosted on Netlify, GitHub Pages, Vercel, etc.

### How Hugo Works

```
Content (Markdown) + Theme (Templates + CSS) + Config → HTML Website
```

1. You write content in **Markdown** files (`content/posts/my-post.md`)
2. Hugo uses **templates** from the theme to format your content
3. Hugo generates **static HTML** files in the `public/` directory
4. You deploy the `public/` folder to your hosting provider

---

## Site Structure

Your website has this structure:

```
melnychuk/
├── content/              # Your content (Markdown files)
│   ├── about.md         # About page
│   ├── projects.md      # Projects page
│   ├── archive.md       # Archive page
│   ├── search.md        # Search page
│   └── posts/           # Blog posts
│       └── hello-world.md
│
├── static/              # Static files (images, PDFs, etc.)
│   ├── img/            # Images
│   │   ├── avatar.png  # Profile picture
│   │   └── favicon.ico # Site icon
│   └── misc/           # Miscellaneous files
│       └── VasylMelnychukCV.pdf
│
├── assets/              # Theme assets (CSS customizations)
│   └── css/
│       └── extended/
│           └── custom.css  # Your custom CSS
│
├── themes/              # Hugo themes
│   └── PaperMod/       # PaperMod theme (git submodule)
│
├── hugo.yaml           # Site configuration
├── netlify.toml        # Netlify deployment config
└── public/             # Generated HTML (don't edit this!)
```

---

## Common Tasks

### Starting the Development Server

To preview your site locally:

```bash
hugo server
```

or with drafts enabled:

```bash
hugo server -D
```

Your site will be available at `http://localhost:1313/`

The server auto-reloads when you save changes!

**Stop the server**: Press `Ctrl+C`

---

### Writing a Blog Post

#### 1. Create a new post file

```bash
hugo new posts/my-awesome-post.md
```

This creates `content/posts/my-awesome-post.md` with default frontmatter.

#### 2. Edit the post

Open the file and edit the frontmatter (metadata) and content:

```markdown
---
title: "My Awesome Post Title"
date: 2026-01-10
draft: false
tags: ["ruby", "rails", "tutorial"]
categories: ["programming"]
description: "A brief description for SEO and preview"
---

Your blog post content goes here in **Markdown** format.

## Heading 2

Paragraph with some text.

### Code Example

```ruby
def hello
  puts "Hello, World!"
end
```

- Bullet point 1
- Bullet point 2

**Bold text** and *italic text*.

[Link to something](https://example.com)
```

#### 3. Preview locally

```bash
hugo server
```

Navigate to your post at `http://localhost:1313/posts/my-awesome-post/`

#### 4. Publish

Change `draft: false` in the frontmatter, then rebuild:

```bash
hugo --gc --minify
```

---

### Updating Existing Pages

#### About Page

Edit `content/about.md`:

```markdown
---
title: "About"
draft: false
hidemeta: true
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowReadingTime: false
ShowShareButtons: false
---

Your about content here...
```

#### Projects Page

Edit `content/projects.md` similarly.

**Important**: Keep the frontmatter settings (`hidemeta: true`, etc.) to hide blog-specific features.

---

### Adding Images

#### For Blog Posts

1. Place image in `static/img/posts/`:

```bash
mkdir -p static/img/posts
cp ~/Downloads/my-image.png static/img/posts/
```

2. Reference in your post:

```markdown
![Alt text](/img/posts/my-image.png)
```

#### For Cover Images

Add to post frontmatter:

```yaml
---
title: "My Post"
cover:
    image: "/img/posts/cover.jpg"
    alt: "Cover image description"
    caption: "Optional caption"
---
```

---

### Updating Your CV

1. Replace the file:

```bash
cp ~/Downloads/NewCV.pdf static/misc/VasylMelnychukCV.pdf
```

2. The download button on the homepage will automatically use the new file.

---

### Updating Your Profile Picture

Replace `static/img/avatar.png` with your new image (recommended: 400x400px).

---

## Configuration

The main config file is `hugo.yaml`. Here are the key sections:

### Basic Settings

```yaml
baseURL: "https://melnychuk.me/"
title: Vasyl Melnychuk
languageCode: en
theme: PaperMod
```

### Profile Mode (Homepage)

```yaml
params:
  profileMode:
    enabled: true
    title: "Vasyl Melnychuk"
    subtitle: "Senior Software Developer • Ruby on Rails Expert"
    imageUrl: "img/avatar.png"
    imageWidth: 180
    imageHeight: 180
    buttons:
      - name: About
        url: /about
      - name: Blog
        url: /posts
      - name: Download CV
        url: /misc/VasylMelnychukCV.pdf
```

**To update your subtitle**: Change the `subtitle` line above.

### Social Icons

```yaml
  socialIcons:
    - name: email
      url: "mailto:vasyl@melnychuk.me"
    - name: github
      url: "https://github.com/sqrel"
    - name: linkedin
      url: "https://www.linkedin.com/in/sqrel/"
```

**Available icon names**: `email`, `github`, `linkedin`, `twitter`, `instagram`, `telegram`, `stackoverflow`, `gitlab`, `reddit`, `facebook`, `youtube`, etc.

See full list: https://github.com/adityatelange/hugo-PaperMod/wiki/Icons

### Menu Navigation

```yaml
menu:
  main:
    - identifier: home
      name: Home
      url: /
      weight: 10
    - identifier: about
      name: About
      url: /about/
      weight: 20
```

**Weight** determines the order (lower numbers appear first).

---

## Theme Customization

### Custom CSS

Edit `assets/css/extended/custom.css` to override theme styles.

**Example** - Change link colors:

```css
.post-content a {
  color: #ff6b6b;
}
```

Hugo will automatically compile this with the theme's CSS.

### Dark Mode

PaperMod includes automatic dark mode based on system preferences.

Users can toggle it manually with the theme switcher button (moon/sun icon) in the header.

**To customize dark mode colors**, add to `assets/css/extended/custom.css`:

```css
[data-theme="dark"] {
  --primary: #your-color;
}
```

---

## Deployment

### Deploying to Netlify

Your site auto-deploys when you push to GitHub. Here's how it works:

#### 1. Make changes locally

```bash
# Edit files
vim content/posts/new-post.md

# Test locally
hugo server

# Build to verify no errors
hugo --gc --minify
```

#### 2. Commit and push

```bash
git add .
git commit -m "Add new blog post"
git push origin master
```

#### 3. Netlify builds automatically

Netlify reads `netlify.toml` and runs:

```bash
hugo --gc --minify
```

Then publishes the `public/` directory to https://melnychuk.me/

**Check deployment status**: https://app.netlify.com (login with your account)

### Manual Deployment

If Netlify fails or you want to deploy elsewhere:

```bash
# Build the site
hugo --gc --minify

# The public/ folder contains your complete website
# Upload this folder to any static hosting provider
```

---

## Troubleshooting

### "Build Failed" on Netlify

**Check Hugo version**:

Your `netlify.toml` specifies Hugo 0.154.3:

```toml
[context.production.environment]
  HUGO_VERSION = "0.154.3"
```

Make sure you test locally with the same version:

```bash
hugo version
# Should output: hugo v0.154.3
```

If different, install the correct version or update `netlify.toml`.

### "Theme not found" error

The PaperMod theme is a git submodule. To initialize it:

```bash
git submodule update --init --recursive
```

### Images not showing

- Images must be in `static/` directory
- Reference them from root: `/img/photo.png` (not `static/img/photo.png`)
- Check filename case sensitivity (Linux/Netlify is case-sensitive)

### CSS changes not appearing

1. Rebuild the site:

```bash
hugo --gc --minify
```

2. Clear browser cache (Ctrl+Shift+R or Cmd+Shift+R)

3. In development, Hugo sometimes needs a restart:

```bash
# Stop server (Ctrl+C), then:
hugo server
```

### Posts not showing on homepage

Check the post's frontmatter:

```yaml
draft: false  # Must be false to publish
date: 2026-01-10  # Can't be in the future (unless buildFuture = true)
```

### "Page not found" for new pages

Hugo needs pages to be in `content/`. Structure matters:

- `content/about.md` → `/about/`
- `content/posts/hello.md` → `/posts/hello/`
- `content/projects/project1.md` → `/projects/project1/`

---

## Useful Hugo Commands

### Development

```bash
hugo server              # Start dev server
hugo server -D           # Include draft posts
hugo server --noHTTPCache # Disable cache
```

### Building

```bash
hugo                     # Build site to public/
hugo --gc --minify       # Build with cleanup and minification
hugo --buildDrafts       # Include drafts in build
hugo --buildFuture       # Include future-dated posts
```

### Content Creation

```bash
hugo new posts/my-post.md        # New blog post
hugo new projects/project1.md    # New project page
```

### Information

```bash
hugo version             # Check Hugo version
hugo config              # View merged configuration
hugo list all           # List all content files
```

---

## Markdown Cheat Sheet

### Headers

```markdown
# H1
## H2
### H3
```

### Emphasis

```markdown
*italic* or _italic_
**bold** or __bold__
***bold italic***
~~strikethrough~~
```

### Lists

```markdown
- Unordered list item
- Another item
  - Nested item

1. Ordered list item
2. Another item
```

### Links

```markdown
[Link text](https://example.com)
[Link with title](https://example.com "Title on hover")
```

### Images

```markdown
![Alt text](/img/photo.png)
![Alt text](/img/photo.png "Optional title")
```

### Code

Inline: \`code\`

Block:
\`\`\`language
code here
\`\`\`

Examples:
\`\`\`ruby
def hello
  puts "Hello"
end
\`\`\`

\`\`\`javascript
console.log("Hello");
\`\`\`

### Blockquotes

```markdown
> This is a quote
> Multiple lines
```

### Horizontal Rule

```markdown
---
```

---

## Advanced: Frontmatter Options

Blog posts support many options:

```yaml
---
title: "Post Title"
date: 2026-01-10
lastmod: 2026-01-11
draft: false
author: "Vasyl Melnychuk"

description: "SEO description"
summary: "Shorter summary for listings"

tags: ["ruby", "tutorial"]
categories: ["programming"]

# Cover image
cover:
    image: "/img/cover.jpg"
    alt: "Alt text"
    caption: "Caption"
    relative: false

# SEO
keywords: ["ruby", "rails", "backend"]

# Display options
ShowToc: true
TocOpen: false
ShowReadingTime: true
ShowShareButtons: true
ShowPostNavLinks: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
disableShare: false
searchHidden: false
hideSummary: false

# Comments (if enabled)
comments: false
---
```

---

## Resources

### Hugo Documentation

- Official Docs: https://gohugo.io/documentation/
- Quick Start: https://gohugo.io/getting-started/quick-start/
- Content Management: https://gohugo.io/content-management/

### PaperMod Theme

- GitHub: https://github.com/adityatelange/hugo-PaperMod
- Demo: https://adityatelange.github.io/hugo-PaperMod/
- Wiki: https://github.com/adityatelange/hugo-PaperMod/wiki
- Features: https://github.com/adityatelange/hugo-PaperMod/wiki/Features
- FAQs: https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs

### Markdown

- Markdown Guide: https://www.markdownguide.org/
- Cheat Sheet: https://www.markdownguide.org/cheat-sheet/

### Hosting

- Netlify: https://www.netlify.com/
- Netlify Docs: https://docs.netlify.com/

---

## Quick Reference Card

| Task | Command |
|------|---------|
| Start dev server | `hugo server` |
| Start with drafts | `hugo server -D` |
| Build site | `hugo --gc --minify` |
| New blog post | `hugo new posts/title.md` |
| Check version | `hugo version` |
| View config | `hugo config` |
| List all content | `hugo list all` |

| File | Purpose |
|------|---------|
| `hugo.yaml` | Site configuration |
| `content/posts/*.md` | Blog posts |
| `content/about.md` | About page |
| `content/projects.md` | Projects page |
| `static/img/` | Images |
| `static/misc/` | Files (CV, etc) |
| `assets/css/extended/custom.css` | Custom CSS |
| `public/` | Generated site (don't edit) |

---

## Getting Help

If you encounter issues:

1. **Check Hugo version**: `hugo version` (should be 0.154.3)
2. **Check build logs**: Look at Netlify deploy logs
3. **Test locally**: Always test with `hugo server` before pushing
4. **Clear cache**: `hugo --gc --minify` rebuilds everything
5. **Check theme docs**: https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs
6. **Hugo community**: https://discourse.gohugo.io/

---

**Last updated**: January 9, 2026
