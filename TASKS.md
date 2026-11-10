# mao-shell — Development Tasks

> Central task tracking document for the incremental development of mao-shell.

## 1. Purpose

This document tracks actionable development work, dependencies, priorities, validation requirements and task completion.

It works together with `ROADMAP.md`:

- `ROADMAP.md` defines the project's development direction and major milestones.
- `TASKS.md` defines the current actionable work and its status.
- `CHANGELOG.md` records implemented changes and project history.

This document follows the project's progressive planning strategy.

Only the active phase should contain a detailed implementation task breakdown. Future phases remain unplanned until they are ready to begin.

## 2. Task Management Principles

All tasks must follow these principles:

- Tasks must be concrete, actionable and independently verifiable whenever possible.
- Each task must have a clear objective and completion criteria.
- Dependencies must be identified before implementation.
- Validation must be appropriate to the task's risk and impact.
- Completed tasks must have supporting validation evidence where applicable.
- Tasks must not be marked complete merely because implementation has finished.
- Existing project specifications and architectural decisions must be respected.
- Unplanned scope expansion must be avoided.
- The task list must reflect the actual repository state.

## 3. Task Status

Use the following statuses consistently.

| Status | Meaning |
|---|---|
| `PENDING` | Not started. |
| `IN_PROGRESS` | Currently being implemented. |
| `BLOCKED` | Cannot proceed because of an unresolved dependency or issue. |
| `VALIDATION` | Implementation is complete and undergoing validation. |
| `COMPLETED` | Acceptance criteria have been satisfied and required validation has passed. |
| `CANCELLED` | Explicitly removed from the project scope. |

A task must not be marked `COMPLETED` while required validation remains outstanding.

## 4. Priority Levels

| Priority | Meaning |
|---|---|
| `CRITICAL` | Required to protect project integrity, security or essential functionality. |
| `HIGH` | Required for the active milestone or a major dependency. |
| `MEDIUM` | Important but does not immediately block progression. |
| `LOW` | Useful improvement that can be deferred. |

Priority must reflect actual project impact, not implementation convenience.

## 5. Task Format

Every actionable task should use this structure:

```markdown
### [PHASE-ID] Task title

- **ID:** P0-001
- **Status:** PENDING
- **Priority:** HIGH
- **Dependencies:** None
- **Description:** What must be accomplished.
- **Acceptance criteria:**
  - [ ] Verifiable requirement.
  - [ ] Verifiable requirement.
- **Validation:**
  - Required checks.
- **Evidence:** Link to validation report or relevant artifact, when applicable.
- **Notes:** Relevant implementation decisions or limitations.
```

Use stable task IDs. Do not reuse IDs after a task has been cancelled or removed.

## 6. Overall Phase Status

The following table provides a high-level overview of project progression.

| Phase | Name | Planning status | Progress |
|---|---|---|---|
| P0 | Project Foundation | IN PROGRESS | 14% (1 of 7 tasks) |
| P1 | Core Architecture | NOT PLANNED | — |
| P2 | Shell Foundation | NOT PLANNED | — |
| P3 | System Integrations | NOT PLANNED | — |
| P4 | Core User Experience | NOT PLANNED | — |
| P5 | Visual Refinement | NOT PLANNED | — |
| P6 | Reliability and Optimization | NOT PLANNED | — |
| P7 | Installation and Release | NOT PLANNED | — |

Progress percentages must reflect completed tasks in the relevant phase, not estimated effort or elapsed time.

A phase must not be treated as complete until its milestone requirements in `ROADMAP.md` have been satisfied.

---

# 7. Active Phase — P0: Project Foundation

**Objective:** Establish a reproducible, organized and validated foundation for mao-shell development.

**Phase status:** IN PROGRESS (P0-001 completed)

## P0-001 — Inspect the Development Environment

- **Status:** COMPLETED
- **Priority:** HIGH
- **Dependencies:** None

**Description:**

Inspect the existing Arch Linux development environment and identify the installed tools and relevant runtime dependencies.

**Acceptance criteria:**

- [x] Installed Quickshell version is identified.
- [x] Relevant Qt and QML runtime versions are identified.
- [x] Wayland and Hyprland environment are verified.
- [x] Available development and validation tools are inventoried.
- [x] Missing dependencies are documented.
- [x] No packages are installed or system configuration modified without authorization.

**Validation:**

- Verify the availability and versions of required executables.
- Confirm the current desktop session and runtime environment.
- Record relevant compatibility limitations.

**Evidence:** Environment inspection report (2026-11, read-only, no changes made):

- Quickshell 0.3.2-1 (Arch package), `/usr/bin/qs`. Linked to Qt6 (Quick, Qml, Wayland Client, Widgets, Network). No instance running. `qs list` reports no `shell.qml` in the default config path.
- Qt 6.12.0: `qt6-base-6.12.0-3`, `qt6-declarative-6.12.0-4`, `qt6-wayland-6.12.0-1`. Note: `qmllint --version` prints `1.0`, which is the tool's own version, not the Qt version.
- Session: Wayland (`XDG_SESSION_TYPE=wayland`), `XDG_CURRENT_DESKTOP=Hyprland`, kernel 7.2.14-arch1-1.
- Hyprland 0.57.0-3, reached through `hyprctl`. One monitor, HDMI-A-1, 3440x1440@59.999 Hz, scale 1. A top reserved area of 39 px is present; its source was not identified.
- Tools present: wallust 3.5.2, qmllint (qt6-declarative), ShellCheck 0.11.0, cava 1.0.0, flameshot 14.0.0, hyprlock 0.9.7, dunst 1.13.2, cliphist 0.7.0, wl-clipboard 2.3.0, yay 13.1.0, PipeWire 1.6.10, WirePlumber 0.5.18, BlueZ 5.87, NetworkManager 1.58.1, grim 1.5.0, slurp 1.5.0.
- Missing: `checkupdates` (pacman-contrib). This affects the non-root update check (COMPONENT_SPECS §17), to be resolved in Phase 3.
- Limitations: no Quickshell API has been verified yet; that belongs to P0-003.

## P0-002 — Inspect and Establish Repository Structure

- **Status:** PENDING
- **Priority:** HIGH
- **Dependencies:** P0-001

**Description:**

Inspect the existing repository and establish the initial directory structure according to the project's architecture and documentation.

**Acceptance criteria:**

- [ ] Existing files and directories have been inspected.
- [ ] Project structure is consistent with `DESIGN_PATTERNS.md`.
- [ ] Application entry point location is established.
- [ ] Core, services, features and reusable UI responsibilities have clear locations.
- [ ] Documentation and development tooling locations are established.
- [ ] No existing user data or unrelated configuration is overwritten.

**Validation:**

- Inspect the resulting directory tree.
- Verify that each directory has a defined responsibility.
- Identify unnecessary duplication or premature abstractions.

**Evidence:** Repository structure review.

**Notes:** The canonical repository layout is defined in `DESIGN_PATTERNS.md` §20 (audit finding S-01). Task status remains `PENDING` until the structure is implemented and reviewed.

## P0-003 — Establish the Minimal Quickshell Entry Point

- **Status:** PENDING
- **Priority:** HIGH
- **Dependencies:** P0-001, P0-002

**Description:**

Create the initial Quickshell entry point and verify that the application can load a minimal QML shell.

**Acceptance criteria:**

- [ ] The application entry point is established as `shell.qml`.
- [ ] The minimal QML structure follows the installed Quickshell API.
- [ ] The shell loads without QML errors.
- [ ] Application startup and shutdown behave correctly.
- [ ] The initial implementation does not introduce unnecessary features.
- [ ] The existing desktop session remains usable during development.

**Validation:**

- Launch the minimal shell.
- Inspect startup logs.
- Verify clean shutdown.
- Check for QML warnings and runtime errors.

**Evidence:** Initial runtime validation report.

## P0-004 — Establish Development Conventions

- **Status:** PENDING
- **Priority:** HIGH
- **Dependencies:** P0-002

**Description:**

Establish practical development conventions for source files, naming, imports, code organization and Git workflow.

**Acceptance criteria:**

- [ ] Naming conventions are consistent.
- [ ] QML and JavaScript conventions are documented or referenced.
- [ ] File organization follows the established architecture.
- [ ] Git practices avoid committing temporary or generated files unnecessarily.
- [ ] Existing agent instructions are consistent with the project conventions.

**Validation:**

- Review conventions against `AGENTS.md` and `DESIGN_PATTERNS.md`.
- Verify that the conventions are practical for the current repository.

**Evidence:** Development conventions review.

## P0-005 — Establish Basic Validation Tooling

- **Status:** PENDING
- **Priority:** HIGH
- **Dependencies:** P0-001, P0-002

**Description:**

Identify and establish the initial validation commands and procedures for the project.

**Acceptance criteria:**

- [ ] Available QML validation options are identified.
- [ ] Basic syntax and runtime checks are documented.
- [ ] Application launch validation is repeatable.
- [ ] Logging and diagnostic procedures are documented.
- [ ] Validation commands do not require unnecessary system modifications.

**Validation:**

- Execute the available checks against the initial project.
- Verify that errors can be identified and reported.
- Document limitations of the available tooling.

**Evidence:** Initial validation procedure.

## P0-006 — Establish Repository Hygiene

- **Status:** PENDING
- **Priority:** MEDIUM
- **Dependencies:** P0-002

**Description:**

Prepare the repository for controlled development and version tracking.

**Acceptance criteria:**

- [ ] `.gitignore` covers appropriate temporary and generated files.
- [ ] Project documentation is included in version control.
- [ ] No secrets, credentials or personal data are committed.
- [ ] Repository changes are reviewable.
- [ ] The working tree is checked before and after significant changes.

**Validation:**

- Inspect tracked and untracked files.
- Review ignore rules.
- Check for accidentally included sensitive data.

**Evidence:** Repository hygiene review.

## P0-007 — Phase 0 Review and Completion

- **Status:** PENDING
- **Priority:** HIGH
- **Dependencies:** P0-001, P0-002, P0-003, P0-004, P0-005, P0-006

**Description:**

Perform a final review of the project foundation and verify that all Phase 0 objectives have been achieved.

**Acceptance criteria:**

- [ ] All required Phase 0 tasks are completed.
- [ ] The minimal Quickshell application launches successfully.
- [ ] Development and validation procedures are documented.
- [ ] Repository structure is consistent with project architecture.
- [ ] No unresolved critical blockers remain.
- [ ] Relevant documentation has been updated.
- [ ] Phase completion is recorded.

**Validation:**

- Perform a clean validation run.
- Review repository structure and development procedures.
- Review all Phase 0 evidence.
- Confirm that Phase 1 prerequisites are satisfied.

**Evidence:** Phase 0 completion report.

---

# 8. Future Phase Planning

Future phases are intentionally not broken down into implementation tasks.

Their planning status will be updated when the project is ready to begin them.

| Phase | Name | Planning status |
|---|---|---|
| P1 | Core Architecture | NOT PLANNED |
| P2 | Shell Foundation | NOT PLANNED |
| P3 | System Integrations | NOT PLANNED |
| P4 | Core User Experience | NOT PLANNED |
| P5 | Visual Refinement | NOT PLANNED |
| P6 | Reliability and Optimization | NOT PLANNED |
| P7 | Installation and Release | NOT PLANNED |

Before starting a future phase:

1. Review its objectives in `ROADMAP.md`.
2. Inspect the current implementation.
3. Identify dependencies and architectural constraints.
4. Define detailed tasks and acceptance criteria.
5. Identify relevant validation procedures.
6. Update this document before implementation begins.

---

# 9. Task Lifecycle and Agent Responsibilities

The development agent must maintain this document as work progresses.

### Before starting a task

- Confirm that its dependencies are satisfied.
- Review relevant project documentation.
- Inspect the current implementation.
- Confirm that its scope and acceptance criteria are clear.
- Mark the task `IN_PROGRESS`.

### During implementation

- Keep changes focused on the task.
- Avoid unrelated refactoring.
- Record relevant technical decisions.
- Identify blockers as soon as they appear.
- Do not silently expand the task scope.

### After implementation

- Mark the task `VALIDATION`.
- Execute the required validation procedures.
- Resolve identified issues or document blockers.
- Record relevant evidence.
- Update related documentation.

### On completion

- Verify all acceptance criteria.
- Mark the task `COMPLETED`.
- Update phase progress.
- Update `CHANGELOG.md` when the changes warrant a project history entry.
- Keep the roadmap and task tracking consistent.

### When blocked

- Mark the task `BLOCKED`.
- Explain the blocking condition.
- Identify the required decision, dependency or corrective action.
- Do not claim completion while the blocker remains unresolved.

## 10. Progress Reporting

At meaningful checkpoints, provide a concise progress report containing:

- Active phase.
- Current task.
- Completed tasks.
- Pending tasks.
- Blocked tasks.
- Validation results.
- Relevant risks.
- Recommended next task.

Progress reports must reflect repository evidence rather than assumptions.

## 11. Change Management

Tasks may be added, modified, split, merged or cancelled when implementation experience justifies it.

Changes must:

- Preserve stable task identifiers where possible.
- Document relevant scope changes.
- Maintain dependency consistency.
- Update phase progress.
- Remain consistent with `ROADMAP.md`.
- Respect approval requirements in `AGENTS.md`.

A cancelled task must remain identifiable in project history rather than having its identifier silently reused.

## 12. Definition of Done

A task is complete only when:

1. Its implementation satisfies the stated objective.
2. All applicable acceptance criteria pass.
3. Required validation has been performed.
4. No unresolved critical issue remains within its scope.
5. Relevant documentation has been updated.
6. The task status and evidence are recorded.

A phase is complete only when all required tasks and the corresponding milestone criteria in `ROADMAP.md` are satisfied.

---

## 13. Guiding Principle

**Plan the next phase in detail, execute in small verifiable steps, and keep the project state synchronized with reality.**

This task tracker is the operational source of truth for mao-shell development.