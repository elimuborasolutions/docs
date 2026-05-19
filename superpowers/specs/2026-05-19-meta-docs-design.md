# Meta-files refresh — design

> Date: 2026-05-19
> Scope: TODOs 2–4 — agent files, `CONTRIBUTING.md`, `README.md`, and the `docs.json` navigation change.
> Out of scope: the content sweep of `.mdx` pages (TODO 5), handled separately afterwards.

## Background

`elimubora-docs` is the public, end-user help center for Elimu Bora ERP, rendered with Mintlify from MDX. The repo's meta files are mostly unmodified template content and carry stale or inaccurate facts. This work corrects them and defines the documentation structure so later content work has a clear contract.

The canonical product reference is the sibling developer-docs folder at `~/Code/elimubora/docs/` (`project-overview.md`, `conventions.md`, `modules/*.md`), backed by the codebase at `~/Code/elimubora`.

## Audit findings (resolved by this design)

- `CLAUDE.md` / `AGENTS.md` / `GEMINI.md` are byte-identical and always updated together — a sync burden.
- Agent files state `PHP 8.3` (actual: 8.4) and describe authorization as role-based with a wrong role list (`Admin, Teacher, Parent/Guardian, Accountant`). Authorization is permission-based via `spatie/laravel-permission` (PR #26).
- Agent files never state what the repo is, who the docs serve, or that `~/Code/elimubora/docs/` is the source of truth.
- `CONTRIBUTING.md` is Mintlify template content, opens with a `Customize this file` note, and links a non-existent `development.mdx`. It does not define the docs structure.
- `README.md` is the Mintlify starter-kit boilerplate — no project description, no links.

## Audience

Primary audience of the help center: non-technical **school staff** using the tenant panel at `<school>.elimuboraerp.com`. This includes **guardians**, who log in with read-only, scoped access (confirmed by recent product commits `80aed00`, `c4e60f6`, `d829208`). Students do not log in. Help content is task-oriented ("how do I…").

## Section 1 — Agent files

`AGENTS.md` becomes the single canonical file. `CLAUDE.md` and `GEMINI.md` are reduced to ~3-line pointers directing the reader to `AGENTS.md`.

`AGENTS.md` sections, in order:

1. **What this repo is** — the public help center (Mintlify/MDX) for Elimu Bora ERP. Audience as above. Explicitly not developer documentation.
2. **Source of truth & verification** — read `~/Code/elimubora/docs/` (`project-overview.md`, `conventions.md`, relevant `modules/*.md`) before writing. Each source doc carries a `Last verified: <sha> on <date>` header; if it looks stale against recent commits, crosscheck the codebase at `~/Code/elimubora`, which is the ultimate authority.
3. **Mintlify Skill / MCP rules** — retained essentially as-is from the current file.
4. **Tech stack** — this repo: Mintlify + MDX. The product: Laravel 12, Filament 4, Livewire 3, Tailwind CSS 4, PHP 8.4, PostgreSQL, permission-based authorization (`spatie/laravel-permission`). The inaccurate role list is removed.
5. **Priority order** — source docs/codebase → `CONTRIBUTING.md` → `AGENTS.md` → Mintlify Skill/MCP → model knowledge.

## Section 2 — CONTRIBUTING.md (full rewrite)

Removes the template boilerplate and the dead `development.mdx` link. Sections:

1. **Intro** — what the repo is, who the docs are for.
2. **Docs structure** — a table mapping each page/folder to its purpose:

   | Page / folder | Purpose |
   |---|---|
   | `introduction`, `quickstart` | Orientation and fast school setup |
   | `modules/*` | One page per product module; each module documents its own features, workflows, and reports & analytics |
   | `roles-permissions` | Pre-seeded roles, locked roles (Super Admin, Guardian), role CRUD, assigning permissions — all RBAC |
   | `staff/overview` | "Teaching & Non-teaching" — what teaching and non-teaching staff do in the system |
   | `guardians/overview` | "Parents & Guardians" — the guardian portal experience |
   | `settings/overview` | School-level configuration |

   `reports/overview` is removed — reports/analytics are documented per module.
3. **How to contribute** — edit-on-GitHub and local development (`npm i -g mint`, `mint dev`).
4. **Page conventions** — frontmatter (`title`, `description`, `icon`); Mintlify components over raw markdown; screenshot-placeholder convention; voice (active, "you", task-oriented); no emojis.
5. **Keeping docs in sync with the product** — when the product changes, the owning module page updates; always verify against `~/Code/elimubora/docs` and the codebase.

## Section 3 — README.md (slim down)

Replaces the starter-kit boilerplate with: one-paragraph repo description, tech stack (Mintlify + MDX), local-dev steps, and links to the deployed help center and the product repo.

**Needs user input:** the deployed help-center URL (likely `help.elimuboraerp.com`) and the product-repo link (GitHub URL, or note it as private/internal).

## Section 4 — Navigation change (docs.json)

- Remove `reports/overview` from the `Documentation` tab page list.
- Insert `staff/overview` immediately before `guardians/overview`.
- Create `staff/overview.mdx` as a frontmatter-only stub so navigation does not break; full content lands in the TODO 5 content sweep.
- Delete `reports/overview.mdx`.

## Out of scope

The content sweep of existing `.mdx` pages (false claims, broken links, duplicate sections) is TODO 5 and handled after these meta files are in place.
