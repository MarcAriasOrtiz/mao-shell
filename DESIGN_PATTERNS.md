# Design Patterns — mao-shell

This document defines the architectural patterns, coding conventions, and implementation rules that every module and service in mao-shell must follow.

Its purpose is to keep the codebase modular, consistent, maintainable, efficient, and predictable as the shell grows.

These rules apply to all new code and modifications to existing code.

---

## 1. Centralized Theme System

All visual design tokens must be managed centrally through `services/theme/`.

Modules and reusable components consume theme values. They must not independently define global colors, spacing scales, typography, radii, or animation standards.

### Theme responsibilities

The theme system provides semantic tokens for:

**Colors**
- `surface`, `surfaceElevated`, `surfaceHovered`
- `primary`, `secondary`, `accent`
- `text`, `textMuted`, `textDisabled`
- `border`, `borderFocused`
- `success`, `warning`, `error`, `info`
- Overlay and scrim colors

**Spacing**
- `spacing`
- `spacingSm`
- `spacingMd`
- `spacingLg`
- `spacingXl`

**Shape**
- `radius`
- `radiusSm`
- `radiusLg`
- `radiusPill`

**Typography**
- `font`
- `fontMono`
- `textSize`
- `textSizeSm`
- `textSizeLg`
- `textSizeXl`

**Animation**
- `duration`
- `durationFast`
- `durationNormal`
- `durationSlow`
- `easing`

**Opacity and effects**
- Surface opacity
- Overlay opacity
- Disabled opacity
- Blur and elevation parameters where supported

### Rules

- Wallust is the default source for dynamic wallpaper-derived colors.
- Wallust colors must be mapped to semantic theme roles before being consumed by UI components.
- A documented static palette must be available as a fallback.
- Components must react to theme changes without requiring a shell restart whenever technically possible.
- The dark appearance is the only required appearance. It is derived from Wallust and the wallpaper. Any future appearance must use the same semantic roles.
- Accessibility modes must be able to override relevant theme values.
- Reduced transparency and high-contrast modes must be supported where practical.

Avoid hardcoded visual values such as arbitrary hexadecimal colors or duplicated spacing and radius values.

Small local layout values are acceptable when they are genuinely component-specific and do not represent a reusable design token.

If a visual value becomes shared or repeated, promote it to the theme system.

---

## 2. Reusable QML Component Library

The `components/` directory contains reusable visual components.

Modules must reuse these components rather than repeatedly implementing the same visual patterns.

### Component organization

```text
components/
├── buttons/
├── cards/
├── panels/
├── popups/
├── icons/
├── inputs/
├── indicators/
├── controls/
└── layouts/
```

Examples include:

- Primary, secondary, icon, and toggle buttons.
- Surface containers and cards.
- Popup windows, menus, and dialogs.
- Icon wrappers and symbolic assets.
- Text inputs and search fields.
- Status indicators and badges.
- Sliders, circular controls, and switches.
- Common layout primitives.

### Rules

- Components must have clear responsibilities and predictable interfaces.
- Prefer composition over large components with many unrelated responsibilities.
- Visual properties must consume centralized theme tokens.
- Interactive components must support keyboard navigation and focus handling.
- Accessibility labels and appropriate interaction states must be provided.
- Components must expose configurable properties rather than relying on module-specific assumptions.
- Avoid excessive abstraction: a component should be reusable or meaningfully simplify the code.

Components must not depend on the internal implementation of a particular module.

---

## 3. Module Independence

Each feature under `modules/` must be independently understandable, maintainable, and testable.

A module owns its presentation, feature-specific state, and local business rules.

### Rules

- Modules must declare their dependencies explicitly.
- Modules may consume shared services and reusable components.
- Modules must not access the private implementation details of other modules.
- Modules must not import internal files from unrelated modules to reuse functionality.
- Shared functionality belongs in `components/` or `services/`.
- Optional modules must be possible to disable without breaking unrelated functionality.
- A failure in one module must not unnecessarily terminate the entire shell.

### Module communication

When modules need to communicate, use explicit interfaces, shared services, or well-defined signals.

Avoid hidden dependencies, direct manipulation of another module's internal state, and circular imports.

For example, a media widget should consume the shared media service rather than accessing the internal state of another media-related module.

---

## 4. Separation of Concerns

The project follows a clear separation between presentation, business logic, system integration, and configuration.

| Layer | Responsibility | Location |
|---|---|---|
| UI presentation | QML views, layouts, animations | `modules/`, `components/` |
| Feature logic | Feature-specific state and rules | `modules/` |
| Shared services | Reusable application and system state | `services/` |
| System integration | Hyprland, audio, network, Bluetooth, etc. | `services/` |
| Theme management | Visual tokens and Wallust integration | `services/theme/` |
| Configuration | Defaults, validation, persistence | `services/config/`, `config/` |
| Assets | Icons, fonts, images and other resources | `assets/` |

### Rules

- QML components should primarily describe presentation and interaction.
- Business rules should not be unnecessarily embedded in visual components.
- System integration must be handled through services.
- File access, D-Bus communication, process execution, and configuration persistence must not be scattered throughout the UI.
- Shared state must have a clearly defined owner.
- Services must expose clean interfaces instead of leaking implementation details.

Avoid creating unnecessary layers for trivial functionality. Architecture should remain understandable and proportional to the problem.

---

## 5. Centralized Configuration Management

Persistent configuration must be managed through a shared configuration service.

Modules must not independently read and write configuration files.

### Responsibilities

The configuration service manages:

- Default configuration values.
- User overrides.
- Configuration validation.
- Persistence and serialization.
- Version tracking.
- Configuration migrations.
- Backup and restoration.
- Runtime configuration updates.
- Import and export.

### Rules

- Use JSON for persistent configuration unless a concrete requirement justifies another format.
- Separate configuration by responsibility or module where appropriate.
- Preserve user settings when defaults or application versions change.
- Validate imported or externally modified configuration before applying it.
- Invalid configuration must not crash the shell.
- Provide safe fallback values.
- Save changes through the centralized service.
- Avoid unnecessary disk writes.
- Support automatic persistence of user preferences.

Configuration changes should propagate to affected modules without requiring a full shell restart whenever possible.

---

## 6. Wallust Color Integration

Wallust is the default provider for wallpaper-derived color palettes.

mao-shell consumes and interprets its output; it does not replace Wallust.

### Responsibilities

The theme service must:

1. Detect the availability of Wallust.
2. Read its generated color palette.
3. Inspect and support the actual output format available on the system.
4. Map raw colors to mao-shell semantic theme roles.
5. Provide a fallback palette when Wallust is unavailable.
6. Propagate palette changes to dependent components.
7. Support smooth transitions between palettes where practical.

### Rules

- Never assume Wallust's output paths or format without inspecting the installed environment.
- Do not overwrite Wallust configuration or templates.
- Do not duplicate Wallust's palette-generation functionality.
- Do not make individual modules read Wallust files directly.
- Keep the theme service responsible for all palette interpretation.

Manual visual overrides must be handled by mao-shell's theme system without silently changing Wallust's configuration.

---

## 7. Shared Services and State Ownership

Services provide reusable functionality and shared application or system state.

Each shared state must have a clearly defined owner.

### Examples

```text
services/
├── theme/
├── config/
├── hyprland/
├── audio/
├── network/
├── bluetooth/
├── notifications/
├── media/
├── clipboard/
├── weather/
├── updates/
├── system/
└── assistant/
```

### Rules

- A service must have one clear responsibility.
- Services must expose documented public interfaces.
- Modules consume service state rather than implementing duplicate integrations.
- Shared state must not have multiple competing owners.
- Services must notify consumers when relevant state changes.
- Service initialization and shutdown must be predictable.
- Optional integrations must report their availability.
- Service failures must be isolated whenever possible.

Do not create duplicate background processes or daemons when the operating system already provides the required functionality.

Prefer existing system services, D-Bus interfaces, and Quickshell capabilities.

---

## 8. Graceful Degradation and Capability Detection

mao-shell must adapt to the actual system environment.

Optional integrations and hardware capabilities must be detected individually.

Examples include:

- PipeWire and audio devices.
- NetworkManager or alternative network management infrastructure.
- BlueZ and Bluetooth adapters.
- GPU monitoring interfaces.
- Battery and power information.
- Wallpaper utilities.
- Hardware temperature and fan sensors.
- Installed screenshot tools.
- Available media players.

### Rules

- Never assume an optional service is installed.
- Never assume hardware capabilities exist.
- Detect capabilities before exposing dependent functionality.
- Hide unavailable information when it would otherwise be misleading.
- Show an explicit unavailable or unsupported state when appropriate.
- Preserve the functionality of unrelated modules.
- Do not install missing system dependencies automatically without user authorization.

A missing optional integration must not prevent mao-shell from starting.

---

## 9. Non-Blocking Process Execution

External processes may be required for system integrations that are not adequately covered by Quickshell APIs.

All process execution must be safe and non-blocking.

### Rules

- Prefer native Quickshell functionality and existing D-Bus interfaces when appropriate.
- Use argument-safe process invocation.
- Never interpolate user-controlled values into shell command strings.
- Validate arguments before execution.
- Avoid unnecessary shell wrappers.
- Never block the UI thread with synchronous operations.
- Handle process completion, errors, and exit codes explicitly.
- Implement timeouts where appropriate.
- Support cancellation for operations that may take a long time.
- Prevent duplicate executions when an operation is already running.
- Report failures clearly.
- Never report success before the operation has actually succeeded.

Process output must be handled according to its purpose and must not be allowed to freeze rendering or consume unbounded resources.

Commands that modify the system or perform sensitive operations must follow the project's authorization and safety rules.

---

## 10. Performance and Resource Management

mao-shell must remain responsive and lightweight.

The shell is a continuously running desktop component, so unnecessary CPU, memory, and GPU consumption must be avoided.

### Rules

- Avoid expensive work during component initialization.
- Avoid unnecessary QML bindings and repeated calculations.
- Avoid excessive timers and polling.
- Prefer event-driven updates when the underlying service supports them.
- Cache expensive results when appropriate.
- Update hardware metrics at reasonable intervals.
- Avoid unnecessary image decoding and thumbnail generation.
- Use asynchronous operations for potentially slow tasks.
- Release resources when modules or windows are no longer needed.
- Avoid unnecessary animations when components are hidden.
- Respect reduced-motion settings.
- Prevent duplicate service instances.

Performance-sensitive features must be evaluated on the actual target environment rather than assumed to be efficient.

---

## 11. Multi-Monitor and Wayland Compatibility

The shell must be designed for Wayland and multi-monitor environments.

Hyprland is the target compositor, but compositor-specific functionality must be isolated behind appropriate services.

### Rules

- Do not assume a single monitor.
- Respect per-monitor resolution, scale, and orientation.
- Support monitor connection and disconnection.
- Handle fractional scaling where supported.
- Associate shell surfaces and widgets with the correct monitor.
- Avoid hardcoded screen coordinates.
- Recalculate layouts when monitor geometry changes.
- Do not manipulate normal application windows unless explicitly required.
- Do not assume X11-specific behavior or APIs.

Quickshell owns shell surfaces; Hyprland manages normal application windows.

Compositor-specific functionality must not be spread throughout unrelated UI components.

---

## 12. Accessibility and Interaction Consistency

Accessibility must be considered from the beginning of component development.

### Rules

- Interactive elements must have meaningful focus behavior.
- Keyboard navigation must be supported wherever practical.
- Focus states must be visible.
- Text must remain readable across supported visual profiles.
- High-contrast mode must be supported where practical.
- UI scaling and text scaling must be handled independently.
- Reduced-motion mode must be respected.
- Disabled, loading, error, and active states must be visually distinguishable.
- Hover must never trigger destructive or irreversible actions.

Interaction patterns should remain consistent across modules.

For example, clicking an audio icon may toggle mute, while hovering only previews information. This distinction must be preserved consistently.

---

## 13. Error Handling and Module Isolation

Errors must be handled explicitly and must not unnecessarily propagate through the entire shell.

### Rules

- Every service must handle unavailable dependencies.
- Every asynchronous operation must have an error path.
- Modules must expose their operational status where appropriate.
- Errors must be logged with enough context to diagnose them.
- User-facing errors must be concise and understandable.
- Repeated failures must not generate unbounded logs or notifications.
- Recoverable services should support controlled retries or restarts.
- A failed optional module must not prevent unrelated modules from working.

Never silently ignore errors or pretend that a failed operation succeeded.

---

## 14. Security and System Safety

mao-shell operates with the permissions of the logged-in user and must not unnecessarily elevate privileges.

### Rules

- Never request root privileges for normal shell functionality.
- Never execute arbitrary commands from untrusted input without validation and authorization.
- Never interpolate user input into shell commands.
- Do not expose sensitive information in logs.
- Protect stored credentials and tokens.
- Do not modify Hyprland configuration silently.
- Do not overwrite existing user configuration without a backup.
- Require explicit authorization for sensitive or destructive operations.
- Keep system-changing operations separate from ordinary UI interactions.
- Show meaningful feedback when an operation is running or requires authorization.

Future assistant integrations must follow least-privilege principles and distinguish between proposed, authorized, and executed actions.

---

## 15. Install and Maintenance Script Contract

The following scripts must follow consistent safety rules:

```text
scripts/
├── install.sh
├── uninstall.sh
├── update.sh
└── check.sh
```

All scripts must be:

**Idempotent**
- Running them repeatedly must not create duplicate configuration or processes.

**Dry-run capable**
- Provide a way to preview changes without applying them.

**Backup-safe**
- Preserve existing user configuration before modifying it.

**Non-destructive**
- Never delete or overwrite Waybar, Rofi, Hyprland, or other existing user configuration without explicit authorization.

**Transparent**
- Clearly report the operations being performed.

**Failure-aware**
- Return non-zero exit codes when an operation fails.

**Reversible**
- Provide recovery or uninstall procedures wherever applicable.

Installation must operate primarily within the user's home directory and must not assume that the system is in a clean or default state.

---

## 16. Incremental Feature Development

Features must be implemented and validated incrementally.

Do not attempt to build the entire shell before validating its foundational architecture.

### Development workflow

1. Inspect the relevant existing code and system environment.
2. Identify dependencies and integration requirements.
3. Define the module's public interface.
4. Implement the smallest useful version.
5. Validate functionality independently where practical.
6. Integrate it with the shell.
7. Test interactions with existing modules.
8. Document limitations and remaining work.

### Completion requirements

A feature is not considered complete until:

- Relevant checks and tests pass.
- The full shell starts successfully.
- Hot reload works where applicable.
- The feature behaves correctly in its target environment.
- Errors and unavailable dependencies are handled.
- Relevant documentation is updated.
- Manual compositor-dependent tests are documented when automated testing is insufficient.

Do not migrate to the next major feature while unresolved problems in the current foundation would compromise it.

---

## 17. Documentation and Code Maintainability

Code must remain understandable to developers and future agents.

### Rules

- Use clear and consistent naming.
- Keep files focused on one responsibility.
- Avoid excessively large QML components.
- Document non-obvious behavior and architectural decisions.
- Document public service interfaces.
- Keep comments useful and accurate.
- Remove obsolete code and dead functionality.
- Avoid unnecessary abstractions and duplicated implementations.
- Update documentation when behavior or architecture changes.

Documentation must describe the actual implementation, not an intended or hypothetical one.

---

## 18. No Speculative Technologies or Dependencies

The default implementation stack is:

- Quickshell.
- QML.
- JavaScript.
- Existing system services and interfaces.

Do not introduce additional technologies without a concrete technical requirement.

Examples of additions that require justification include:

- C++ plugins.
- Compiled resource bundles.
- `.ui` files.
- Compiled translation systems.
- Additional background daemons.
- New systemd services.
- Additional external dependencies.
- Separate processes for functionality already provided by Quickshell or existing system services.

Before introducing a dependency:

1. Verify that the requirement cannot be met cleanly with the existing stack.
2. Inspect available native APIs and installed system capabilities.
3. Evaluate maintenance and runtime costs.
4. Explain why the dependency is necessary.
5. Prefer the simplest solution that meets the requirement.

Do not add technology merely because it is commonly used in other desktop shells.

---

## 19. Architectural Decision Principle

When multiple implementations are possible, prioritize them in this order:

1. Correctness and system safety.
2. Compatibility with the actual target environment.
3. Clear separation of responsibilities.
4. Reuse of existing services and infrastructure.
5. Modularity and maintainability.
6. Runtime performance and resource efficiency.
7. Simplicity and minimal dependencies.
8. Visual consistency and user experience.

Avoid optimizing for hypothetical future requirements at the expense of the current implementation.

Architecture should evolve when real requirements justify it, not through speculative complexity.

---

## 20. Canonical Repository Structure

This section is the single source of truth for the repository layout. `AGENTS.md` §5, `COMPONENT_SPECS.md` §4 and the skills reference it and must not define a competing tree.

```text
mao-shell/
├── shell.qml
├── modules/
│   ├── bar/
│   ├── launcher/
│   ├── wallpaper/
│   ├── dashboard/
│   ├── control-center/
│   ├── power-menu/
│   ├── notifications/
│   ├── media/
│   ├── clipboard/
│   ├── files/
│   ├── lock-screen/
│   ├── settings/
│   └── assistant/
├── components/
│   ├── buttons/
│   ├── cards/
│   ├── panels/
│   ├── popups/
│   ├── icons/
│   ├── inputs/
│   ├── indicators/
│   ├── controls/
│   └── layouts/
├── services/
│   ├── theme/
│   ├── config/
│   ├── hyprland/
│   ├── audio/
│   ├── network/
│   ├── bluetooth/
│   ├── notifications/
│   ├── media/
│   ├── clipboard/
│   ├── weather/
│   ├── updates/
│   ├── system/
│   └── assistant/
├── config/
│   ├── defaults.json
│   ├── modules/
│   ├── profiles/
│   └── shortcuts/
├── assets/
│   ├── icons/
│   ├── fonts/
│   └── sounds/
├── translations/
├── scripts/
│   ├── install.sh
│   ├── uninstall.sh
│   ├── update.sh
│   └── check.sh
├── tests/
├── docs/
│   └── validation/
├── README.md
├── LICENSE
├── AGENTS.md
├── ROADMAP.md
├── TASKS.md
├── COMPONENT_SPECS.md
├── DESIGN_PATTERNS.md
└── CHANGELOG.md
```

### Placement rules

- The layout is flat. "Core UI" and "feature components" are conventions expressed through the dependency rules in §4 and §2, not through extra directories.
- Listed directories are reserved locations. Create a directory only when the phase that needs it begins, and only with content.
- Typography, spacing, radius and animation tokens belong to `services/theme/`, not to `components/`.
- Hardware metrics and sensors belong to `services/system/`.
- The settings interface belongs to `modules/settings/`.
- Wallpaper images are user data. They are not stored in the repository; the wallpaper module scans the user's configured directory.
- `translations/` and `assets/sounds/` are reserved. Their formats are pending (`COMPONENT_SPECS.md` §34.9 and §31.1).
- Validation documents belong in `docs/validation/`. Large evidence artifacts (screenshots, benchmarks, raw logs) are stored outside the repository in `~/.local/share/mao-shell/evidence/` and are never committed.
- Changes to this structure must be made in this section first. Other documents follow it.

---

## 21. Development Conventions

These conventions apply to all code and documentation. Rules marked *validate in P0-005* depend on tooling that P0-005 runs for the first time.

### 21.1 Naming

- **Directories:** lowercase kebab-case (`control-center`, `lock-screen`), as in §20.
- **QML files:** PascalCase, named after the type they declare (`ActionButton.qml`). One visual component per file. The entry point is the exception: it is `shell.qml`, as fixed by AGENTS §5.
- **Service files:** PascalCase with the `Service` suffix (`AudioService.qml`).
- **JavaScript files:** camelCase (`formatDuration.js`).
- **Shell scripts:** kebab-case with `.sh` (`check.sh`, `install.sh`).
- **Properties, functions and object ids:** camelCase. Ids name the role of the object (`root`, `label`), not its type.
- **Signals:** camelCase, named for the event (`clicked`, `valueChanged`).
- **Theme tokens:** camelCase semantic names from §1 (`surfaceElevated`, `textMuted`).
- **Configuration keys in JSON:** camelCase.
- **Translation keys:** lowercase, dot-separated by area (`bar.clock.label`).
- **Component names** must describe purpose, not appearance. Avoid prefixes that describe the project or the framework.

### 21.2 QML and JavaScript organisation

- Import order: Quickshell modules first, then Qt modules, then project imports.
- A module imports services and components only through their public interfaces (§3). It never imports another module's internal files.
- Injected dependencies are declared with `required property`, for example `required property var theme`.
- Presentation stays in QML. Pure logic (no QML objects) goes to `.js` files, marked with `.pragma library` when it is stateless.
- Avoid global mutable state in JavaScript (AGENTS §6).
- Comments explain non-obvious decisions only (AGENTS, code comments rule).

### 21.3 Shell scripts

- Start with `#!/usr/bin/env bash` and `set -euo pipefail`.
- Pass arguments as arrays. Never interpolate user-controlled values into a command string (§9).
- Every script must pass `bash -n` and ShellCheck (AGENTS §17). *Validate in P0-005.*

### 21.4 Git practice

- Commit messages start with `Add:`, `Fix:` or `Feat:` (AGENTS §19).
- Each commit is one logical change. Documentation and code for the same task may share a commit.
- Commit only when the user has authorized commits for the task (AGENTS §26).
- Never commit validation evidence, logs, screenshots or local configuration. `.gitignore` covers these.
- Release tags use `vMAJOR.MINOR.PATCH`, following SemVer (`COMPONENT_SPECS.md` §41).
- `master` is the integration branch. A branching strategy is defined when there is more than one contributor.

### 21.5 Validation tooling (*validate in P0-005*)

- QML: `qmllint`, from `qt6-declarative`, over `shell.qml` and the modules.
- Scripts: `bash -n` and ShellCheck.
- Runtime: the isolated nested-Hyprland procedure used in P0-003.

---

## Final Principle

mao-shell must be built as a cohesive desktop shell, not as a collection of unrelated widgets.

Every module should follow the same architectural conventions, consume shared services, respect centralized configuration and theming, and remain independently maintainable.

Prefer explicit interfaces, predictable behavior, safe system integration, and simple implementations over unnecessary abstraction or complexity.

**Build a shell that is expressive in its design, disciplined in its architecture, and reliable in everyday use.**