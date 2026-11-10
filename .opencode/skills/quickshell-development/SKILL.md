---
name: quickshell-development
description: Specialized development guidance for mao-shell using Quickshell, QML and JavaScript. Use automatically when creating, modifying, debugging or reviewing Quickshell applications, QML components, JavaScript logic, reactive state, shell surfaces, Wayland integration, lifecycle management, performance-sensitive UI or Quickshell architecture.
---

# Quickshell Development

## 1. Purpose

This skill defines the technical development standards for mao-shell using Quickshell, QML and JavaScript.

Its purpose is to ensure that implementations are:

- Compatible with the installed Quickshell version.
- Modular, maintainable and easy to extend.
- Built around native QML reactivity.
- Efficient in long-running desktop environments.
- Compatible with Wayland and Hyprland.
- Safe when interacting with the operating system.
- Properly documented and incrementally validated.

This skill provides specialized technical guidance. It does not replace the project's architectural documentation or functional specifications.

The agent must apply these instructions whenever the task involves Quickshell development.

---

## 2. When to Use This Skill

Use this skill when working on:

- Quickshell applications and shell entrypoints.
- QML components and visual elements.
- JavaScript logic used by QML.
- Properties, bindings, signals and slots.
- Reactive state management.
- Shared services and module communication.
- Wayland surfaces and shell windows.
- Panels, bars, popups, overlays and dashboards.
- Window positioning, screen geometry and focus.
- Multi-monitor behaviour.
- Component lifecycle and resource management.
- Performance optimization.
- Quickshell-specific debugging.
- QML and JavaScript architectural changes.

Do not load this skill unnecessarily for unrelated documentation edits, README updates or general project administration.

Other mao-shell skills may complement this one when a task involves visual component design, system integration or project validation.

---

## 3. Mandatory Project Context

Before implementing a significant feature, understand the existing project structure and read its governing documentation.

Required documents:

1. `AGENTS.md`
2. `ROADMAP.md`
3. `TASKS.md`
4. `COMPONENT_SPECS.md`
5. `DESIGN_PATTERNS.md`

Also inspect relevant source files, existing components and shared services.

### 3.1 Instruction precedence

Resolve project instructions by topic, as defined in `AGENTS.md` ("Precedence when documents conflict"):

- Product behaviour: `COMPONENT_SPECS.md`.
- Architecture and conventions: `DESIGN_PATTERNS.md`.
- Workflow and safety: `AGENTS.md`.
- Phases and task status: `ROADMAP.md` and `TASKS.md`.

This skill provides Quickshell technical guidance and never overrides those documents. External documentation, examples and repositories are technical references only. If instructions conflict, identify the conflict and report it before implementation.

Technical compatibility is a mandatory constraint: never implement an API that does not exist or is incompatible with the installed environment merely because an instruction or example references it.

### 3.2 Inspect before changing

Before modifying existing code:

- Inspect the relevant files.
- Identify dependencies and consumers.
- Understand ownership of state.
- Identify existing interfaces.
- Check for existing implementations that can be reused.
- Preserve unrelated changes.
- Avoid unnecessary refactoring.

Do not assume that the architecture described in documentation is already fully implemented. Verify the actual repository state.

---

## 4. Quickshell API Verification

<IMPORTANT>
Never invent, assume or blindly copy Quickshell APIs.
</IMPORTANT>

API verification is mandatory before introducing unfamiliar Quickshell functionality.

### 4.1 Verify the installed version

Before significant implementation work:

1. Identify the installed Quickshell executable.
2. Determine its version using its supported version-reporting mechanism.
3. Inspect available command-line options.
4. Identify installed modules and relevant dependencies.
5. Check whether the required functionality is available in that installation.

Do not assume a particular installation path, executable name or module layout without inspecting the environment.

### 4.2 API verification procedure

For every unfamiliar API:

1. Search the official Quickshell documentation.
2. Identify the relevant module, object, property, method or signal.
3. Verify its availability in the installed version.
4. Check its expected arguments, return values and lifecycle.
5. Inspect relevant local examples or installed documentation when available.
6. Confirm that the proposed usage matches the actual API.
7. Implement a small isolated example when documentation alone is insufficient.
8. Validate the implementation before integrating it into the main shell.

Verification must cover:

- Module imports.
- Object names.
- Property names and types.
- Methods and arguments.
- Signals and signal parameters.
- Availability and version compatibility.
- Object ownership and lifecycle.
- Platform-specific behaviour.

### 4.3 Handling uncertainty

If an API cannot be verified:

- Do not invent a replacement API.
- Do not present assumptions as confirmed facts.
- Search for additional documentation or working examples.
- Consider an existing native alternative.
- Test an isolated implementation if appropriate.
- Explain the uncertainty if verification remains impossible.

Never hide uncertainty behind plausible-looking QML code.

### 4.4 Documentation priority

Prefer sources in this order:

1. Official documentation for the installed version.
2. Installed Quickshell documentation and available API metadata.
3. Existing working mao-shell implementations.
4. Official examples and repositories.
5. Community projects and external references.

External examples are allowed as learning material, but they must be independently verified and adapted.

Never assume an external repository targets the same Quickshell version.

---

## 5. Architecture and Code Organization

Use a modular and balanced architecture.

The goal is to separate responsibilities without creating unnecessary abstractions or excessive file fragmentation.

### 5.1 Separation of responsibilities

Use the following conceptual boundaries:

**QML presentation**
- Visual composition.
- Component structure.
- Property bindings.
- Signals and UI interaction.
- Layout and visual state.

**JavaScript logic**
- Data transformation.
- Reusable calculations.
- Lightweight business logic.
- Utility functions.
- Processing that does not belong to presentation.

**Shared services**
- Shared application state.
- Data providers.
- System-facing functionality.
- Cross-module coordination.
- Configuration access.

**Application composition**
- Shell entrypoint.
- Module instantiation.
- Top-level ownership.
- Connections between major subsystems.

Follow the existing project structure instead of introducing a competing architecture.

### 5.2 Modular design rules

- Give each module a clear responsibility.
- Keep public interfaces small and understandable.
- Avoid exposing internal implementation details.
- Reuse components when reuse provides genuine value.
- Avoid duplicating shared logic.
- Avoid oversized QML files.
- Avoid splitting trivial code into unnecessary files.
- Avoid circular dependencies.
- Avoid excessive abstraction.
- Prefer explicit dependencies over hidden coupling.

A component should be understandable without requiring the reader to inspect unrelated modules.

### 5.3 Clean Code principles

Apply pragmatic Clean Code:

- Use descriptive names.
- Keep functions focused.
- Avoid unnecessary nesting.
- Avoid duplicated logic.
- Keep state ownership explicit.
- Prefer straightforward implementations.
- Document non-obvious decisions.
- Avoid premature generalization.
- Follow existing project naming conventions.

Do not optimize for theoretical architectural purity at the expense of maintainability.

---

## 6. QML Development Guidelines

QML is the primary language for mao-shell's presentation layer.

Use QML features naturally instead of recreating their behaviour manually in JavaScript.

### 6.1 Component design

A reusable component should have:

- A clear purpose.
- Explicit public properties.
- Predictable signals.
- Well-defined inputs and outputs.
- Controlled internal state.
- A documented lifecycle.
- Consistent visual and interaction behaviour.

Avoid components with too many unrelated responsibilities.

Do not expose internal properties unless another component genuinely needs them.

### 6.2 Properties and bindings

Prefer declarative property bindings.

Example of a reactive binding:

```qml
Item {
    property bool active: false

    opacity: active ? 1.0 : 0.5
}
```

The displayed opacity automatically follows the value of `active`.

Avoid manually synchronizing values that QML can derive through bindings.

Bad pattern:

```qml
Item {
    property bool active: false
    property real displayedOpacity: 0.5

    function updateOpacity() {
        displayedOpacity = active ? 1.0 : 0.5
    }
}
```

This creates redundant state and requires manual synchronization.

### 6.3 Binding safety

- Preserve reactive bindings.
- Avoid accidentally replacing bindings through imperative assignments.
- Do not maintain duplicate representations of the same state.
- Avoid binding loops.
- Keep binding expressions understandable.
- Move expensive processing outside frequently evaluated bindings.
- Use explicit derived properties when they improve readability.

### 6.4 Signals and slots

Use signals to communicate events and actions.

Guidelines:

- Define clear signal names.
- Keep signal parameters minimal.
- Avoid signals that duplicate existing properties or events.
- Avoid creating multiple connections for the same purpose.
- Understand connection lifetime.
- Ensure dynamically created connections are cleaned up appropriately.
- Prefer direct component interfaces over global event mechanisms.

### 6.5 JavaScript inside QML

Use JavaScript for logic that benefits from procedural implementation.

Examples include:

- Formatting data.
- Transforming collections.
- Calculations.
- Small reusable utilities.
- Processing event data.

Avoid:

- Large business-logic implementations inside visual components.
- Manual state synchronization.
- Excessive imperative UI updates.
- Dynamic code evaluation.
- Unnecessary global variables.
- Blocking operations.

Keep JavaScript modules focused and reusable.

### 6.6 QML lifecycle

Understand the lifecycle of every relevant object.

Pay attention to:

- Object creation.
- Component completion.
- Destruction.
- Signal connections.
- Timers.
- Processes.
- Asynchronous operations.
- References between objects.

Never assume a component remains alive while an asynchronous operation is running.

Protect against stale callbacks and references to destroyed objects.

Use the actual lifecycle mechanisms supported by the installed Quickshell and QML versions.

---

## 7. Reactive State Management

Native QML reactivity is the default state-management approach.

### 7.1 Core principles

- Prefer properties and bindings.
- Use signals for events.
- Maintain a single source of truth.
- Keep derived state derived.
- Avoid manually updating dependent UI.
- Avoid duplicated state across modules.
- Keep state ownership explicit.

### 7.2 Shared state

When multiple components require the same data, use an appropriate shared service.

Examples:

- Active workspace information.
- Audio state.
- Media playback information.
- Screen information.
- Shared application configuration.
- System status.

The service owns the shared data and exposes a defined interface.

Components consume that interface instead of accessing each other's internal state.

### 7.3 Communication model

Use a hybrid model:

- Shared services for shared data and functionality.
- QML signals for component events.
- Properties and bindings for reactive updates.
- Explicit interfaces for cross-module communication.

Avoid introducing a global event bus unless a concrete architectural need justifies it.

### 7.4 State categories

Keep the following categories separate:

**Persistent configuration**
- User preferences.
- Profiles.
- Layout configuration.
- Module settings.

**Transient UI state**
- Expanded panels.
- Hover states.
- Current selection.
- Temporary visibility.
- Animation state.

**External system state**
- Audio devices.
- Workspaces.
- Network status.
- Media playback.
- Monitor information.

Do not persist transient UI state unless the product specification explicitly requires it.

---

## 8. Wayland and Shell Surfaces

Wayland integration is a core responsibility of Quickshell development.

mao-shell is not simply a conventional desktop application. It manages desktop shell surfaces and interacts with the compositor.

### 8.1 Surface management

Understand the available Quickshell mechanisms for:

- Panels.
- Bars.
- Popups.
- Overlays.
- Floating surfaces.
- Desktop-level UI.
- Layer-shell integration.
- Surface positioning.
- Surface visibility.
- Focus behaviour.

Verify every relevant API against the installed version.

Do not assume that ordinary application windows and shell surfaces have identical behaviour.

### 8.2 Hyprland integration

- Prefer Quickshell's native Hyprland integration when it covers the requirement.
- Verify the installed module and its supported functionality.
- Keep compositor-specific logic separate from generic UI.
- Avoid unnecessary dependencies on external Hyprland commands.
- Do not assume that every Wayland compositor behaves like Hyprland.
- Do not modify Hyprland configuration as part of ordinary QML implementation.

When a feature genuinely depends on Hyprland, make that dependency explicit.

### 8.3 Layer and positioning behaviour

When implementing shell surfaces:

- Verify the available layer and anchoring mechanisms.
- Consider exclusive zones where applicable.
- Understand keyboard interactivity.
- Consider focus acquisition and release.
- Handle popup positioning and boundaries.
- Avoid covering important desktop interaction areas unintentionally.
- Validate behaviour with actual compositor interactions.

Do not assume that a visually correct surface automatically has correct Wayland behaviour.

### 8.4 Focus and interaction

Pay special attention to:

- Keyboard focus.
- Focus restoration.
- Popup dismissal.
- Escape handling.
- Pointer interaction.
- Surface stacking.
- Input regions.
- Interaction with normal application windows.

A panel or popup must not unexpectedly steal focus or make the desktop unusable.

### 8.5 Graceful platform behaviour

When an operation is unavailable:

- Detect the limitation.
- Avoid crashing the shell.
- Provide an appropriate fallback where possible.
- Report the limitation clearly.
- Do not simulate successful functionality that is unavailable.

---

## 9. Multi-Monitor Support

Multi-monitor support is a first-class architectural requirement.

Do not design new components assuming that only one screen exists.

### 9.1 Screen awareness

Use verified Quickshell screen-management capabilities.

Consider:

- Multiple connected screens.
- Different resolutions.
- Different scale factors.
- Different geometries.
- Different orientations.
- Primary and secondary screens.
- Screen connection and disconnection.
- Runtime configuration changes.

### 9.2 Dynamic screen lifecycle

The application must handle screens appearing and disappearing.

When a screen becomes unavailable:

- Detect the change.
- Avoid references to invalid screen objects.
- Destroy or reassign dependent surfaces appropriately.
- Release resources.
- Preserve application stability.

When a screen becomes available:

- Create the necessary surfaces.
- Apply the appropriate configuration.
- Restore the correct relationships.
- Avoid duplicate instances.

### 9.3 Component responsibilities

Do not make every widget responsible for screen discovery.

Centralize screen-related information where appropriate.

Individual components should receive their screen context through explicit interfaces.

### 9.4 Geometry and scaling

Avoid assumptions about:

- Fixed screen dimensions.
- One global scale factor.
- Identical monitor orientations.
- A permanent primary monitor.
- Static screen availability.

Validate geometry-dependent UI under different display configurations whenever practical.

---

## 10. Performance Requirements

Performance is a strict design requirement.

mao-shell is a long-running desktop process. Every continuously active module contributes to resource consumption.

### 10.1 General principles

- Avoid blocking the UI thread.
- Avoid unnecessary polling.
- Avoid excessive process creation.
- Avoid redundant data processing.
- Avoid expensive bindings.
- Avoid unnecessary object creation.
- Avoid repeated allocations in frequently executed code.
- Avoid duplicate subscriptions to shared data.
- Avoid unnecessary visual updates.

### 10.2 Asynchronous operations

Potentially slow operations must not block the interface.

Use appropriate asynchronous mechanisms for:

- External processes.
- File operations.
- Network operations.
- Data retrieval.
- System queries.
- Expensive calculations.

Prefer native Quickshell facilities when available and verified.

### 10.3 Polling

Prefer event-driven updates.

When polling is genuinely necessary:

- Justify why events are unavailable.
- Use an appropriate interval.
- Avoid multiple timers performing the same work.
- Stop polling when the relevant functionality is inactive.
- Avoid polling more frequently than the data requires.

Do not use timers to compensate for missing understanding of the framework's reactive capabilities.

### 10.4 Shared resource usage

- Share expensive data providers.
- Avoid launching duplicate processes.
- Reuse system information where appropriate.
- Keep service ownership explicit.
- Release unused resources.
- Avoid keeping inactive modules unnecessarily busy.

### 10.5 Animations

Animations should be subtle and functional.

- Prefer short, coherent transitions.
- Reuse animation conventions.
- Avoid continuous decorative animations.
- Avoid unnecessary timers.
- Avoid blocking interactions during transitions.
- Handle interrupted animations.
- Respect reduced-motion preferences.

### 10.6 Performance validation

Do not claim performance improvements without evidence.

When optimizing:

1. Identify the suspected cost.
2. Inspect relevant code.
3. Measure or observe the actual behaviour.
4. Implement the smallest reasonable improvement.
5. Validate that functionality remains correct.
6. Compare results where practical.

Avoid speculative micro-optimizations.

---

## 11. Lifecycle and Resource Management

Long-running shell processes require careful lifecycle management.

### 11.1 Resource ownership

Every resource must have a clear owner.

This includes:

- Processes.
- Timers.
- Signal connections.
- Subscriptions.
- File handles.
- Asynchronous operations.
- Temporary objects.
- Cached data.

### 11.2 Cleanup

When a component or service is destroyed or becomes inactive:

- Stop unnecessary timers.
- Terminate owned processes when appropriate.
- Disconnect dynamically managed connections.
- Release references.
- Cancel operations when supported.
- Prevent callbacks from accessing destroyed objects.

Do not leave background work running indefinitely without justification.

### 11.3 Shared services

Shared services must have an explicit lifecycle.

Avoid starting duplicate instances of the same system integration.

Use clear ownership and initialization rules.

### 11.4 Asynchronous result safety

An asynchronous result may arrive after the original component has disappeared or its context has changed.

Ensure that:

- Results are checked before being applied.
- Stale results do not overwrite newer state.
- Destroyed components are not accessed.
- Cancellation is used when available.
- Failures are handled explicitly.

---

## 12. Error Handling and Recovery

Errors must be explicit, diagnosable and recoverable where possible.

### 12.1 Error principles

- Never silently swallow errors.
- Never pretend an operation succeeded.
- Validate external data.
- Handle unavailable services.
- Handle missing dependencies.
- Handle process failures.
- Distinguish expected unavailability from unexpected errors.
- Preserve the functionality of unrelated modules.

### 12.2 Module isolation

A failure in one module should not unnecessarily terminate the entire shell.

Where possible:

- Isolate failures.
- Keep unrelated modules operational.
- Expose the failure state.
- Provide recovery mechanisms.
- Avoid repeated automatic retries without limits.

Do not hide architectural failures by wrapping everything in broad exception handling.

### 12.3 Recovery behaviour

Use controlled recovery.

Examples:

- Reconnect to a service when it becomes available.
- Refresh stale data.
- Recreate a failed component when safe.
- Disable an unavailable optional feature.
- Provide a meaningful fallback.

Avoid infinite retry loops.

### 12.4 Development and production

**Development mode**

- Detailed diagnostic information.
- Explicit errors.
- Clear warnings.
- Easier failure reproduction.
- Fast feedback during implementation.

**Production mode**

- Controlled recovery.
- Graceful degradation.
- Useful diagnostic records.
- No unnecessary user-facing technical noise.

Production behaviour must not hide important failures from diagnostics.

---

## 13. Structured Logging and Diagnostics

Use structured, module-oriented logging.

### 13.1 Logging conventions

Logs should identify:

- The originating module or service.
- The severity.
- The relevant operation.
- Useful contextual information.
- The failure reason when applicable.

Use appropriate severity levels:

- Debug.
- Information.
- Warning.
- Error.

Prefer native Quickshell and QML logging facilities.

Verify their actual availability and usage.

### 13.2 Logging quality

- Avoid repetitive messages.
- Avoid logging every reactive update.
- Avoid noisy messages inside frequently evaluated bindings.
- Avoid logging sensitive information.
- Include enough context to diagnose failures.
- Do not use temporary debugging output as permanent logging.

### 13.3 Module diagnostics

Where useful, make module state observable:

- Initializing.
- Active.
- Unavailable.
- Degraded.
- Failed.
- Recovering.

Use consistent terminology across the project.

Do not create a separate logging framework without a justified requirement.

---

## 14. Configuration Integration

Quickshell components must consume mao-shell's centralized configuration service.

Do not create parallel configuration systems.

### 14.1 Configuration ownership

The central configuration system owns persistent user preferences.

Components should:

- Read configuration through its public interface.
- React to configuration changes.
- Respect validated defaults.
- Avoid direct file access.
- Avoid writing configuration independently.

### 14.2 State separation

Distinguish:

- Persistent user configuration.
- Temporary interface state.
- External system state.

Do not store transient interaction state in persistent configuration without an explicit product requirement.

### 14.3 Adding configuration options

When a feature needs new settings:

1. Inspect the existing configuration architecture.
2. Identify the appropriate configuration section.
3. Define sensible defaults.
4. Validate accepted values.
5. Integrate through the existing service.
6. Document the new option.
7. Avoid introducing another persistence mechanism.

Do not independently implement JSON loading, saving or migrations inside ordinary components.


The complete architecture is located in DESIGN_PATTERNS.md

---

## 15. Security Requirements

Security is strict and mandatory.

### 15.1 External process execution

When external processes are unavoidable:

- Prefer native Quickshell functionality first.
- Use argument-safe process APIs.
- Never concatenate untrusted input into shell commands.
- Avoid unnecessary shell interpreters.
- Validate arguments.
- Handle exit codes.
- Handle standard output and standard error appropriately.
- Apply timeouts where appropriate.
- Support cancellation where possible.
- Avoid blocking the interface.

Never assume external process output is trustworthy.

### 15.2 Dynamic execution

Avoid:

- `eval()`.
- Unnecessary dynamic code execution.
- Unsafe command construction.
- Unvalidated executable paths.
- Arbitrary execution of user-provided strings.

Use explicit, constrained execution paths.

### 15.3 Privileges

- Never introduce unnecessary root privileges.
- Do not invoke `sudo` from ordinary shell functionality.
- Do not modify system files from QML components.
- Do not install packages automatically.
- Do not change system services without authorization.

### 15.4 Sensitive information

Do not expose or log:

- Credentials.
- Authentication tokens.
- Private keys.
- Sensitive environment variables.
- Unnecessary personal information.

Treat data received from external processes and services as untrusted.

### 15.5 Destructive operations

Destructive actions must have appropriate user confirmation.

Do not perform irreversible operations merely because they appear convenient during development.

---

## 16. Accessibility and Interaction

Accessibility is a required part of component development.

### 16.1 Keyboard interaction

Interactive components should support:

- Keyboard navigation.
- Appropriate focus behaviour.
- Visible focus indicators.
- Predictable keyboard actions.
- Appropriate Escape behaviour.
- Logical interaction order.

Do not rely exclusively on pointer interaction.

### 16.2 Visual accessibility

Consider:

- Text contrast.
- Background contrast.
- Readable typography.
- Independent font scaling.
- Appropriate interaction sizes.
- Clear disabled states.
- Clear active and selected states.

Do not use colour as the only indicator of state.

### 16.3 Motion

Respect reduced-motion preferences.

Provide appropriate alternatives to unnecessary animations.

### 16.4 Accessibility interfaces

Use available QML accessibility and semantic facilities where supported.

Verify the relevant APIs and properties instead of assuming they exist.

### 16.5 Hover behaviour

Hover may provide additional information, but it must not be the only way to access important functionality.

Ensure equivalent interaction paths where appropriate.

---

## 17. Documentation Requirements

Documentation is strict.

Document modules and public interfaces consistently.

### 17.1 What must be documented

Document:

- Module purpose.
- Component responsibilities.
- Public properties.
- Public signals.
- Public functions.
- Inputs and outputs.
- Shared service interfaces.
- Dependencies.
- Lifecycle expectations.
- Important limitations.
- Non-obvious architectural decisions.
- Workarounds.
- External integration requirements.

### 17.2 Documentation quality

Documentation must be:

- Accurate.
- Concise but sufficiently detailed.
- Consistent with the implementation.
- Updated when interfaces change.
- Written in clear technical English.

Do not document implementation details that are likely to change unless they are relevant to understanding the system.

### 17.3 Documentation maintenance

Whenever an implementation changes:

1. Check whether its documented interface changed.
2. Update affected documentation.
3. Remove obsolete instructions.
4. Verify examples against the current implementation.

Never leave documentation knowingly inconsistent with the code.

---

## 18. External References and Dependencies

External repositories and community projects are allowed as technical references.

### 18.1 External references

They may be used to:

- Discover implementation approaches.
- Understand framework behaviour.
- Study architectural patterns.
- Investigate Wayland integration.
- Identify known limitations.
- Compare alternative solutions.

However:

- Never copy code blindly.
- Never assume version compatibility.
- Verify all APIs.
- Understand the implementation before adapting it.
- Preserve mao-shell's architecture and identity.
- Check licensing when incorporating external code.

External examples are references, not project specifications.

### 18.2 Dependency policy

Use a minimal and native-first approach.

Priority:

1. Quickshell core functionality.
2. Official Quickshell modules.
3. Existing mao-shell services.
4. Already installed system tools.
5. Additional dependencies only when justified.

Before introducing a dependency:

- Explain why it is necessary.
- Check whether native functionality already exists.
- Check whether an existing dependency can provide it.
- Consider maintenance and compatibility.
- Consider resource consumption.
- Explain any additional installation requirement.

Never install or update system packages without authorization.

Do not reinvent complex functionality simply to avoid every external dependency.

---

## 19. Compatibility and Version Management

The installed Quickshell version is the compatibility target.

### 19.1 Compatibility rules

- Prefer APIs available in the installed version.
- Avoid assuming newer APIs exist.
- Avoid obsolete APIs when a supported alternative exists.
- Verify module availability.
- Verify imported types.
- Verify platform-specific behaviour.
- Validate compatibility before integrating unfamiliar functionality.

### 19.2 Version changes

Do not update Quickshell simply to make a preferred implementation possible.

If an update appears necessary:

1. Explain the technical limitation.
2. Identify compatible alternatives.
3. Explain the consequences of upgrading.
4. Ask for authorization before changing the environment.

### 19.3 No speculative compatibility

Do not introduce compatibility layers for hypothetical versions unless the project genuinely needs them.

Prioritize the real environment.

---

## 20. Incremental Development Workflow

Development must proceed in small, verifiable steps.

Do not implement an entire feature set before testing its foundations.

### 20.1 Before implementation

1. Read the relevant project documentation.
2. Inspect existing code.
3. Identify the feature's responsibilities.
4. Identify its dependencies.
5. Verify unfamiliar APIs.
6. Define the smallest useful implementation.
7. Identify how it can be tested.
8. Check whether another skill should complement this one.

### 20.2 During implementation

Work in small increments.

For each increment:

1. Implement a focused change.
2. Inspect the resulting code.
3. Check imports and API usage.
4. Validate bindings and signals.
5. Check lifecycle and resource ownership.
6. Run relevant diagnostics.
7. Test the behaviour.
8. Correct known issues before proceeding.

Avoid accumulating unverified changes.

### 20.3 After implementation

- Review the final changes.
- Check for duplicated logic.
- Check for unnecessary abstractions.
- Check resource cleanup.
- Check error handling.
- Check accessibility.
- Check performance implications.
- Update documentation.
- Report validation results.

### 20.4 Testing philosophy

Use incremental validation.

Automated tests are encouraged when they are practical and meaningful.

Manual validation remains important for:

- Visual behaviour.
- Wayland surfaces.
- Focus.
- Animations.
- Multi-monitor behaviour.
- Compositor interactions.
- Real system integrations.

Never claim that a feature works merely because its source code was written successfully.

---

## 21. Development Runtime Isolation

Testing must not interfere with the user's active desktop session.

Use an isolated development environment whenever practical.

### 21.1 Isolation requirements

Before launching another Quickshell instance:

- Identify the active instance.
- Understand how the installed version handles instance identification.
- Check for socket or IPC conflicts.
- Check configuration and runtime directory behaviour.
- Check process ownership.
- Check whether the new instance could create conflicting surfaces.
- Establish an appropriate isolated configuration.

Do not assume that a separate process automatically means a separate runtime.

### 21.2 Protect the active session

Never:

- Kill the active Quickshell process without authorization.
- Replace the production configuration automatically.
- Modify the user's startup configuration.
- Change Hyprland startup behaviour without authorization.
- Replace the active desktop shell as an ordinary testing step.

### 21.3 When isolation is insufficient

If a feature cannot be safely tested using an isolated instance:

1. Explain the limitation.
2. Prepare the necessary test procedure.
3. Identify the potential impact.
4. Request authorization before affecting the active session.

### 21.4 Development and production

Keep development and production concerns separate.

Do not mix experimental settings, test data or temporary files with the user's production configuration.

Preserve a clear distinction between the two environments.

---

## 22. Code Examples and Anti-Patterns

Examples must teach correct concepts without pretending that unverified Quickshell APIs are universal.

### 22.1 Reactive QML example

A simple property binding:

```qml
Item {
    property bool expanded: false

    opacity: expanded ? 1.0 : 0.0
    visible: expanded
}
```

The presentation follows the component's state declaratively.

### 22.2 Derived state anti-pattern

Avoid maintaining redundant values:

```qml
Item {
    property bool expanded: false
    property real opacityValue: 0.0

    function updateState() {
        opacityValue = expanded ? 1.0 : 0.0
    }
}
```

This requires manual synchronization and creates opportunities for inconsistent state.

Prefer a binding when the value is directly derived.

### 22.3 Shared state anti-pattern

Avoid making unrelated components responsible for maintaining separate copies of the same external state.

Bad architectural pattern:

```text
VolumeWidget
    └── Own audio state

AudioDashboard
    └── Own another audio state

ControlCenter
    └── Own another audio state
```

Prefer:

```text
Shared Audio Service
    ├── VolumeWidget
    ├── AudioDashboard
    └── ControlCenter
```

The shared service owns the state and exposes an appropriate interface.

### 22.4 Excessive abstraction anti-pattern

Avoid creating generic frameworks for functionality that only requires a small reusable component.

Do not introduce abstraction layers without a demonstrated need.

### 22.5 Blocking execution anti-pattern

Never perform potentially slow process or network operations synchronously on the UI thread.

Use verified asynchronous facilities.

### 22.6 Unverified API anti-pattern

Never write code based on an assumed import, property or method.

Bad reasoning:

"The API probably exists because another Quickshell example uses a similar name."

Correct reasoning:

"Verify the module and API against the installed version before implementation."

### 22.7 Lifecycle anti-pattern

Avoid starting timers, processes or connections without defining who owns them and when they stop.

Every resource must have an explicit lifecycle.

### 22.8 Logging anti-pattern

Avoid printing diagnostic messages on every frequent reactive update.

Log meaningful transitions and failures instead.

### 22.9 External process anti-pattern

Never build shell commands by concatenating untrusted values.

Use argument-safe process execution.

### 22.10 Multi-monitor anti-pattern

Never assume that a screen object, geometry or primary monitor will remain unchanged throughout the application's lifetime.

Handle screen changes explicitly.

---

## 23. Collaboration With Other mao-shell Skills

This skill is part of a broader development workflow.

Use complementary skills when appropriate.

### `qml-component`

Responsible for detailed visual component implementation standards.

Use it when:

- Creating reusable visual controls.
- Building cards, surfaces and popups.
- Applying the design system.
- Implementing interaction states.
- Maintaining visual consistency.

`quickshell-development` provides the underlying technical QML and Quickshell guidance.

### `system-integration`

Responsible for system-facing integrations.

Use it when:

- Integrating PipeWire.
- Working with MPRIS.
- Interacting with Bluetooth.
- Accessing network information.
- Communicating with Hyprland.
- Managing external processes.
- Integrating operating system services.

`quickshell-development` covers how those integrations fit into the Quickshell architecture.

### `project-validation`

Responsible for broader project validation.

Use it when:

- Reviewing completed features.
- Running project-wide checks.
- Auditing dependencies.
- Reviewing architectural consistency.
- Validating installation and project health.

`quickshell-development` remains responsible for incremental technical validation during implementation.

Do not duplicate the full responsibilities of these skills.

---

## 24. Working Autonomy

Use balanced autonomy.

### Act independently when:

- The requirement is clear.
- The API is verified.
- The implementation follows existing architecture.
- The decision is a minor technical detail.
- The change is reversible and limited in scope.

### Ask the user when:

- Functional behaviour is ambiguous.
- Multiple reasonable product interpretations exist.
- An important architectural decision is required.
- Requirements conflict.
- A destructive operation is involved.
- The active desktop session may be affected.
- A system dependency or package change is required.
- A Quickshell upgrade appears necessary.

Do not ask unnecessary questions about routine implementation details.

When making a reasonable technical assumption, keep it consistent with the project documentation.

---

## 25. Definition of Done

A Quickshell implementation is complete only when the relevant conditions below have been satisfied.

### API correctness

- [ ] Unfamiliar APIs were verified.
- [ ] Imports and types are compatible.
- [ ] The implementation matches the installed version.
- [ ] No speculative API usage remains.

### Architecture

- [ ] Responsibilities are clearly separated.
- [ ] State ownership is explicit.
- [ ] Interfaces are understandable.
- [ ] Unnecessary dependencies are avoided.
- [ ] No avoidable circular dependencies exist.

### QML and JavaScript

- [ ] Bindings remain reactive.
- [ ] Signals and connections are correctly managed.
- [ ] Derived state is not unnecessarily duplicated.
- [ ] Components have clear responsibilities.
- [ ] Code follows project conventions.

### Wayland and screens

- [ ] Surface behaviour is correct.
- [ ] Focus behaviour is considered.
- [ ] Positioning and layer behaviour are verified where relevant.
- [ ] Multi-monitor assumptions are avoided.
- [ ] Screen lifecycle is handled where relevant.

### Performance and lifecycle

- [ ] No unnecessary blocking operations exist.
- [ ] Polling is justified.
- [ ] Resources have clear ownership.
- [ ] Timers and processes are managed.
- [ ] Stale asynchronous results are handled.
- [ ] No obvious resource leaks remain.

### Error handling and security

- [ ] Errors are handled explicitly.
- [ ] Failures do not unnecessarily affect unrelated modules.
- [ ] External inputs are validated.
- [ ] Process execution is safe.
- [ ] No unauthorized privileged or destructive operation exists.

### Accessibility

- [ ] Keyboard interaction is considered.
- [ ] Focus is visible.
- [ ] Visual states are understandable.
- [ ] Reduced motion is respected where relevant.
- [ ] Hover is not the only interaction mechanism.

### Documentation and validation

- [ ] Required documentation is updated.
- [ ] Relevant diagnostics were executed.
- [ ] The implementation was tested incrementally.
- [ ] Development testing did not interfere with production.
- [ ] Limitations and unverified behaviour are reported.

Not every checklist item applies to every change. Apply the relevant requirements and explain any meaningful exception.

---

## 26. Final Development Principles

When developing mao-shell with Quickshell:

1. Verify before implementing.
2. Prefer native Quickshell capabilities.
3. Use QML reactivity instead of unnecessary imperative state management.
4. Keep architecture modular and understandable.
5. Treat performance as a permanent requirement.
6. Design for Wayland and multiple monitors.
7. Make failures explicit and recoverable.
8. Keep the active desktop session safe.
9. Document public interfaces and architectural decisions.
10. Validate every meaningful implementation increment.
11. Avoid unnecessary dependencies and abstractions.
12. Preserve the project's existing architecture and identity.

The objective is not simply to produce working QML code.

The objective is to build a cohesive, reliable, maintainable and efficient desktop shell that feels native to the user's Arch Linux and Wayland environment.