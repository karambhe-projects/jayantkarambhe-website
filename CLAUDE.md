# CLAUDE.md — Website Setup Instructions

## Project Overview

Build a personal website for a medical doctor (GP) who specialises in lifestyle medicine. The site serves as a landing page with profile photo and biography, a directory of open-source courses (static HTML hosted on GitHub Pages), and a future blog. The doctor should be able to manage content via a browser-based CMS without touching code.

**Stack:** Hugoplate (Hugo + Tailwind CSS) → Netlify (hosting + identity) → Decap CMS (visual editor)
**Cost:** Free (domain already owned)

---

## Context & Constraints

- The doctor is NOT technical. He will never use a terminal, Git, or code editor.
- His son (the repo owner/collaborator) handles all structural/technical changes via Git.
- The doctor manages day-to-day content (blog posts, course entries, about page) through Decap CMS at `[domain]/admin/`.
- Courses are static HTML sites already hosted on separate GitHub Pages repos. The website just links to them.
- The landing page MUST feature: a professional profile photo (circular), the doctor's name and title, a brief biography about his GP career and lifestyle medicine expertise, and clear navigation to Courses and Blog.
- Colour palette should feel medical/trustworthy: muted blues, greens, or neutral tones. Avoid anything flashy.

---

## Phase 1: Scaffold the Hugoplate Project

### 1.1 Clone and set up Hugoplate

```bash
git clone https://github.com/zeon-studio/hugoplate.git .
npm run project-setup
npm install
```

After `project-setup`, the exampleSite contents are moved to the root. Verify you have:
- `hugo.toml` in root
- `config/_default/params.toml`
- `data/theme.json`
- `content/` directory with example content
- `themes/hugoplate/` directory

### 1.2 Clean out example content

Remove all example/demo content but preserve the directory structure:

```bash
# Remove example blog posts
rm -rf content/english/blog/*
# Remove example pages (but keep the directory)
rm -rf content/english/pages/*
# Remove example authors
rm -rf content/english/authors/*
# Keep content/english/_index.md (homepage) — we'll rewrite it
```

Do NOT delete:
- `content/english/_index.md` (homepage — rewrite it)
- `layouts/` directory
- `themes/` directory
- `config/` directory
- `data/` directory

### 1.3 Verify the dev server works

```bash
npm run dev
```

Confirm site loads at `http://localhost:1313` before making changes.

---

## Phase 2: Configure the Site

### 2.1 Update `hugo.toml`

Set the following values (leave everything else as default):

```toml
baseURL = "https://DOMAIN_PLACEHOLDER.com/"
title = "Dr [NAME] — General Practitioner & Lifestyle Medicine"
```

**IMPORTANT:** Replace `DOMAIN_PLACEHOLDER.com` with the actual domain. Replace `[NAME]` with the doctor's actual name throughout all files.

### 2.2 Update `config/_default/params.toml`

Key fields to update:
- `description` — "Dr [NAME] is a General Practitioner and lifestyle medicine specialist. Browse free medical education courses and articles."
- `author` — "Dr [NAME]"
- `image` — Set to the doctor's Open Graph image path (e.g. `/images/og-image.png`)
- `contact_form_action` — Leave empty or remove if no contact form needed initially
- `logo` — Can be text-based initially (just the doctor's name)
- `logo_darkmode` — Same
- Disable or remove the announcement banner (`[notification]` section)
- Disable any Mailchimp/newsletter integration unless requested later

### 2.3 Update `data/theme.json`

Set a medical/professional colour scheme:

```json
{
  "colors": {
    "default": {
      "theme_color": {
        "primary": "#1e6b5c",
        "body": "#1a1a2e",
        "border": "#e0e0e0",
        "theme_light": "#f0f7f5",
        "theme_dark": "#16534a"
      },
      "font_color": {
        "default": "#333333",
        "dark": "#222222",
        "light": "#666666"
      }
    }
  },
  "fonts": {
    "font_family": {
      "primary": "DM Sans:wght@400;500;600;700",
      "primary_type": "sans-serif",
      "secondary": "",
      "secondary_type": ""
    },
    "font_size": {
      "base": "16",
      "scale": "1.250"
    }
  }
}
```

> Note: These are starting values. Adjust the primary colour (currently a calming teal-green) if the doctor prefers something different. Other good options:
> - Medical blue: `#1a5276`
> - Slate professional: `#2c3e50`
> - Sage green: `#5a7d6a`

### 2.4 Update `data/social.json`

Set only the relevant social links. At minimum:
```json
[
  {
    "icon": "FaEnvelope",
    "link": "mailto:EMAIL_PLACEHOLDER"
  },
  {
    "icon": "FaLinkedin",
    "link": "https://linkedin.com/in/LINKEDIN_PLACEHOLDER"
  }
]
```

Remove any social links that don't apply (Twitter, Facebook, etc. unless specified).

### 2.5 Update navigation menus

Edit `config/_default/menus.en.toml`:

```toml
[[main]]
  name = "Home"
  url = "/"
  weight = 1

[[main]]
  name = "Courses"
  url = "/courses/"
  weight = 2

[[main]]
  name = "Blog"
  url = "/blog/"
  weight = 3

[[main]]
  name = "About"
  url = "/about/"
  weight = 4

[[footer]]
  name = "Courses"
  url = "/courses/"
  weight = 1

[[footer]]
  name = "Blog"
  url = "/blog/"
  weight = 2

[[footer]]
  name = "About"
  url = "/about/"
  weight = 3
```

---

## Phase 3: Create the Landing Page (Homepage)

### 3.1 Rewrite `content/english/_index.md`

The homepage must have a hero section with:
- A profile photo of the doctor (circular crop)
- His full name and title ("Dr [NAME]")
- Subtitle: "General Practitioner · Lifestyle Medicine Specialist"
- A 2-3 sentence biography paragraph about his career and expertise
- A prominent "Browse Courses" CTA button linking to `/courses/`
- Optionally a secondary "Read the Blog" link

Use Hugoplate's front matter banner/hero configuration. Structure the front matter like:

```yaml
---
title: "Dr [NAME]"
# Use Hugoplate's banner config
banner:
  title: "Dr [NAME]"
  content: "General Practitioner and lifestyle medicine specialist with [X] years of experience. Passionate about empowering patients and medical professionals through evidence-based lifestyle medicine education."
  image: "/images/profile.png"
  button:
    enable: true
    label: "Browse Courses"
    link: "/courses/"
---
```

### 3.2 Create a custom hero partial (if needed)

Hugoplate's default banner may not centre a circular profile photo. If the default hero layout doesn't support a circular centred portrait, create a custom partial:

**`layouts/partials/hero-profile.html`**:

A hero section that displays:
1. Circular profile image (centred, ~180px, with a subtle border/shadow)
2. Name as an `<h1>`
3. Subtitle/role as a `<p>` with muted colour
4. 2-3 sentence bio paragraph
5. CTA button to `/courses/`

Style it using Tailwind classes consistent with Hugoplate's design system. Keep the layout centred and clean.

If Hugoplate's default banner already supports an image alongside text in a split layout (text left, image right), that also works — adjust to use a circular crop on the image via Tailwind's `rounded-full` class.

### 3.3 Add profile image

Place the doctor's profile photo at:
- `assets/images/profile.png` (or `.jpg`)

If no photo is available yet, create a placeholder. The image should be:
- Square aspect ratio (at least 400×400px)
- Will be displayed as circular via CSS `border-radius: 50%`

---

## Phase 4: Create the Courses Section

### 4.1 Create courses list page

Create `content/english/courses/_index.md`:

```yaml
---
title: "Courses"
meta_title: "Free Medical Education Courses — Dr [NAME]"
description: "Free, open-source medical education courses on lifestyle medicine and general practice."
---

Browse all available courses below. Each course is free, self-paced, and open source.
```

### 4.2 Create individual course entries

For each course, create a Markdown file in `content/english/courses/`. Example:

**`content/english/courses/course-name.md`**:

```yaml
---
title: "Introduction to Lifestyle Medicine"
meta_title: "Intro to Lifestyle Medicine — Free Course"
description: "A beginner-friendly course covering the six pillars of lifestyle medicine."
image: "/images/courses/lifestyle-medicine-intro.png"
course_url: "https://USERNAME.github.io/course-repo-name/"
weight: 1
draft: false
---

A short description of what students will learn in this course.

**Topics covered:**
- The six pillars of lifestyle medicine
- Evidence-based behaviour change
- Nutrition fundamentals

[Start the course →](https://USERNAME.github.io/course-repo-name/)
```

### 4.3 Create a courses list template

Hugoplate doesn't have a built-in "courses" content type, so create a custom list layout.

**`layouts/courses/list.html`**:

```html
{{ define "main" }}
  <section class="section">
    <div class="container">
      <div class="text-center mb-14">
        <h1 class="mb-4">{{ .Title }}</h1>
        {{ if .Content }}
          <div class="content text-lg text-text">
            {{ .Content }}
          </div>
        {{ end }}
      </div>

      <div class="row gy-4 justify-center">
        {{ range .Pages.ByWeight }}
          <div class="col-12 sm:col-6 lg:col-4">
            <div class="rounded-xl bg-theme-light p-6 h-full flex flex-col">
              {{ if .Params.image }}
                <img
                  src="{{ .Params.image }}"
                  alt="{{ .Title }}"
                  class="rounded-lg mb-4 w-full h-40 object-cover"
                />
              {{ end }}
              <h3 class="mb-2 text-lg">{{ .Title }}</h3>
              <p class="text-text mb-4 flex-grow">{{ .Description }}</p>
              {{ if .Params.course_url }}
                <a
                  href="{{ .Params.course_url }}"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="btn btn-primary btn-sm mt-auto"
                >
                  Start Course →
                </a>
              {{ end }}
            </div>
          </div>
        {{ end }}
      </div>
    </div>
  </section>
{{ end }}
```

Also create **`layouts/courses/single.html`** that redirects to the external course URL or shows the course detail page:

```html
{{ define "main" }}
  <section class="section">
    <div class="container max-w-3xl">
      <h1 class="mb-4">{{ .Title }}</h1>
      <p class="text-lg text-text mb-6">{{ .Description }}</p>

      <div class="content mb-8">
        {{ .Content }}
      </div>

      {{ if .Params.course_url }}
        <a
          href="{{ .Params.course_url }}"
          target="_blank"
          rel="noopener noreferrer"
          class="btn btn-primary"
        >
          Start Course →
        </a>
      {{ end }}
    </div>
  </section>
{{ end }}
```

---

## Phase 5: Create Supporting Pages

### 5.1 About page

Create `content/english/about.md` (or `content/english/pages/about.md` depending on Hugoplate's routing):

```yaml
---
title: "About"
meta_title: "About Dr [NAME]"
description: "Learn more about Dr [NAME]'s career in general practice and lifestyle medicine."
image: "/images/profile.png"
layout: "about"
draft: false
---

A longer biography goes here. Multiple paragraphs covering:

- Medical training and qualifications
- Years of GP experience
- Interest and expertise in lifestyle medicine
- Why he created these courses
- Any professional memberships or affiliations

## Get in Touch

For enquiries, email [EMAIL_PLACEHOLDER](mailto:EMAIL_PLACEHOLDER).
```

### 5.2 Blog section

Create `content/english/blog/_index.md`:

```yaml
---
title: "Blog"
meta_title: "Blog — Dr [NAME]"
description: "Articles on lifestyle medicine, general practice, and medical education."
---
```

Create one example post so the section isn't empty:

**`content/english/blog/welcome.md`**:

```yaml
---
title: "Welcome to My Blog"
meta_title: ""
description: "An introduction to this blog and what to expect."
date: 2026-04-09T00:00:00Z
image: "/images/blog/welcome.png"
categories: ["General"]
author: "Dr [NAME]"
tags: ["lifestyle medicine", "welcome"]
draft: false
---

A welcome post introducing the blog, what topics will be covered, and the doctor's motivation for writing.
```

---

## Phase 6: Netlify Configuration

### 6.1 Create `netlify.toml` in the repo root

```toml
[build]
  command = "npm run build"
  publish = "public"

[build.environment]
  HUGO_VERSION = "0.145.0"
  NODE_ENV = "production"
  HUGO_ENV = "production"

[context.production.environment]
  HUGO_ENV = "production"

[context.deploy-preview]
  command = "npm run build"

[[redirects]]
  from = "/admin"
  to = "/admin/"
  status = 301
```

> Check the latest Hugo version at gohugo.io and update `HUGO_VERSION` accordingly.

---

## Phase 7: Decap CMS Setup (Visual Editor for the Doctor)

### 7.1 Create `static/admin/index.html`

```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Content Manager</title>
  <script src="https://unpkg.com/decap-cms@^3.0.0/dist/decap-cms.js"></script>
</head>
<body></body>
</html>
```

### 7.2 Create `static/admin/config.yml`

```yaml
backend:
  name: git-gateway
  branch: main

media_folder: static/images
public_folder: /images

slug:
  encoding: "ascii"
  clean_accents: true

collections:
  # ─── Blog Posts ───
  - name: "blog"
    label: "Blog Posts"
    folder: "content/english/blog"
    create: true
    slug: "{{slug}}"
    summary: "{{title}} — {{year}}/{{month}}/{{day}}"
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Description", name: "description", widget: "string", required: false }
      - { label: "Date", name: "date", widget: "datetime" }
      - { label: "Image", name: "image", widget: "image", required: false, hint: "Featured image for the post" }
      - { label: "Categories", name: "categories", widget: "list", default: ["General"], hint: "Comma-separated list" }
      - { label: "Tags", name: "tags", widget: "list", required: false }
      - { label: "Author", name: "author", widget: "string", default: "Dr [NAME]" }
      - { label: "Draft", name: "draft", widget: "boolean", default: false }
      - { label: "Body", name: "body", widget: "markdown" }

  # ─── Courses ───
  - name: "courses"
    label: "Courses"
    folder: "content/english/courses"
    create: true
    slug: "{{slug}}"
    filter: { field: "title", pattern: "^(?!Courses$)" }
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Description", name: "description", widget: "string" }
      - { label: "Course URL", name: "course_url", widget: "string", hint: "Full URL to the GitHub Pages course (e.g. https://username.github.io/course-name/)" }
      - { label: "Image", name: "image", widget: "image", required: false, hint: "Course thumbnail image" }
      - { label: "Sort Order", name: "weight", widget: "number", default: 10, hint: "Lower numbers appear first" }
      - { label: "Draft", name: "draft", widget: "boolean", default: false }
      - { label: "Body", name: "body", widget: "markdown", required: false, hint: "Optional longer description shown on course detail page" }

  # ─── Standalone Pages ───
  - name: "pages"
    label: "Pages"
    files:
      - label: "About"
        name: "about"
        file: "content/english/about.md"
        fields:
          - { label: "Title", name: "title", widget: "string" }
          - { label: "Description", name: "description", widget: "string", required: false }
          - { label: "Image", name: "image", widget: "image", required: false }
          - { label: "Body", name: "body", widget: "markdown" }

      - label: "Homepage"
        name: "homepage"
        file: "content/english/_index.md"
        fields:
          - { label: "Title", name: "title", widget: "string" }
          - label: "Banner"
            name: "banner"
            widget: "object"
            fields:
              - { label: "Title", name: "title", widget: "string" }
              - { label: "Content", name: "content", widget: "text", hint: "Short biography paragraph" }
              - { label: "Image", name: "image", widget: "image" }
              - label: "Button"
                name: "button"
                widget: "object"
                fields:
                  - { label: "Enable", name: "enable", widget: "boolean", default: true }
                  - { label: "Label", name: "label", widget: "string", default: "Browse Courses" }
                  - { label: "Link", name: "link", widget: "string", default: "/courses/" }
```

### 7.3 Add Netlify Identity widget to the site

Create or edit **`layouts/partials/custom/head.html`** (check Hugoplate's docs for the correct partial hook — it may be `layouts/partials/hooks/head.html` or similar):

```html
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
```

Create or edit **`layouts/partials/custom/footer.html`** (or equivalent hook):

```html
<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("init", user => {
      if (!user) {
        window.netlifyIdentity.on("login", () => {
          document.location.href = "/admin/";
        });
      }
    });
  }
</script>
```

**IMPORTANT:** Hugoplate's partial hook location may differ from standard Hugo themes. Check `themes/hugoplate/layouts/_default/baseof.html` to find where custom head/footer partials are injected. Common patterns:
- `layouts/partials/hooks/head-end.html`
- `layouts/partials/custom-head.html`
- `layouts/partials/extend_head.html`

If none of these hooks exist, add the script tags directly by overriding `baseof.html`:
1. Copy `themes/hugoplate/layouts/_default/baseof.html` to `layouts/_default/baseof.html`
2. Add the Identity widget `<script>` tag before `</head>`
3. Add the footer script before `</body>`

---

## Phase 8: Post-Setup Manual Steps (NOT for Claude Code)

These steps are done by the human in the browser after the code is pushed:

### 8.1 Netlify Setup
1. Log into Netlify with the doctor's GitHub account
2. Import the repo → set build command to `npm run build`, publish dir to `public`
3. Set environment variable `HUGO_VERSION` = latest
4. Add custom domain in Domain Management
5. Configure DNS at the registrar (point nameservers to Netlify OR add ALIAS/CNAME records)
6. Enable HTTPS (auto-provisions via Let's Encrypt)

### 8.2 Netlify Identity
1. Site settings → Identity → Enable Identity
2. Set Registration to **Invite only**
3. Services → Git Gateway → Enable Git Gateway
4. Identity tab → Invite users → enter the doctor's email
5. Doctor receives email, sets password, can now access `/admin/`

### 8.3 Verify CMS Access
1. Go to `[domain]/admin/`
2. Log in with the doctor's email/password
3. Create a test blog post → Publish → verify it appears on site
4. Add a course → Publish → verify it appears on `/courses/`
5. Edit the About page → Publish → verify changes

---

## Placeholder Values to Replace

Search the codebase for these placeholders and replace them with real values:

| Placeholder | Replace with |
|---|---|
| `DOMAIN_PLACEHOLDER.com` | The actual domain |
| `Dr [NAME]` | Doctor's full name with title |
| `[NAME]` | Doctor's surname or full name |
| `[X] years` | Actual years of experience |
| `EMAIL_PLACEHOLDER` | Doctor's email address |
| `LINKEDIN_PLACEHOLDER` | LinkedIn profile slug |
| `USERNAME.github.io/course-repo-name/` | Actual GitHub Pages course URLs |

---

## File Structure (Final)

```
repo-root/
├── hugo.toml
├── netlify.toml
├── package.json
├── config/
│   └── _default/
│       ├── params.toml
│       ├── menus.en.toml
│       └── ...
├── content/
│   └── english/
│       ├── _index.md              ← Homepage with hero/profile
│       ├── about.md               ← About page
│       ├── blog/
│       │   ├── _index.md          ← Blog listing
│       │   └── welcome.md         ← First blog post
│       └── courses/
│           ├── _index.md          ← Courses listing
│           ├── course-one.md      ← Individual course entry
│           └── course-two.md
├── layouts/
│   ├── courses/
│   │   ├── list.html              ← Custom courses grid
│   │   └── single.html            ← Course detail page
│   ├── partials/
│   │   └── hero-profile.html      ← Custom profile hero (if needed)
│   └── _default/
│       └── baseof.html            ← Override for Identity widget (if no hook)
├── static/
│   ├── images/
│   │   └── profile.png            ← Doctor's photo
│   └── admin/
│       ├── index.html             ← Decap CMS entry point
│       └── config.yml             ← Decap CMS configuration
├── assets/
│   └── images/
│       └── profile.png            ← Source image for Hugo processing
├── data/
│   ├── theme.json                 ← Colour scheme & fonts
│   └── social.json                ← Social links
└── themes/
    └── hugoplate/                 ← Theme (do not modify directly)
```

---

## Testing Checklist

Before pushing to production, verify:

- [ ] Homepage loads with profile photo, name, bio, and CTA button
- [ ] Navigation shows: Home, Courses, Blog, About
- [ ] `/courses/` shows course cards with links to external GitHub Pages sites
- [ ] Course links open in a new tab and go to the correct GitHub Pages URL
- [ ] `/blog/` shows blog listing (at least the welcome post)
- [ ] `/about/` shows the full biography
- [ ] Site looks good on mobile (responsive)
- [ ] Colour scheme feels professional and medical
- [ ] `static/admin/index.html` exists and loads Decap CMS
- [ ] `static/admin/config.yml` has correct content paths
- [ ] `netlify.toml` has correct build configuration
- [ ] Dark mode works and looks acceptable
- [ ] No leftover Hugoplate demo content (example posts, placeholder images)
- [ ] All `PLACEHOLDER` values have been replaced with real content
- [ ] `baseURL` in `hugo.toml` matches the actual domain

---

## Design Principles

- **Trustworthy:** Muted colour palette, clean typography, generous whitespace
- **Professional:** No flashy animations, no stock illustrations, no gimmicks
- **Content-first:** The doctor's credentials and courses should be immediately visible
- **Accessible:** Good contrast ratios, readable font sizes, proper heading hierarchy
- **Fast:** Minimal custom JS, optimised images, leverage Hugo's static generation
