# CLAUDE.md — Website Development Instructions

## Project Overview

This repository contains a custom website developed with an emphasis on user experience, accessibility, performance, maintainability, and visual consistency.

The project's purpose, audience, and feature requirements are defined in `PRD.md`.

**Do not make assumptions about project requirements when an authoritative document already defines them.**

## Source of Truth

Read the appropriate project documents before beginning work:

| Document | Responsibility |
|---|---|
| `PRD.md` | Product vision, audience, goals, features, and scope |
| `content-model.md` | Page sections, content structure, and data fields |
| `site-contract.md` | Functional behavior, interactions, and page requirements |
| `design-standard.md` | Visual design, typography, colors, spacing, and responsive rules |

Use these documents according to the task.

If specifications conflict, identify the conflict and request clarification rather than silently choosing an interpretation.

`CLAUDE.md` governs the development workflow; it does not replace the specifications above.

## Development Principles

- Preserve existing functionality unless a change is explicitly requested.
- Prefer incremental, maintainable changes over unnecessary rewrites.
- Reuse established components, patterns, and utilities.
- Follow the existing technology stack and project conventions.
- Avoid introducing dependencies without a clear technical justification.
- Do not invent features, content, or design requirements.
- Keep presentation, behavior, and data responsibilities appropriately separated.

## UI/UX Standards

Before modifying the interface, consult `design-standard.md` and `site-contract.md`.

- Maintain consistency across pages and components.
- Use semantic HTML and accessible interaction patterns.
- Follow WCAG 2.2 AA principles unless the project specifies otherwise.
- Support keyboard navigation and visible focus indicators.
- Respect reduced-motion preferences.
- Follow the project's responsive layout and breakpoint conventions.
- Avoid unnecessary visual changes outside the requested scope.

## Implementation Workflow

For each development task:

1. **Inspect** the relevant files and documentation.
2. **Plan** the smallest appropriate implementation.
3. **Implement** the requested changes.
4. **Validate** functionality, responsiveness, and accessibility.
5. **Report** what changed, what was tested, and any unresolved issues.

For substantial architectural changes, explain the proposed approach before implementation.

Do not modify unrelated functionality.

## Code Quality

- Write readable, maintainable code.
- Follow existing naming and formatting conventions.
- Avoid duplicate logic.
- Handle errors and unexpected input appropriately.
- Do not expose credentials or secrets.
- Preserve compatibility with the existing application architecture.
- Add or update tests when behavior changes.

## Git and Deployment

- Do not commit, push, merge, or deploy without explicit authorization.
- Never force-push unless explicitly instructed.
- Do not modify environment variables or production configuration without approval.
- Do not include unrelated files in commits.
- Verify changes locally before requesting release approval.

## Communication

- Treat the project owner as an experienced designer and developer.
- Be concise, technical, and specific.
- Explain significant architectural tradeoffs.
- Identify assumptions and unresolved questions.
- Do not repeatedly explain established decisions.
- Report test results accurately; never claim testing that was not performed.

## Definition of Done

A task is complete when:

- Requested functionality is implemented.
- Relevant specifications are satisfied.
- Existing functionality is preserved.
- Appropriate tests have passed.
- Accessibility and responsive behavior have been considered.
- Changes and limitations are documented.

**Stop after local validation unless further actions have been explicitly authorized.**
