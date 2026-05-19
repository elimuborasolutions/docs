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
