# Blog Standards & Agent Guide

This repository is the canonical source for Hari's blog posts.  
Any AI agent (Claude, OpenClaw, DeepSeek, or any other) can create, edit, and publish posts by following this document exactly.

---

## Quick Start (for agents)

1. Create folder `posts/YYYY-MM-DD-slug/`
2. Copy `templates/post.html` → `posts/YYYY-MM-DD-slug/index.html`, fill placeholders
3. Copy `templates/meta.json` → `posts/YYYY-MM-DD-slug/meta.json`, fill all fields
4. Prepend entry to `posts/index.json` (newest first)
5. Commit all three files to `main` in one commit
6. Wait ~90 seconds for GitHub Pages to deploy
7. Import into Medium: paste `https://ylnhari.github.io/blog/posts/YYYY-MM-DD-slug/` at `https://medium.com/p/import`
8. After publishing, update `meta.json` → set `status: "published"` and `medium_url`

---

## Folder Structure

```
blog/
├── STANDARDS.md                        ← this file (do not modify)
├── index.html                          ← blog homepage (auto-generated from posts/index.json)
├── assets/
│   └── style.css                       ← shared stylesheet (do not modify per-post)
├── templates/
│   ├── post.html                       ← copy for every new post
│   └── meta.json                       ← copy for every new post
└── posts/
    ├── index.json                      ← master registry (update for every post)
    └── YYYY-MM-DD-slug/
        ├── index.html                  ← post HTML content
        └── meta.json                   ← post metadata
```

---

## Naming Convention

| Item | Format | Example |
|------|--------|---------|
| Folder | `posts/YYYY-MM-DD-slug/` | `posts/2026-06-22-ai-assisted-blogging/` |
| Date | ISO 8601 (`YYYY-MM-DD`) | `2026-06-22` |
| Slug | lowercase, hyphens only, no special chars, max 60 chars | `ai-assisted-blogging` |
| HTML file | always `index.html` | fixed — never vary |
| Meta file | always `meta.json` | fixed — never vary |

---

## meta.json Schema

```json
{
  "title": "Human-readable post title",
  "date": "YYYY-MM-DD",
  "author": "ylnhari",
  "description": "1–2 sentence summary for SEO and blog index card. Max 200 chars.",
  "tags": ["tag1", "tag2"],
  "status": "draft",
  "medium_url": null,
  "cover_image": "https://images.unsplash.com/photo-XXXXXXX?w=1200&q=80"
}
```

Field rules:
- `status`: must be `"draft"` or `"published"` — no other values
- `medium_url`: `null` until published; then the full `https://medium.com/...` URL
- `cover_image`: must be an absolute HTTPS URL; cannot be a relative path
- `tags`: 2–5 tags, lowercase preferred

---

## posts/index.json Schema

Maintain as a JSON array, newest post first:

```json
[
  {
    "slug": "2026-06-22-ai-assisted-blogging",
    "title": "AI-Assisted Blogging: A Technical Deep Dive",
    "date": "2026-06-22",
    "description": "How to automate blog creation with AI agents and GitHub Pages.",
    "tags": ["AI", "automation", "blogging"],
    "cover_image": "https://images.unsplash.com/photo-1519389950473-47ba0277781c?w=1200&q=80",
    "status": "published",
    "medium_url": null
  }
]
```

When adding a new post, prepend its entry. Do not re-sort existing entries.

---

## HTML Post Structure

Every `index.html` MUST follow this exact outer structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="author" content="ylnhari">
  <meta name="description" content="POST_DESCRIPTION">
  <meta property="og:title" content="POST_TITLE">
  <meta property="og:description" content="POST_DESCRIPTION">
  <meta property="og:image" content="COVER_IMAGE_URL">
  <title>POST_TITLE</title>
  <link rel="stylesheet" href="../../assets/style.css">
</head>
<body>
<article>

<h1>POST_TITLE</h1>
<p class="byline">By ylnhari · POST_DATE · POST_TAGS</p>

<!-- === POST CONTENT STARTS HERE === -->



<!-- === POST CONTENT ENDS HERE === -->

<hr>
<p class="footer-note"><em>Published via automated pipeline · <a href="https://ylnhari.github.io/blog">ylnhari.github.io/blog</a></em></p>

</article>
</body>
</html>
```

Placeholders to replace:
- `POST_TITLE` — exact title string, no HTML entities needed for basic ASCII
- `POST_DESCRIPTION` — same as `meta.json` description field
- `POST_DATE` — human-readable date, e.g. `June 22, 2026`
- `POST_TAGS` — comma-separated, e.g. `AI, automation`
- `COVER_IMAGE_URL` — same as `meta.json` cover_image field

---

## Supported HTML Elements

| Tag | Medium mapping | Usage |
|-----|---------------|-------|
| `<h1>` | Title | Once only, at the top |
| `<h2>` | Section heading | Major sections |
| `<h3>` | Sub-heading | Sub-sections, use sparingly |
| `<p>` | Paragraph | All body text |
| `<strong>` | **Bold** | Key terms, emphasis |
| `<em>` | *Italic* | Titles, foreign words, soft emphasis |
| `<a href="...">` | Hyperlink | Must be absolute URLs |
| `<img src="..." alt="...">` | Image | Must be absolute HTTPS URL |
| `<blockquote>` | Pull quote | Quotes, important callouts |
| `<pre><code>` | Code block | Multi-line code |
| `<code>` | Inline code | Short code references in text |
| `<ul><li>` | Bullet list | Unordered list |
| `<ol><li>` | Numbered list | Ordered list |
| `<hr>` | Separator (· · ·) | Section dividers, use max 2 per post |

---

## NEVER Use (breaks Medium import or causes save errors)

- `<script>` or any JavaScript
- `<style>` tags or `style="..."` inline attributes
- `<iframe>` (stripped by Medium)
- `<table>`, `<tr>`, `<td>` (Medium has no table support)
- `<video>`, `<audio>` (not supported)
- `<div>`, `<span>` with styling
- H4, H5, H6 (Medium only supports H1–H3)
- Relative image URLs (Medium's importer cannot resolve them)
- Self-referencing anchor links (`<a href="#section">`)

---

## Images

Must be absolute HTTPS URLs. Recommended free sources:

```html
<!-- Unsplash (free, high quality) -->
<img src="https://images.unsplash.com/photo-{PHOTO_ID}?w=1200&q=80" alt="Description">

<!-- Wikimedia Commons -->
<img src="https://upload.wikimedia.org/wikipedia/commons/path/to/file.jpg" alt="Description">

<!-- Your own assets in this repo (raw GitHub URL) -->
<img src="https://raw.githubusercontent.com/ylnhari/blog/main/posts/{SLUG}/images/filename.png" alt="Description">
```

To store images in the repo: place them in `posts/YYYY-MM-DD-slug/images/`.

---

## YouTube / Video Embeds

Medium doesn't support `<video>` or `<iframe>`. Workaround for YouTube:

```html
<!-- Put the YouTube URL as plain text in a paragraph -->
<p>https://www.youtube.com/watch?v=VIDEO_ID</p>
```

After importing to Medium, click the URL in the editor → Medium auto-converts it to a video embed.

---

## GitHub Pages URL Pattern

```
https://ylnhari.github.io/blog/posts/{YYYY-MM-DD-slug}/
```

This is the URL to paste into `https://medium.com/p/import`.

Build time after a commit: typically 60–120 seconds. Check build status at:
`https://github.com/ylnhari/blog/actions`

---

## Agent Checklist (copy this when creating a post)

```
[ ] Folder created:       posts/YYYY-MM-DD-slug/
[ ] index.html created:   copied from templates/post.html, all placeholders replaced
[ ] meta.json created:    all fields filled, cover_image is absolute HTTPS URL
[ ] posts/index.json:     new entry prepended (newest first)
[ ] No JavaScript in HTML
[ ] No inline CSS in HTML
[ ] No relative image URLs
[ ] All <img src="..."> use absolute HTTPS URLs
[ ] Committed to main branch
[ ] GitHub Actions build passed (check /actions)
[ ] Medium import tested at medium.com/p/import
[ ] (After publish) meta.json status → "published", medium_url filled
```

---

## Error Recovery

If Medium shows "not saved properly" during browser automation:
1. Refresh the page — Medium auto-restores the last saved state
2. Resume from the last successfully saved element
3. Do NOT use: `execCommand('insertParagraph')`, `innerHTML =`, `execCommand('selectAll')`
4. DO use: `document.execCommand('insertText', false, text)` for typing; CDP `Input.dispatchKeyEvent` for Enter key

---

## Commit Message Convention

```
feat(posts): add "POST_TITLE" [YYYY-MM-DD]
fix(posts): update "POST_TITLE" meta/content
chore: update posts/index.json
```
