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
