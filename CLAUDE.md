# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal website/portfolio built with Hugo v0.154.3 using the **vncnt-hugo** theme (minimal landing page theme). The site is deployed to Netlify at https://melnychuk.me/.

**Site Purpose:** Minimal personal landing page with profile info, social links, and CV download. Can be extended to include blog posts, projects showcase, and more detailed pages.

## IMPORTANT: IMPROVEMENT_PLAN.md

**READ THIS FIRST:** There is a comprehensive improvement plan in `/Users/vmelnychuk/other/melnychuk/IMPROVEMENT_PLAN.md` that outlines:

- **Phase 1:** Quick wins (remove dead social links, add CV highlights)
- **Phase 2:** Content additions (About page, Projects showcase, Blog setup)
- **Phase 3:** Technical improvements (SEO, dark mode toggle, performance optimization)
- **Phase 4:** Human verification checklist with detailed testing instructions

**This plan is designed for AI agents to execute sequentially.** Each task includes:
- Clear objectives and specific file paths
- Exact code/config to add or modify
- Verification steps after completion
- Final human testing checklist

**When working on site improvements, always consult IMPROVEMENT_PLAN.md first.** Do not improvise features - follow the plan or ask the user for clarification.

## Development Commands

### Local Development
```bash
# Start local development server (with drafts)
hugo server -D

# Start server without drafts
hugo server

# Server runs at http://localhost:1313/
```

### Build
```bash
# Production build (same as Netlify)
hugo --gc --minify

# Output is in public/ directory
```

### Clean Build Artifacts
```bash
rm -rf public/ resources/_gen/ .hugo_build.lock
```

## Architecture

### Theme Structure (vncnt-hugo)

The theme uses a **flat, single-page layout** rather than traditional Hugo blog structure:

- **Homepage (`layouts/index.html`)**: Simple template that calls `{{ partial "head" . }}` and `{{ partial "body" . }}`
- **Body partial (`layouts/partials/body.html`)**: Contains the actual homepage content (avatar, bio, social links)
- **No traditional baseof.html**: The theme uses `index.html` directly for the landing page

**Key difference from typical Hugo themes:** Content is template-driven rather than markdown-driven. The homepage shows data from `config.toml` params, not from `content/` files.

### Layout Override Strategy

To customize layouts without modifying the theme:

1. **Override layouts**: Create files in `/layouts/` matching theme structure
   - Example: `/layouts/index.html` overrides `/themes/vncnt-hugo/layouts/index.html`
   - Example: `/layouts/partials/body.html` overrides theme partial

2. **Override partials**: Create in `/layouts/partials/`
   - Useful for modifying: `head.html`, `body.html`, `scripts.html`

3. **Add custom CSS**: Place in `/static/css/custom.css` and include in custom head partial

### Content Structure

- **`/content/`**: Currently mostly empty (only `.keep` file)
- **Blog posts**: Would go in `/content/posts/` with frontmatter
- **Pages**: Regular pages go in `/content/` (e.g., `about.md`, `projects.md`)
- **Static files**: `/static/` maps to site root (images, CV, fonts)

### Configuration

All site data lives in `/config.toml`:

- **`[params]`**: Site owner info (author, bio, email, avatar, favicon)
- **`[params.contact]`**: Social links (key must match Font Awesome brand icon name)
- **`[permalinks]`**: URL structure for content types
- **`[markup.goldmark]`**: Markdown rendering settings (hardWraps enabled)

### Theme Customization via Config

Social links are **lexicographically sorted** due to Hugo's `range` behavior over maps. Keys must match [Font Awesome brand icons](https://fontawesome.com/icons?s=brands).

Example:
```toml
[params.contact]
  github = "https://github.com/username"
  linkedin = "https://www.linkedin.com/in/username/"
```

### Dark Mode

The theme includes **OS-based dark mode** via CSS media query `@media (prefers-color-scheme: dark)`. No JavaScript toggle exists yet.

Dark mode styles are in `/themes/vncnt-hugo/static/css/vncnt.css`.

## Important Files

- **`config.toml`**: All site configuration, personal info, social links
- **`netlify.toml`**: Deployment config (Hugo version, build command)
- **`themes/vncnt-hugo/layouts/partials/body.html`**: Main homepage content template
- **`themes/vncnt-hugo/layouts/partials/scripts.html`**: Script includes (Google Analytics removed)
- **`static/img/avatar.png`**: Profile image (62KB)
- **`static/misc/VasylMelnychukCV.pdf`**: Downloadable CV

## Deployment

- **Platform**: Netlify
- **Build command**: `hugo --gc --minify`
- **Hugo version**: 0.154.3 (set in netlify.toml)
- **Deploy trigger**: Push to `master` branch

Netlify reads configuration from `/netlify.toml`. Build happens automatically on push.

## Theme Notes

**vncnt-hugo specific behaviors:**

1. **Social links auto-sort alphabetically** - Cannot control order in config
2. **Font Awesome required** - Social icon keys must match FA brand names
3. **No navigation by default** - Theme is single-page focused
4. **Empty header/footer partials** - Intentionally minimal design
5. **Google Analytics removed** - The `_internal/google_analytics_async.html` call was deprecated and removed from `scripts.html`

## Adding Blog Functionality

The theme supports blog posts but they're not currently used:

1. Create posts in `/content/posts/`
2. Add frontmatter with `title`, `date`, `slug`, `tags`
3. Use permalink config: `posts = "posts/:slug/"`
4. Theme may need list/single templates for posts (check `/themes/vncnt-hugo/layouts/`)

## Extension Strategy

**CRITICAL: See IMPROVEMENT_PLAN.md for the full roadmap and implementation details.**

When extending the site:

1. **Follow IMPROVEMENT_PLAN.md phases** - Don't improvise, execute the documented plan
2. **Don't modify theme files directly** - Use Hugo's override mechanism
3. **Create layouts in root `/layouts/`** to override theme
4. **Add custom CSS in `/static/css/`** and include via custom head partial
5. **Store new content in `/content/`** with proper frontmatter
6. **Test locally with `hugo server`** before pushing
7. **Complete Phase 4 human verification checklist** before considering any phase done

## Git Workflow

```bash
# Theme is a git submodule
git submodule update --init --recursive

# When making changes
git add [files]
git commit -m "message"
git push origin master  # Triggers Netlify deploy
```

## Common Pitfalls

1. **Modified theme files won't persist** if theme submodule is updated - Always override in root
2. **RSS is disabled** - Set `disableKinds = ["RSS"]` in config
3. **Relative URLs enabled** - Site works offline but may cause issues with absolute URL requirements
4. **No default templates for content types** - May need to create list/single layouts when adding pages
5. **Theme's baseof.html is unused** - Homepage uses simpler index.html + partials pattern
