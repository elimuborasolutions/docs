## AI Agent Skill Usage Rules

IMPORTANT: Use retrieval-led reasoning over training-led reasoning for this project. Prefer the Mintlify Skill, Mintlify MCPs, and the actual Elimu Bora codebase over model memory or generic documentation knowledge.

### When to use the Mintlify Skill
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

### When to use the Mintlify Docs MCP
The agent SHOULD use the Mintlify Docs MCP when:
- Looking up the latest Mintlify documentation
- Verifying component APIs or syntax
- Checking current best practices
- Confirming supported features before generating docs
- Researching examples of advanced Mintlify implementations

### When to use the Mintlify Dashboard MCP
The agent SHOULD use the Mintlify Dashboard MCP when:
- Working with deployment issues
- Investigating indexing/search problems
- Managing documentation configuration
- Debugging dashboard-specific behavior
- Reviewing project-level Mintlify settings

### Documentation workflow expectations
Before creating or modifying documentation:
1. Inspect the Elimu Bora source code first.
2. Verify the documentation approach against Mintlify Skill/MCP guidance if the task involves Mintlify features or structure.
3. Prefer official Mintlify patterns over improvised MDX structures.
4. Reuse existing documentation patterns already established in this repository.

### Agent behavior rules
- Never invent Mintlify components or props.
- Never assume deprecated syntax still works.
- Prefer composable Mintlify components over large raw markdown blocks.
- Prefer shallow, readable page structures over deeply nested sections.
- Keep documentation visually scannable and optimized for non-technical school staff.
- If unsure about a Mintlify feature, consult the Mintlify Skill or Docs MCP before writing content.

### Priority order
When conflicts occur, use this priority order:
1. Actual Elimu Bora source code
2. Repository documentation standards in this file
3. Mintlify Skill and MCP documentation
4. Existing documentation patterns in the repo
5. Model training knowledge

## Tech stack awareness

The documentation project uses:
- Mintlify for documentation rendering
- MDX for page authoring
- Laravel-based backend source code (`~/Code/elimubora`) - uses Laravel 12, Filament 4, Livewire 3, Tailwind CSS 4, and PHP 8.3
- Role-based workflows (Admin, Teacher, Parent/Guardian, Accountant)

The agent should avoid generating examples or components that conflict with the current Mintlify schema or Laravel implementation.
