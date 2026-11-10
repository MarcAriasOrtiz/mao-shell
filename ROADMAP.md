# mao-shell — Development Roadmap

> A progressive development roadmap for mao-shell, a modular desktop shell built with Quickshell, QML and JavaScript for Arch Linux, Wayland and Hyprland.

## 1. Purpose

This document defines the long-term development direction of mao-shell.

It establishes the project's major development phases, their objectives, dependencies and completion criteria without prematurely defining every implementation task.

mao-shell follows a **progressive planning strategy**:

- All major development phases are defined in advance.
- Detailed tasks are created immediately before each phase begins.
- Future phases may be refined as technical knowledge and implementation experience evolve.
- Completed milestones must be validated before the project advances.
- Changes to scope and architecture must remain consistent with the project's specifications and design principles.

This roadmap is a living document and must be updated as the project evolves.

## 2. Project Direction

mao-shell aims to become a distinctive, reliable and highly customizable desktop shell for Arch Linux environments using Wayland and Hyprland.

Its development is guided by the following principles:

- Modular and maintainable architecture.
- Native Quickshell, QML and JavaScript implementation.
- First-class multi-monitor support.
- Responsive and accessible user interfaces.
- Consistent visual identity and customizable appearance.
- Reliable system integrations with graceful degradation.
- Low resource consumption and efficient rendering.
- Safe interaction with the host system.
- Progressive implementation supported by continuous validation.

The project must remain usable and maintainable throughout development.

## 3. Development Strategy

Development is organized into major phases.

Each phase follows the same lifecycle:

1. **Phase preparation:** Review the roadmap, specifications, design patterns and relevant skills.
2. **Task definition:** Create a detailed task breakdown with dependencies and acceptance criteria.
3. **Implementation:** Develop small, independently testable units.
4. **Validation:** Execute the appropriate static, automated, integration and runtime checks.
5. **Documentation:** Update technical documentation and implementation records.
6. **Phase review:** Evaluate completion criteria and identify outstanding risks.
7. **Progression:** Approve the next phase when its prerequisites are satisfied.

A phase must not be considered complete merely because its planned code has been written.

## 4. Development Phases

### Phase 0 — Project Foundation

**Objective:** Establish a reproducible and well-organized development environment and repository foundation.

Scope:

- Initialize and organize the project repository.
- Establish the initial directory structure.
- Configure the Quickshell entry point.
- Define project-level development conventions.
- Verify the installed Quickshell and Qt environment.
- Establish basic development and validation commands.
- Configure version control and repository hygiene.
- Verify that the initial shell can be launched and stopped safely.

Expected outcome:

A minimal, reproducible Quickshell project that serves as the foundation for further development.

Completion criteria:

- Project structure is established and documented.
- The entry point loads successfully.
- The development environment is verified.
- Basic validation procedures are available.
- No unintended modifications to the user's desktop configuration are introduced.

### Phase 1 — Core Architecture

**Objective:** Build the architectural foundation that all subsequent shell features will depend on.

Scope:

- Define the application bootstrap and lifecycle.
- Establish shared services and service ownership.
- Implement centralized configuration management.
- Establish the semantic design token system.
- Integrate Wallust as the primary color source, with fallback behavior.
- Define theme and visual profile infrastructure.
- Establish reusable core UI primitives.
- Implement multi-monitor discovery and lifecycle management.
- Define error handling and logging conventions.

Expected outcome:

A functional core architecture capable of supporting independent shell features without unnecessary coupling.

Completion criteria:

- Shared services have clear responsibilities.
- Configuration and theme systems are centralized.
- UI components consume consistent semantic tokens.
- Monitor discovery and lifecycle handling work reliably.
- The architecture supports incremental feature development.
- Critical architectural decisions are documented.

### Phase 2 — Shell Foundation

**Objective:** Implement the primary desktop shell surfaces and interaction infrastructure.

Scope:

- Main panel and bar infrastructure.
- Reusable popup and overlay systems.
- Surface positioning and sizing.
- Monitor-specific surface management.
- Navigation and interaction foundations.
- Keyboard and focus management.
- Surface visibility and lifecycle control.
- Responsive layouts and scaling behavior.

Expected outcome:

A stable shell foundation with functional surfaces and reusable interaction patterns.

Completion criteria:

- Main shell surfaces render correctly.
- Surface lifecycle and visibility behave predictably.
- Multiple monitors are supported.
- Keyboard interaction and focus behavior are functional.
- Normal desktop windows remain managed by Hyprland.
- Shell surfaces do not interfere with ordinary window management.

### Phase 3 — System Integrations

**Objective:** Establish reliable communication between mao-shell and the host desktop environment.

Scope:

- Hyprland integration.
- CPU, memory and GPU monitoring.
- Audio integration through PipeWire.
- Media controls through MPRIS.
- Network management integration.
- Bluetooth integration.
- Notification integration.
- Hardware discovery and hotplug handling.
- Capability detection and fallback mechanisms.

Expected outcome:

A set of reusable, independently managed services that expose reliable system state to the UI.

Completion criteria:

- Supported integrations operate correctly in the detected environment.
- Missing dependencies are handled gracefully.
- External processes execute asynchronously.
- Failures in one integration do not destabilize unrelated services.
- Resource usage remains controlled.
- Host-system modifications require explicit authorization.

### Phase 4 — Core User Experience

**Objective:** Deliver the main user-facing functionality of mao-shell.

Scope:

- Application launcher.
- Quick settings and system controls.
- Dashboard infrastructure.
- Wallpaper management.
- System status interfaces.
- Media and audio controls.
- Network and Bluetooth controls.
- User-facing shell configuration.
- Integration of the previously implemented system services.

Expected outcome:

A coherent and usable desktop shell experience built on the established architecture.

Completion criteria:

- Core interactions work end to end.
- UI state remains synchronized with actual system state.
- User actions provide appropriate feedback.
- Errors and unavailable capabilities are communicated clearly.
- Core features remain functional across supported monitor configurations.

### Phase 5 — Visual Refinement and Customization

**Objective:** Develop the visual identity and customization capabilities of mao-shell.

Scope:

- Minimal, Glass, Balanced and Compact visual profiles.
- Custom profile support.
- Typography and spacing refinement.
- Surface materials, transparency and blur.
- Consistent component styling.
- Animation and transition refinement.
- Responsive layout improvements.
- Accessibility and reduced-motion behavior.
- High-contrast and scaling options.

Expected outcome:

A visually cohesive, distinctive and customizable shell experience.

Completion criteria:

- Visual profiles are consistent across shell components.
- Theme changes propagate correctly.
- Components remain usable at supported scaling levels.
- Reduced-motion and accessibility settings are respected.
- Visual effects do not introduce unnecessary performance overhead.
- Customization does not compromise architectural consistency.

### Phase 6 — Reliability, Compatibility and Optimization

**Objective:** Improve stability, performance and operational reliability before distribution.

Scope:

- Regression testing.
- Runtime and integration testing.
- Multi-monitor and hotplug testing.
- Long-running stability and soak testing.
- CPU, memory and GPU profiling.
- Idle and active performance measurements.
- Error recovery and service restart behavior.
- Configuration persistence and migration testing.
- Security review.
- Accessibility verification.
- Compatibility testing against relevant Arch Linux environments.

Expected outcome:

A thoroughly validated shell suitable for regular daily use.

Completion criteria:

- Critical functionality passes its validation requirements.
- No known unresolved critical security or stability issues remain.
- Resource consumption is measured and reviewed.
- Recovery mechanisms behave predictably.
- Configuration changes and migrations are tested.
- Regression coverage protects established functionality.
- Validation evidence is recorded.

### Phase 7 — Installation, Maintenance and Release

**Objective:** Prepare mao-shell for safe installation, maintenance and distribution.

Scope:

- User-local installation workflow.
- Installation prerequisites and capability detection.
- Backup and rollback mechanisms.
- Update and upgrade workflow.
- Configuration preservation.
- Installation diagnostics.
- Uninstallation workflow.
- Release packaging and versioning.
- User and developer documentation.
- README and contribution guidelines.
- Initial release preparation.

Expected outcome:

A documented and maintainable release that users can install, operate, update and remove safely.

Completion criteria:

- Installation is reproducible.
- Existing user configuration is preserved.
- Update and rollback procedures are validated.
- Uninstallation does not leave unintended modifications.
- Release documentation reflects actual implementation.
- Known limitations are documented.
- Release validation requirements are satisfied.

## 5. Phase Dependencies

The expected development sequence is:

```text
Phase 0: Project Foundation
        |
        v
Phase 1: Core Architecture
        |
        v
Phase 2: Shell Foundation
        |
        v
Phase 3: System Integrations
        |
        v
Phase 4: Core User Experience
        |
        v
Phase 5: Visual Refinement
        |
        v
Phase 6: Reliability and Optimization
        |
        v
Phase 7: Installation and Release
```

This is the default progression, not an absolute restriction.

Some independent work may overlap when dependencies, architecture and validation requirements allow it. Such parallel development must not introduce unnecessary coupling or compromise the stability of shared foundations.

## 6. Progressive Task Planning

Detailed task planning must happen immediately before a phase begins.

For each phase, the agent must:

- Review the current repository state.
- Read relevant project specifications and design patterns.
- Identify applicable development skills.
- Inspect existing implementations before proposing changes.
- Identify dependencies and technical risks.
- Break the phase into small, independently testable tasks.
- Define acceptance criteria for each task.
- Identify required validation procedures.
- Estimate task complexity and implementation order.
- Record the resulting tasks in `TASKS.md`.

Future phases must not be expanded into speculative implementation tasks without a concrete reason.

The roadmap describes the intended direction. `TASKS.md` describes the actionable work.

## 7. Milestone Management

Each phase represents a major project milestone.

A milestone can be considered complete only when:

- Its required tasks are completed.
- Its acceptance criteria are satisfied.
- Relevant validation procedures pass.
- Outstanding issues are documented.
- Technical documentation is updated.
- No unresolved critical blocker prevents progression.

Milestone completion must be recorded in the project tracking documentation.

## 8. Scope and Priority Management

Development priorities must be evaluated according to:

1. Architectural dependencies.
2. Functional importance.
3. Security and reliability risks.
4. User experience impact.
5. Performance implications.
6. Implementation complexity.
7. Long-term maintenance cost.

New ideas and feature requests should be evaluated against the existing project specifications before being added to the roadmap.

Unplanned features must not silently expand the scope of an active phase.

If a change affects established architectural decisions or project-wide requirements, the agent must explain the implications and obtain approval when required by `AGENTS.md`.

## 9. Roadmap Maintenance

This document must be updated when:

- A milestone is completed.
- The development sequence changes.
- A major architectural dependency is discovered.
- Project scope changes.
- A phase is split, merged or redefined.
- A significant technical limitation affects the intended direction.

Completed phases should remain recorded as historical project milestones.

The roadmap must reflect the actual state of the project rather than an idealized or outdated plan.

## 10. Related Project Documentation

The roadmap must be used together with:

- `AGENTS.md` — Agent responsibilities, workflow and safety constraints.
- `COMPONENT_SPECS.md` — Functional product specifications.
- `DESIGN_PATTERNS.md` — Architecture and implementation conventions.
- `.opencode/skills/` — Specialized technical development and validation guidance.
- `TASKS.md` — Current actionable development tasks.
- `CHANGELOG.md` — Implemented changes and project history.

## 11. Unphased Backlog

Features from `COMPONENT_SPECS.md` that are not yet assigned to a phase. Each item is assigned to a phase when that phase is planned.

- Clipboard history.
- Screenshot integration.
- Files and trash shortcuts.
- mao Assistant frontend.
- Profiles and automation.
- Global shortcut manager.
- System tray.
- Clock and weather dashboard.
- Power menu.
- Do Not Disturb.
- First-run setup assistant.
- Diagnostics dashboard.
- Usage statistics.
- Spanish and English localization.
- Audio visualizer.
- Lock screen.

## 12. Guiding Principle

**Build the foundation carefully, develop incrementally, validate continuously and evolve the roadmap based on real implementation experience.**

mao-shell should progress through verified capabilities, not simply through completed code or elapsed development time.