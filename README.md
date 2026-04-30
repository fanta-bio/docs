# fanta.bio Documentation

Source for [docs.fanta.bio](https://docs.fanta.bio), built with [Hugo](https://gohugo.io/) and the [Hextra](https://imfing.github.io/hextra/) theme.

## Prerequisites

- **Git** with submodule support
- **Hugo extended**, v0.132 or newer ([install](https://gohugo.io/installation/))

## Setup

The Hextra theme is pulled in as a Git submodule, so the clone needs `--recurse-submodules` (or `git submodule update --init` after the fact):

```bash
git clone --recurse-submodules https://github.com/fanta-bio/docs.git fanta-bio-docs
cd fanta-bio-docs
```

If you forgot the flag:

```bash
git submodule update --init
```

## Local development

```bash
hugo server -p 8080
```

Open <http://localhost:8080/>. Live reload is on by default — edits to anything under `content/`, `static/`, `hugo.yaml`, or the theme show up in the browser within a second or two.

## Content layout

```
content/
├── _index.md      ← homepage with section cards
├── about.md       ← what fanta.bio is + how to cite
├── website/       ← guide to the fanta.bio web interface
├── api/           ← REST API at api.fanta.bio
└── mcp/           ← MCP server at mcp.fanta.bio/mcp (for AI assistants)
```

The `about.md` content here is kept in sync with `data/pages/about.md` in the [`fanta-bio-web`](https://github.com/fanta-bio/fanta-bio-web) repo, which is the source of truth.

## Updating the theme

```bash
cd themes/hextra
git fetch --tags
git checkout vX.Y.Z   # latest release
cd ../..
git add themes/hextra
git commit -m "chore(theme): bump hextra to vX.Y.Z"
```

## Deployment

The site builds to `public/` and deploys to Cloudflare Workers Static Assets. The `static/_redirects` file handles edge-level redirects (e.g. legacy `/v1.1/` → `/website/`).

Local Cloudflare deploy config (`wrangler.toml`) is gitignored — set up your own if you need to deploy from a fork.

## License

[MIT](LICENSE)
