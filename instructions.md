# Solvendis Blog Workflow (GitHub + Markdown)

This blog is **runtime-fetched** by the site. That means the website does **not** rebuild when you post. You only update files in a GitHub repo, and the site pulls new posts automatically.

This file is designed for an AI coding agent to follow using only:
- This instruction file
- Two article texts (English + Spanish)

The agent must create the necessary files in the blog repo and update the index.

---

## Repository Structure (required)
Example repo: `solvendis-blog`

```
solvendis-blog/
  index.json
  posts/
    2026-02-07-ai-news-roundup.en.md
    2026-02-07-ai-news-roundup.es.md
  images/
    2026-02-07-cover.jpg
```

Notes:
- `index.json` lists **all** posts.
- `posts/` contains Markdown files.
- `images/` contains cover images or inline images (optional).

---

## Language Support (important)
The website filters posts by language toggle:
- English toggle shows `lang: "en"`
- Spanish toggle shows `lang: "es"`

Therefore, every entry in `index.json` must include `lang`.

---

## `index.json` Format (required)
This file is what the site reads to list posts and locate the Markdown.

```json
[
  {
    "slug": "2026-02-07-ai-news-roundup",
    "title": "AI News Roundup – Feb 7, 2026",
    "date": "2026-02-07",
    "lang": "en",
    "summary": "Key AI launches, model updates, and funding news.",
    "coverImage": "https://raw.githubusercontent.com/yourname/solvendis-blog/main/images/2026-02-07-cover.jpg",
    "contentUrl": "https://raw.githubusercontent.com/yourname/solvendis-blog/main/posts/2026-02-07-ai-news-roundup.en.md",
    "baseUrl": "https://raw.githubusercontent.com/yourname/solvendis-blog/main/"
  },
  {
    "slug": "2026-02-07-ai-news-roundup",
    "title": "Resumen de Noticias de IA – 7 Feb 2026",
    "date": "2026-02-07",
    "lang": "es",
    "summary": "Lanzamientos clave, actualizaciones de modelos y noticias de inversión.",
    "coverImage": "https://raw.githubusercontent.com/yourname/solvendis-blog/main/images/2026-02-07-cover.jpg",
    "contentUrl": "https://raw.githubusercontent.com/yourname/solvendis-blog/main/posts/2026-02-07-ai-news-roundup.es.md",
    "baseUrl": "https://raw.githubusercontent.com/yourname/solvendis-blog/main/"
  }
]
```

### Field requirements
- `slug`: URL slug used at `/blog/<slug>` (same slug can be reused for EN/ES pair).
- `title`: Post title in the given language.
- `date`: `YYYY-MM-DD`.
- `lang`: `"en"` or `"es"`.
- `summary`: 1–2 sentence summary in the given language.
- `coverImage`: Absolute URL to cover image (recommended).
- `contentUrl`: Absolute URL to the raw Markdown file.
- `baseUrl`: Optional. Allows relative image paths in Markdown.

### Ordering
Keep `index.json` sorted by `date` descending. Most recent posts first.

---

## Markdown File Format
Each post is a **plain Markdown file** with a single language.

Example `posts/2026-02-07-ai-news-roundup.en.md`:
```
# AI News Roundup – Feb 7, 2026

Here are today’s highlights.

## 1) OpenAI releases new tool
- What changed
- Why it matters

![Chart](images/2026-02-07-chart.jpg)

Read more: [OpenAI blog](https://openai.com/)
```

### Image paths
- If you use `images/...` in Markdown, ensure `baseUrl` is set in `index.json` to the repo root.
- Otherwise, use absolute image URLs.

---

## How to Publish a New Daily Post (Agent Checklist)
Given two article texts (English + Spanish), the agent must:

1. **Create two Markdown files**:
   - `posts/<slug>.en.md` with the English article
   - `posts/<slug>.es.md` with the Spanish article

2. **Add/Update images** (optional):
   - Upload any cover/inline images to `images/`

3. **Update `index.json`**:
   - Add two entries (one `lang: "en"`, one `lang: "es"`)
   - Use the same `slug` for both
   - Set `contentUrl` for each language
   - Point `coverImage` to the uploaded image
   - Keep the list sorted by date (newest first)

4. **Commit and push** to GitHub

No Netlify rebuild required. The site will display the new post immediately.

---

## Environment Variable (site config)
The website reads the index from this variable:

```
VITE_BLOG_INDEX_URL=https://raw.githubusercontent.com/yourname/solvendis-blog/main/index.json
```

Where to set it:
- Local dev: `.env`
- Netlify: Site settings → Environment variables
