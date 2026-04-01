# MITOS Portfolio

This repository contains the MITOS portfolio documentation site. It is a MkDocs project that organizes past MITOS projects, writeups, posts, descriptions, and use cases into a searchable documentation website.

## Local Setup

This project uses:

- Python `3.11.7`
- Poetry for dependency management and local commands

If you use `pyenv`, the repository already pins the Python version in [`.python-version`](/Users/yucheng/Documents/Projects/portfolio/.python-version).

Install dependencies with Poetry:

```bash
poetry install
```

## Local Development

Run the local MkDocs development server with:

```bash
poetry run mkdocs serve
```

By default, the site will be available at `http://127.0.0.1:8000/`.

## Build

Build the static site locally with:

```bash
poetry run mkdocs build
```

## Deploy

Deployment is handled by GitHub Actions on pushes to `main`. Best practice: create PR to merge to Main, on merging, the workflow runs:

```bash
poetry install
poetry run mkdocs gh-deploy --force
```
