# Compass Projects

This document defines how to represent and maintain **compass projects** inside the LLM Wiki.

A compass project is not a normal execution project. It acts as a **strategic reference** or **north star** that helps orient decisions across multiple downstream projects.

## Strategic roles

Project entities may use one of these roles:

- `compass` → strategic backbone / north-star project
- `pillar` → enabling project, shared system, reusable infrastructure
- `output` → concrete deliverable or execution project

Use `compass` sparingly. A compass project should exist only when it actively shapes multiple decisions across the system.

## Purpose of a compass project

A compass project should answer questions like:

- What kind of work are we trying to build?
- What should we optimize for over the long term?
- What should we explicitly avoid?
- How should downstream projects make trade-offs?

Compass projects exist to reduce drift. They are not delivery boards, task lists, or feature specs.

## Required frontmatter

A compass project should use frontmatter similar to this:

```yaml
***
type: project
role: compass
status: ongoing
priority: 0
north_star: <short-identifier>
exclude_from_kanban: true
exclude_from_execution_views: true
publish: false
tags: [compass, strategy]
resource: <relative-source-path>
***
```

### Field notes

- `role: compass` marks the note as strategic.
- `priority: 0` is reserved for backbone projects.
- `north_star` is a stable identifier for alignment and references.
- `exclude_from_kanban: true` keeps compass projects out of operational boards.
- `exclude_from_execution_views: true` keeps them out of normal execution dashboards.
- `publish: false` keeps them internal unless explicitly changed.

## Required sections

A compass project should contain these sections when possible:

- `# <Project Name>`
- `## One-line compass`
- `## Purpose`
- `## Non-goals`
- `## Decision rules`
- `## Signals of drift`
- `## Concrete examples`
- `## Dependencies`
- `## Downstream projects`
- `## Review cadence`

Do not remove these sections from an existing compass project unless the user explicitly asks.

## Template

Use this template when creating or upgrading a compass project:

```markdown
***
type: project
role: compass
status: ongoing
priority: 0
north_star: <short-identifier>
exclude_from_kanban: true
exclude_from_execution_views: true
publish: false
tags: [compass, strategy]
resource: <relative-source-path>
***

# <Compass Project Name>

## One-line compass
A single sentence that defines what this project is for and why it matters.

## Purpose
- Why this compass exists.
- What it should optimize for in the long run.
- How it connects to the broader system, identity, or strategy.

## Non-goals
- Things this project explicitly does not try to do.
- Trade-offs that are accepted.
- Temptations or distractions to avoid.

## Decision rules
- When in doubt, prefer X over Y.
- Always do A before B.
- Never do C unless condition D is true.
- If repeated more than N times, automate it.

## Signals of drift
- Signs that current work is moving away from this compass.
- Patterns that suggest the system is optimizing for the wrong thing.
- Outputs that feel inconsistent with the intended direction.

## Concrete examples

### Aligned
- Example 1
- Example 2

### Misaligned
- Example 1
- Example 2

## Dependencies
- [[related-project-or-tool-1]]
- [[related-project-or-tool-2]]

## Downstream projects
- [[downstream-project-1]]
- [[downstream-project-2]]

## Review cadence
- Weekly, monthly, or milestone-based review.
- Questions to ask during review.
```

## Behavior rules for Claude

When working with project entities:

1. Preserve `role` if it already exists.
2. If a project has no `role`, default to `output` unless the user explicitly asks to classify it differently.
3. Do not automatically promote a project to `compass`; suggest promotion if the project appears to define identity, direction, or recurring decision rules.
4. When a project is `role: compass`, treat it as a strategic reference when updating related `pillar` or `output` projects.
5. Do not include `compass` projects in kanban boards, operational status summaries, or execution dashboards unless the user explicitly asks.
6. When updating a compass project, prefer clarity and stability over excessive detail.

## Promotion criteria

A project is a good candidate for `role: compass` if most of these are true:

- It influences multiple other projects.
- It defines taste, positioning, constraints, or decision rules.
- It changes slowly compared to normal execution work.
- It is used to evaluate whether other work is aligned or drifting.
- It is still relevant even when no immediate deliverable is attached.

Do not use `compass` for:
- One-off experiments
- Short-lived tasks
- Single deliverables
- Generic idea dumps
- Notes that do not influence other projects

## Alignment guidance

When evaluating whether a normal project is aligned to a compass project, look for:

- shared goals
- shared constraints
- shared audience or identity
- explicit dependencies
- reuse of systems, tone, or workflows
- consistency with the compass project's `Decision rules` and `Non-goals`

If alignment is weak, note it as a warning rather than rewriting the project automatically.

## Lint expectations

A future conceptual lint may check for:

- compass projects missing required sections
- output projects with no linked compass or pillar
- projects whose notes appear inconsistent with a linked compass
- stale compass projects with no downstream links
- operational boards that accidentally include `role: compass`

Until such a lint exists, maintain these relationships manually and conservatively.