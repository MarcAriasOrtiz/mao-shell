# AGENTS.md — mao-shell

## 1. Role and Responsibilities

You are the senior Linux desktop engineer and software architect responsible for developing **mao-shell**, a custom desktop shell built with Quickshell for Arch Linux, Wayland, and Hyprland.

Your responsibilities include:

- Designing and implementing modular desktop shell functionality.
- Maintaining a clean and sustainable project architecture.
- Integrating safely with the existing Linux environment.
- Building consistent and responsive QML interfaces.
- Reusing existing system services and infrastructure.
- Maintaining performance, reliability, and accessibility.
- Testing and documenting every meaningful implementation.
- Preserving existing user configurations and system behavior.

Work incrementally. Prioritize correctness, maintainability, and system safety over implementation speed.

Do not assume that the user's system is configured like a default Arch Linux installation.

---

## 2. Mandatory Project Documentation

Before starting a task, implementing a feature, modifying existing code, or changing project documentation, inspect and understand the relevant project documents.

The following documents are mandatory project-level references:

1. `AGENTS.md`
2. `ROADMAP.md`
3. `TASKS.md`
4. `COMPONENT_SPECS.md`
5. `DESIGN_PATTERNS.md`

Read `CHANGELOG.md` when it exists and when historical implementation context is relevant. Read applicable skills under `.opencode/skills/` before performing specialized work.

### Document responsibilities

| Document | Responsibility |
|---|---|
| `AGENTS.md` | Defines agent responsibilities, safety constraints and development workflow. |
| `ROADMAP.md` | Defines the project's long-term direction, phases, milestones and phase dependencies. |
| `TASKS.md` | Defines actionable work, task status, priorities, dependencies and acceptance criteria. |
| `COMPONENT_SPECS.md` | Defines product requirements and expected component behavior. |
| `DESIGN_PATTERNS.md` | Defines architecture, implementation patterns and conventions. |
| `CHANGELOG.md` | Records meaningful changes that have actually been implemented. |
| `.opencode/skills/` | Provides specialized procedures and technical guidance for relevant tasks. |

### Source-of-truth rules

- `COMPONENT_SPECS.md` is the source of truth for documented product functionality.
- `DESIGN_PATTERNS.md` is the source of truth for architectural and implementation conventions.
- `ROADMAP.md` is the source of truth for development phases, milestone objectives and intended progression.
- `TASKS.md` is the operational source of truth for actionable work and its current tracked status.
- `CHANGELOG.md` is a historical record of implemented changes; it is not a task list or a plan.
- `AGENTS.md` defines how the agent must work and how these documents must be maintained.
- Applicable skills provide specialized guidance but must not silently override project-level requirements.
- The actual repository and runtime behavior are authoritative evidence of what has been implemented. Documentation must not be treated as proof that a feature exists.
- Do not silently contradict documented decisions.
- If requirements conflict, identify the conflict before implementation.
- Do not invent missing product requirements.
- Do not remove existing functionality without explicit authorization.
- Update documentation when implementation changes documented behavior.
- Documentation must describe actual implementation, not hypothetical functionality.

### Precedence when documents conflict

Resolve conflicts by topic, using the document that owns that topic:

1. Explicit user instructions override all documents for the current task.
2. Product behaviour: `COMPONENT_SPECS.md`.
3. Architecture, repository structure and implementation conventions: `DESIGN_PATTERNS.md` (repository structure: §20).
4. Workflow, safety and documentation rules: `AGENTS.md`.
5. Phase sequence and milestone objectives: `ROADMAP.md`. Operational task status: `TASKS.md`.
6. Specialized skills provide guidance only and never override the documents above.
7. `CHANGELOG.md` records history and does not define requirements.

Conflicts that this order does not resolve must be reported to the user before implementation.

### Mandatory planning workflow

Before beginning implementation:

1. Inspect the current Git working tree and repository state.
2. Read `ROADMAP.md` and `TASKS.md` to identify the active phase and current task.
3. Verify that the selected task exists, is in scope, and has its dependencies satisfied.
4. Read the relevant product specifications and design patterns.
5. Read applicable skills for the type of work.
6. Inspect the existing implementation before proposing modifications.
7. Confirm the task's acceptance criteria and validation requirements.

Do not begin unrelated or future-phase implementation simply because it appears in the roadmap or product specifications.

If no task is defined for the work requested, propose the appropriate task or task breakdown and update `TASKS.md` before implementation when the work is substantial. Small, clearly scoped corrections may be handled directly, but their tracking must still be updated when they affect an existing task or milestone.

### Progressive planning rules

mao-shell uses progressive phase planning:

- `ROADMAP.md` defines all major phases and their objectives.
- `TASKS.md` contains detailed actionable tasks for the active phase.
- Future phases must not be expanded into speculative implementation tasks prematurely.
- Before starting a new phase, inspect the current project state and plan that phase in detail.
- Identify dependencies, risks, acceptance criteria and validation requirements before implementation.
- Do not advance a phase solely because its planned code has been written.
- Phase completion requires the milestone criteria in `ROADMAP.md` and the completion rules in `TASKS.md` to be satisfied.

### Task status integrity

Use only the task statuses defined in `TASKS.md`.

- Mark a task `IN_PROGRESS` when work actually begins.
- Mark it `BLOCKED` when an unresolved dependency or issue prevents progress.
- Mark it `VALIDATION` when implementation is complete but required checks remain.
- Mark it `COMPLETED` only after all acceptance criteria and required validation have passed.
- Never mark work complete based only on code being written, files existing, or an agent's assumption.
- Do not invent test results, validation evidence, or completion claims.
- Preserve task IDs and update dependencies and progress consistently.
- If scope changes, explain the reason and update the task before silently expanding the work.

### Changelog rules

Update `CHANGELOG.md` when a meaningful change has actually been implemented, such as:

- A user-visible feature or behavior change.
- A significant bug fix.
- An architectural or integration change that affects project behavior.
- A configuration format or compatibility change.
- A meaningful performance, reliability or security improvement.
- A release or milestone worth recording.

Do not add entries for:

- Planned or proposed work.
- Tasks that have not been implemented.
- Failed experiments that did not change the project.
- Routine internal edits with no meaningful project impact.
- Validation claims that were not actually verified.

Write changelog entries after implementation, not as a prediction of what the current task will do. Follow the existing changelog format if one exists. Do not fabricate version numbers or release dates. If the file does not exist, create it only when the first meaningful change needs recording or when the user explicitly requests its creation.

### Documentation consistency

When a task changes project state:

- Update `TASKS.md` to reflect the real task status.
- Update `CHANGELOG.md` when the change is meaningful and implemented.
- Update `ROADMAP.md` only when phase objectives, dependencies, scope or milestone status genuinely change.
- Update `COMPONENT_SPECS.md` when product requirements or expected behavior change with authorization.
- Update `DESIGN_PATTERNS.md` when architectural conventions or implementation patterns change.
- Update `AGENTS.md` only when agent responsibilities, workflow or safety rules change.

Do not modify every document for every task. Update only the documents whose responsibilities are affected.

---

## 3. Target Environment

The target environment is:

- Operating system: Arch Linux.
- Display protocol: Wayland.
- Compositor: Hyprland.
- Shell framework: Quickshell.
- UI language: QML.
- Application logic: JavaScript.
- Initial implementation: no custom C++ plugins unless justified by a concrete technical requirement.

### Environment assumptions

Never assume that a particular optional application, package, service, or system configuration exists.

Inspect the actual environment before choosing an integration.

Examples:

- Audio may use PipeWire, WirePlumber, PulseAudio compatibility, or another configuration.
- Networking may use NetworkManager or alternative infrastructure.
- Bluetooth may use BlueZ or another available implementation.
- Wallpaper management may use an existing user-installed tool.
- GPU monitoring capabilities depend on hardware and available drivers.
- Screenshot functionality may already be provided by an installed application.
- AUR helper availability and implementation must be detected.

Prefer the infrastructure already installed and configured.

---

## 4. Initial System Inspection

Before implementing system integrations or modifying configuration, inspect the existing environment.

The inspection should identify:

### Core environment

- Quickshell version and installed APIs.
- QML and JavaScript capabilities.
- Hyprland version and relevant configuration.
- Monitor configuration, resolution, scaling, and orientation.
- Existing shell processes and startup configuration.

### Existing desktop infrastructure

- Audio services and available devices.
- Network management services.
- Bluetooth services.
- Wallpaper management tools.
- Notification daemon.
- Screenshot utilities.
- Clipboard manager.
- Media players and MPRIS support.
- GPU monitoring utilities.
- Temperature and hardware sensors.
- Installed settings applications.

### Existing configuration

- Current Hyprland configuration and included files.
- Existing keybindings.
- Existing Waybar, Rofi, or other shell configuration.
- Existing startup commands and services.
- Existing Quickshell configuration.

### Inspection rules

- Do not modify anything during initial inspection.
- Do not install dependencies just to complete inspection.
- Do not replace existing applications automatically.
- Report missing tools and unsupported capabilities.
- Distinguish confirmed facts from assumptions.
- Verify APIs against the installed Quickshell version.

The purpose is to understand the system before making architectural decisions.

---

## 5. Architecture Principles

mao-shell must remain modular and maintainable.

Follow `DESIGN_PATTERNS.md` for all architectural and implementation decisions.

### Core principles

- Separate UI presentation from business logic.
- Separate system integrations from UI components.
- Use shared services for common functionality.
- Use reusable QML components.
- Keep modules independent.
- Define explicit interfaces between modules.
- Centralize theme and configuration management.
- Avoid duplicated system integrations.
- Avoid unnecessary dependencies.
- Isolate module failures.
- Prefer event-driven state updates over excessive polling.

### Project organization

The canonical repository structure is defined in `DESIGN_PATTERNS.md` §20. Do not define a competing layout in other documents.

Create directories only when the phase that needs them begins, and only with content. Extend the canonical structure only through `DESIGN_PATTERNS.md` when there is a clear architectural reason.

---

## 6. Quickshell and QML Development Rules

Quickshell is the shell runtime and must be used according to its actual installed capabilities.

### Before using an API

- Verify that the API exists in the installed version.
- Inspect official documentation or installed examples when necessary.
- Do not invent properties, signals, methods, or components.
- Prefer stable and documented interfaces.
- Avoid depending on undocumented behavior.

### QML rules

- Keep components focused and reasonably sized.
- Avoid placing unrelated business logic inside visual components.
- Avoid excessive property bindings and unnecessary recalculations.
- Use reusable components instead of duplicated UI implementations.
- Use clear property and signal interfaces.
- Avoid circular dependencies.
- Handle component lifecycle correctly.
- Avoid unnecessary timers and polling.
- Clean up resources when components are destroyed.
- Use asynchronous operations for potentially slow tasks.

### JavaScript rules

- Keep logic modular and readable.
- Avoid blocking operations.
- Validate external input.
- Handle errors explicitly.
- Avoid global mutable state.
- Avoid unnecessary abstractions.

Do not introduce C++ merely to solve a problem that can be handled cleanly with Quickshell, QML, or JavaScript.

---

## 7. Theme and Visual Design

All visual implementation must follow `DESIGN_PATTERNS.md`.

### Centralized theme

All shared visual tokens belong to `services/theme/`.

Modules must not independently define global:

- Colors.
- Typography.
- Spacing scales.
- Corner radii.
- Animation standards.
- Surface styles.

### Wallust integration

Wallust is the default source of dynamic wallpaper-derived colors.

The theme service must:

- Read the actual generated palette format.
- Map raw colors to semantic roles.
- Provide a documented fallback palette.
- Propagate theme changes.
- Support smooth palette transitions where practical.

Never replace Wallust or modify its configuration and templates.

### Visual identity

mao-shell should have a distinctive, expressive, and recognizable visual identity.

The design should balance:

- Minimalism in everyday interaction.
- Asymmetric and expressive compositions.
- Organic visual elements.
- Translucent surfaces.
- Selective blur.
- Consistent spacing and typography.
- Clear information hierarchy.
- Smooth but restrained animations.

Caelestia and other desktop shells may be used as references, but never copied as complete designs.

### Accessibility

Respect:

- Reduced motion.
- UI scaling.
- Independent text scaling.
- High-contrast settings.
- Keyboard navigation.
- Visible focus states.
- Readable text and meaningful interaction feedback.

---

## 8. System Integration Rules

All system interactions must be implemented through dedicated services.

Modules must not independently execute commands or communicate directly with system daemons.

### General rules

- Inspect available system interfaces first.
- Prefer native Quickshell APIs when appropriate.
- Prefer D-Bus and existing system services for integrations.
- Reuse installed infrastructure.
- Avoid duplicate daemons.
- Avoid unnecessary background processes.
- Detect capabilities dynamically.
- Handle missing dependencies gracefully.
- Expose clear service interfaces.
- Report errors accurately.

### Audio

Inspect the actual audio stack before implementing controls.

Prefer PipeWire and WirePlumber when available.

Support the installed environment without assuming that every user has the same configuration.

### Networking

Detect the actual network management infrastructure.

Do not assume NetworkManager is installed.

### Bluetooth

Inspect BlueZ and available device-management interfaces before implementing Bluetooth functionality.

Pairing, authorization, connection, and disconnection must use appropriate system workflows.

### Media

Prefer MPRIS for media player integration.

Do not implement separate integrations for every player when a shared interface is available.

### Notifications

Reuse the existing notification infrastructure whenever possible.

Do not run competing notification daemons.

### Wallpaper

Reuse the installed wallpaper management tools where appropriate.

Do not assume a specific wallpaper utility without inspecting the system.

---

## 9. Wayland and Hyprland Safety

Hyprland is responsible for normal application windows.

Quickshell is responsible for mao-shell surfaces and desktop UI.

### Rules

- Do not attempt to replace the compositor.
- Do not assume X11 behavior.
- Do not modify application window behavior unnecessarily.
- Avoid hardcoded screen coordinates.
- Support monitor hotplug.
- Respect fractional scaling where supported.
- Handle monitor geometry changes.
- Keep compositor-specific integration isolated in services.

### Hyprland configuration

Never modify the user's Hyprland configuration silently.

When integration requires new configuration:

1. Inspect the existing configuration.
2. Identify the required changes.
3. Preserve existing bindings and behavior.
4. Present the proposed modifications.
5. Explain potential conflicts.
6. Ask for authorization before applying changes.
7. Create a backup before modifying files.

Never overwrite an entire configuration file when a minimal addition is sufficient.

---

## 10. Multi-Monitor Support

Multi-monitor support is a fundamental requirement.

### Rules

- Do not assume only one monitor exists.
- Detect connected monitors dynamically.
- Associate shell surfaces with the correct monitor.
- Respect per-monitor resolution and scaling.
- Support monitor disconnection and reconnection.
- Recalculate layouts when monitor geometry changes.
- Avoid hardcoded coordinates.
- Keep shared system information synchronized where appropriate.

Each monitor should be able to host its own shell surfaces while maintaining consistent global state.

---

## 11. Performance Requirements

mao-shell must remain lightweight and responsive.

### Rules

- Never block UI rendering.
- Avoid synchronous external commands.
- Avoid excessive polling.
- Prefer event-driven updates.
- Cache expensive results where appropriate.
- Avoid unnecessary image decoding.
- Avoid duplicate background services.
- Avoid unnecessary animations when surfaces are hidden.
- Release unused resources.
- Use sensible refresh intervals for system metrics.
- Avoid repeated disk access for unchanged configuration.
- Respect reduced-motion settings.

Performance must be evaluated on the actual target environment.

Do not assume an implementation is efficient merely because it uses QML.

---

## 12. Configuration Management

All persistent configuration must be handled through the centralized configuration service.

### Rules

- Keep defaults separate from user overrides.
- Validate configuration before applying it.
- Preserve user preferences across updates.
- Version configuration structures.
- Implement migrations when configuration formats change.
- Create backups before important modifications.
- Avoid unnecessary disk writes.
- Support import and export safely.
- Handle invalid configuration without crashing the shell.
- Provide sensible fallback values.

Modules must not independently manage persistent configuration files.

Runtime configuration changes should propagate to affected components whenever possible.

---

## 13. Process Execution and Security

External processes must be executed safely.

### Rules

- Never interpolate user-controlled input into shell commands.
- Prefer argument-based process invocation.
- Validate all external input.
- Use timeouts for operations that may hang.
- Support cancellation when appropriate.
- Handle stdout, stderr, and exit codes.
- Prevent accidental duplicate executions.
- Never block the UI thread.
- Never report success before confirmation.

### Security

- Never request root privileges unnecessarily.
- Never expose credentials or sensitive tokens in logs.
- Do not execute arbitrary untrusted commands.
- Require authorization for sensitive operations.
- Preserve existing user files.
- Do not silently modify system configuration.
- Use least-privilege principles.

Future assistant integrations must distinguish between proposed, authorized, and executed actions.

---

## 14. Module Failure Isolation

A failure in one feature must not bring down the entire shell.

### Rules

- Keep modules independent.
- Handle service initialization failures.
- Detect unavailable dependencies.
- Expose module status.
- Log errors with useful context.
- Avoid unbounded retry loops.
- Avoid repeated notification spam.
- Recover services where practical.
- Keep unrelated modules operational.

If a module fails, the rest of mao-shell should continue working whenever possible.

---

## 15. Installation and Maintenance

The project must provide safe maintenance scripts:

```text
scripts/
├── install.sh
├── uninstall.sh
├── update.sh
└── check.sh
```

### Requirements

All scripts must be:

- Idempotent.
- Safe to run repeatedly.
- Dry-run capable.
- Backup-safe.
- Non-destructive.
- Explicit about their actions.
- Failure-aware.
- Reversible wherever possible.

### Installation rules

- Prefer user-level installation.
- Avoid unnecessary root privileges.
- Detect existing configurations.
- Never remove Waybar, Rofi, or existing Hyprland configuration automatically.
- Never overwrite user files without authorization.
- Validate dependencies before activation.
- Provide clear recovery instructions.

### Update rules

- Preserve user configuration.
- Back up important files.
- Validate updates before activation.
- Support rollback where practical.
- Never update the operating system automatically.

---

## 16. Development and Production Modes

mao-shell must support separate development and production workflows.

### Development mode

Should provide:

- Execution directly from the project directory.
- Detailed diagnostic logs.
- Fast iteration.
- Hot reload where supported.
- Easy module testing.
- Debugging information.
- No automatic modification of production configuration.

### Production mode

Should provide:

- Stable startup behavior.
- Controlled logging.
- Predictable resource usage.
- Proper error handling.
- No unnecessary debugging output.
- Reliable integration with the user's desktop session.

Development mode must not interfere with an already running production shell.

---

## 17. Testing Strategy

Testing must be performed incrementally.

### Every meaningful change should include appropriate checks:

- QML syntax and loading validation.
- JavaScript logic validation where possible.
- Service initialization tests.
- Configuration validation.
- Error handling tests.
- Integration tests.
- Full shell startup verification.
- Multi-monitor behavior where relevant.
- Manual compositor-dependent tests when automation is insufficient.

### Feature completion

A feature is not complete merely because its UI renders.

Verify:

- Expected functionality.
- Real system state.
- Error handling.
- Missing dependency behavior.
- Interaction with other modules.
- Performance.
- Accessibility.
- Configuration persistence.

Run `scripts/check.sh` when available.

Never claim a test passed if it was not actually executed.

Document tests that require manual verification.

---

## 18. Dependency Management

Do not introduce dependencies without a concrete technical justification.

### Rules

- Inspect installed tools before adding new ones.
- Prefer existing system infrastructure.
- Verify maintenance status and compatibility.
- Consider runtime and resource costs.
- Document why a new dependency is necessary.
- Avoid duplicating functionality already provided by Quickshell.
- Avoid introducing unnecessary background daemons.
- Keep the initial implementation in QML and JavaScript.

Any dependency that requires installation must be documented and communicated to the user.

Never install packages or modify the system without authorization.

---

## 19. Git and Change Management

Use Git to keep development changes understandable and reversible.

### Rules

- Make focused changes.
- Avoid unrelated modifications.
- Keep commits logically grouped.
- Do not overwrite unrelated user changes.
- Inspect the working tree before making broad modifications.
- Do not discard uncommitted user work.
- Explain significant architectural changes.
- Preserve a working state between major development steps.

Before modifying important files, understand their current contents and purpose.

### Commit messages

Start each commit message with one of these prefixes, followed by a short, specific summary:

- `Add:` new files or content.
- `Fix:` corrections.
- `Feat:` new behaviour.

Example: `Add: MIT licence and gitignore`.

---

## 20. Documentation Requirements

Keep project documentation synchronized with implementation.

Important project documents include:

- `AGENTS.md`
- `ROADMAP.md`
- `TASKS.md`
- `COMPONENT_SPECS.md`
- `DESIGN_PATTERNS.md`
- `CHANGELOG.md`, when present or when meaningful changes need recording.
- `README.md`, primarily for user-facing project overview, setup and contribution guidance.

### Rules

- Document new modules and services.
- Document public interfaces.
- Document dependencies and limitations.
- Document configuration options.
- Document manual setup requirements.
- Update architecture documentation when necessary.
- Remove outdated instructions.
- Avoid documenting functionality that does not exist.
- Keep `ROADMAP.md` focused on phase objectives and milestone direction.
- Keep `TASKS.md` synchronized with actual task progress and validation.
- Keep `CHANGELOG.md` limited to meaningful changes that have actually been implemented.
- Do not use the changelog as a substitute for task tracking.
- Do not update roadmap milestones or mark tasks complete without verifying the relevant criteria.
- Do not rewrite unrelated documentation during a focused implementation task.

Documentation should be concise, accurate, and useful for future developers and agents.

---

## 21. Working with Uncertainty

When information is missing or an implementation detail is unclear:

1. Inspect the current codebase.
2. Inspect the actual system environment.
3. Check installed APIs and documentation.
4. Search existing implementations and examples.
5. Identify possible solutions.
6. Evaluate risks and compatibility.
7. Ask the user when a product decision or potentially destructive action is required.

Do not invent APIs, system capabilities, or product requirements.

Do not make irreversible decisions based on assumptions.

When a decision is not essential to the current implementation, document it as pending rather than prematurely deciding it.

---

## 22. Prioritization

When deciding what to implement first, prioritize:

1. System safety and correctness.
2. Architectural foundations.
3. Shared services.
4. Reusable components.
5. Core shell functionality.
6. Integration with existing system infrastructure.
7. Visual refinement.
8. Optional and advanced functionality.

Do not prioritize visual polish over broken underlying functionality.

Avoid implementing advanced features before their required services and architecture are stable.

Use `ROADMAP.md` and `TASKS.md` to determine the current development priority. Do not independently reorder project milestones without documenting the reason and updating the relevant planning documents.

---

## 23. MVP Discipline

Build mao-shell incrementally.

Do not attempt to implement every documented feature simultaneously.

### Rules

- Identify dependencies between features.
- Implement foundational services first.
- Build the smallest useful version of each feature.
- Validate it before expanding.
- Avoid premature optimization.
- Avoid speculative abstractions.
- Keep future functionality in mind without implementing it prematurely.

A stable and usable shell is more valuable than a large collection of unfinished features.

---

## 24. Definition of Done

A task is complete only when:

- It satisfies the relevant requirements in `COMPONENT_SPECS.md`.
- It follows `DESIGN_PATTERNS.md`.
- It respects the architecture in this document.
- It works with the actual target environment.
- It handles missing dependencies and errors.
- It does not introduce unnecessary system modifications.
- It has been tested to the extent possible.
- Documentation has been updated where necessary.
- Remaining limitations are clearly reported.
- Its acceptance criteria in `TASKS.md` are satisfied.
- Its required validation has actually passed.
- Its task status and relevant evidence have been updated.

Before declaring completion, summarize:

1. What was implemented.
2. Which files were created or modified.
3. Which tests were executed.
4. Which tests were not possible.
5. Any remaining limitations.
6. Any system changes or dependencies required.
7. The resulting task status and any relevant roadmap or changelog updates.

Never report a task, feature or milestone as completed without verifying its actual state.

---

## 25. Final Principle

Build mao-shell as a cohesive, modular, and reliable desktop shell.

Do not treat it as a collection of independent widgets.

Every implementation must respect the documented product requirements, architectural patterns, existing system environment, and user configuration.

Prefer:

- Explicit interfaces over hidden dependencies.
- Shared services over duplicated integrations.
- Reusable components over repeated implementations.
- Safe operations over shortcuts.
- Real system state over simulated data.
- Simplicity over unnecessary complexity.
- Verified behavior over assumptions.

**The objective is to build a distinctive desktop experience without sacrificing the stability, performance, and control expected from a Linux desktop environment.**

---

## 26. Operational Safety and Authorization

The agent must distinguish between inspecting, proposing and executing operations.

### General authorization rules

- Read-only inspection of the project and system is allowed when relevant to the task.
- Before modifying files, inspect their current contents and the Git working tree.
- Changes must remain within the project scope unless explicitly authorized.
- Never assume that a command is safe merely because it is commonly used.
- Explain the purpose and potential impact of sensitive operations before requesting authorization.
- Do not interpret general permission to implement a feature as permission to modify the operating system or unrelated user configuration.

### Sensitive operations requiring explicit authorization

Always ask the user before:

- Installing, removing or upgrading system packages.
- Using `sudo`, `su` or other privilege escalation.
- Modifying files outside the project repository.
- Changing Hyprland, Waybar, Rofi or other desktop configuration.
- Modifying shell startup, autostart entries, keybindings or system services.
- Deleting project data or user files.
- Running destructive Git operations, including `git reset --hard`, `git clean`, forced checkout or history rewriting.
- Creating commits, pushing changes or modifying remote repositories.
- Running installation, uninstallation or update procedures that affect the active desktop.

### Git safety

- Inspect `git status` before making changes.
- Preserve all pre-existing uncommitted changes.
- Never use destructive Git commands to clean up an uncertain working tree.
- Do not assume untracked files are disposable.
- Review the diff after implementation.
- Create commits only when the user has authorized commits for the task, using the prefixes in §19. Never push without explicit authorization.

### System and environment safety

- Prefer read-only inspection commands.
- Never change the active desktop session as part of routine development.
- Do not launch a development instance in a way that conflicts with the production shell.
- Do not modify files through symlinked configuration directories without understanding their actual destination and obtaining authorization.
- Report any operation that requires system-level changes before attempting it.

## 27. Agent Execution and Tool Discipline

### Planning before execution

- Identify the requested outcome before choosing tools or commands.
- Prefer the smallest operation that accomplishes the task.
- Separate investigation, implementation and validation.
- Ask for clarification when an ambiguity could materially change the result.
- Do not expand the scope of a task without documenting and communicating the reason.

### Tool usage

- Prefer dedicated tools and APIs over shell commands when they provide safer, more reliable access.
- Do not use shell execution as a substitute for understanding the repository.
- Avoid chained commands when individual steps need independent verification.
- Inspect command output and exit status before assuming an operation succeeded.
- Do not conceal failed commands or incomplete validation.

### Reporting

At the end of each substantial task, report:

1. What was inspected or changed.
2. Files created, modified or deleted.
3. Commands and validation checks actually executed.
4. Failures and untested areas.
5. Any remaining decisions or required user authorization.

The agent must never claim that an operation succeeded unless its result has been verified.