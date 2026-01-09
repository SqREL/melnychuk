# Website Improvement Plan

This document outlines a comprehensive improvement plan for melnychuk.me, organized from quick wins to more complex enhancements.

---

## Phase 1: Quick Wins (30 minutes)

### 1.1 Remove Ello Social Link
**Objective:** Remove dead social network link from configuration.

**Actions:**
- Edit `/Users/vmelnychuk/other/melnychuk/config.toml`
- Remove the `ello` line from `[params.contact]` section (if it exists in the theme config)
- Check theme files at `/Users/vmelnychuk/other/melnychuk/themes/vncnt-hugo/config.toml` for any ello references

**Verification:**
- Run `hugo server` and check homepage
- Verify ello icon does not appear in contact section
- Check browser console for any errors

---

### 1.2 Add CV Highlights Section
**Objective:** Create a skills/highlights section above or near the CV download link.

**Actions:**
1. Modify homepage layout or create a custom partial:
   - Option A: Override theme's `layouts/index.html` by creating `/Users/vmelnychuk/other/melnychuk/layouts/index.html`
   - Option B: Create a new partial at `/Users/vmelnychuk/other/melnychuk/layouts/partials/highlights.html`

2. Add to `config.toml` under `[params]`:
   ```toml
   [params.highlights]
     skills = ["Go", "Python", "Kubernetes", "AWS", "React", "PostgreSQL"]
     cv_url = "/misc/VasylMelnychukCV.pdf"
   ```

3. Create HTML structure for highlights section:
   - Skills displayed as tags or list
   - Clear "Download CV" button
   - Responsive layout matching existing design

4. Style the section:
   - Match existing vncnt theme aesthetic
   - Use similar typography (Raleway font)
   - Ensure dark mode compatibility

**Verification:**
- Run `hugo server`
- Check homepage displays skills section
- Test dark mode (toggle OS dark mode preference)
- Verify CV download link still works
- Check responsive behavior on mobile viewport

---

## Phase 2: Content Additions (2-3 hours)

### 2.1 Create Expanded About Page
**Objective:** Add a dedicated About page with detailed professional information.

**Actions:**
1. Create content file: `/Users/vmelnychuk/other/melnychuk/content/about.md`
   - Use frontmatter:
     ```yaml
     ---
     title: "About"
     date: [current-date]
     draft: false
     ---
     ```

2. Write content sections:
   - **Introduction** - Who you are, what you do
   - **Background** - Professional journey
   - **Expertise** - Technical skills, domains of knowledge
   - **Interests** - What you're passionate about
   - **Currently** - What you're working on/learning

3. Add navigation link:
   - Modify layout to include About link in header/footer
   - Option: Create `/Users/vmelnychuk/other/melnychuk/layouts/partials/nav.html`
   - Or override theme's navigation partial

4. Style the About page:
   - Ensure it uses the same base template
   - Typography consistent with landing page
   - Add any custom styling if needed in `/Users/vmelnychuk/other/melnychuk/static/css/custom.css`

**Verification:**
- Run `hugo server`
- Navigate to `/about` or `/about/` URL
- Verify content displays correctly
- Check navigation link works
- Test responsive layout
- Verify dark mode styling

---

### 2.2 Create Projects Showcase
**Objective:** Add a Projects page displaying notable work and GitHub projects.

**Actions:**
1. Create content file: `/Users/vmelnychuk/other/melnychuk/content/projects.md`
   - Frontmatter with title, date, draft: false

2. Define project data structure in `config.toml`:
   ```toml
   [[params.projects]]
     name = "Project Name"
     description = "Brief description"
     url = "https://github.com/sqrel/project"
     tags = ["Go", "Docker", "API"]
     featured = true

   [[params.projects]]
     name = "Another Project"
     description = "Another description"
     url = "https://github.com/sqrel/another"
     tags = ["Python", "ML"]
     featured = false
   ```

3. Create projects layout:
   - Create `/Users/vmelnychuk/other/melnychuk/layouts/projects/single.html`
   - Or use `layouts/_default/single.html` and add conditional logic
   - Display projects as cards/list with:
     - Project name (linked to GitHub)
     - Description
     - Tech stack tags
     - Highlight featured projects

4. Add navigation link to Projects page

5. Style projects page:
   - Card-based or list-based layout
   - Tag styling for tech stack
   - Hover effects on project cards
   - Dark mode compatible

**Verification:**
- Run `hugo server`
- Navigate to `/projects` URL
- Verify all projects display
- Click project links to ensure they open correctly
- Test responsive layout
- Verify tags render properly
- Check dark mode styling

---

### 2.3 Setup Blog Infrastructure
**Objective:** Enable blog/writing section for technical posts.

**Actions:**
1. Create blog content directory:
   - Directory already exists: `/Users/vmelnychuk/other/melnychuk/content/posts/`
   - Create first sample post: `/Users/vmelnychuk/other/melnychuk/content/posts/hello-world.md`
   - Frontmatter:
     ```yaml
     ---
     title: "Hello World"
     date: [current-date]
     draft: false
     slug: "hello-world"
     tags: ["meta", "blog"]
     ---
     ```

2. Verify/create blog list layout:
   - Check if theme has `layouts/posts/list.html`
   - If not, create `/Users/vmelnychuk/other/melnychuk/layouts/posts/list.html`
   - Display posts with date, title, excerpt
   - Sort by date (newest first)

3. Verify/create single post layout:
   - Check if theme has `layouts/posts/single.html`
   - If not, create one with:
     - Post title
     - Publication date
     - Content
     - Tags
     - Navigation (prev/next posts)

4. Add Blog navigation link:
   - Add link to posts list page in navigation

5. Update homepage to show recent posts:
   - Modify homepage layout to include "Recent Posts" section
   - Show 3 most recent posts with links

6. Re-enable RSS feed:
   - In `config.toml`, change `disableKinds = ["RSS"]` to remove RSS or set to empty array
   - Configure RSS settings if needed

**Verification:**
- Run `hugo server`
- Navigate to `/posts` URL
- Verify post list displays
- Click on sample post, verify it opens
- Check post metadata (date, tags) displays
- Verify homepage shows recent posts
- Check RSS feed at `/index.xml`
- Test dark mode on all blog pages

---

## Phase 3: Technical Improvements (2-3 hours)

### 3.1 SEO Enhancement
**Objective:** Improve search engine visibility and social sharing.

**Actions:**
1. Update `config.toml` with better metadata:
   ```toml
   [params]
     description = "Vasyl Melnychuk - Software Developer specializing in [your specialties]. Blog about [topics]."
     keywords = ["software developer", "golang", "kubernetes", "your-keywords"]
   ```

2. Add OpenGraph and Twitter Card meta tags:
   - Create `/Users/vmelnychuk/other/melnychuk/layouts/partials/seo.html`
   - Include OpenGraph tags:
     - og:title
     - og:description
     - og:image (use avatar or create social card)
     - og:url
     - og:type
   - Include Twitter Card tags:
     - twitter:card
     - twitter:title
     - twitter:description
     - twitter:image

3. Update base template to include SEO partial:
   - Copy theme's `baseof.html` to `/Users/vmelnychuk/other/melnychuk/layouts/_default/baseof.html`
   - Add `{{ partial "seo.html" . }}` in `<head>` section

4. Add JSON-LD structured data:
   - Create `/Users/vmelnychuk/other/melnychuk/layouts/partials/structured-data.html`
   - Add Person schema for homepage
   - Add BlogPosting schema for blog posts
   - Add BreadcrumbList for navigation

5. Optimize robots.txt:
   - Create `/Users/vmelnychuk/other/melnychuk/layouts/robots.txt`
   - Allow all crawlers
   - Link to sitemap

6. Improve sitemap configuration:
   - In `config.toml`, add:
     ```toml
     [sitemap]
       changefreq = "weekly"
       priority = 0.5
     ```

**Verification:**
- Run `hugo server`
- View page source on homepage, verify meta tags present
- Use browser dev tools to inspect OpenGraph tags
- Test with OpenGraph debugger: https://www.opengraph.xyz/
- Use Twitter Card Validator (if available)
- Check `/robots.txt` is accessible
- Check `/sitemap.xml` is accessible and well-formed
- Verify structured data with Google Rich Results Test

---

### 3.2 Dark Mode Toggle
**Objective:** Add manual dark mode toggle instead of OS-only preference.

**Actions:**
1. Create dark mode toggle JavaScript:
   - Create `/Users/vmelnychuk/other/melnychuk/static/js/dark-mode.js`
   - Implement:
     - Check localStorage for user preference
     - Check OS preference as fallback
     - Toggle dark class on `<html>` or `<body>`
     - Save preference to localStorage
     - Expose toggle function

2. Create toggle UI component:
   - Create `/Users/vmelnychuk/other/melnychuk/layouts/partials/dark-mode-toggle.html`
   - Add button/switch UI (moon/sun icon)
   - Style to match site aesthetic
   - Position in header or corner of page

3. Update CSS for dark mode:
   - Modify or create `/Users/vmelnychuk/other/melnychuk/static/css/custom.css`
   - Change from media query to class-based:
     - Instead of `@media (prefers-color-scheme: dark)`
     - Use `.dark-mode` or `[data-theme="dark"]`
   - Keep all existing dark mode colors
   - Ensure smooth transitions

4. Include toggle in base template:
   - Add `{{ partial "dark-mode-toggle.html" . }}` to header
   - Add `<script src="/js/dark-mode.js"></script>` before `</body>`

5. Update Content Security Policy (if exists):
   - Ensure inline scripts are allowed or use nonce

**Verification:**
- Run `hugo server`
- Verify toggle button appears on all pages
- Click toggle, verify theme switches
- Check localStorage persists preference
- Reload page, verify preference is remembered
- Test with different OS preferences
- Verify all page elements respect dark mode
- Check transitions are smooth
- Test on mobile viewport

---

### 3.3 Performance Optimization
**Objective:** Ensure site loads quickly and efficiently.

**Actions:**
1. Optimize images:
   - Check avatar image size at `/Users/vmelnychuk/other/melnychuk/static/img/avatar.png`
   - If > 100KB, optimize using Hugo image processing or external tool
   - Create WebP versions for modern browsers
   - Add responsive image sizes

2. Implement lazy loading:
   - Update image tags to include `loading="lazy"`
   - Update avatar and any project images

3. Add resource hints:
   - In base template `<head>`, add:
     - `<link rel="preconnect">` for external resources
     - `<link rel="dns-prefetch">` for external domains
     - `<link rel="preload">` for critical assets (fonts, CSS)

4. Optimize font loading:
   - Check Font Awesome and Raleway font loading
   - Use `font-display: swap` in CSS
   - Consider subsetting fonts for used characters only

5. Minify and bundle CSS:
   - Ensure Hugo minification is working (already in netlify.toml)
   - Consider combining CSS files to reduce requests

6. Add caching headers:
   - Create `/Users/vmelnychuk/other/melnychuk/static/_headers` for Netlify
   - Set cache headers for static assets:
     ```
     /fonts/*
       Cache-Control: public, max-age=31536000, immutable
     /css/*
       Cache-Control: public, max-age=31536000, immutable
     /js/*
       Cache-Control: public, max-age=31536000, immutable
     /img/*
       Cache-Control: public, max-age=31536000, immutable
     ```

7. Enable Brotli compression:
   - Netlify handles this automatically, verify in netlify.toml

**Verification:**
- Run `hugo server`
- Open browser DevTools Network tab
- Check page load time (should be < 1s)
- Verify images load correctly (including lazy loading)
- Check WebP images served to supporting browsers
- Use Lighthouse audit (in Chrome DevTools)
  - Target: Performance score > 95
  - Target: Best Practices score > 95
  - Target: Accessibility score > 95
  - Target: SEO score > 95
- Test on slow 3G network throttling
- Verify font loading doesn't block rendering
- Check total page size < 500KB

---

## Phase 4: Human Verification & Testing

### Final Testing Checklist

**Objective:** Comprehensive manual testing before deployment.

#### 4.1 Local Build Test
```bash
# Clean previous builds
rm -rf public/

# Build site locally
hugo --gc --minify

# Serve the built site
hugo server
```

#### 4.2 Visual Testing
**Test all pages in browser at http://localhost:1313/**

Homepage:
- [ ] Profile information displays correctly
- [ ] CV highlights/skills section visible
- [ ] Social links work (GitHub, LinkedIn, Instagram, Telegram)
- [ ] No Twitter or Ello links present
- [ ] Recent posts section shows latest posts (if any)
- [ ] Avatar image loads
- [ ] Dark mode toggle visible and functional

About Page:
- [ ] Navigate to http://localhost:1313/about/
- [ ] Content displays with proper formatting
- [ ] Typography consistent with homepage
- [ ] Navigation works

Projects Page:
- [ ] Navigate to http://localhost:1313/projects/
- [ ] All projects display correctly
- [ ] Project links open in new tab/window
- [ ] Tech stack tags render properly
- [ ] Featured projects highlighted

Blog:
- [ ] Navigate to http://localhost:1313/posts/
- [ ] Post list displays with dates
- [ ] Click on a post, verify it opens
- [ ] Post content readable
- [ ] Tags display correctly
- [ ] Navigation between posts works

#### 4.3 Responsive Testing
**Resize browser window or use DevTools device emulation**

- [ ] Test mobile view (375px width)
- [ ] Test tablet view (768px width)
- [ ] Test desktop view (1200px+ width)
- [ ] Verify all pages are readable at all sizes
- [ ] Check touch targets are large enough on mobile
- [ ] Verify images scale appropriately

#### 4.4 Dark Mode Testing
**Toggle dark mode using the new toggle button**

- [ ] Homepage looks good in dark mode
- [ ] About page looks good in dark mode
- [ ] Projects page looks good in dark mode
- [ ] Blog pages look good in dark mode
- [ ] All text is readable (sufficient contrast)
- [ ] Images have appropriate opacity/styling
- [ ] Toggle persists after page reload
- [ ] Test with OS dark mode on AND off

#### 4.5 Performance Testing
**Use Chrome DevTools**

```bash
# Open Chrome
# Navigate to http://localhost:1313/
# Open DevTools (F12)
# Go to Lighthouse tab
# Run audit for Desktop and Mobile
```

- [ ] Performance score > 90
- [ ] Accessibility score > 95
- [ ] Best Practices score > 95
- [ ] SEO score > 95
- [ ] Check Network tab: Total page size < 500KB
- [ ] Check Network tab: Load time < 1 second

#### 4.6 SEO Testing
**Verify meta tags and structured data**

- [ ] View page source, check `<title>` tag
- [ ] Verify `<meta name="description">` present
- [ ] Check OpenGraph tags (og:title, og:description, og:image)
- [ ] Check Twitter Card tags
- [ ] Navigate to http://localhost:1313/sitemap.xml - verify it loads
- [ ] Navigate to http://localhost:1313/robots.txt - verify it loads
- [ ] Paste homepage HTML into https://validator.schema.org/ - verify structured data

#### 4.7 Link Testing
**Verify all links work**

- [ ] Click all social media links - verify they open correct profiles
- [ ] Click CV download link - verify PDF downloads
- [ ] Click all navigation links
- [ ] Click all internal page links
- [ ] Verify no broken links (404 errors)

#### 4.8 Cross-Browser Testing (if possible)
**Test in multiple browsers**

- [ ] Chrome/Chromium - main testing
- [ ] Firefox - verify compatibility
- [ ] Safari - verify compatibility (especially dark mode)
- [ ] Edge - verify compatibility

#### 4.9 Content Review
**Proofread all content**

- [ ] Check for typos in bio
- [ ] Check for typos in About page
- [ ] Check for typos in project descriptions
- [ ] Verify all links point to correct URLs
- [ ] Verify email address is correct
- [ ] Check dates are current

#### 4.10 Git Status Check
**Before committing**

```bash
git status
```

- [ ] Review all modified files
- [ ] Ensure no unwanted files staged (no .DS_Store, no node_modules, no public/)
- [ ] Verify .gitignore is working correctly

---

### Final Sign-off

Once all checklist items pass:

1. **Commit changes:**
   ```bash
   git add .
   git commit -m "Comprehensive site improvements: quick wins, content additions, technical enhancements"
   ```

2. **Test build on Netlify:**
   - Push to master
   - Wait for Netlify deploy
   - Check deploy logs for errors
   - Visit deployed site at https://melnychuk.me/

3. **Post-deployment verification:**
   - Test all functionality on live site
   - Check SSL certificate
   - Verify custom domain works
   - Test from different devices/networks
   - Share site with a friend for feedback

---

## Notes for AI Agent

- **Context Awareness:** Read existing files before modifying them
- **Theme Preservation:** Maintain the minimal aesthetic of vncnt-hugo theme
- **Consistency:** Match existing code style and formatting
- **Incremental Testing:** Run `hugo server` after each phase to catch issues early
- **Rollback Strategy:** Commit after each phase so changes can be reverted if needed
- **Documentation:** Comment complex code for future maintenance
- **Accessibility:** Ensure all interactive elements are keyboard accessible
- **Error Handling:** Check browser console for JavaScript errors after each change

---

## Estimated Timeline

- Phase 1 (Quick Wins): 30 minutes
- Phase 2 (Content): 2-3 hours
- Phase 3 (Technical): 2-3 hours
- Phase 4 (Testing): 1 hour

**Total: 5-7 hours of development time**

---

## Success Criteria

At completion, the site should:
1. ✅ Have no dead social links (Ello, Twitter removed)
2. ✅ Display skills/CV highlights prominently
3. ✅ Have a detailed About page
4. ✅ Showcase projects effectively
5. ✅ Support blogging with proper layouts
6. ✅ Have excellent SEO (Lighthouse score > 90)
7. ✅ Include manual dark mode toggle
8. ✅ Load quickly (< 1s, < 500KB)
9. ✅ Be fully responsive
10. ✅ Pass all accessibility checks

---

*This plan is designed to be executed sequentially. Complete each phase before moving to the next. Stop and request human review if any major issues arise.*
