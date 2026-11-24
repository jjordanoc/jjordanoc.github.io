# Editing Guide

Quick reference for updating your personal website (`jjordanoc.github.io`).

## Where to Edit Content

| I want to change...                               | File                              | Notes                                                                                          |
| ------------------------------------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------- |
| Bio, "What I'm working on", "What I've worked on" | `_pages/about.md`                 | Edit the Markdown lists directly; add/update `[GitHub](url)` links inline.                     |
| Profile photo                                     | `_pages/about.md` + `assets/img/` | Drop the image into `assets/img/` and set `profile.image` in `_pages/about.md`.                |
| Name, site title, description, URL                | `_config.yml`                     | Also controls `scholar` author highlighting and excluded files.                                |
| Social icons (email, GitHub, LinkedIn, Scholar)   | `_data/socials.yml`               | Add e.g. `scholar_userid:` or `x_username:` when ready.                                        |
| News / announcements                              | `_news/*.md`                      | Add a new `.md` file with front matter (template below).                                       |
| Publications                                      | `_bibliography/papers.bib`        | Add `selected = {true}` to feature a paper on the home page.                                   |
| Projects page                                     | `_projects/*.md`                  | One file per project; categories are `[current, past, side projects]` in `_pages/projects.md`. |
| CV page                                           | `_data/cv.yml`                    | Uses RenderCV YAML format (`_pages/cv.md` sets `cv_format: rendercv` and optional `cv_pdf:`).  |
| Show or hide top navigation pages                 | `_pages/*.md`                     | Set `nav: true` or `nav: false` and `nav_order:` in each page's front matter.                  |
| GitHub profile / repository cards                 | `_data/repositories.yml`          | Uncomment repos once they are public (private repos render broken cards).                      |

---

## Common Tasks

### 1. Changing Your Profile Photo

Other candidate photos are still in `joaquin_raw_migrate/` (`photo_lake.jpg`, `photo_sf.JPG`).

- For `.jpg`/`.png` files: copy into `assets/img/` and set `profile.image` in `_pages/about.md`.
- For iPhone `.HEIC` files, convert and resize on macOS first:

```bash
sips -s format jpeg -Z 1200 joaquin_raw_migrate/your_photo.HEIC --out assets/img/prof_pic_new.jpg
```

Then update `image: prof_pic_new.jpg` in `_pages/about.md`.

### 2. Adding a News Item

Create `_news/YYYY-MM-DD-short-slug.md`:

```markdown
---
layout: post
date: 2026-12-01 09:00:00-0500
inline: true
related_posts: false
---

Your one-line announcement with [a link](https://example.com).
```

### 3. Adding a Project

Create `_projects/my-project.md`:

```markdown
---
layout: page
title: Project Title
description: One-line summary shown on the card.
importance: 1
category: current # one of: current, past, side projects
---

Longer description, results, and links go here.
```

---

## Local Preview

### Option A: Ruby / Bundler (macOS / Linux)

```bash
bundle install
bundle exec jekyll serve
```

Then open <http://localhost:4000/>.

### Option B: Docker

```bash
docker compose up
```

Then open <http://localhost:8080/>. (Ensure Docker Desktop is running first.)

---

## Formatting Before Committing

This repo uses Prettier (including `@shopify/prettier-plugin-liquid`). Format any files you edit before pushing:

```bash
npx prettier --write _pages/about.md _data/cv.yml
```

Check everything with:

```bash
npm run lint:prettier
npm run lint:style-contract
```

---

## Deploying to GitHub Pages

1. Push your commits to the `main` (or `master`) branch.
2. `.github/workflows/deploy.yml` builds the site and pushes the output to the `gh-pages` branch.
3. In your GitHub repo:
   - Go to **Settings -> Pages** and set the source branch to **`gh-pages`** (root `/`), **not** `main` (GitHub's default `pages build and deployment` job on `main` uses legacy Jekyll 3.10 and will fail on `al_folio_core`).
   - Go to **Settings -> Actions -> General -> Workflow permissions** and select **Read and write permissions** so the deploy action can push to `gh-pages`.
