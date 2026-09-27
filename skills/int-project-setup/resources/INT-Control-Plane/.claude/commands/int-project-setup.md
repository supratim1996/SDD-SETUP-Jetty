---
name: int-project-setup
description: Initialize a new software project using the INT AI-First standard Control Plane, project-specific AI context, technology discovery gate, and repository baseline.
---

# INT AI-First Project Setup Workflow

This workflow triggers the **`int-project-setup`** skill.

For the full detailed specification, templates, and execution protocols, see:
[int-project-setup/SKILL.md](.agents/skills/int-project-setup/SKILL.md) (or `<global-skills-root>/int-project-setup/SKILL.md`)

## Summary of Steps:

1. **Technology & Architecture Discovery Gate**:
   - Confirm Project Type (Full Stack, Frontend Only, Backend Only, Mobile)
   - Confirm Architecture Style (MANDATORY FOR ALL TYPES: Monolithic vs Modular Monolith / Microservices-Ready vs Microservices vs Clean Architecture vs Feature-Sliced Architecture)
   - Confirm Tech Stack (Frontend, Backend, Database, ORM, Auth, Deployment)
   - Confirm Gate 1 Reviewer(s) (Project Manager / Product Owner) and Gate 2 Reviewer(s) (Technical Lead / Architect) names & emails.
   - **Governance Aspect & Reviewer Roster Validation (MANDATORY)**: Inspect `.ai-context/project_context.md` and `.ai-context/constitution.md`. If TL, PM, or reviewer emails are missing or default placeholders (`<email@domain.com>`), prompt the developer to input real emails and update `project_context.md` and `constitution.md` automatically!
2. **Native OS Bulk Copy INT Control Plane & Setup Governance**:
   - Execute bulk native OS copy of `resources/INT-Control-Plane/.agent/` to `.agent/` (Excludes `int-sync-global-skills.md` which is strictly restricted to master admin).
   - Auto-generate `AGENTS.md` in workspace root for vendor-agnostic governance.
   - Execute bulk native OS copy of sub-skills into `.agent/skills/` (**STRICT RULE: Copy ONLY skill directory and `SKILL.md` file**, strictly excluding nested `resources/` subfolders).
   - Auto-generate `.gitignore` with standard rules protecting `.agent/`, `.ai-context/`, and `.agents/`.
3. **Initialize `.ai-context/` Knowledge Base**:
   - Create subdirectories (`specs`, `plans`, `tasks`, `test_cases`, etc.).
   - Execute bulk native OS copy of all 12 mandatory engineering templates from global config `templates/` into `.ai-context/templates/`.
   - Initialize `constitution.md`, `project_context.md`, `architecture.md`, `BRD.md`, `status.md`, `prompt_history.md`.

4. **Automated CLI-Driven Scaffolding & Dynamic Architecture Sync**:
   - Map confirmed technology stack (Frontend, Backend, Database, ORM, Architecture style) to its official framework CLI generator tool.
   - Execute the CLI generator command via CMD / terminal in non-interactive mode (`npx create-vite`, `npx @nestjs/cli`, `dotnet new`, `django-admin`, `cargo new`, `composer create-project`, `flutter create`, `curl start.spring.io`).
   - Scan generated directory structure from disk (`list_dir`).
   - Dynamically populate `.ai-context/architecture.md` and `.ai-context/project_context.md` with the scanned actual folder hierarchy, entry points, and module boundaries.



