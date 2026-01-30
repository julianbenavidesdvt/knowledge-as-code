---
name: context7_manager
description: Manage project documentation and technical context following the Context7 standard.
---

# Context7 Manager

This skill ensures that the project's technical context and documentation are maintained according to the "Context7" standard, facilitating "Knowledge as Code".

## What is Context7?

Context7 is a documentation approach that maps the current state of the system (Infrastructure, Backend, Frontend) into structured markdown files that serve as the primary source of truth for both developers and AI agents.

## Instructions

1.  **Context Mapping**:
    *   Identify the core components of the current task (e.g., a new GCP resource, a NestJS service, or an Angular component).
    *   Locate or create the relevant documentation file (e.g., `CONTEXT7.md` or specific docs in `docs/`).

2.  **Infrastructure Updates**:
    *   When infrastructure is created or modified via Terraform, document the resource ID, purpose, and connections in the infrastructure context.
    *   Include links to GCP Console where relevant (but prefer local documentation).

3.  **Cross-Reference**:
    *   Ensure that Backend and Frontend documentation references the same naming conventions and architectural patterns.
    *   If a database schema changes, update the Data Model section in the context.

4.  **Verification**:
    *   Verify that the documentation matches the actual implementation.
    *   Remove obsolete documentation to avoid "knowledge drift".

## Best Practices

- **Keep it Atomic**: Document changes alongside the code they describe.
- **AI-Friendly**: Use clear headings, bullet points, and code blocks that are easy for AI agents to parse.
- **Searchable**: Use keywords and consistent terminology.
- **Traceable**: Link to relevant tasks or commits if possible.
