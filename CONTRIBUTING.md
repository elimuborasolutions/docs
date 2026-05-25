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
- **Voice.** Use active voice. Address the reader as "you". One idea per sentence. Lead with the goal ("To enroll a student, ..."). Use consistent terminology that matches the labels used in the product UI.
- **No emojis, no em-dashes, no informal language.** Use commas, periods, or "and" / "or" instead of em-dashes. Hyphens in compound words are fine.
- **Headings.** The page body uses `##` and `###`. Keep the structure shallow.
- **Prerequisite callouts.** Every workflow that depends on prior setup, an integration, a setting, a permission, or another record existing must open with a `<Note>` callout that names the dependency and links to where it is set up. Examples: M-Pesa workflows depend on the M-Pesa integration being configured; an invoice depends on an active term and the billable existing; attaching a sponsor depends on the sponsor record existing; refunding an earmark depends on an active earmark with a positive balance. Skip the callout only when the prerequisite is satisfied automatically by the system (for example, a wallet is auto-created with a student) **or** is pre-seeded and read-only (for example, curriculum stages and structure are pre-seeded and not editable, so they are always available without setup).
- **Panel URLs.** Elimu Bora has two panels, each served at the root of its own subdomain. The **staff panel** is at the tenant subdomain root (`<school>.elimuboraerp.com/...`), and is the only panel a school ever uses. The **admin panel** is a company-internal panel at `admin.elimuboraerp.com/...` and is not part of the help-center audience. Both panels serve their routes at the root, not under `/admin`. Refer to staff-panel paths as root-relative on the tenant subdomain (for example, `/invoices`, `/billables`, `/wallets`). The system applies role-based access control to those routes automatically. Do not write `/admin/<resource>` paths anywhere on user-facing pages — that path prefix does not exist in the product, even though some developer docs quote it.
- **Immutable records.** Some records cannot be edited or deleted once created because doing so would break a downstream ledger or audit trail (for example, invoices and invoice payments). On those pages, do not document an "Edit" or "Delete" workflow; instead, document the read view and explain the alternative path (a new record, a refund, a replacement payment) the user should take.
- **No developer-level detail.** Help-center pages are read by non-technical school staff. Do not use any of the following on user-facing pages: database table or column names, migration filenames, class names, service names, job names, observer names, enum case identifiers, scheduled-command strings, method names, or code-style permission identifiers such as `invoices.forceDelete` or `payment-integrations.manage`. Describe permissions in plain English and reference the role-editor label when needed. The only product internals that may appear are values a user sees in the panel (status names, action button labels, navigation menu items, URLs they need to copy into Safaricom Daraja, and so on).
- **Source docs are guides, not templates.** The engineering wiki under `~/Code/elimubora-wiki/docs/` (live: `https://wiki.elimuboraerp.com`) is internal company intellectual property. It guides your understanding of how the product works. Do not transcribe its structure, technical depth, schema details, or terminology into help-center pages. Translate, do not copy. If a fact lives only in the engineering wiki (a database column, an internal service name, a job class) and is not visible to the user in the panel, it does not belong on a help-center page.

## Level of detail

Write thoroughly. A school admin should not have to guess or experiment — the page should answer the question before they have to ask it. When in doubt, document more, not less.

- **Statuses.** Where a record has a status (an invoice, a requisition, an assessment, an academic year), document **every** status — list each one, explain what it means, and explain what the user can do while a record is in it.
- **Transitions.** Explain how a record moves from one status to the next: what action triggers the change, who can perform it, and whether the move can be reversed. Do not leave a status flow implicit.
- **Diagrams.** Use Mermaid `stateDiagram-v2` for status lifecycles. Mintlify renders Mermaid from a fenced code block:

  ````
  ```mermaid
  stateDiagram-v2
      [*] --> PendingApproval
      PendingApproval --> Approved: approved by bursar
      PendingApproval --> Rejected: rejected by bursar
      Approved --> Issued: stock released
  ```
  ````

  Do not include `erDiagram`s on user-facing pages. The school staff audience does not need database structure, and an `erDiagram` typically leaks internal table names, join tables, and foreign-key columns that are company intellectual property. Where relationships need explaining, describe them in plain prose using the user-facing record names. Place the diagram alongside the prose that describes it; the diagram supports the text, it does not replace it.

## Keeping docs in sync with the product

The product is the source of truth. The canonical reference is the engineering wiki (live: https://wiki.elimuboraerp.com):

- `~/Code/elimubora-wiki/docs/get-started/project-overview.mdx` (live: https://wiki.elimuboraerp.com/get-started/project-overview)
- `~/Code/elimubora-wiki/docs/architecture/*.mdx` (live: https://wiki.elimuboraerp.com/architecture/multitenancy and siblings)
- `~/Code/elimubora-wiki/docs/modules/*.mdx` (live: https://wiki.elimuboraerp.com/modules/identity and siblings)

When the product changes, update the owning module page in the same effort. Before revising a page, read the matching wiki page; if its `Last verified` commit looks stale against recent product changes, crosscheck the codebase at `~/Code/elimubora`. Never document behavior you have not verified against the wiki or the code.
