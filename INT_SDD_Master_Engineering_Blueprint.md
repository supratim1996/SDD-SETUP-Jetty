# INT AI-First SDD Engineering Blueprint & Setup Bible

**Version:** 1.0  
**Authority:** INT SDD Blueprint Standard  
**Compatibility:** Universal / Platform-Agnostic (Antigravity/Gemini, Claude Code, Cursor, Windsurf, Copilot, Custom Agents)

---

## Executive Summary

The **INT Spec-Driven Development (SDD) Blueprint** is a vendor-agnostic software engineering methodology designed for AI-first development. Its core mandate is simple:

> **Code is generated output.** Requirements, specifications, architecture, and tests govern all development. An AI assistant must NEVER edit code directly without an approved Specification, Gate 1 review, and a failing test case (TDD RED).

This master document details the end-to-end architecture, global system rules, workspace folder management, complete source definitions for all sub-skills, dynamic control plane (`rules/` and `workflows/`), complete project knowledge base (`.ai-context/`), change management protocol, and step-by-step lifecycle for setting up and executing INT SDD across any AI tool or IDE.

---

## Table of Contents

1. [Universal Architecture & Folder Locations](#1-universal-architecture--folder-locations)
2. [Global AI Instruction Configuration (`GEMINI.md` / `CLAUDE.md` / `.cursorrules`)](#2-global-ai-instruction-configuration)
3. [Master INT Control Plane Blueprint (`rules/` and `workflows/`)](#3-master-int-control-plane-blueprint-rules-and-workflows)
4. [Workspace Folder Management & Project Setup](#4-workspace-folder-management--project-setup)
5. [Dynamic Control Plane (`.agent/`)](#5-dynamic-control-plane-agent)
6. [Complete Project Knowledge Base (`.ai-context/`)](#6-complete-project-knowledge-base-ai-context)
7. [Complete Sub-Skills Specifications (All 8 Sub-Skills)](#7-complete-sub-skills-specifications-all-8-sub-skills)
   - [Skill 1: `int-project-setup`](#skill-1-int-project-setup)
   - [Skill 2: `int-sdd-lifecycle`](#skill-2-int-sdd-lifecycle)
   - [Skill 3: `int-brd-ingestion`](#skill-3-int-brd-ingestion)
   - [Skill 4: `int-incident-management`](#skill-4-int-incident-management)
   - [Skill 5: `int-hotfix-management`](#skill-5-int-hotfix-management)
   - [Skill 6: `int-release-management`](#skill-6-int-release-management)
   - [Skill 7: `int-session-continuation`](#skill-7-int-session-continuation)
   - [Skill 8: `int-reverse-engine-workflow`](#skill-8-int-reverse-engine-workflow)
8. [Client Observations & Multi-Spec Change Protocol](#8-client-observations--multi-spec-change-protocol)
9. [Session Recovery & Retrofitting Existing Projects](#9-session-recovery--retrofitting-existing-projects)

---

## 1. Universal Architecture & Folder Locations

The INT SDD architecture separates **System-Wide AI Rules**, **Master Control Plane Source Blueprint**, **Global Sub-Skills**, and **Project Workspace Output**.

```text
┌────────────────────────────────────────────────────────────────────────┐
│ Global OS User Directory (System-Wide AI Configuration)               │
│                                                                        │
│ ├── GEMINI.md / CLAUDE.md / .cursorrules   ◄── System-wide rules       │
│ │                                                                      │
│ └── config/skills/ (or .cursor/skills/ or .claude/skills/)             │
│     ├── int-project-setup/                 ◄── Setup Sub-Skill         │
│     │   ├── SKILL.md                                                   │
│     │   ├── image/                                                     │
│     │   └── resources/INT-Control-Plane/.agent/ ◄── Master Control Plane│
│     │       ├── rules/                        (Source blueprint rules) │
│     │       │   ├── int-standards.md                                   │
│     │       │   ├── auto-log.md                                        │
│     │       │   └── .agentignore                                       │
│     │       └── workflows/                    (Source workflow guides) │
│     │           ├── int-code-review.md                                 │
│     │           ├── int-project-from-brd.md                            │
│     │           ├── int-generate-tests.md                              │
│     │           ├── int-hotfix-management.md                           │
│     │           ├── int-brd-ingestion.md                               │
│     │           ├── int-production-incident.md                         │
│     │           └── int-release-management.md                          │
│     ├── int-sdd-lifecycle/SKILL.md                                     │
│     ├── int-brd-ingestion/SKILL.md                                     │
│     ├── int-incident-management/SKILL.md                               │
│     ├── int-hotfix-management/SKILL.md                                 │
│     ├── int-release-management/SKILL.md                                │
│     └── int-session-continuation/SKILL.md                              │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ Governs Agent Behavior & Tools
┌───────────────────────────────────▼────────────────────────────────────┐
│ Project Workspace Root                                                  │
│                                                                        │
│ ├── .agent/                    ◄── Dynamic Control Plane (Project)     │
│ │   ├── rules/                 (Copied dynamically from master)        │
│ │   └── workflows/             (Copied dynamically from master)        │
│ │                                                                      │
│ ├── .ai-context/               ◄── Complete Project Knowledge Base     │
│ │   ├── change_requests/       (Archived change request screenshots & docs)│
│ │   ├── decisions/             (Architectural Decision Records)        │
│ │   ├── hotfixes/              (Hotfix specifications)                 │
│ │   ├── incidents/             (Incident triage logs)                  │
│ │   ├── plans/                 (Implementation Plans)                  │
│ │   ├── releases/              (Release Notes & Deployment Logs)       │
│ │   ├── specs/                 (Feature Specifications)                │
│ │   ├── tasks/                 (Executable Task Breakdowns)            │
│ │   ├── templates/             (Mandatory artifact templates)          │
│ │   ├── test_cases/            (Test-case specifications)              │
│ │   ├── architecture.md        (System design & boundaries)            │
│ │   ├── assumptions.md         (Project assumptions & dependencies)    │
│ │   ├── brd-change-log.md      (BRD revision & delta log)              │
│ │   ├── BRD.md                 (Authoritative BRD baseline)            │
│ │   ├── constitution.md        (Non-functional baseline)               │
│ │   ├── project_context.md     (Tech stack & parameters)               │
│ │   ├── prompt_history.md      (Turn-by-turn execution log)            │
│ │   └── status.md              (Repository project status board)       │
│ │                                                                      │
│ ├── docs/                      ◄── BRD & Raw Input Documents           │
│ ├── src/                       ◄── Generated Code Output               │
│ └── tests/                     ◄── Executable Test Code (TDD)          │
└────────────────────────────────────────────────────────────────────────┘
```

### Global Skill Setup Locations by AI Tool

| AI Assistant Tool | Global Skills Root Path | Workspace-Specific Skills Path |
|---|---|---|
| **Antigravity / Gemini** | `~/.gemini/config/skills/<skill-name>/SKILL.md` | `<workspace>/.agents/skills/<skill-name>/SKILL.md` |
| **Claude Code** | `~/.claude/skills/<skill-name>/SKILL.md` | `<workspace>/.claude/skills/<skill-name>/SKILL.md` |
| **Cursor** | `~/.cursor/skills/<skill-name>/SKILL.md` | `<workspace>/.cursor/skills/<skill-name>/SKILL.md` |
| **Windsurf** | `~/.codeium/windsurf/skills/<skill-name>/SKILL.md` | `<workspace>/.windsurf/skills/<skill-name>/SKILL.md` |
| **Custom / Generic Agent** | `~/.config/ai-skills/<skill-name>/SKILL.md` | `<workspace>/.agents/skills/<skill-name>/SKILL.md` |

---

## 2. Global AI Instruction Configuration

Every AI assistant platform reads a global instructions file at runtime. 

Paste the exact content below into your global configuration file (e.g., `GEMINI.md`, `CLAUDE.md`, or `.cursorrules`):

```markdown
# INT AI-First Engineering Policy

## Authority
Follow the INT SDD Blueprint V1.0.
The project repository is the primary source of project context.
Do not rely on chat history as the project's permanent source of truth.

---

## Artifact Authority
1. constitution.md
2. BRD.md
3. Approved Spec
4. Approved Plan
5. Approved Tasks
6. Test Cases
7. Existing Implementation

Code is generated output.
Do not use implementation code to silently redefine requirements.

---

## Mandatory Lifecycle
BRD → Gate 0 Review (BRD Approval) → Spec → Gate 1 Review (Spec Approval) → Pre-Dev Check → Plan → Architecture Check → Tasks → Test-first RED → Implementation GREEN → Gate 2 Review (Code Approval) → Merge → Release

---

## Core Rules
- No implementation without an approved Spec.
- No direct code edits when client feedback/screenshots/prompts are provided; auto-detect affected Spec ID(s) (or generate a Multi-Spec Impact Matrix), update Specs, and wait for Gate 1 approval first.
- No implementation before tests exist and are confirmed RED.
- One task per agent execution.
- Prompt by stable artifact ID (`<feature-slug>`).
- Preserve existing implementation.
- Do not invent requirements or technology choices.
- Do not bypass Gate 1 or Gate 2.
- **Strict Turn Termination on Gate 1 & Gate 2 HALT**:
  - When presenting a Spec for **Gate 1 Approval**, the agent MUST present the request and **IMMEDIATELY END ITS TURN** (stop calling tools). No plan, task, or code generation is permitted before explicit human approval.
  - When development and test verification are complete and requesting **Gate 2 Approval**, the agent MUST present the code review request and **IMMEDIATELY END ITS TURN** (stop calling tools). No release notes, status board release sync, or next spec transition is permitted before explicit human approval in a subsequent turn.
- Keep architecture.md and status.md current.
- Do not expose secrets or PII in project artifacts.

---

## Agent Behavior on Feedback / Ambiguity
When receiving client observations, screenshots, or design changes:
STOP. Do NOT modify application code directly. Auto-detect affected Spec ID(s) from `.ai-context/specs/`, draft Spec updates, present to user, and WAIT for Gate 1 approval first.

When requirements are ambiguous:
STOP. Do not guess. Request clarification or update the relevant artifact.
```

---

## 3. Master INT Control Plane Blueprint (`rules/` and `workflows/`)

The Master Control Plane source folder (`config/skills/int-project-setup/resources/INT-Control-Plane/.agent/`) contains all authoritative rule and workflow documents that are copied into projects by `int-project-setup` and `int-session-continuation`.

### Complete Master Control Plane Tree
```text
config/skills/int-project-setup/resources/INT-Control-Plane/.agent/
├── rules/
│   ├── int-standards.md                 ◄── Master INT engineering policies & governance
│   ├── auto-log.md                      ◄── Prompt & turn logging automation rules
│   └── .agentignore                     ◄── File scan exclusion boundaries
└── workflows/
    ├── int-pr-gate-workflow.md          ◄── PR Gate workflow (Spec Gen → Gate 1 → Dev Selection → Gate 2)
    ├── int-code-review.md               ◄── Gate 2 Code Review & verification checklist
    ├── int-project-from-brd.md          ◄── BRD ingestion to module config workflow
    ├── int-generate-tests.md            ◄── TDD Test-first generation workflow
    ├── int-hotfix-management.md         ◄── Production incident hotfix workflow
    ├── int-brd-ingestion.md             ◄── Ingestion & baseline creation workflow
    ├── int-production-incident.md       ◄── Incident triage & classification workflow
    └── int-release-management.md        ◄── Spec release transition workflow
```

---

## 4. Workspace Folder Management & Project Setup

When initializing a project workspace, `int-project-setup` executes the **Technology & Architecture Discovery Gate**, copies the Control Plane (`rules/` and `workflows/`), copies local project skills into `.agents/skills/`, writes `AGENTS.md`, creates `.gitignore`, and builds the complete execution folder hierarchy:

```text
<workspace-root>/
├── .agent/                      ◄── Dynamic Control Plane (rules/ & workflows/)
│   ├── rules/
│   │   ├── int-standards.md
│   │   ├── auto-log.md
│   │   └── .agentignore
│   └── workflows/
│       ├── int-pr-gate-workflow.md
│       ├── int-code-review.md
│       ├── int-project-from-brd.md
│       ├── int-generate-tests.md
│       ├── int-hotfix-management.md
│       ├── int-brd-ingestion.md
│       ├── int-production-incident.md
│       └── int-release-management.md
├── .agents/                     ◄── Local Project-Level Skills (Vendor-Agnostic)
│   └── skills/
│       ├── int-project-setup/
│       ├── int-sdd-lifecycle/
│       ├── int-brd-ingestion/
│       ├── int-incident-management/
│       ├── int-hotfix-management/
│       ├── int-release-management/
│       └── int-session-continuation/
├── .ai-context/                 ◄── Complete Project Knowledge Base
│   ├── decisions/
│   ├── hotfixes/
│   ├── incidents/
│   ├── plans/
│   ├── releases/
│   ├── specs/
│   ├── tasks/
│   ├── templates/
│   ├── test_cases/
│   ├── architecture.md
│   ├── brd-change-log.md
│   ├── BRD.md
│   ├── constitution.md
│   ├── project_context.md
│   ├── prompt_history.md
│   └── status.md
├── docs/                        ◄── BRD & Raw Input Documents
├── src/                         ◄── Execution layer (Derived 100% dynamically at runtime per selected stack post-approval)
├── tests/                       ◄── Test layer (Derived 100% dynamically at runtime per selected stack post-approval)
├── .gitignore                   ◄── Automated Git Exclusion Rules
└── AGENTS.md                    ◄── Project Vendor-Agnostic AI Policy

```

---

## 5. Dynamic Control Plane (`.agent/`)

The `.agent/` folder lives at the root of every project workspace and contains standard organizational rules and step-by-step workflows.

### Dynamic Discovery & Auto-Sync Protocol
1. **No Hardcoded Lists**: The setup and continuation skills MUST dynamically scan `skills/int-project-setup/resources/INT-Control-Plane/.agent/` and copy ALL rule and workflow files into `.agent/`.
2. **Self-Healing on Restart**: Whenever a session starts or resumes, the agent checks if any new workflow or rule file exists in the master Control Plane directory and automatically syncs it into the project's `.agent/` folder.

---

## 6. Complete Project Knowledge Base (`.ai-context/`)

The `.ai-context/` directory stores project memory. All artifact files inside subdirectories MUST be flat files (no subfolders per feature). All file links MUST be **relative to the repository root**.

```text
.ai-context/
├── change_requests/             ◄── Change Request docs (CR-<YYYYMMDD>-<slug>.md) & archived screenshot attachments
├── decisions/                   ◄── Architectural Decision Records (ADR-NNN.md)
├── hotfixes/                    ◄── Hotfix specs (HOTFIX-<slug>.spec.md)
├── incidents/                   ◄── Incident logs (INC-YYYY-NNN.md)
├── plans/                       ◄── Flat plans (<slug>.plan.md)
├── releases/                    ◄── Release notes (RELEASE-vX.Y.Z.md)
├── specs/                       ◄── Flat feature specs (<slug>.spec.md)
├── tasks/                       ◄── Flat task breakdowns (<slug>.tasks.md)
├── templates/                   ◄── Mandatory artifact templates
│   ├── spec.template.md
│   ├── plan.template.md
│   ├── tasks.template.md
│   ├── test_cases.template.md
│   ├── adr.template.md
│   ├── incident.template.md
│   ├── hotfix-spec.template.md
│   ├── release.template.md
│   ├── change-request.template.md
│   └── gate-review-dashboard-design.html
├── test_cases/                  ◄── Flat test case specs (<slug>.test_cases.md)
├── architecture.md              ◄── System design & module boundaries
├── assumptions.md               ◄── Project assumptions & dependencies tracking file
├── brd-change-log.md            ◄── BRD revision history & delta impact log
├── BRD.md                       ◄── Authoritative BRD baseline
├── constitution.md               ◄── Non-functional baseline & constraints
├── project_context.md           ◄── Technology stack & environment parameters
├── prompt_history.md            ◄── Turn-by-turn execution log & CR links (STRICT APPEND-ONLY)
└── status.md                    ◄── Live Project Status Board
```

---

## 7. Complete Sub-Skills Specifications (All 7 Sub-Skills)

Below are the exact, production-ready specifications for all 7 sub-skills. Save each file in your AI tool's skills path under `<skill-name>/SKILL.md`.

---

### Skill 1: `int-project-setup`
**Location:** `<skills-root>/int-project-setup/SKILL.md`

```markdown
---
name: int-project-setup
description: Initialize a new software project using the exact INT AI-First standard Control Plane, project-specific AI context, technology-aware execution structure, and foundational repository baseline.
---

# INT AI-First Project Setup

## Purpose
Initialize a new software project according to the INT AI-First development architecture.
This skill is responsible for project initialization and baseline setup only.

---

# MANDATORY STEP 1 — Technology & Architecture Discovery Gate, User Confirmation Halt & Zero-Harm Codebase Protocol

Before generating execution layer folders or creating baseline files, the agent MUST inspect the project root on disk first (`list_dir`, `view_file`):

1. **Zero-Harm Existing Codebase Guarantee**:
   - Under NO circumstances will `int-project-setup` modify, delete, overwrite, re-structure, or harm any existing source code files, codebase folders, or ongoing project architectures. Existing codebase files are 100% preserved and untouched.

2. **Existing Codebase User Confirmation Halt**:
   - If source code or build configuration files (`package.json`, `pom.xml`, `go.mod`, `Cargo.toml`, `requirements.txt`, `src/`, `controllers/`, `routes/`, `models/`, etc.) exist on disk, the agent MUST **HALT** and present an explicit prompt to the user:
     - **Option 1 (Recommended)**: *Auto-Detect from Codebase* — Inspect codebase files/folders to populate `project_context.md` and `architecture.md` without asking redundant stack questions.
     - **Option 2**: *Manual Stack Entry* — Answer interactive discovery questions to manually define or verify technology stack & architecture parameters.
   - **Reviewer Roster Check**: Inspect `.ai-context/project_context.md` / `constitution.md` for Gate 0, Gate 1, and Gate 2 reviewer emails. If missing, prompt the user ONLY for missing reviewer emails.

3. **Clean / Empty Project Interactive Discovery Protocol (New Projects Only)**:
   If the project directory is empty or contains no code/build manifests, prompt the user to confirm foundational parameters:
   - **Project Name & Type**: Full Stack, Frontend Only, Backend Only, or Mobile.
   - **Architecture Style**: Monolithic, Modular Monolith, Microservices, Clean Architecture, Feature-Sliced Architecture (FSD), etc.
   - **Frontend Technology & Styling**: (e.g., React / Vite + Vanilla CSS or Tailwind).
   - **Backend Technology & Framework**: (e.g., Node.js Express / Python FastAPI / Go).
   - **Database & Data Access / ORM Layer**: (e.g., PostgreSQL + Sequelize / Prisma / Mongoose).
   - **Authentication & Security Strategy**: (e.g., JWT / OAuth2).
   - **Deployment Target**: (e.g., Docker / AWS / Vercel).
   - **Gate 0 / Gate 1 / Gate 2 Reviewers**: Assigned PMs, Tech Leads, and Senior Developers.

Immediately populate `.ai-context/project_context.md` and `.ai-context/architecture.md` with these confirmed choices and assigned reviewer lists.

---

# MANDATORY STEP 2 — Automated CLI-Driven Scaffolding & Dynamic Architecture Sync Protocol

1. **EXISTING CODEBASE BYPASS**: If code or build manifests exist on disk, **BYPASS CLI SCAFFOLDING ENTIRELY** (for both Option 1 and Option 2). Preserve all existing files, entry points, and structures untouched. Perform dynamic architecture inspection (`list_dir`) and document current entry points in `.ai-context/architecture.md`.
2. **CLEAN PROJECT AUTOMATED CLI SCAFFOLDING**: If project is clean/empty, automatically execute official framework CLI initializer commands (`npx create-vite`, `npx @nestjs/cli`, `dotnet new`, `django-admin`, `cargo new`, `composer create-project`, `flutter create`, `curl start.spring.io`) via CMD / terminal matching the confirmed setup plan.
3. **NO MANUAL CONFIRMATION PROMPT FOR CLEAN PROJECTS**: Scaffolding for clean projects is executed automatically via CLI without pausing for folder structure approval.
4. **DYNAMIC ARCHITECTURE DOCUMENTATION**: Immediately inspect generated/existing disk structure (`list_dir`) and populate `.ai-context/architecture.md` and `.ai-context/project_context.md` with the scanned real directory hierarchy, entry points, and module boundaries.




---

# CRITICAL RULE — INT CONTROL PLANE (DYNAMIC COPY & SYNC)

The following directory is the authoritative source for `skills/int-project-setup/resources/INT-Control-Plane/.agent/`

```text
skills/int-project-setup/resources/INT-Control-Plane/.agent/`

You MUST dynamically copy the entire contents of this directory into the project root `.agent/`.

## Control Plane Folder Structure To Copy:
```text
skills/int-project-setup/resources/INT-Control-Plane/.agent/
├── rules/
│   ├── int-standards.md
│   ├── auto-log.md
│   └── .agentignore
└── workflows/
    ├── int-code-review.md
    ├── int-project-from-brd.md
    ├── int-generate-tests.md
    ├── int-hotfix-management.md
    ├── int-brd-ingestion.md
    ├── int-production-incident.md
    └── int-release-management.md
```

## Dynamic Copying Protocol (Native OS Bulk Direct Copy):
1. **HIGH-SPEED NATIVE OS BULK COPY**: Execute bulk direct file copying via native OS terminal commands (`run_command` via PowerShell `Copy-Item`) for Control Plane assets (`.agent/`), Knowledge Base templates (`.ai-context/templates/`), and local project sub-skills (`.agent/skills/`). Prohibit LLM line-by-line file generation for static assets.
2. **STRICT SKILL EXCLUSION RULE**: When copying sub-skills into `.agent/skills/`, copy **ONLY the skill directory and its `SKILL.md` file**, strictly excluding nested subdirectories (such as `resources/` or `image/`).
3. **Master Admin Workflow Exclusion**: The master global sync workflow `int-sync-global-skills.md` is strictly restricted to master global administration. It MUST NEVER be shipped, copied, or exposed inside user project repositories under `.agent/workflows/`.


---

# Project Knowledge Base Setup

Create the project-specific AI context directory structure under `.ai-context/`:
```text
.ai-context/
├── decisions/
├── hotfixes/
├── incidents/
├── plans/
├── releases/
├── specs/
├── tasks/
├── templates/
├── test_cases/
├── architecture.md
├── assumptions.md
├── brd-change-log.md
├── BRD.md
├── constitution.md
├── project_context.md
├── prompt_history.md
└── status.md
```
```

---

### Skill 2: `int-sdd-lifecycle`
**Location:** `<skills-root>/int-sdd-lifecycle/SKILL.md`

```markdown
---
name: int-sdd-lifecycle
description: Manage the end-to-end INT SDD feature development lifecycle, including feature specs, status board updates, Gate 1 peer reviews, implementation plans, task breakdown, test-first TDD, Gate 2 code reviews, and Definitions of Ready & Done.
---

# INT SDD Feature Engineering Lifecycle

## Scope Restriction — PR Gate Workflow Enhancement
- **Before Spec Generation (🔒 NO CHANGE)**: BRD, setup, permissions, discovery remain 100% unchanged.
- **Spec Generation → Gate 1 → Development → Gate 2 (✅ ENHANCEMENT ALLOWED)**: Non-blocking parallel specs, reviewer detection, decision prompts, work selection, standardized review templates, role validation, dashboard sync.
- **After Gate 2 Approval (🔒 NO CHANGE)**: Release Management, CRs, Hotfixes, deployment downstream remain 100% unchanged.

## Non-Blocking Parallel Spec Generation
Multiple specs exist and progress independently. Pending Gate 1 reviews do not block drafting or progressing other specs.

## Reviewer Detection After Git Pull
Detect Git user identity (`git config user.name`/`user.email`). If user is assigned reviewer, prompt:
> **"You are assigned as a PR reviewer. What would you like to do?"**
> **[ Option 1 — Review Pending Specs ]**
> **[ Option 2 — Work on Approved Specs ]**

- **Option 1 — Review Pending Specs**: Filter assigned specs waiting for review based on reviewer role (Gate 1, Gate 2, or Both in same listing) -> execute interactive Q&A review evaluating BOTH Feature Spec (`.spec.md`) and linked BRD Requirement (`.ai-context/BRD.md#BRD-NNN`) -> save dedicated review file under `.ai-context/pr_reviews/` -> validate Git user credentials -> Approve / Reject / Request Changes -> 5-artifact sync (`pr_reviews/`, `dashboard.html`, `.spec.md`, `status.md`, `prompt_history.md`) -> loop back to remaining pending PR reviews.
- **Option 2 — Work on Approved Specs**: Display eligible Gate 1 approved specs -> developer selects spec -> execute Pre-Development PR Review Check (`.ai-context/pr_reviews/GATE1-<slug>-*.md`).
  - **If Approved**: Notify developer of reviewer comments -> proceed through Plan (`.plan.md`) -> Tasks (`.tasks.md`) -> Test Cases (`.test_cases.md`) -> TDD RED -> GREEN -> Submit for Gate 2 Review.
  - **If Rejected / Changes Requested**: **STRICTLY BLOCK** planning and code development -> display rejection feedback -> require author to update `.spec.md` and re-submit for Gate 1 approval.

## Standardized Review Templates & 5-Artifact Synchronization
- **Interactive Q&A Review Process**: Gathers reviewer criteria evaluations (including BRD Traceability & Alignment for Gate 1), review description, and line-item comments followed by approval outcome.
- **Dedicated Review Files (`.ai-context/pr_reviews/`)**: Saved under `GATE1-<slug>-<timestamp>.md` or `GATE2-<slug>-<timestamp>.md`.
- **Single Source of Truth**: Dashboard HTML stores complete PR review record. Spec stores current workflow state. Synchronize across 5 core artifacts simultaneously on review completion.
```

---

### Skill 3: `int-brd-ingestion`
**Location:** `<skills-root>/int-brd-ingestion/SKILL.md`

```markdown
---
name: int-brd-ingestion
description: Manage BRD document ingestion from docs/, maintain .ai-context/BRD.md requirement baselines, analyze BRD changes via brd-change-log.md, and generate Gate 1-approved business module structures.
---

# INT BRD Ingestion & Business Module Generation

Client BRD source documents must be placed under `docs/` (`.pdf`, `.docx`, `.md`).
`.ai-context/BRD.md` is the authoritative requirement baseline for downstream SDD work.

## Mandatory BRD Template Sections
`Dependencies`, `Non-Functional Requirements (NFRs)`, `Out of Scope`, and `Assumptions` are strictly mandatory must-have sections in `.ai-context/BRD.md` alongside Objectives, Scope, Actors, Functional Requirements (IDed), Business Rules, Open Questions, and Acceptance Criteria.

## BRD Processing Flow
1. Parse client BRD under `docs/`.
2. Generate `.ai-context/BRD.md` with mandatory sections (`Objectives`, `Scope`, `Actors`, `Functional Requirements`, `NFRs`, `Business Rules`, `Dependencies`, `Assumptions`, `Out of Scope`, `Open Questions`, `Acceptance Criteria`).
3. Generate **`.ai-context/assumptions.md`** at the time of `.ai-context/BRD.md` creation/update for Gate 0 PR review approval.
4. **Continuous Assumptions Sync**: Whenever there is any update or revision in the existing BRD, `.ai-context/assumptions.md` MUST be updated as well along with `BRD.md`, `brd-change-log.md`, and all other parameters.
5. Track revisions in `.ai-context/brd-change-log.md`.
6. Extract project constitution constraints into `.ai-context/constitution.md`.
7. **Mandatory Reviewer Open Questions Resolution**: If there are any pending/open questions listed in `.ai-context/BRD.md` under `Open Questions`, the assigned PR reviewer MUST provide explicit answers and resolutions for each question during Gate 0 PR review. Reviewer answers are recorded in `.ai-context/pr_reviews/BRD-<timestamp>.md` and updated in `.ai-context/BRD.md`.
8. Require explicit Gate 0 BRD PR Review approval before downstream spec generation or module directory derivation.
```

---

### Skill 4: `int-incident-management`
**Location:** `<skills-root>/int-incident-management/SKILL.md`

```markdown
---
name: int-incident-management
description: Manage production incident reporting, triage, classification (Spec Gap vs Defect vs New Requirement), incident artifact logging under .ai-context/incidents/, and resolution tracking.
---

# INT Incident Management & Triage

1. When a production issue or bug report is received, generate `.ai-context/incidents/INC-YYYY-NNN.md` from `incident.template.md`.
2. Perform Root Cause Analysis (RCA) and log exact error traces/logs.
3. **Classify Incident**:
   - **Spec Gap**: Route to update existing `.spec.md` -> Gate 1 review.
   - **Implementation Defect**: Route to `int-hotfix-management` -> Author `HOTFIX-<slug>.spec.md`.
   - **New Requirement**: Route to `int-brd-ingestion` -> Ingest into `.ai-context/BRD.md`.
```

---

### Skill 5: `int-hotfix-management`
**Location:** `<skills-root>/int-hotfix-management/SKILL.md`

```markdown
---
name: int-hotfix-management
description: Manage emergency hotfix workflows for production incidents, maintain HOTFIX-<incident-slug> artifacts, author compressed hotfix specs, execute test-first fixes, and enforce post-hoc Gate 2 reviews.
---

# INT Hotfix Engineering Workflow

1. Author compressed hotfix spec under `.ai-context/specs/hotfix-<slug>.spec.md` derived from `hotfix-spec.template.md`.
2. Create dedicated hotfix branch `fix/<incident-slug>`.
3. **TDD RED**: Write automated failing test reproducing the production defect.
4. **Implementation GREEN**: Apply minimum surgical fix to pass test.
5. **Post-Hoc Gate 2 Review**: Present diff, test results, and post-mortem analysis for human sign-off.
```

---

### Skill 6: `int-release-management`
**Location:** `<skills-root>/int-release-management/SKILL.md`

```markdown
---
name: int-release-management
description: Manage software releases, verify spec release readiness in status.md, transition specs to Released (vX.Y.Z), generate spec-driven release notes, and sync releases via CI/CD.
---

# INT Release Management Workflow

1. **Verify Release Readiness**: Inspect `.ai-context/status.md` to ensure all target specs are marked `Ready for Release` with Gate 2 approvals.
2. **Generate Release Artifact**: Create `.ai-context/releases/RELEASE-vX.Y.Z.md` derived from `release.template.md`.
3. **Transition Spec Statuses**: Update active specs in `status.md` and `.spec.md` files to `Released (vX.Y.Z)`.
4. **Git Tagging**: Tag release commit as `vX.Y.Z`.
```

---

### Skill 7: `int-session-continuation`
**Location:** `<skills-root>/int-session-continuation/SKILL.md`

```markdown
---
name: int-session-continuation
description: Resume context and continue engineering tasks across session restarts by reading repository persistent memory (.ai-context/status.md, specs, plans, tasks, git status).
---

# INT Session Continuation & State-Driven Resume Engine

Before continuing work after a new session, workstation restart, or Git pull, execute the **State Reconstruction Chain**:

```text
Inspect Repository Memory (.ai-context/status.md, dashboard.html, project_context.md)
       ↓
Inspect BRD & Gate 0 Review State (.ai-context/BRD.md, pr_reviews/BRD-*.md)
       ↓
Inspect Manual BRD Changes (Git diff vs last reviewed commit/record)
       ↓
Inspect All Feature Specs & Gate 1 Status (.ai-context/specs/*.spec.md, pr_reviews/GATE1-*.md)
       ↓
Present Multi-Spec Interactive Selection Table (If BRD & Gate 0 Approved)
       ↓
Inspect Selected Spec Downstream State (.plan.md, .tasks.md, .test_cases.md)
       ↓
Inspect Development & TDD State (tests/, src/, incomplete - [ ] tasks)
       ↓
Inspect Gate 2 Review State (pr_reviews/GATE2-*.md)
       ↓
Inspect Release State & Release Artifacts (.ai-context/releases/)
       ↓
Inspect Git Working Tree & PR Comments (Branch, commit, working tree diff)
       ↓
Validate Workflow Consistency & Present Action Options to Developer
       ↓
Wait for Developer Confirmation & Execute Selected Action
```

# Handling Existing & Ongoing Projects (Retrofit & Auto-Sync)
- **Automatic Control Plane Upgrade**: Step 2 inspects `.agent/rules/` and `.agent/workflows/` and copies missing files from `skills/int-project-setup/resources/INT-Control-Plane/.agent/` without overwriting existing code.
- **Immediate Governance**: Global rules (`GEMINI.md`) apply universally to all old/existing projects.
```

---

### Skill 8: `int-reverse-engine-workflow`
**Location:** `<skills-root>/int-reverse-engine-workflow/SKILL.md`

```markdown
---
name: int-reverse-engine-workflow
description: Reverse engineer an existing, ongoing, or legacy codebase to generate authoritative .ai-context/BRD.md and .ai-context/assumptions.md baselines for Gate 0 PR review approval.
---

# INT Reverse Engineering & Codebase Discovery Workflow

Analyze an existing, ongoing, or legacy codebase across **ALL workspace directories and subdirectories** (without restricting to specific folder names like `src/` or `controllers/`) and reverse-engineer its current implementation into an SDD-ready state.

## Core Execution Protocol
1. **Tier 1 Workspace Scan & Filename Pattern Identification (0 LLM Tokens)**: Recursively inspect ALL workspace directories (excluding ONLY build/dependency dirs like `node_modules/`, `dist/`, `build/`, `.git/`, `vendor/`, `bin/`, `obj/`, `target/`, binaries, lockfiles). Dynamically identify artifact roles by **Filename, Extension, and Pattern Signature**:
   - Routes/Controllers: `*controller*`, `*Handler*`, `*router*`, `views.py`, `@Controller`, `@RestController`, `route.ts`, `web.php`
   - Services/Logic: `*service*`, `*provider*`, `*usecase*`, `*repository*`, `*dao*`, `*Helper*`
   - Models/Schemas: `*model*`, `*entity*`, `*schema*`, `*.sql`, `*Dto*`, `models.py`, `structs`
   - UI Components: `*.jsx`, `*.tsx`, `*.vue`, `*.svelte`, `*Component*`, `*Page*`, `*View*`, `*Screen*`
   - Build Configs: `package.json`, `pom.xml`, `go.mod`, `Cargo.toml`, `requirements.txt`, `composer.json`, `build.gradle`, `.env*`, `application.yml`, `settings.py`, `Program.cs`
2. **Tier 2 Targeted LLM Semantic Extraction**: Analyze Tier 1 inventory + primary entry points to synthesize functional requirements (`BRD-XXX`), mandatory `Dependencies`, `NFRs`, `Out of Scope`, and `Business Rules`.
3. **Evidence Classification**: Classify items as `Confirmed Code`, `Confirmed DB`, `Inferred`, or `Unknown / Human Confirmation Required`.
4. **Baseline File & Dashboard Generation**: Generate `.ai-context/BRD.md` with all 11 mandatory sections, `.ai-context/assumptions.md` with technical/business uncertainties, and **generate/update `.ai-context/dashboard.html` BEFORE requesting Gate 0 PR review approval**.
5. **Existing Ingestion Handoff & Gate 0 Review**: Delegate baseline registration to `int-brd-ingestion`, log change in `brd-change-log.md`, and trigger Gate 0 PR review presenting `BRD.md`, `assumptions.md`, and `dashboard.html`, requiring the assigned PM/TL reviewer to answer all pending open questions before approval.
```

---

## 8. Client Observations & Multi-Spec Change Protocol

When client feedback, screenshots, or bug reports arrive:

### Prompt Keyword Classification Rule
- **Formal Change Request**: The system ONLY triggers the formal Change Request workflow (creating `.ai-context/change_requests/`, drafting Spec updates, and requiring Gate 1 re-approval) if the user prompt explicitly contains the keyword phrase **"Change Request"** (or `"CR"`).
- **Development-Related Fix / UI Bug Fix**: If the prompt does NOT contain the specific words **"Change Request"**, the system treats the request as a **Development-Related Fix** (such as small UI alignment bugs, styling fixes, edge case corrections, or minor dev tweaks) under the active approved spec without forcing a Spec update or Gate 1 re-approval.

```text
Client Feedback / Bug Report / Screenshot
                 ↓
Contains "Change Request"?
 ├── YES ──► Create CR Doc ──► Multi-Spec Impact Matrix ──► Spec Update ──► HALT for Gate 1 Approval
 └── NO  ──► Development-Related Fix ──► Apply Fix under Active Approved Spec (TDD RED → GREEN) ──► Gate 2 Review
```

---

## 9. Session Recovery & Retrofitting Existing Projects

1. **Self-Healing Sync**: When opening any existing or legacy project, `int-session-continuation` automatically syncs missing rules and workflows into `.agent/` from the global master template.
2. **Non-Destructive Baseline**: If `.ai-context/` is missing, `int-project-setup` creates `.ai-context/` baseline artifacts while preserving 100% of existing application code (`src/`, `tests/`).
