# LeetCode Wiki

Jekyll-based static site (Just the Docs theme) for a collaborative LeetCode problem-solving wiki.

**Live site:** https://rutvij26.github.io/leetcode-wiki

## Commands

```bash
bundle install              # Install dependencies
bundle exec jekyll serve    # Serve locally with auto-rebuild (http://localhost:4000)
bundle exec jekyll build    # Build static site to _site/
```

## Project Structure

```
topics/             # All content lives here
  <topic-name>/
    README.md       # Topic theory page
    <problem>.md    # Individual problem pages
index.md            # Homepage
getting-started.md  # User guide
_config.yml         # Jekyll + Just the Docs configuration
Gemfile             # Ruby gem dependencies
```

## Content Conventions

### Adding a New Topic
1. Create `topics/<topic-name>/README.md`
2. Add the topic to `topics/README.md` index table
3. Set Jekyll front matter:
   ```yaml
   ---
   title: <Topic Name>
   nav_order: <number>
   has_children: true
   permalink: /topics/<topic-name>
   ---
   ```

### Adding a Problem Page
Create `topics/<topic-name>/<problem-slug>.md` with:
```yaml
---
title: "<LeetCode Number>. Problem Title"
parent: <Parent Topic Title>
nav_order: <number>
---
```

### Front Matter Fields
- `title` — page title shown in nav and heading
- `nav_order` — ordering within parent (topics use 1, 2, 3...; problems use problem number)
- `parent` — must exactly match the parent topic's `title`
- `has_children: true` — required on topic index pages
- `permalink` — canonical URL for topic index pages

## Tech Stack
- **Jekyll** ~> 4.0
- **Just the Docs** theme ~> 0.4.0 (dark color scheme)
- **Markdown renderer:** kramdown
- **Plugins:** jekyll-seo-tag, jekyll-sitemap
- **Deployment:** GitHub Pages
