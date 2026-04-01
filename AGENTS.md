## Working Rule

Decompose from first principles. Optimize for minimal, precise edits. Avoid structural churn unless required.

## Repository Context

This repository is documentation-only and is built with MkDocs.

Primary sources of truth:

- `docs/` contains the documentation pages.
- `mkdocs.yml` contains site configuration, theme settings, plugins, and any explicit navigation if present.
- `README.md` explains the repo purpose and local usage.

There are no spec folders, generated plans, or code-driven contracts to reconcile against. The documentation itself is the system.

## Required Context

Before making any non-trivial change, read:

- `mkdocs.yml` to understand site structure, enabled plugins, and whether navigation is explicit or implicit.
- The relevant file or files under `docs/`.
- `README.md` to understand repo intent and audience.

When editing an existing page, read nearby or related pages first so tone, structure, terminology, and depth remain consistent.

## Content Model

Treat the documentation as the product surface:

- Pages define concepts, workflows, and conventions.
- `mkdocs.yml` defines site behavior and may define canonical navigation.
- Links between pages define relationships across topics.

If structure and content disagree:

- fix structure when the information is correct but discoverability is wrong
- fix content when the meaning is wrong

If `mkdocs.yml` does not define `nav`, treat the current file layout and in-page links as the effective navigation model until an explicit nav is introduced.

## Editing Rules

- Prefer editing existing pages over creating new ones.
- Keep pages scoped and atomic.
- Avoid duplication; link to related material instead of repeating it.
- Keep explanations concrete, operational, and specific to actual repo usage.
- Preserve existing terminology unless there is a clear inconsistency to resolve.
- Keep examples and snippets aligned with current documented workflows.

## When To Create New Pages

Create a new page only if:

- the concept does not fit cleanly into an existing page
- adding it would otherwise overload an existing page
- the topic is reusable across multiple parts of the documentation

When adding a new page:

- place it in the correct location under `docs/`
- register it in `mkdocs.yml` if explicit navigation exists or if adding explicit navigation is part of the fix
- add links from related pages when discoverability would otherwise be weak

## Navigation Discipline

- Treat `mkdocs.yml` as canonical when explicit `nav` exists.
- Otherwise, preserve a simple and predictable file layout under `docs/`.
- Group content by reader mental model, not by internal ownership.
- Avoid orphan pages; each page should be reachable from navigation, an index page, or related-page links.

## Consistency Constraints

- Terminology must stay stable across pages.
- Similar playbooks should use similar section structure where practical.
- Do not introduce new naming without updating existing references.
- Cross-links and asset paths must remain valid after edits.

## Change Scope

- Make the smallest change that resolves the issue.
- Do not refactor large sections unless the task requires it.
- Preserve the working documentation structure unless there is a clear structural flaw.

## Review Expectations

For every non-trivial documentation change, verify:

- internal links still resolve
- asset references still resolve
- navigation remains coherent
- no duplicate or conflicting explanations were introduced
- Markdown renders correctly under MkDocs

If a change affects structure or navigation, verify both the edited page and the adjacent discovery path a reader would use.

## External Knowledge

When documenting external systems, frameworks, tools, or services:

- prefer official documentation
- verify current behavior before documenting it
- avoid speculative, stale, or inferred patterns unless clearly marked

## Repo-Local Skills

Prefer these repo-local skills when applicable:

- `.agents/skills/docs-writer/` for drafting or restructuring documentation pages in the style of existing docs
- `.agents/skills/docs-reviewer/` for reviewing documentation pages and `README.md` for clarity, consistency, structure, and MkDocs fit
