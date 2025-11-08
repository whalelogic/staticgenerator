# Go Static Blog Generator

A lightweight **static site generator** written in Go.  
It reads blog posts from a JSON file, converts Markdown content into HTML, and generates a full static website — including an index page and one HTML file per post — using Go’s `html/template`.

---

## 🧩 Features

- 📄 **Generates static HTML** from JSON blog data  
- 🧠 **Markdown → HTML** conversion with [goldmark](https://github.com/yuin/goldmark)  
- 🧰 **Reusable Go templates** (`base`, `list`, `post`)  
- 🗂️ **Automatic directory structure** (`/public` for build output)  
- 🕒 **Build timestamp injection** into every page (`GeneratedAt`)  
- ⚡ **Fast, dependency-light**, and easily extendable for APIs, Markdown, or databases

---

## 🏗️ Directory Structure

.
├── main.go
├── posts.json
├── templates/
│ ├── base.tmpl
│ ├── list.tmpl
│ └── post.tmpl
└── public/
├── index.html
└── using-finance-apis.html


---

## 🚀 Usage

### 1. Install Dependencies

Make sure you have Go 1.22+ installed.

```bash
go mod init staticgen
go get github.com/yuin/goldmark
```

### 2. Add Templates and JSON

Put your JSON blog posts in posts.json and the templates in /templates (see examples in this repo).

### 3. Build the Site

Run the generator:

```go
go run main.go
```

This creates a /public/ directory containing:

    index.html — the blog homepage

    slug.html — one page per post

### 4. Preview

You can serve it locally using Go’s built-in HTTP server:
```go 

go run main.go serve

```

Then open: ```http://localhost:8080```
### 💻 Example JSON

```json
[
  {
    "id": "14",
    "slug": "using-finance-apis",
    "title": "Using Finance APIs to Build Smart Financial Tools",
    "subtitle": "Using Finance APIs to Build Smart Financial Tools",
    "author": "Keith Thomson",
    "content": "## Secure your API keys in environment variables\n\nValidate responses and handle exceptions gracefully.\n\n## Conclusion\n\nFinance APIs unlock powerful capabilities for developers.",
    "summary": "Build financial dashboards using real-time market APIs.",
    "read_time": "6 min read",
    "tags": "finance,api,stock market",
    "category": "General",
    "created_on": "2025-04-13 01:30:33"
  }
]
```

### ⚙️ How It Works

    Load JSON from posts.json

    Parse each record into a Post struct

    Convert the Content field (Markdown) into HTML using goldmark

    Execute templates using Go’s html/template

    Write output files into /public/


## 🧩 Extending the Application

This project is intentionally modular and minimal — you can extend it in many directions:
1. More Content Formats

    🔹 Add YAML or Markdown readers in addition to JSON

    🔹 Generate pages from CSV, TOML, or direct DB queries

    🔹 Add data/ directory scanning for structured content (like Hugo)

2. Add Front Matter Support

Parse front matter blocks (---) in Markdown files so users can define metadata directly in content files.
3. RSS / Atom Feed

Generate an RSS feed using XML templates:

t := template.Must(template.New("rss").Parse(rssTemplate))
t.Execute(out, posts)

4. Integrated HTTP Server

Serve your generated pages live with hot reload:

    Watch JSON and template files using fsnotify

    Rebuild automatically when changes occur

    Serve /public dynamically via Go’s net/http

5. Theming System

Allow custom templates/ directories per theme:

themes/minimal/templates/
themes/modern/templates/

6. CLI Interface

Add subcommands:

staticgen build
staticgen serve
staticgen new "My New Post"

Use cobra or urfave/cli to handle command parsing.
7. Plugin System

Load custom data sources or processors using Go interfaces:

type Source interface { Load() ([]Post, error) }
type Renderer interface { Render(Post) (string, error) }

8. API Integration

Fetch remote posts or sync with a CMS (e.g., Ghost, Notion, or WordPress REST APIs).
9. Search Index

Generate a JSON search index for use with a client-side search library like Fuse.js

.
10. Deploy Automation

Add GitHub Actions or a Makefile target to:

    Build static site

    Deploy to GitHub Pages, Netlify, or AWS S3/CloudFront

## 🧠 Design Philosophy

This project embraces Go’s simplicity:

    Build static HTML directly with templates (no JS framework needed)

    Use plain data structures for content

    Make everything composable — you can swap out the JSON loader, template engine, or Markdown renderer easily.

## 🪴 Future Vision

Incremental rebuilds for changed posts

Tag and category index pages

Image optimization and metadata injection

Auto summary and reading time calculation

Support for .mdx or .adoc content formats

    Multi-language output and localization

## 🧑‍💻 Author

Keith Thomson
Army Veteran, Software Engineer, and API Developer.
Website: https://whalelogic.io
📜 License

MIT License — use it freely for personal or commercial projects.


