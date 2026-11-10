---
name: qml-component
description: Design, implement, review and maintain reusable QML components for mao-shell. Use when creating or modifying visual components, controls, layouts, popups, animations, component APIs, accessibility or component-specific interaction logic.
---

# QML Component Development

## 1. Purpose

This skill defines the standards, architecture, implementation practices, and validation requirements for creating and maintaining reusable visual components in mao-shell.

Its purpose is to ensure that every component contributes to a cohesive, accessible, performant, maintainable, and visually distinctive desktop shell.

The skill applies to:

- Core UI components.
- Feature-specific visual components.
- Composite components.
- Interactive controls.
- Popups and dashboards.
- Panels and overlays.
- Layout primitives.
- Visual indicators.
- Component refactoring and maintenance.

This skill covers the visual layer and its local interaction logic.

It does not replace the general Quickshell development skill or the system integration skill.

## 2. Scope and Responsibilities

### 2.1 Responsibilities

The agent is responsible for:

- Designing reusable QML components.
- Implementing component APIs.
- Managing component-local state.
- Implementing visual states and interactions.
- Applying the centralized theme system.
- Maintaining consistent visual behavior.
- Implementing component animations.
- Supporting accessibility requirements.
- Handling responsive and adaptive layouts.
- Defining integration contracts with shared services.
- Documenting public component APIs.
- Testing component behavior.
- Maintaining component performance.
- Preventing unnecessary duplication.
- Preserving existing component compatibility.

### 2.2 Out of Scope

The agent must not use this skill as authorization to independently implement system integration.

The following responsibilities belong primarily to `system-integration`:

- Hardware discovery.
- Operating system commands.
- Process management.
- D-Bus communication.
- IPC communication.
- Network management.
- Bluetooth device management.
- Audio device discovery.
- System service lifecycle.
- Direct system configuration changes.

Components may consume the results of these operations through approved service interfaces.

### 2.3 Responsibility Boundary

A component may own:

- Its visual presentation.
- Its interaction state.
- Its local validation.
- Its internal transitions.
- Its display formatting.
- Its local presentation logic.

A component must not independently own system-wide state or duplicate system integration services.

## 3. Mandatory Project Documentation

Before implementing or modifying components, inspect the relevant project documentation.

Required documents:

1. `AGENTS.md`
2. `ROADMAP.md`
3. `TASKS.md`
4. `COMPONENT_SPECS.md`
5. `DESIGN_PATTERNS.md`

Also inspect the applicable skill documentation and existing component implementations.

### 3.1 Documentation Authority

Resolve conflicts by topic, as defined in `AGENTS.md` ("Precedence when documents conflict"). Functional requirements come from `COMPONENT_SPECS.md`, architectural patterns from `DESIGN_PATTERNS.md`, and project-wide rules from `AGENTS.md`. This skill provides component-specific guidance and never overrides those documents.

Technical compatibility and actual API availability are mandatory regardless of documentation order.

### 3.2 Resolve Conflicts

If documentation conflicts:

- Identify the conflict.
- Avoid silently choosing a different product behavior.
- Preserve architectural and technical safety.
- Ask the user when the conflict affects functionality or architecture.
- Update documentation when an approved decision changes existing requirements.

Do not invent requirements to fill documentation gaps.

## 4. Component Architecture

### 4.1 Hybrid Component Architecture

mao-shell uses two component layers:

- Core UI components.
- Feature-specific components.

Core UI provides reusable visual primitives and interaction patterns.

Feature components implement specialized visual behavior for individual shell modules.

### 4.2 Core UI

Core UI should contain generic components such as:

- Buttons.
- Cards.
- Popups.
- Inputs.
- Indicators.
- Controls.
- Icons.
- Layout primitives.
- Separators.
- Tooltips.
- Menus.
- Progress indicators.
- Loading indicators.
- Dialog primitives.

Core components must remain independent of feature-specific implementation details.

### 4.3 Feature Components

Feature components belong to their respective modules.

Examples:

- Audio controls.
- Network status.
- Bluetooth device rows.
- Notification cards.
- Workspace indicators.
- Media player controls.
- Performance widgets.
- Calendar elements.

Feature components may consume Core UI components.

Core UI must never depend on feature modules.

### 4.4 Dependency Direction

The preferred dependency direction is:

`Feature Components -> Core UI -> Theme and Shared Infrastructure`

Avoid reverse dependencies.

Do not introduce circular imports.

Do not allow one feature module to depend on the internal implementation of another feature module.

Use documented shared interfaces when communication is required.

### 4.5 Suggested Organization

The repository layout is defined in `DESIGN_PATTERNS.md` §20. Core UI and feature components are distinguished by the dependency rules in §4.4, not by separate directories.

Do not create directories speculatively.

Follow the existing repository structure when it already provides an appropriate organization.

## 5. Component Design Philosophy

### 5.1 Reusability

Create reusable components when they represent a meaningful and recurring visual or interaction pattern.

Do not abstract every small visual element.

A component should have a clear responsibility.

Avoid abstractions that introduce more complexity than they remove.

### 5.2 Composition First

Prefer composing complex components from smaller reusable components.

For example, a device card may combine:

- A container.
- An icon.
- A title.
- A status indicator.
- An action button.

Use self-contained components when encapsulation makes a specific feature simpler.

Do not force composition when it makes the implementation unnecessarily fragmented.

### 5.3 Base Components and Variants

Use a base component when multiple components share the same fundamental behavior.

Introduce semantic variants for meaningful differences.

Examples:

- Primary button.
- Secondary button.
- Destructive button.
- Icon button.
- Compact button.

Do not create separate components for superficial differences that can be represented by a well-defined variant.

Do not build one enormous component containing dozens of unrelated conditional branches.

### 5.4 Component Responsibilities

Every component must have a clearly defined responsibility.

A component should not simultaneously manage:

- Global configuration.
- System integration.
- Unrelated feature logic.
- Persistent application state.
- Its own visual presentation.

Separate these responsibilities through established project services and component boundaries.

## 6. QML API Design

### 6.1 Hybrid API Model

Use explicit QML properties for common configuration.

Use configuration objects when a component genuinely needs a larger or more complex configuration surface.

The public API must remain understandable and predictable.

### 6.2 Explicit Properties

Prefer appropriately typed properties.

Examples:

```qml
property string title: ""
property bool enabled: true
property bool loading: false
property real radius: 12
property color accentColor
```

Use property aliases when exposing an existing internal property is appropriate.

Do not expose implementation details unnecessarily.

### 6.3 Configuration Objects

Configuration objects may be appropriate for:

- Complex visual variants.
- Multiple related options.
- Structured component configuration.
- Specialized layouts.

Do not use a configuration object merely to avoid declaring a few simple properties.

### 6.4 Public API Rules

Every public property must have:

- A clear purpose.
- A meaningful name.
- An appropriate type.
- A documented default when applicable.
- Defined behavior when changed.

Every public signal must represent a meaningful event.

Avoid signals that duplicate existing QML behavior without providing value.

### 6.5 API Stability

Before changing a public component API:

1. Search for existing consumers.
2. Identify affected modules.
3. Evaluate compatibility.
4. Prefer backward-compatible changes when practical.
5. Update documentation and examples.
6. Validate affected consumers.

Ask for approval before making changes with significant architectural or functional impact.

## 7. QML and Quickshell Technical Correctness

### 7.1 Verify APIs

Never invent QML or Quickshell APIs.

Before using an unfamiliar API:

1. Inspect the installed Quickshell version.
2. Consult the corresponding official documentation.
3. Verify the actual property, signal, method, and type.
4. Check availability in the installed Qt version.
5. Confirm that the intended behavior is supported.

Do not assume that an API from a newer version exists in the installed environment.

### 7.2 Prefer Native Capabilities

Prefer native QML and Quickshell functionality when appropriate.

Avoid unnecessary JavaScript implementations of functionality already supported by the framework.

Use JavaScript for suitable local logic, calculations, formatting, and transformations.

Do not introduce C++, CMake, qmake, or additional build systems.

### 7.3 Declarative Design

Prefer declarative QML bindings.

Avoid imperative property updates when a binding expresses the relationship more clearly.

Do not break reactive bindings accidentally through unnecessary assignments.

### 7.4 JavaScript Usage

Keep JavaScript functions focused and reasonably small.

Avoid large JavaScript blocks inside QML components.

Move reusable logic into appropriate project modules when justified.

Do not use JavaScript as a replacement for proper component architecture.

### 7.5 Runtime Validation

A component must be tested against the actual runtime.

Static inspection alone is insufficient for behavior involving:

- Bindings.
- Signals.
- Focus.
- Pointer events.
- Layouts.
- Animations.
- Wayland surfaces.
- Quickshell-specific properties.

## 8. Theme and Visual Styling

### 8.1 Centralized Theme System

The centralized mao-shell theme system is the default source for visual tokens.

Components must consume semantic theme properties rather than independently reconstructing the global palette.

The theme system is responsible for mapping Wallust colors into semantic roles.

### 8.2 Semantic Tokens

Use semantic roles such as:

- Background.
- Surface.
- Elevated surface.
- Primary text.
- Secondary text.
- Muted text.
- Accent.
- Border.
- Divider.
- Success.
- Warning.
- Error.
- Focus.
- Disabled.

Do not couple component behavior directly to arbitrary palette positions.

### 8.3 Local Styling Exceptions

Local visual values are permitted when they serve a clear design purpose.

Examples:

- A specialized visualizer.
- A distinctive dashboard illustration.
- A component-specific status visualization.
- A unique decorative element.

Local exceptions must not undermine theme consistency.

Document important exceptions.

### 8.4 Color Management

Do not hardcode global colors when a semantic token exists.

Do not assume that Wallust always provides every expected color.

Use the centralized theme fallback behavior.

Handle runtime theme changes reactively.

### 8.5 Transparency and Blur

Use transparency and blur selectively.

Do not assume that every surface should be translucent.

Avoid visual effects that reduce readability.

Consider the performance cost of effects across multiple monitors.

### 8.6 Geometry and Spacing

Prefer centralized spacing and geometry tokens.

Use consistent radii, padding, gaps, and component proportions.

Allow justified local geometry when required by a specific visual composition.

Do not introduce arbitrary values without a reason.

### 8.7 Typography

Respect the centralized typography configuration.

Use consistent font weights and sizes.

Use monospaced numerals where numerical alignment benefits the interface.

Do not use typography differences as a substitute for proper hierarchy.

## 9. Visual Identity

### 9.1 Design Direction

mao-shell should remain:

- Minimalist in everyday use.
- Visually distinctive.
- Expressive without becoming visually noisy.
- Consistent across modules.
- Responsive to interaction.
- Easy to understand.

Asymmetry and organic composition are permitted when they improve the experience.

### 9.2 Controlled Creativity

The agent may introduce new visual patterns when they provide a clear improvement.

New patterns must:

- Respect the established visual identity.
- Use the centralized theme system.
- Preserve accessibility.
- Avoid unnecessary complexity.
- Be documented when they establish a reusable design convention.

Do not copy reference projects directly.

### 9.3 Visual Consistency

Consistency does not require every component to look identical.

It requires predictable behavior, coherent styling, and recognizable visual relationships.

Different components may have distinctive presentations when their purpose justifies them.

## 10. Component State Management

### 10.1 Hybrid State Ownership

Use local component state for presentation and local interaction.

Use shared services as the source of truth for system-wide and application-wide data.

Examples of local state:

- Expanded or collapsed.
- Selected tab.
- Hovered state.
- Pressed state.
- Local input value.
- Temporary animation state.

Examples of shared state:

- Current audio device.
- Network connection.
- Bluetooth device state.
- Global theme.
- Notification collection.
- Current workspace information.
- Persistent configuration.

### 10.2 No Duplicated Global State

Do not create independent copies of shared state.

Avoid maintaining a local system-wide value that can become inconsistent with the actual service state.

Use reactive bindings and documented interfaces.

### 10.3 State Ownership

Every meaningful state must have a clear owner.

Do not allow multiple components to independently modify the same authoritative state.

A component may request a change through its service interface.

The service remains responsible for reporting the resulting authoritative state.

### 10.4 Derived State

Prefer derived values over duplicated state.

For example, a component should derive its visual status from the underlying service state instead of maintaining a second manually synchronized status variable.

### 10.5 Persistent State

Components must not implement independent persistence for settings that belong to the centralized configuration service.

Do not write configuration files directly from visual components.

## 11. Interaction and Visual States

### 11.1 Hybrid Interaction Model

Use explicit states when they meaningfully affect behavior or presentation.

Use native QML properties, signals, and bindings for straightforward interactions.

Do not build unnecessary state machines.

### 11.2 Common States

Where applicable, components should account for:

- Idle.
- Hovered.
- Pressed.
- Focused.
- Disabled.
- Loading.
- Active.
- Selected.
- Error.
- Unavailable.

Not every component needs every state.

### 11.3 State Distinction

Different states must be visually distinguishable.

Do not communicate important state changes exclusively through subtle color changes.

Use suitable combinations of:

- Color.
- Shape.
- Opacity.
- Icons.
- Text.
- Motion.
- Focus indicators.

### 11.4 Pointer Interactions

Define pointer behavior explicitly.

A component must distinguish between:

- Hover.
- Click.
- Press.
- Release.
- Drag.
- Double-click, when relevant.

Hover must not unexpectedly execute destructive or consequential actions.

### 11.5 Keyboard Interactions

Interactive components must support appropriate keyboard behavior.

Ensure that keyboard activation produces behavior equivalent to pointer activation where appropriate.

### 11.6 Signals

Signals should represent meaningful user actions or component events.

Do not hide important side effects inside unrelated property changes.

Keep interaction contracts explicit.

## 12. Animation and Transitions

### 12.1 Adaptive Animation Strategy

Use subtle animations by default.

More expressive animation is permitted in selected components such as:

- Dashboards.
- Floating panels.
- Overlays.
- Visualizers.
- Contextual interactions.
- Shell navigation.

### 12.2 Animation Quality

Animations should communicate:

- State changes.
- Appearance and disappearance.
- Navigation.
- Hierarchy.
- Interaction feedback.

Avoid animations that exist only for decoration.

### 12.3 Timing

Use centralized animation timing conventions where available.

Avoid inconsistent durations and arbitrary easing choices.

Transitions should feel coherent across modules.

### 12.4 Reduced Motion

Respect the centralized reduced-motion setting.

When reduced motion is active:

- Disable nonessential movement.
- Avoid exaggerated scaling.
- Reduce or remove decorative transitions.
- Preserve necessary state feedback.
- Prefer immediate or minimally animated transitions.

### 12.5 Performance

Avoid continuously running animations when they are not visible or useful.

Stop unnecessary animations when components become inactive.

Do not use expensive effects for simple feedback.

## 13. Layout and Adaptive Behavior

### 13.1 Adaptive Hybrid Layout

Components should use intentional geometry and controlled adaptation.

Do not rely entirely on fixed coordinates.

Do not allow unrestricted responsive behavior to destroy the intended visual composition.

### 13.2 Available Space

Components must account for their available width and height.

Define reasonable minimum and maximum dimensions where appropriate.

Handle content overflow intentionally.

### 13.3 Density Profiles

Support the mao-shell density profiles:

- Compact.
- Normal.
- Enlarged.

Density must affect component sizing consistently.

Do not implement unrelated scaling logic in every component.

### 13.4 Scaling

Respect the configured UI scale and font scale.

Do not assume a particular DPI or display resolution.

Avoid manually applying scaling twice when the framework already handles it.

### 13.5 Orientation

Consider horizontal and vertical layouts when components may appear in different panel orientations.

Do not assume that every component will always be used in a top or bottom bar.

### 13.6 Multi-Monitor

Components must not assume a single display.

Use the appropriate monitor context provided by the shell architecture.

Avoid global references that unintentionally select the wrong monitor.

### 13.7 Dynamic Content

Layouts must handle changing content.

Examples:

- Longer device names.
- Different notification lengths.
- Missing media artwork.
- Multiple audio devices.
- Different workspace counts.
- Different display sizes.

Do not rely on one example's content dimensions.

## 14. Wayland and Shell Surface Awareness

Components may be displayed inside different Quickshell surfaces.

Understand the distinction between:

- Panels.
- Popups.
- Overlays.
- Floating dashboards.
- Tooltips.
- Dialogs.
- Other shell surfaces.

Do not assume that a component's visual position is independent of its parent surface.

Respect:

- Surface ownership.
- Layer placement.
- Focus behavior.
- Popup positioning.
- Monitor association.
- Available screen geometry.
- Visibility lifecycle.

Actual surface configuration belongs to the appropriate shell architecture.

Do not introduce independent Wayland surface management inside ordinary reusable controls.

## 15. Accessibility

### 15.1 Accessibility First

Accessibility is a mandatory design requirement.

Do not treat accessibility as an optional final improvement.

Consider accessibility when designing the public API, visual states, layout, and interaction behavior.

### 15.2 Keyboard Navigation

Interactive components must provide appropriate keyboard navigation.

Avoid keyboard traps.

Ensure that focus order is logical.

### 15.3 Focus Visibility

Focused elements must be clearly identifiable.

Do not remove focus indicators without providing an appropriate alternative.

### 15.4 Hover Alternatives

Never make essential information or functionality available only through hover.

Provide keyboard-accessible and otherwise appropriate alternatives.

### 15.5 Contrast

Maintain sufficient contrast for text, icons, borders, and important indicators.

Validate contrast across different theme palettes.

### 15.6 Color Independence

Do not communicate critical information using color alone.

Combine color with other visual signals.

### 15.7 Interactive Targets

Interactive areas must be sufficiently large for reliable interaction.

Do not make the clickable area unnecessarily smaller than the visible control.

### 15.8 Text Scaling

Components must tolerate configured text scaling.

Avoid fixed heights that clip text.

### 15.9 Reduced Motion

Respect reduced-motion preferences consistently.

### 15.10 Accessibility Validation

Check:

- Keyboard operation.
- Focus order.
- Focus visibility.
- Contrast.
- Text scaling.
- Interactive target size.
- Hover alternatives.
- State communication.
- Reduced motion.

## 16. Component Organization and Discovery

### 16.1 Categorization

Organize Core UI components by category.

Organize feature components by functional module.

### 16.2 Component Discovery

Before creating a new component:

1. Search the Core UI library.
2. Search the relevant feature module.
3. Inspect similar implementations.
4. Determine whether an existing component can be extended.
5. Avoid creating duplicates.

### 16.3 Naming

Use descriptive and consistent component names.

Names should communicate purpose rather than implementation details.

Avoid ambiguous names.

### 16.4 File Organization

Keep related implementation files together.

Separate reusable visual primitives from specialized feature components.

Do not create unnecessary wrapper files.

### 16.5 Imports

Use clear and predictable imports.

Avoid circular dependencies.

Avoid importing entire feature modules when only a small shared interface is required.

## 17. Feature Integration Contracts

### 17.1 Service Consumption

Components must consume shared services through established interfaces.

Do not independently implement system discovery or process execution.

### 17.2 Service State

Display the state reported by the relevant service.

Handle unavailable, loading, disconnected, and error states where applicable.

### 17.3 User Actions

Components may emit user actions through documented service interfaces.

For example:

- Request playback.
- Request mute.
- Request device selection.
- Request connection.
- Request disconnection.
- Request dismissal.
- Request workspace activation.

The component must not assume that the action succeeded before receiving confirmation.

### 17.4 Asynchronous Operations

Represent asynchronous operations through suitable loading and result states.

Prevent duplicate submissions where appropriate.

Handle failures visibly and predictably.

### 17.5 Failure Isolation

A feature component must degrade gracefully when its backing service becomes unavailable.

Do not allow one failed feature to break unrelated UI components.

### 17.6 Data Formatting

Keep display formatting separate from system data acquisition.

Use consistent units, labels, and number formatting.

Do not silently invent unavailable values.

## 18. Performance and Resource Management

### 18.1 Performance Requirements

Components must remain lightweight.

Avoid unnecessary work during frequent QML updates.

### 18.2 Binding Efficiency

Keep bindings simple and predictable.

Avoid expensive calculations in bindings evaluated frequently.

Move complex transformations into appropriate reusable logic.

### 18.3 Repeated Components

Use suitable QML models and delegates for repeated content.

Avoid creating unnecessary object instances.

Handle delegate lifecycle correctly.

### 18.4 Animations

Avoid expensive continuous animations.

Do not animate large areas unnecessarily.

### 18.5 Timers

Do not use timers for information that can be obtained reactively.

Avoid polling inside individual components.

### 18.6 Images

Avoid repeatedly loading the same image.

Use appropriate image caching and loading strategies supported by the project.

Provide fallbacks for unavailable images.

### 18.7 Resource Lifecycle

Ensure that dynamically created objects are managed correctly.

Avoid retaining objects unnecessarily.

Clean up subscriptions, connections, and resources when the component lifecycle ends.

### 18.8 Large Lists

Use efficient model-based approaches for large or changing collections.

Avoid unnecessarily constructing every visual delegate when it is not needed.

## 19. Error Handling and Validation

### 19.1 Input Validation

Validate externally supplied component properties when invalid values could break layout or behavior.

Use safe defaults where appropriate.

### 19.2 Invalid Configuration

Do not silently accept invalid configuration.

Use the centralized validation strategy.

Provide meaningful fallbacks when appropriate.

### 19.3 Missing Data

Components must handle missing data explicitly.

Examples:

- Missing artwork.
- Unknown device names.
- Unavailable metrics.
- Empty lists.
- Disconnected services.
- Missing optional metadata.

### 19.4 Error Presentation

Display errors proportionately.

Do not expose technical stack traces in normal UI.

Use the centralized diagnostic system for technical details.

### 19.5 No Silent Failures

Do not silently swallow meaningful errors.

Do not display a successful state when an operation has failed or remains unconfirmed.

## 20. Security

### 20.1 Trust Boundaries

Treat data received from services, applications, and external sources as potentially untrusted.

### 20.2 Display Safety

Do not interpret arbitrary external content as executable QML or JavaScript.

### 20.3 Text Handling

Handle potentially long or unusual external text safely.

Avoid unsafe HTML or rich-text interpretation.

### 20.4 Actions

Do not execute arbitrary commands directly from visual components.

Use approved service interfaces.

### 20.5 Sensitive Information

Avoid exposing sensitive information unnecessarily.

Do not include secrets or credentials in component logs.

### 20.6 Configuration

Do not independently modify system configuration files.

Component settings must use the centralized configuration architecture.

## 21. Dependencies

### 21.1 Native First

Prefer:

- Quickshell.
- Qt.
- QML.
- Existing project utilities.

### 21.2 External Libraries

External dependencies are permitted only when they provide a meaningful benefit.

Before introducing one:

1. Identify the problem it solves.
2. Check whether native APIs already solve it.
3. Verify compatibility.
4. Evaluate maintenance status.
5. Consider security implications.
6. Consider runtime and performance costs.
7. Document the justification.

### 21.3 Dependency Restrictions

Do not introduce speculative dependencies.

Do not install packages without authorization.

Do not update the installed environment without authorization.

## 22. Documentation Requirements

### 22.1 Component Documentation

Every reusable component must have documentation appropriate to its complexity.

Document:

- Purpose.
- Responsibilities.
- Public properties.
- Signals.
- Methods.
- Defaults.
- Variants.
- States.
- Interactions.
- Theme dependencies.
- Accessibility behavior.
- Service dependencies.
- Usage examples.

### 22.2 Complex Components

Complex components should also document:

- Internal composition.
- State ownership.
- Data flow.
- Lifecycle.
- Error behavior.
- Performance considerations.
- Integration contracts.

### 22.3 Examples

Provide examples showing how the component should be used.

Examples must be technically valid.

### 22.4 Component Catalog

Maintain or update the component catalog when the project provides one.

The catalog should help agents discover existing reusable components.

### 22.5 Documentation Synchronization

Update documentation whenever a public API, contract, or important behavior changes.

Do not leave examples inconsistent with implementation.

## 23. Implementation Examples

The following examples illustrate preferred patterns.

They are architectural examples. Verify all APIs and types against the installed Qt and Quickshell versions before using them.

Component names such as `ActionButton` are illustrative until the naming convention is defined (P0-004). Colours are expressed as semantic tokens from `DESIGN_PATTERNS.md` §1, supplied through a `theme` object. The mechanism that provides that object is defined in Phase 1 with the theme service. Colours are derived from Wallust and the wallpaper, so no colour value may be hardcoded in a component.

### 23.1 Example: Reusable Button

```qml
import QtQuick

Item {
    id: root

    required property var theme

    property string text: ""
    property bool enabled: true
    property bool loading: false
    property bool emphasized: false

    signal clicked()

    implicitWidth: content.implicitWidth + 24
    implicitHeight: 40

    Rectangle {
        anchors.fill: parent
        radius: 10

        color: root.enabled
            ? (root.emphasized ? root.theme.accent : root.theme.surfaceElevated)
            : root.theme.surface

        opacity: root.enabled ? 1.0 : 0.55

        Text {
            id: content

            anchors.centerIn: parent
            text: root.loading ? "Loading..." : root.text
            color: root.theme.text
        }

        MouseArea {
            anchors.fill: parent
            enabled: root.enabled && !root.loading

            onClicked: root.clicked()
        }
    }
}
```

This example illustrates:

- A clear public API.
- Explicit enabled and loading states.
- A meaningful public signal.
- Encapsulation of internal visual details.

The production implementation must additionally provide appropriate keyboard focus, accessibility, theme integration, and verified interaction behavior. Text colour on the accent token must be checked for contrast across all supported palettes (§15.5).

### 23.2 Example: Preferred Component Consumption

```qml
Column {
    spacing: 8

    ActionButton {
        text: "Connect"
        emphasized: true

        onClicked: connectionService.requestConnect()
    }

    ActionButton {
        text: "Cancel"

        onClicked: dialog.close()
    }
}
```

The component exposes user intent.

The service owns the actual operation.

### 23.3 Example: Reactive Service State

```qml
Item {
    id: root

    required property var audioService

    readonly property bool muted:
        audioService.muted

    Text {
        text: root.muted ? "Muted" : "Active"
    }
}
```

The component reflects authoritative service state.

It does not maintain an independent global mute value.

### 23.4 Example: Derived Presentation State

```qml
Item {
    id: root

    required property real progress

    readonly property real normalizedProgress:
        Math.max(0, Math.min(1, progress))

    Rectangle {
        width: parent.width * root.normalizedProgress
        height: 4
    }
}
```

Derived properties should remain simple and deterministic.

### 23.5 Example: Component Variant

```qml
ActionButton {
    text: "Delete"
    emphasized: false

    // A production component should expose
    // a documented semantic destructive variant.
}
```

Do not use unrelated visual properties as substitutes for semantic variants in the production API.

### 23.6 Anti-pattern: Duplicated Global State

```qml
Item {
    property bool localMuted: false

    MouseArea {
        anchors.fill: parent

        onClicked: {
            localMuted = !localMuted
        }
    }
}
```

This is incorrect when audio state is owned by a shared service.

The local value may diverge from the actual system state.

Request the operation through the service and observe its authoritative result.

### 23.7 Anti-pattern: Excessive Configuration

Avoid APIs containing dozens of unrelated boolean properties.

A component with many combinations of behavior becomes difficult to understand and validate.

Prefer semantic variants and specialized components when responsibilities differ.

### 23.8 Anti-pattern: Direct System Commands

Do not execute system commands directly inside ordinary visual components.

Do not introduce shell command strings or synchronous process execution into UI logic.

Delegate these operations to `system-integration`.

### 23.9 Anti-pattern: Hardcoded Global Styling

Do not independently hardcode the global palette, typography, or geometry across feature components.

Consume centralized theme tokens.

### 23.10 Anti-pattern: Hover-Only Interaction

Do not make important controls available exclusively through hover.

Provide keyboard-accessible and otherwise appropriate interaction alternatives.

## 24. Testing and Validation

### 24.1 Hybrid Testing Strategy

Use automated tests where appropriate and real runtime validation.

Neither approach replaces the other.

### 24.2 Automated Validation

Test applicable:

- Property behavior.
- State transitions.
- Input validation.
- Derived values.
- Component signals.
- Variant behavior.
- Data formatting.
- Error states.

### 24.3 Visual Validation

Inspect:

- Spacing.
- Alignment.
- Typography.
- Contrast.
- Theme behavior.
- Component proportions.
- Animation quality.
- Overflow.
- Responsive behavior.

### 24.4 Runtime Validation

Test components in the actual Quickshell runtime.

Check:

- Component creation.
- Property bindings.
- Signal delivery.
- Focus behavior.
- Pointer interaction.
- Popup behavior.
- Surface placement.
- Resource lifecycle.

### 24.5 Wayland Validation

Where relevant, validate behavior under the actual Wayland compositor.

Do not assume that a component working in isolation guarantees correct behavior inside the desktop shell.

### 24.6 Multi-Monitor Validation

Test relevant components across different monitors and scaling configurations.

Check monitor-specific context and geometry.

### 24.7 Accessibility Testing

Validate keyboard navigation, focus, contrast, reduced motion, and scaling.

### 24.8 Regression Testing

Changes to Core UI require checking affected feature consumers.

Avoid fixing one component by breaking other modules.

### 24.9 Test Reporting

Report:

- Tests executed.
- Tests passed.
- Tests failed.
- Runtime conditions.
- Unverified behavior.
- Known limitations.

Never claim that a test passed when it was not executed.

## 25. Development Workflow

Follow this workflow when creating or modifying components.

### Step 1: Understand

Inspect the relevant specifications and architecture.

Identify the intended component responsibility.

### Step 2: Discover

Search for existing components and similar patterns.

Determine whether an existing implementation can be reused.

### Step 3: Define

Establish:

- Public API.
- State ownership.
- Visual responsibility.
- Variants.
- Integration contracts.
- Accessibility requirements.

### Step 4: Design

Determine the component composition and visual behavior.

Use the centralized theme system.

### Step 5: Implement

Build the smallest implementation that satisfies the requirement.

Prefer incremental changes.

### Step 6: Validate

Verify QML and Quickshell APIs.

Run relevant automated and runtime tests.

### Step 7: Refine

Check performance, accessibility, consistency, and maintainability.

### Step 8: Document

Update component documentation and examples.

### Step 9: Review

Check consumers, dependencies, and potential regressions.

### Step 10: Report

Summarize implementation, validation, and remaining limitations.

## 26. Autonomy and Decision Making

### 26.1 Balanced Autonomy

The agent may independently decide:

- Internal component composition.
- Minor visual refinements.
- Appropriate QML implementation details.
- Small local refactoring.
- Standard interaction patterns.
- Minor layout improvements.

### 26.2 Ask Before Significant Changes

Ask the user before:

- Changing product behavior.
- Redesigning major visual patterns.
- Changing public contracts with broad impact.
- Introducing significant dependencies.
- Changing shared state ownership.
- Modifying unrelated modules in a consequential way.
- Performing destructive operations.

### 26.3 Preserve Existing Work

Never overwrite unrelated work.

Inspect existing modifications before editing files.

Do not discard uncommitted changes.

### 26.4 Avoid Unnecessary Questions

Do not ask the user to decide ordinary implementation details that are already covered by the project architecture.

Use the documented patterns and exercise reasonable technical judgment.

## 27. Definition of Done

A component implementation is complete only when applicable requirements are satisfied.

### Architecture

- Responsibility is clear.
- Component belongs to the correct layer.
- Dependencies follow the architecture.
- Existing reusable components were considered.
- No unnecessary duplication was introduced.

### API

- Public properties are clear.
- Signals are meaningful.
- Defaults are sensible.
- Configuration is validated.
- Contracts are documented.

### Visual Design

- Theme tokens are used appropriately.
- Visual hierarchy is coherent.
- Variants are consistent.
- Layout handles expected content.
- Animations are purposeful.

### Interaction

- States are correctly represented.
- Pointer behavior is predictable.
- Keyboard behavior is appropriate.
- Asynchronous operations are represented honestly.

### Accessibility

- Focus is visible.
- Keyboard navigation works.
- Contrast is appropriate.
- Hover alternatives exist.
- Reduced motion is respected.
- Text scaling is supported.

### Performance

- No unnecessary polling.
- No avoidable expensive bindings.
- No unnecessary object creation.
- No uncontrolled continuous animation.
- Resources are managed correctly.

### Reliability

- Missing data is handled.
- Errors are not silently ignored.
- Service failures do not break unrelated UI.
- Runtime behavior has been checked.

### Documentation and Testing

- Component documentation is updated.
- Examples are valid.
- Relevant tests were executed.
- Validation results are reported honestly.

## 28. Final Principles

Every mao-shell component should follow these principles:

1. Reusability must have a practical purpose.
2. Composition is preferred over unnecessary inheritance or duplication.
3. Public APIs must remain small and understandable.
4. Shared services own authoritative global state.
5. The centralized theme system defines the visual foundation.
6. Local styling exceptions must be justified.
7. Accessibility is mandatory.
8. Animations must communicate meaningful changes.
9. Adaptive layouts must preserve design intent.
10. Components must remain lightweight.
11. Feature modules must remain independent.
12. System integration belongs to the appropriate integration layer.
13. External dependencies require justification.
14. Technical APIs must be verified.
15. Documentation must reflect actual implementation.
16. Testing must include real runtime validation where applicable.
17. Creative decisions must respect mao-shell's identity.
18. Simplicity is preferred over unnecessary abstraction.

**The ultimate goal is to build a coherent, expressive, accessible, and reliable component library that makes mao-shell feel like one carefully designed desktop environment rather than a collection of unrelated widgets.**