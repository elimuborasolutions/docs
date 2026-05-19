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
- **Screenshots.** The audience is non-technical, so pages should be image-rich — a school admin should be able to follow along by matching the docs to what they see on screen. Add a screenshot anywhere it helps the reader orient: the relevant page, form, modal, or status badge.

  Where a screenshot belongs but is not yet available, leave a placeholder on its own line so an admin can later drop in the real image:

  ```
  [Insert screenshot: <description>]
  ```

  Make the description specific enough that someone who has never seen the screen knows exactly what to capture — name the page or modal, the key fields or controls, and any state worth showing. For example:

  ```
  [Insert screenshot: Requisition list showing statuses: Pending Approval, Approved, Issued, and Rejected]
  ```

  Vague placeholders like `[Insert screenshot: the page]` are not acceptable.
- **Voice.** Use active voice. Address the reader as "you". One idea per sentence. Lead with the goal ("To enroll a student, ..."). Use consistent terminology — match the labels used in the product UI.
- **No emojis.**
- **Headings.** The page body uses `##` and `###`. Keep the structure shallow.

## Level of detail

Write thoroughly. A school admin should not have to guess or experiment — the page should answer the question before they have to ask it. When in doubt, document more, not less.

- **Statuses.** Where a record has a status (an invoice, a requisition, an assessment, an academic year), document **every** status — list each one, explain what it means, and explain what the user can do while a record is in it.
- **Transitions.** Explain how a record moves from one status to the next: what action triggers the change, who can perform it, and whether the move can be reversed. Do not leave a status flow implicit.
- **Diagrams.** For status flows and entity relationships, use a Mermaid diagram. Mintlify renders Mermaid from a fenced code block:

  ````
  ```mermaid
  stateDiagram-v2
      [*] --> PendingApproval
      PendingApproval --> Approved: approved by bursar
      PendingApproval --> Rejected: rejected by bursar
      Approved --> Issued: stock released
  ```
  ````

  Use a `stateDiagram-v2` for status lifecycles and an `erDiagram` for how records relate to one another. Place the diagram alongside the prose that describes it — the diagram supports the text, it does not replace it.

## Keeping docs in sync with the product

The product is the source of truth. The canonical reference is the developer-docs folder in the product repository:

- `~/Code/elimubora/docs/project-overview.md`
- `~/Code/elimubora/docs/conventions.md`
- `~/Code/elimubora/docs/modules/*.md`

When the product changes, update the owning module page in the same effort. Before revising a page, read the matching source doc; if its `Last verified` commit looks stale against recent product changes, crosscheck the codebase at `~/Code/elimubora`. Never document behavior you have not verified against the source docs or the code.
