# Elimu Bora Documentation Standards

These instructions guide all documentation work for Elimu Bora. Adhere to these standards for both major overhauls and incremental updates to ensure consistency, clarity, and ease of use for our school partners.

## Source Context & Accuracy
- **Primary Codebase**: `~/Code/elimubora` or `https://github.com/elimuborasolutions/erp`.
- **Truth Source**: Always reference the primary codebase for feature accuracy. The `README.md` and `erp-technical-overview.md` in the codebase may be outdated; prioritize actual code analysis (controllers, routes, views) to document current behavior.

## Mintlify MCP & Context
To maintain deep context on Mintlify components and best practices, ensure these resources are available:
- **Mintlify Skill**: `npx skills add https://mintlify.com/docs`
- **Mintlify Docs MCP**: `https://mintlify.com/docs/mcp`
- **Mintlify Dashboard MCP**: `https://mcp.mintlify.com`

## Core Documentation Principles

### 1. Simple, User-Centric Tone
- **Audience**: School administrators, teachers, and guardians in Kenya.
- **Language**: Use simple, accessible English. Avoid technical jargon (e.g., use "setup" instead of "configuration," "history" instead of "audit logs").
- **Voice**: Active voice and second person ("You can mark attendance...").
- **Inspiration**: Model clarity after Stripe, Laravel, and FilamentPHP.

### 2. Action-Oriented Structure
- **Headings**: Use sentence case for headings (e.g., "How to mark a register" not "Register Marking Process").
- **Steps**: Use `<Steps>` components for procedures.
- **Visual Placeholders**: Always include descriptive placeholders for screenshots where visual clarity is needed: `[Insert screenshot: Detailed description of the UI element/page]`.

### 3. Mintlify Component Usage
Heavily utilize [Mintlify components](https://www.mintlify.com/docs/components/index) to create a rich, interactive experience:
- **CardGroup & Card**: Use on Overview pages for visual navigation. Always include an icon and a brief description for each card.
- **Steps**: Mandatory for all "How-to" guides. Keep each step focused on a single action.
- **Tabs**: Use to separate instructions for different roles (e.g., **Admin** vs. **Teacher**) or platforms (e.g., **Web Dashboard** vs. **Mobile App**).
- **Accordion**: Use for FAQs, Troubleshooting details, or secondary information that shouldn't clutter the main flow.
- **Callouts (Info, Warning, Tip, Note)**:
    - `Info`: For important context that users shouldn't miss.
    - `Warning`: For actions that are irreversible (e.g., deleting a student record or reversing a payment).
    - `Tip`: For shortcuts or "pro-tips" that improve efficiency.
    - `Note`: For minor details or edge cases.
- **Frame**: Wrap all image placeholders and future screenshots in a `<Frame>` component to maintain consistent styling and borders.
- **ResponseField / ParamField**: Use these when documenting specific form fields (e.g., "Add Student" fields) or school settings to provide clear definitions for each input.
- **Tooltip**: Use for brief definitions of terms that might be unfamiliar to new users (e.g., "CBC", "STK Push").

## Content Architecture & Sidebar

### Navigation Strategy
- **Sidebar Organization**: Group by functional domains (Modules, Roles, Settings, etc.).
- **Flat File Structure**: Prefer a flat file structure within domain folders where appropriate, but use nested navigation in `docs.json` for hierarchy.
- **Module Pages**: Each module (e.g., Attendance) should have a dedicated folder/file structure that covers Overview, common tasks, and troubleshooting.

### Formatting Conventions
- **UI Elements**: **Bold** for UI elements: Click **Settings**.
- **Code/Paths**: Use `code formatting` for file names, paths, or technical references.
- **Consistency**: Maintain a uniform layout across all module pages (Overview -> How-to -> Troubleshooting/FAQ).
