# Meta-files Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Correct and rewrite the repository meta files (agent files, `CONTRIBUTING.md`, `README.md`) and adjust the `docs.json` navigation so the help center has an accurate, well-defined foundation before the content sweep.

**Architecture:** `AGENTS.md` becomes the single canonical agent guide; `CLAUDE.md` and `GEMINI.md` become short pointers to it. `CONTRIBUTING.md` and `README.md` are rewritten from template boilerplate into project-specific content. The `docs.json` navigation drops the standalone reports page and adds a "Teaching & Non-teaching" staff page.

**Tech Stack:** Mintlify, MDX, JSON (`docs.json`). Verification uses the Mintlify CLI (`mint`).

**Reference:** Design spec at `superpowers/specs/2026-05-19-meta-docs-design.md`.

---

## File Structure

- `AGENTS.md` — canonical agent guide (rewritten in full).
- `CLAUDE.md` — pointer to `AGENTS.md` (replaced).
- `GEMINI.md` — pointer to `AGENTS.md` (replaced).
- `CONTRIBUTING.md` — contributor guide with docs structure and sync workflow (rewritten in full).
- `README.md` — short repo description (rewritten in full).
- `docs.json` — navigation page list edited.
- `staff/overview.mdx` — new stub page.
- `reports/overview.mdx` — deleted.

No `.mdx` page links to `reports/overview`; only `docs.json` references it, so its removal breaks no internal links.

---

## Task 1: Rewrite AGENTS.md as the canonical agent guide

**Files:**
- Modify: `AGENTS.md` (full replacement)

- [ ] **Step 1: Overwrite `AGENTS.md` with the full content below**

````markdown
# Elimu Bora ERP Help Center — Agent Guide

This repository is the **public help center** for Elimu Bora ERP, authored in MDX and rendered with [Mintlify](https://mintlify.com). It is deployed at `https://help.elimuboraerp.com`.

The audience is **non-technical school staff** who operate the Elimu Bora tenant panel at `<school>.elimuboraerp.com` — principals, deputies, teachers, bursars, registrars, HR, and **guardians** (who log in with read-only, scoped access). Content is task-oriented: it explains how to get things done in the system. This is **not** developer documentation.

## Source of truth and verification

IMPORTANT: Use retrieval-led reasoning over training-led reasoning for this project. Prefer the source docs, the Elimu Bora codebase, and the Mintlify Skill/MCPs over model memory or generic documentation knowledge.

The canonical product reference is the developer-docs folder in the product repository at `~/Code/elimubora/docs/`:

- `project-overview.md` — what the platform is, tech stack, module list
- `conventions.md` — multitenancy, module gating, authorization, panel structure
- `modules/*.md` — one developer doc per product module

Before writing or revising any help page:

1. Read the relevant source docs above.
2. Each source doc carries a `Last verified: <sha> on <date>` header. If that SHA looks stale against recent product commits, crosscheck the codebase at `~/Code/elimubora` directly — the running code is the ultimate authority.
3. Reuse documentation patterns already established in this repository.

## When to use the Mintlify Skill

The agent MUST use the Mintlify Skill whenever working on:
- Mintlify components and layouts
- `docs.json` navigation/sidebar structure
- OpenAPI/API documentation formatting
- Tabs, Accordions, Cards, Frames, Callouts, Steps, Tooltips, and other Mintlify UI components
- MDX syntax and formatting
- Troubleshooting broken Mintlify pages/builds
- Search, SEO, metadata, anchors, pagination, and navigation behavior
- Interactive documentation patterns
- Writing docs that should follow current Mintlify best practices
- Verifying whether a Mintlify component or syntax is deprecated or has changed
- Any uncertainty about Mintlify capabilities or schema behavior

## When to use the Mintlify Docs MCP

The agent SHOULD use the Mintlify Docs MCP when:
- Looking up the latest Mintlify documentation
- Verifying component APIs or syntax
- Checking current best practices
- Confirming supported features before generating docs
- Researching examples of advanced Mintlify implementations

## When to use the Mintlify Dashboard MCP

The agent SHOULD use the Mintlify Dashboard MCP when:
- Working with deployment issues
- Investigating indexing/search problems
- Managing documentation configuration
- Debugging dashboard-specific behavior
- Reviewing project-level Mintlify settings

## Documentation workflow expectations

Before creating or modifying documentation:
1. Verify the facts against `~/Code/elimubora/docs/` and, when in doubt, the codebase at `~/Code/elimubora`.
2. Verify the documentation approach against Mintlify Skill/MCP guidance if the task involves Mintlify features or structure.
3. Prefer official Mintlify patterns over improvised MDX structures.
4. Follow the structure and page conventions in `CONTRIBUTING.md`.

## Agent behavior rules

- Never invent Mintlify components or props.
- Never assume deprecated syntax still works.
- Never document product behavior you have not verified against the source docs or code.
- Prefer composable Mintlify components over large raw markdown blocks.
- Prefer shallow, readable page structures over deeply nested sections.
- Keep documentation visually scannable and optimized for non-technical school staff.
- If unsure about a Mintlify feature, consult the Mintlify Skill or Docs MCP before writing content.

## Tech stack

**This repository:**
- Mintlify — documentation rendering and hosting
- MDX — page authoring

**The product being documented (`~/Code/elimubora`):**
- PHP 8.4, Laravel 12, Filament 4, Livewire 3, Tailwind CSS 4
- PostgreSQL with row-level security for tenant isolation
- Multitenant — each school is a tenant on its own subdomain
- Permission-based authorization via `spatie/laravel-permission`; tenants get pre-seeded roles and can create custom ones

Avoid generating examples or components that conflict with the current Mintlify schema or the Laravel implementation.

## Priority order

When sources conflict, trust them in this order:

1. The Elimu Bora codebase (`~/Code/elimubora`)
2. The product developer docs (`~/Code/elimubora/docs/`)
3. `CONTRIBUTING.md` in this repository
4. This file (`AGENTS.md`)
5. Mintlify Skill and MCP documentation
6. Model training knowledge
````

- [ ] **Step 2: Verify the file is valid and complete**

Run: `head -5 AGENTS.md && wc -l AGENTS.md`
Expected: first line is `# Elimu Bora ERP Help Center — Agent Guide`; line count is roughly 90-100.

- [ ] **Step 3: Commit**

```bash
git add AGENTS.md
git commit -m "docs: rewrite AGENTS.md as canonical agent guide

Correct stale tech-stack facts (PHP 8.4, permission-based auth), state
the repo purpose and audience, and point agents at the product source
docs as the verification reference."
```

---

## Task 2: Replace CLAUDE.md and GEMINI.md with pointers

**Files:**
- Modify: `CLAUDE.md` (full replacement)
- Modify: `GEMINI.md` (full replacement)

- [ ] **Step 1: Overwrite `CLAUDE.md` with the content below**

````markdown
# Agent Guide

This project keeps its agent guidance in a single canonical file.

Read [`AGENTS.md`](AGENTS.md) and follow it in full.
````

- [ ] **Step 2: Overwrite `GEMINI.md` with the content below**

````markdown
# Agent Guide

This project keeps its agent guidance in a single canonical file.

Read [`AGENTS.md`](AGENTS.md) and follow it in full.
````

- [ ] **Step 3: Verify both files**

Run: `cat CLAUDE.md GEMINI.md`
Expected: both files show the same three-line pointer content.

- [ ] **Step 4: Commit**

```bash
git add CLAUDE.md GEMINI.md
git commit -m "docs: reduce CLAUDE.md and GEMINI.md to pointers

AGENTS.md is now the single source of truth for agent guidance;
the other two files point to it to avoid sync drift."
```

---

## Task 3: Rewrite CONTRIBUTING.md

**Files:**
- Modify: `CONTRIBUTING.md` (full replacement)

- [ ] **Step 1: Overwrite `CONTRIBUTING.md` with the full content below**

`````markdown
# Contributing to the Elimu Bora Help Center

This repository holds the public help center for Elimu Bora ERP. Pages are written in MDX and rendered with Mintlify. The audience is non-technical school staff — principals, teachers, bursars, registrars, HR, and guardians — who use the Elimu Bora tenant panel. Content is task-oriented: it explains how to do things in the system.

## How to contribute

### Edit directly on GitHub

1. Open the page you want to change.
2. Click the pencil icon to edit.
3. Describe your change and open a pull request.

### Local development

1. Fork and clone this repository.
2. Install the Mintlify CLI: `npm i -g mint`
3. Create a branch for your change.
4. Run `mint dev` from the repository root and preview at `http://localhost:3000`.
5. Make your changes, then commit and open a pull request.

Run `mint broken-links` before opening a pull request to catch broken internal links.

## Docs structure

The help center is a single **Documentation** tab. Its page order lives in `docs.json` under `navigation.tabs`. Every page maps to one purpose:

| Page / folder | Purpose |
|---|---|
| `introduction` | What Elimu Bora is, who uses it, and how the platform is organized. |
| `quickstart` | Get a school set up quickly. |
| `modules/*` | One page per product module. Each module page documents that module's features, workflows, and its own reports and analytics. |
| `roles-permissions` | Role-based access control: the pre-seeded roles and their permissions, locked roles (Super Admin, Guardian), creating and editing roles, and assigning permissions. |
| `staff/overview` | "Teaching & Non-teaching" — what teaching staff (class teachers, subject teachers, HODs) and non-teaching staff (bursars, registrars, librarians, HR) can do in the system. |
| `guardians/overview` | "Parents & Guardians" — the guardian portal experience. |
| `settings/overview` | School-level configuration. |

There is no separate reports section — reports and analytics are documented inside the module that owns them.

The product modules are defined in the product itself (`App\Enums\Module`). When a new module ships, add a `modules/<slug>.mdx` page and a corresponding entry in `docs.json`.

## Page conventions

- **Frontmatter.** Every page starts with `title`, `description`, and `icon`:

  ```yaml
  ---
  title: "Attendance"
  description: "Mark daily registers and follow up on absences."
  icon: "clipboard-list"
  ---
  ```

  The `title` is the page H1 — do not add a separate `#` heading. Icons use the Lucide library (set in `docs.json`).
- **Components over raw markdown.** Prefer Mintlify components — `Steps`, `Tabs`, `Card`, `Accordion`, `Note`, `Tip`, `Warning` — for anything structured. Use the Mintlify Skill when unsure of a component's syntax.
- **Screenshots.** Where a screenshot belongs but is not yet available, leave a placeholder on its own line: `[Insert screenshot: <description>]`.
- **Voice.** Use active voice. Address the reader as "you". One idea per sentence. Lead with the goal ("To enroll a student, ..."). Use consistent terminology — match the labels used in the product UI.
- **No emojis.**
- **Headings.** The page body uses `##` and `###`. Keep the structure shallow.

## Keeping docs in sync with the product

The product is the source of truth. The canonical reference is the developer-docs folder in the product repository:

- `~/Code/elimubora/docs/project-overview.md`
- `~/Code/elimubora/docs/conventions.md`
- `~/Code/elimubora/docs/modules/*.md`

When the product changes, update the owning module page in the same effort. Before revising a page, read the matching source doc; if its `Last verified` commit looks stale against recent product changes, crosscheck the codebase at `~/Code/elimubora`. Never document behavior you have not verified against the source docs or the code.
`````

- [ ] **Step 2: Verify the file**

Run: `head -1 CONTRIBUTING.md && grep -c "^## " CONTRIBUTING.md`
Expected: first line is `# Contributing to the Elimu Bora Help Center`; the `## ` heading count is 4.

- [ ] **Step 3: Commit**

```bash
git add CONTRIBUTING.md
git commit -m "docs: rewrite CONTRIBUTING.md with docs structure

Replace the Mintlify template boilerplate with a project-specific guide:
the navigation structure and what each page is for, page conventions, and
the workflow for keeping help docs in sync with the product source docs."
```

---

## Task 4: Slim down README.md

**Files:**
- Modify: `README.md` (full replacement)

- [ ] **Step 1: Overwrite `README.md` with the full content below**

`````markdown
# Elimu Bora ERP — Help Center

The public help center for [Elimu Bora ERP](https://elimuboraerp.com), a multi-tenant school management system for Kenyan schools running the CBC and 8-4-4 curricula. It documents how school staff use the platform day to day.

Live site: **[help.elimuboraerp.com](https://help.elimuboraerp.com)**

## Tech stack

- [Mintlify](https://mintlify.com) — documentation rendering and hosting
- MDX — page authoring

## Local development

Install the Mintlify CLI:

```bash
npm i -g mint
```

Run the dev server from the repository root (where `docs.json` lives):

```bash
mint dev
```

Preview at `http://localhost:3000`.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the docs structure, page conventions, and how to keep content in sync with the product.

## Related

- Product repository: `https://github.com/elimuborasolutions/erp` (private)
`````

- [ ] **Step 2: Verify the file**

Run: `head -1 README.md && grep -c "Mintlify Starter Kit" README.md`
Expected: first line is `# Elimu Bora ERP — Help Center`; the `Mintlify Starter Kit` count is 0.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: replace starter-kit README with project description

Describe the repository, its tech stack, local development, and link to
the deployed help center and product repository."
```

---

## Task 5: Update navigation — drop reports, add staff page

**Files:**
- Modify: `docs.json:47-50`
- Create: `staff/overview.mdx`
- Delete: `reports/overview.mdx`

- [ ] **Step 1: Edit the `docs.json` navigation page list**

In `docs.json`, the `navigation.tabs[0].pages` array currently ends with:

```json
          "roles-permissions",
          "guardians/overview",
          "settings/overview",
          "reports/overview"
```

Replace those four lines with:

```json
          "roles-permissions",
          "staff/overview",
          "guardians/overview",
          "settings/overview"
```

This removes `reports/overview` and inserts `staff/overview` immediately before `guardians/overview`.

- [ ] **Step 2: Verify docs.json is still valid JSON**

Run: `python3 -c "import json; json.load(open('docs.json')); print('valid')"`
Expected: `valid`

- [ ] **Step 3: Create `staff/overview.mdx`**

Create the file `staff/overview.mdx` with this content (a short stub; full content is written in the content-sweep phase):

````mdx
---
title: "Teaching & Non-teaching Staff"
description: "What teaching and non-teaching staff can do in Elimu Bora."
icon: "briefcase"
---

This guide walks through the day-to-day tasks available to teaching staff — class teachers, subject teachers, and heads of department — and to non-teaching staff such as bursars, registrars, librarians, and HR.
````

- [ ] **Step 4: Delete the reports page**

Run: `git rm reports/overview.mdx`
Expected: `rm 'reports/overview.mdx'`. If the `reports/` directory is now empty, it will no longer be tracked by git.

- [ ] **Step 5: Verify the build resolves all navigation pages and links**

Run: `mint broken-links`
Expected: no broken links reported. Every page named in `docs.json` (`introduction`, `quickstart`, `modules/*`, `roles-permissions`, `staff/overview`, `guardians/overview`, `settings/overview`) resolves to a file.

If `mint` is not installed, run `npm i -g mint` first.

- [ ] **Step 6: Commit**

The `reports/overview.mdx` deletion is already staged by the `git rm` in Step 4.

```bash
git add docs.json staff/overview.mdx
git commit -m "docs: restructure navigation for staff page

Remove the standalone reports page (reports are now documented per
module) and add a Teaching & Non-teaching staff page before the
guardians page."
```

---

## Verification

After all tasks:

- [ ] Run `mint broken-links` — no broken links.
- [ ] Run `mint dev` and visually confirm the sidebar shows the Teaching & Non-teaching page above Parents & Guardians, and no Reports entry.
- [ ] Confirm `git status` is clean and the branch has five commits (one per task).

## Out of scope

The content sweep of existing `.mdx` pages — false claims (student/guardian portal access), broken links, duplicate sections in `introduction.mdx`, and writing real content for `staff/overview.mdx` — is handled separately after this plan completes.
