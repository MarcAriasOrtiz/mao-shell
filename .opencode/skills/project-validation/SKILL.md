---
name: project-validation
description: Project-level validation for mao-shell, including release readiness, security auditing, performance profiling, regression management, compatibility checks and documentation consistency reviews. Use when validating a completed feature, auditing the project, or preparing a release. For routine implementation and incremental checks, use quickshell-development, qml-component or system-integration.
---

# Project Validation Skill

## 1. Purpose and Scope

This skill defines the validation methodology for mao-shell.

Its purpose is to ensure that every component, service, integration, configuration change, and release meets appropriate standards for correctness, reliability, security, performance, accessibility, maintainability, and compatibility.

Validation must reflect the actual architecture and target environment:

- Arch Linux.
- Wayland.
- Hyprland.
- Quickshell.
- Qt and QML.
- JavaScript-based application logic.
- User-local installation.
- Multi-monitor desktop environments.

Validation must assess both isolated functionality and its interaction with the complete desktop shell.

The objective is not simply to make tests pass. It is to establish sufficient evidence that the software behaves correctly under realistic conditions.

## 2. Mandatory Project Documentation

Before designing or executing substantial validation work, read:

1. `AGENTS.md`
2. `ROADMAP.md`
3. `TASKS.md`
4. `COMPONENT_SPECS.md`
5. `DESIGN_PATTERNS.md`
6. Relevant implementation documentation.
7. Relevant skill documentation.

Resolve conflicts by topic, as defined in `AGENTS.md` ("Precedence when documents conflict"). `COMPONENT_SPECS.md` defines intended behaviour and `DESIGN_PATTERNS.md` defines expected implementation patterns. Validation reports discrepancies against them and never overrides them. Verified external technical documentation is a reference only.

Technical compatibility requirements are mandatory even when a project document contains an outdated assumption.

When documentation conflicts with the installed environment, investigate and report the discrepancy rather than silently choosing an interpretation.

## 3. Core Validation Principles

All validation activities must follow these principles:

- Evidence over assumptions.
- Reproducibility over anecdotal observations.
- Risk-based testing.
- Incremental validation.
- Regression prevention.
- Isolation of failures.
- Non-destructive execution.
- Measurable performance.
- Explicit reporting.
- Compatibility with the installed environment.
- Preservation of the active desktop session.
- Proportional testing effort.

Never claim that a feature is validated merely because it compiles, loads, or appears to work once.

Never report a test as passed when it was not executed.

Never invent test results, performance measurements, coverage percentages, or compatibility claims.

## 4. Hybrid Validation Strategy

Use a combination of validation methods.

### 4.1 Static analysis

Inspect code without executing the complete application.

Check:

- Syntax.
- Invalid references.
- Undefined identifiers.
- Suspicious property bindings.
- Incorrect signal connections.
- Potential type mismatches.
- Unused or duplicated code.
- Dependency relationships.
- Security-sensitive operations.
- Architectural consistency.
- Resource lifecycle problems.

Use available QML and JavaScript tooling where appropriate.

Do not assume that a generic JavaScript analyzer fully understands QML semantics.

### 4.2 Automated testing

Create automated tests for deterministic behavior.

Prioritize:

- Pure JavaScript logic.
- Configuration validation.
- Data transformation.
- Formatting.
- State transitions.
- Input validation.
- Error handling.
- Integration adapters.
- Recovery logic.
- Migration routines.

Tests should be deterministic, repeatable, isolated, and maintainable.

### 4.3 Integration testing

Validate communication between project components and external services.

Include:

- Shared services.
- QML components.
- IPC.
- D-Bus.
- Hyprland integrations.
- PipeWire integrations.
- Network integrations.
- Bluetooth integrations.
- MPRIS.
- Configuration services.
- Process execution.

### 4.4 Runtime testing

Validate actual behavior using the installed Quickshell and Wayland environment.

Check:

- Startup.
- Surface creation.
- Layer-shell behavior.
- Focus.
- Popups.
- Overlays.
- Monitor placement.
- Scaling.
- Animations.
- Input handling.
- Resource cleanup.
- Runtime errors.

### 4.5 Visual testing

Use screenshots, visual inspection, and automated comparisons when technically feasible.

### 4.6 Performance testing

Measure actual resource consumption and response behavior.

### 4.7 Security testing

Inspect execution boundaries, data validation, permissions, IPC, dependencies, and sensitive information.

## 5. Hybrid Validation Checkpoints

Use three primary validation checkpoints.

### 5.1 Incremental validation

Run targeted checks after meaningful changes.

Examples:

- A QML component change triggers component-level checks.
- A configuration change triggers schema and migration checks.
- A process integration change triggers execution and lifecycle tests.
- A theme change triggers visual and accessibility checks.

Avoid running the complete test suite after every insignificant edit.

### 5.2 Feature completion validation

When a feature is complete, execute the complete validation set relevant to that feature.

Include:

- Static analysis.
- Automated tests.
- Integration checks.
- Runtime validation.
- Visual review when applicable.
- Security checks according to risk.
- Performance checks according to risk.
- Documentation verification.

### 5.3 Global validation

Perform broader validation when:

- Requested by the user.
- Preparing a release.
- Completing major architectural changes.
- Modifying shared infrastructure.
- Investigating widespread regressions.
- Preparing a production-ready build.

## 6. Risk-Based Test Coverage

Classify functionality according to its impact and failure potential.

### 6.1 Critical risk

Examples:

- Central configuration.
- Process execution.
- Security boundaries.
- Shared services.
- Wayland surface management.
- Installation and maintenance scripts.
- System integrations.
- State ownership.
- Application startup and recovery.

Requirements:

- Extensive automated testing.
- Integration testing.
- Runtime validation where applicable.
- Security review.
- Failure recovery testing.
- Regression coverage.

### 6.2 High risk

Examples:

- Notifications.
- Audio.
- Network.
- Bluetooth.
- Media integrations.
- Workspaces.
- Multi-monitor state.
- Dashboard interactions.

Requirements:

- Strong automated coverage.
- Integration testing.
- Failure scenarios.
- Runtime validation.
- Relevant regression tests.

### 6.3 Medium risk

Examples:

- Dashboard layouts.
- Widgets.
- Interactive controls.
- Visual profiles.
- Calendar interfaces.
- Launcher presentation.

Requirements:

- Component tests.
- Visual validation.
- Accessibility checks.
- Relevant runtime testing.

### 6.4 Low risk

Examples:

- Decorative elements.
- Static visual details.
- Non-interactive presentation changes.

Requirements:

- Appropriate static checks.
- Visual inspection when relevant.

Risk classification must be revisited when a component gains responsibilities or dependencies.

## 7. Risk Assessment Methodology

For each meaningful validation target, evaluate:

- Impact of failure.
- Likelihood of failure.
- Exposure to external input.
- Number of dependent modules.
- Recovery complexity.
- User-visible consequences.
- Security implications.
- Potential damage to the active session.

Use qualitative categories unless reliable quantitative data exists.

Do not invent numerical risk scores.

A low-complexity visual change must not receive the same validation overhead as a process execution or configuration migration change.

## 8. Test Isolation and Desktop Safety

Testing must not compromise the active desktop session.

### 8.1 Isolated automated testing

Prefer:

- Temporary directories.
- Mock services.
- Mock IPC endpoints.
- Controlled environment variables.
- Dedicated test configurations.
- Disposable test data.
- Independent test processes.

### 8.2 Runtime testing

Real Wayland validation may be necessary.

Before executing it:

1. Identify the target process.
2. Identify the relevant session.
3. Understand whether the operation affects active surfaces.
4. Determine whether the test can run in isolation.
5. Assess possible disruption.

Never automatically:

- Terminate the active Quickshell session.
- Replace the active shell.
- Restart Hyprland.
- Close user applications.
- Modify active compositor configuration.
- Change session startup behavior.

Ask for authorization when a test requires these operations.

### 8.3 Development isolation

Use the project's development mode whenever available.

Prefer separate runtime instances and temporary configurations.

Ensure test processes are identifiable and can be safely terminated.

## 9. Static Analysis and Code Quality

Perform static analysis according to the installed language and tooling capabilities.

### 9.1 QML validation

Inspect:

- Imports.
- Component references.
- Property declarations.
- Signal declarations.
- Property bindings.
- Anchors and layouts.
- Object ownership.
- Dynamic creation.
- Component lifecycle.
- Binding loops.
- Focus behavior.
- Invalid references.

### 9.2 JavaScript validation

Inspect:

- Undefined variables.
- Invalid assumptions.
- Error handling.
- Promise handling.
- Async operations.
- Input validation.
- Data ownership.
- State mutation.
- Resource cleanup.
- Unsafe string interpolation.

### 9.3 Architectural quality

Detect:

- Circular dependencies.
- Duplicated responsibilities.
- Excessive coupling.
- Unnecessary abstractions.
- Repeated service instances.
- Incorrect state ownership.
- Direct access to internal module implementation.
- Unjustified dependencies.
- Architectural violations.

### 9.4 Maintainability

Evaluate:

- Naming consistency.
- Function complexity.
- Duplication.
- Module boundaries.
- Readability.
- Error handling consistency.
- Documentation quality.

Do not refactor unrelated code merely to improve a quality metric.

## 10. Test Design and Quality

Every test must have a clear purpose.

A useful test should establish:

- What is being validated.
- What conditions are required.
- What behavior is expected.
- What failure would look like.
- Whether the result is deterministic.

Prefer small tests with specific responsibilities.

Avoid tests that depend on unrelated modules.

Avoid excessive mocking that removes the behavior being tested.

Mocks should reproduce the relevant interface and failure conditions.

### 10.1 Positive tests

Verify expected behavior under valid conditions.

### 10.2 Negative tests

Verify behavior under invalid or unexpected conditions.

### 10.3 Boundary tests

Test:

- Empty values.
- Missing values.
- Maximum supported values.
- Minimum supported values.
- Invalid types.
- Unexpected states.
- Rapid state changes.

### 10.4 Regression tests

Every important reproducible bug should receive an appropriate regression test when technically possible.

## 11. Regression Management

Use hybrid regression management.

### 11.1 Automated regressions

When a defect is reproducible and deterministic:

1. Reproduce it.
2. Identify the relevant behavior.
3. Create a regression test.
4. Correct the underlying issue.
5. Execute the regression test.
6. Execute relevant neighboring tests.

The test must fail under the original defective conditions whenever practical.

### 11.2 Manual regressions

Some problems require visual or runtime inspection.

Examples:

- Incorrect popup positioning.
- Unexpected animation behavior.
- Focus problems.
- Layer-shell conflicts.
- Monitor-specific layout issues.

Document:

- Reproduction steps.
- Expected result.
- Actual result.
- Environment.
- Evidence.
- Resolution status.

Never mark a regression as resolved without verifying the corrected behavior.

## 12. Adaptive Performance Profiling

Performance validation must reflect the importance of the feature.

Critical services require deeper profiling.

Visual-only changes may require lighter checks.

### 12.1 Metrics

Measure relevant metrics:

- CPU usage.
- Memory usage.
- GPU activity.
- Process count.
- Process lifetime.
- Startup time.
- Event latency.
- UI response time.
- Animation smoothness.
- Idle resource consumption.
- Resource growth.
- Background activity.

### 12.2 Baselines

Establish measured baselines when possible.

Compare changes against previous measurements under comparable conditions.

Document:

- Hardware.
- Software versions.
- Resolution.
- Scaling.
- Monitor count.
- Test duration.
- Workload.
- Relevant background services.

Do not invent performance thresholds.

If a baseline does not exist, establish one before making strong claims about regression or improvement.

### 12.3 Performance scenarios

Test:

- Startup.
- Idle.
- Normal interaction.
- High activity.
- Multiple monitors.
- Multiple simultaneous widgets.
- Repeated state changes.
- Recovery after service interruption.

### 12.4 Performance regressions

Investigate:

- Increased idle CPU.
- Memory growth.
- Unnecessary GPU activity.
- Excessive process creation.
- Repeated polling.
- Duplicate timers.
- Excessive signal handling.
- Blocking operations.
- Unreleased resources.

## 13. Advanced Power Profiling

Evaluate the energy-related behavior of mao-shell.

### 13.1 Idle behavior

Check that inactive components do not continue unnecessary work.

Examples:

- Audio visualizers.
- Animations.
- Weather refreshes.
- Hardware monitoring.
- Network monitoring.
- Background integrations.

### 13.2 Wakeups and timers

Inspect:

- Excessive wakeups.
- Unnecessary timers.
- Polling frequency.
- Repeated process invocation.
- Background refresh behavior.

### 13.3 Workload comparisons

Compare:

- Idle shell.
- Active shell.
- Dashboard open.
- Media playback.
- Audio visualization.
- Multiple active monitors.

### 13.4 Energy estimates

Use hardware-supported energy measurements where available.

When direct measurement is unavailable, distinguish estimates from measured energy consumption.

Never present estimated energy values as direct measurements.

## 14. Hybrid Compatibility Validation

Use the installed Arch Linux environment as the primary reference.

Validate the real environment before making compatibility assumptions.

### 14.1 Environment discovery

Inspect relevant versions and capabilities:

- Arch Linux environment.
- Quickshell.
- Qt.
- QML runtime.
- Hyprland.
- Wayland.
- PipeWire.
- D-Bus.
- Network managers.
- Bluetooth managers.
- GPU utilities.
- Relevant command-line tools.

Use available system inspection tools.

Do not assume a dependency is installed simply because it is common on Arch Linux.

### 14.2 Reference compatibility

Prioritize the actual installed versions.

Verify APIs against the installed environment and corresponding documentation.

### 14.3 Alternative capabilities

When an integration supports multiple tools or service providers:

- Detect available alternatives.
- Test relevant compatibility paths.
- Document unavailable capabilities.
- Verify fallback behavior.

Do not expand compatibility testing into unsupported environments without a clear reason.

### 14.4 Compatibility reporting

Record:

- Tested version.
- Available capability.
- Selected integration path.
- Alternative paths tested.
- Known limitations.

## 15. Advanced Security Validation

Security testing must be risk-based.

High-risk execution and integration boundaries require deeper inspection.

### 15.1 Process execution

Review:

- Argument construction.
- Input validation.
- Shell injection risks.
- Command injection risks.
- Environment variables.
- Timeouts.
- Cancellation.
- Process permissions.
- Output handling.
- Exit status handling.

Prefer argument arrays and safe process APIs.

Never rely on interpolating untrusted input into shell command strings.

### 15.2 Configuration security

Inspect:

- File permissions.
- Configuration validation.
- Imported data.
- Malformed values.
- Sensitive values.
- Path handling.
- Backup exposure.

### 15.3 IPC and D-Bus

Review:

- Message validation.
- Sender assumptions.
- Payload sizes.
- Access boundaries.
- Unexpected responses.
- Error handling.
- Resource exhaustion.

### 15.4 External data

Validate data received from:

- System services.
- Applications.
- Network sources.
- Devices.
- Imported configuration.
- External processes.

Never trust external data solely because it comes from a local service.

### 15.5 Sensitive information

Ensure logs do not expose:

- Credentials.
- Tokens.
- Private network details when unnecessary.
- Sensitive user paths.
- Authentication material.
- Private application content.

### 15.6 Dependencies

Review:

- Necessity.
- Maintenance.
- Compatibility.
- Security status.
- License considerations.
- Redundant functionality.

Do not install dependencies without authorization.

### 15.7 Security limitations

Document checks that cannot be performed with available tools.

Do not claim a security audit is exhaustive when its scope is limited.

## 16. Advanced Integration Testing

Validate the behavior of integrations with actual system capabilities.

Relevant integrations include:

- Hyprland.
- PipeWire.
- NetworkManager.
- Alternative network managers.
- BlueZ.
- MPRIS.
- D-Bus.
- System monitoring tools.
- Notification services.
- Other detected desktop utilities.

### 16.1 Service availability

Test:

- Available service.
- Missing service.
- Delayed startup.
- Service restart.
- Unexpected termination.

### 16.2 Device behavior

Test:

- Device discovery.
- Connection.
- Disconnection.
- Reconnection.
- Hotplug.
- Device removal.
- Capability changes.

### 16.3 Alternative integrations

Verify fallback behavior when multiple supported implementations exist.

Do not assume identical APIs or behavior across providers.

### 16.4 Integration failures

Verify:

- Clear errors.
- Bounded retries.
- Backoff.
- Cancellation.
- Recovery.
- Module isolation.

A failure in one integration must not unnecessarily disable unrelated shell functionality.

## 17. Advanced Concurrency Testing

Concurrency must be tested wherever shared state or asynchronous operations exist.

### 17.1 Race conditions

Investigate:

- Simultaneous events.
- Repeated state changes.
- Concurrent process execution.
- Multiple consumers of shared services.
- Overlapping refresh operations.

### 17.2 Event ordering

Test:

- Delayed responses.
- Out-of-order events.
- Duplicate events.
- Stale responses.
- Interrupted operations.

### 17.3 Cancellation

Verify that cancelled operations:

- Stop when appropriate.
- Release resources.
- Do not update stale state.
- Do not emit false success signals.

### 17.4 State consistency

Verify that shared services remain the source of truth.

Presentation components must not introduce conflicting copies of authoritative system state.

### 17.5 Stress testing

Increase event frequency and concurrency in controlled environments.

Do not use uncontrolled stress tests against the active desktop session.

## 18. Advanced Visual Testing

Visual correctness is a first-class validation concern.

### 18.1 Screenshot evidence

Capture relevant states when possible.

Include:

- Default state.
- Active state.
- Hover state.
- Focus state.
- Expanded state.
- Error state.
- Empty state.
- Loading state.

### 18.2 Visual comparison

When reliable screenshot comparison is available:

- Compare reference and current images.
- Detect unexpected geometry changes.
- Detect missing elements.
- Detect color and opacity regressions.
- Detect clipping.
- Detect alignment issues.

Use reasonable tolerances for rendering differences.

Do not treat every pixel variation as a defect.

### 18.3 Layout coverage

Test relevant:

- Screen resolutions.
- UI scales.
- Font scales.
- Monitor arrangements.
- Monitor orientations.
- Panel geometries.
- Popup positions.
- Available screen space.

### 18.4 Wayland-specific visuals

Inspect:

- Layer surfaces.
- Surface placement.
- Focus behavior.
- Popup stacking.
- Overlay behavior.
- Screen edges.
- Monitor association.

### 18.5 Evidence limitations

If screenshots cannot be captured automatically, request or perform a manual inspection rather than claiming visual verification.

## 19. Advanced Accessibility Testing

Accessibility must be included in component and feature validation.

### 19.1 Keyboard navigation

Verify:

- Logical navigation order.
- Focus visibility.
- Focus entry and exit.
- Keyboard shortcuts.
- Escape behavior.
- Modal navigation.
- Popup navigation.

### 19.2 Visual accessibility

Check:

- Contrast.
- Text legibility.
- Font scaling.
- UI scaling.
- High contrast.
- Information conveyed by color alone.
- State indicators.

### 19.3 Interaction accessibility

Review:

- Interactive target sizes.
- Input clarity.
- Labels.
- Tooltips.
- Focus feedback.
- Error communication.

### 19.4 Motion

Verify:

- Reduced motion.
- Animation disabling or simplification.
- Avoidance of unnecessary motion.
- Consistent transitions.

### 19.5 Automated tooling

Use available accessibility tools when appropriate.

Do not assume that all QML accessibility properties are fully covered by generic web accessibility scanners.

### 19.6 Manual review

Manually review behavior that cannot be reliably inferred from static or automated checks.

## 20. Advanced Soak Testing

Long-duration tests are required for important services and shell infrastructure.

### 20.1 Objectives

Detect:

- Memory leaks.
- Resource leaks.
- Process accumulation.
- Timer accumulation.
- Duplicate services.
- Unbounded logs.
- Increasing idle consumption.
- Progressive latency degradation.

### 20.2 Scenarios

Test:

- Continuous idle operation.
- Repeated dashboard opening.
- Repeated popup creation.
- Repeated media changes.
- Device reconnect cycles.
- Service restart cycles.
- Monitor hotplug.
- Configuration reloads.

### 20.3 Measurement

Record relevant metrics over time.

Compare initial and final conditions.

Document duration and workload.

### 20.4 Safety

Use controlled environments.

Do not run prolonged destructive scenarios against the user's active desktop.

## 21. Failure Investigation

When a validation fails, investigate before modifying code.

### 21.1 Investigation workflow

1. Confirm the failure.
2. Capture relevant logs.
3. Record the environment.
4. Identify affected components.
5. Inspect recent changes.
6. Reproduce the issue.
7. Form hypotheses.
8. Test hypotheses safely.
9. Identify the probable root cause.
10. Prepare a correction proposal.

### 21.2 Evidence quality

Separate:

- Confirmed facts.
- Observed symptoms.
- Probable causes.
- Unverified hypotheses.

Do not present a suspected cause as a confirmed root cause.

### 21.3 Non-destructive investigation

Prefer read-only inspection, mocks, temporary files, and isolated runtime tests.

Do not modify the active environment merely to test an unverified hypothesis.

## 22. Hybrid Remediation Strategy

Corrections should match their risk.

### 22.1 Safe corrections

The agent may correct clearly localized implementation errors when doing so is consistent with the user's instructions.

Examples:

- Incorrect local condition.
- Missing cleanup.
- Invalid binding.
- Incorrect data formatting.
- Missing validation.
- Deterministic logic errors.

### 22.2 Changes requiring approval

Request approval before:

- Architectural changes.
- Functional redesign.
- Destructive operations.
- Changes affecting user data.
- System configuration modifications.
- Dependency installation.
- Active desktop disruption.

### 22.3 Remediation verification

After a correction:

1. Re-run the original failing test.
2. Run relevant neighboring tests.
3. Check regressions.
4. Inspect logs.
5. Verify expected behavior.
6. Update documentation.

Never disable a failing test simply to obtain a passing result.

## 23. Advanced Chaos Testing

Chaos testing evaluates resilience under controlled failure injection.

### 23.1 Supported scenarios

When safe and isolated, simulate:

- Service termination.
- Delayed responses.
- IPC failures.
- D-Bus interruptions.
- Invalid payloads.
- Temporary dependency loss.
- Process timeouts.
- Partial operation failures.
- Reconnection storms.
- Resource unavailability.

### 23.2 Expected behavior

Verify:

- Failure isolation.
- Clear error reporting.
- Bounded recovery attempts.
- Backoff.
- Resource cleanup.
- Correct state restoration.
- No infinite retry loops.
- No process accumulation.

### 23.3 Safety boundaries

Never inject failures into the active Hyprland compositor, active desktop session, or critical user services without explicit authorization.

Prefer mocks and disposable test services.

## 24. Configuration Validation

The central configuration system is critical infrastructure.

### 24.1 Schema validation

Check:

- JSON validity.
- Expected properties.
- Data types.
- Required fields.
- Allowed values.
- Numeric ranges.
- Unknown properties.
- Cross-module consistency.

### 24.2 Defaults

Verify:

- Correct defaults.
- Missing property handling.
- Fallback behavior.
- User override precedence.
- Runtime consistency.

### 24.3 Corruption handling

Test:

- Invalid JSON.
- Truncated files.
- Unexpected types.
- Missing files.
- Invalid references.
- Unsupported schema versions.

Verify safe recovery.

Never silently replace user configuration without preserving a backup where appropriate.

### 24.4 Migration testing

For each migration:

1. Identify the source schema.
2. Identify the destination schema.
3. Create representative source fixtures.
4. Execute the migration in isolation.
5. Validate the destination.
6. Verify preservation of user values.
7. Test migration failure.
8. Verify rollback or backup recovery.

### 24.5 Import and export

Test:

- Valid profiles.
- Invalid profiles.
- Incomplete profiles.
- Unsupported versions.
- Duplicate identifiers.
- Malformed values.
- Large inputs.

Do not trust imported files.

## 25. Installation Lifecycle Testing

Installation scripts require strict validation because they can affect the user's desktop configuration.

### 25.1 Installation

Test:

- Clean installation.
- Repeated installation.
- Missing prerequisites.
- Existing installation.
- Partial installation.
- Interrupted installation.

### 25.2 Updates

Test:

- Updates from previous versions.
- Repeated updates.
- Interrupted updates.
- Failed migrations.
- Existing user configuration.

### 25.3 Rollback

Verify that rollback restores the expected project state.

Ensure user configuration and unrelated files remain intact.

### 25.4 Uninstallation

Verify that only project-owned files are removed.

Preserve user-owned files unless deletion has been explicitly requested.

### 25.5 Idempotence

Repeated execution should not create duplicate files, services, processes, or configuration entries.

### 25.6 Dry-run

Where supported, verify that dry-run mode accurately describes intended operations without applying them.

### 25.7 Privileges

Prefer user-level execution.

Never introduce sudo-based installation behavior without explicit authorization.

## 26. Advanced Git Validation

Use hybrid validation around Git operations.

### 26.1 Before commits

Perform relevant quick checks:

- Syntax.
- Static analysis.
- Targeted tests.
- Whitespace and formatting checks.
- Change inspection.

### 26.2 Before integration

Perform broader checks:

- Feature tests.
- Regression tests.
- Integration tests.
- Relevant security checks.
- Documentation consistency.

### 26.3 Change safety

Never:

- Discard uncommitted user changes.
- Force-reset branches without authorization.
- Rewrite shared history without authorization.
- Commit unrelated changes.
- Hide failing tests.
- Automatically merge changes.

### 26.4 Reporting

Clearly state which checks ran and which were skipped.

Do not describe a commit or integration as verified unless the relevant checks were performed.

## 27. Production-Grade Release Validation

Every release candidate must pass a broader validation process.

### 27.1 Installation

Validate:

- Clean installation.
- Existing installation update.
- Configuration preservation.
- Migration behavior.
- Rollback.

### 27.2 Functional validation

Check major functionality and relevant integration paths.

### 27.3 Compatibility

Record tested versions and capabilities.

### 27.4 Performance

Compare against available baselines.

### 27.5 Security

Review critical execution and integration boundaries.

### 27.6 Accessibility

Perform relevant automated and manual checks.

### 27.7 Stability

Execute soak testing where release risk justifies it.

### 27.8 Documentation

Verify:

- Version.
- Changelog.
- Installation instructions.
- Compatibility notes.
- Known issues.
- Validation report.

### 27.9 Release acceptance

A release must not be represented as production-ready while unresolved critical or high-risk defects remain.

Document accepted limitations and residual risks.

## 28. Risk-Based Readiness Criteria

A feature may be considered validated when:

- Applicable tests have passed.
- Critical and high-risk defects are resolved.
- Relevant regressions have been checked.
- Integration behavior has been verified.
- Runtime checks have been performed where necessary.
- Security concerns have been reviewed according to risk.
- Documentation is synchronized.
- Known limitations are documented.
- Sufficient evidence supports the result.

Lower-priority issues may remain open when their impact is understood and documented.

Do not hide unresolved failures behind a general success status.

Use explicit states:

- Passed.
- Passed with limitations.
- Failed.
- Blocked.
- Not tested.
- Not applicable.

## 29. Comprehensive Validation Reporting

Every substantial validation operation must produce a useful report.

### 29.1 Required report information

Include:

1. Validation objective.
2. Scope.
3. Environment.
4. Software versions.
5. Tests executed.
6. Individual results.
7. Failed checks.
8. Warnings.
9. Performance observations.
10. Security findings.
11. Compatibility findings.
12. Regression status.
13. Evidence references.
14. Known limitations.
15. Recommended actions.
16. Overall readiness.

### 29.2 Test evidence

Where available, reference:

- Logs.
- Screenshots.
- Metrics.
- Test output.
- Reproduction steps.
- Relevant source locations.

### 29.3 Honest reporting

Distinguish a test that passed from one that was not executed.

Distinguish measured results from estimates.

Distinguish confirmed causes from hypotheses.

A report must never exaggerate validation coverage.

## 30. Hybrid Evidence Management

Maintain a balance between traceability and storage efficiency.

### 30.1 Permanent evidence

Preserve important:

- Validation reports.
- Performance baselines.
- Regression summaries.
- Compatibility findings.
- Release acceptance reports.
- Relevant visual references.

### 30.2 Temporary evidence

Apply retention policies to:

- Large logs.
- Temporary screenshots.
- Profiling traces.
- Temporary test output.
- Disposable test artifacts.

### 30.3 Sensitive evidence

Remove or redact sensitive information.

Never store credentials or private tokens in validation artifacts.

### 30.4 Storage efficiency

Avoid storing unnecessary generated data in Git.

Use references to external evidence when appropriate.

## 31. Validation Documentation Structure

Maintain validation documentation inside the project.

Recommended structure:

    docs/
    └── validation/
        ├── VALIDATION_STRATEGY.md
        ├── TEST_COVERAGE.md
        ├── COMPATIBILITY_MATRIX.md
        ├── REGRESSION_LOG.md
        ├── PERFORMANCE_BASELINES.md
        ├── SECURITY_VALIDATION.md
        ├── ACCESSIBILITY_VALIDATION.md
        ├── INSTALLATION_TESTING.md
        ├── RELEASE_CHECKLIST.md
        └── KNOWN_LIMITATIONS.md

Only create documents when their contents are justified.

Avoid empty or redundant documentation.

Keep the structure synchronized with the actual project.

Store large evidence files outside the repository in a local directory excluded from Git.

The actual evidence location must be discovered or agreed upon; never assume a directory exists.

## 32. Advanced Documentation Validation

Documentation must remain consistent with the implementation.

### 32.1 References

Check:

- Internal links.
- External links where appropriate.
- File references.
- Section references.
- Code references.

### 32.2 API documentation

Verify documented APIs against:

- Installed Quickshell version.
- Installed Qt version.
- Official documentation.
- Actual implementation.

Never preserve speculative API examples as verified instructions.

### 32.3 Specification traceability

Check the relationship between:

- `COMPONENT_SPECS.md`.
- `DESIGN_PATTERNS.md`.
- `AGENTS.md`.
- Specialized skills.
- Implementation.
- Validation procedures.

### 32.4 Executable examples

Execute examples when feasible and safe.

If execution is not possible, mark them as unverified.

### 32.5 Documentation consistency

Detect:

- Contradictory instructions.
- Obsolete procedures.
- Missing feature documentation.
- Incorrect compatibility claims.
- Outdated screenshots.
- References to removed modules.

## 33. Technical Debt Management

Manage technical debt proactively.

### 33.1 Identification

Detect:

- Duplicated logic.
- Excessive coupling.
- Fragile integrations.
- Unnecessary complexity.
- Missing tests.
- Missing documentation.
- Resource lifecycle problems.
- Repeated workarounds.
- Architectural inconsistencies.

### 33.2 Classification

Classify issues according to:

- Impact.
- Risk.
- Maintenance cost.
- Probability of future failure.
- Remediation complexity.

### 33.3 Prioritization

Prioritize work that reduces significant risk or improves long-term maintainability.

Avoid refactoring for aesthetic reasons alone.

### 33.4 Refactoring controls

Prepare proposals for architectural changes.

Explain:

- Current problem.
- Proposed change.
- Affected modules.
- Risks.
- Compatibility.
- Validation requirements.

Obtain approval before implementing architectural changes.

## 34. Controlled Parallel Testing

Use controlled parallelism.

### 34.1 Parallel candidates

Independent tests may run simultaneously when they do not share mutable resources.

### 34.2 Resource limits

Consider:

- CPU load.
- Memory availability.
- GPU contention.
- Disk activity.
- Process limits.
- Active desktop responsiveness.

### 34.3 Sequential dependencies

Keep dependent tests sequential.

Examples:

- Configuration migration before migration-dependent integration tests.
- Service startup before service-dependent runtime tests.
- Environment setup before runtime validation.

### 34.4 Isolation

Use independent temporary directories and test data.

Avoid concurrent tests modifying the same configuration.

### 34.5 Cancellation

Cancel related tests when a critical prerequisite fails.

Report the reason for cancellation.

## 35. Advanced Observability Testing

Validate the project's diagnostic capabilities.

### 35.1 Structured logging

Check:

- Log levels.
- Timestamps.
- Module identity.
- Context.
- Error information.
- Correlation identifiers where appropriate.

### 35.2 Cross-process tracing

Where supported, verify that related operations can be followed across:

- Quickshell.
- External processes.
- IPC.
- D-Bus.
- Shared services.

### 35.3 Metrics

Check consistency between recorded metrics and observed behavior.

### 35.4 Error visibility

Detect:

- Silent failures.
- Missing error context.
- Duplicate error reports.
- Misleading success messages.
- Lost process output.

### 35.5 Retention

Validate rotation and retention behavior.

### 35.6 Privacy

Verify that sensitive information is redacted.

## 36. Advanced Integration Recovery

Recovery behavior must be validated independently from normal operation.

### 36.1 Recovery scenarios

Test:

- Service unavailable at startup.
- Service becomes unavailable at runtime.
- Service restarts.
- Device disappears.
- Device reconnects.
- IPC connection is interrupted.
- External process exits unexpectedly.

### 36.2 Recovery expectations

Verify:

- Correct error state.
- Bounded retry attempts.
- Backoff.
- Recovery without duplication.
- Resource cleanup.
- Correct restored state.
- No unnecessary restart of unrelated modules.

### 36.3 Failure isolation

A single module failure must not unnecessarily crash the shell.

Validate the boundaries between independent modules.

## 37. Advanced Performance and Resource Lifecycle Validation

Every long-lived service and component must have an appropriate resource lifecycle.

Inspect:

- Timers.
- Connections.
- Signal handlers.
- Processes.
- File handles.
- IPC connections.
- Dynamic QML objects.
- Temporary data.
- Event subscriptions.

Verify cleanup when:

- Components are destroyed.
- Modules are disabled.
- Devices disappear.
- Operations are cancelled.
- Services restart.
- The application shuts down.

Watch for duplicate event subscriptions and multiple instances of supposedly shared services.

## 38. Quickshell and Wayland Runtime Validation

Validate against the actual installed environment.

### 38.1 Quickshell

Check:

- Runtime startup.
- QML loading.
- Component creation.
- Signal handling.
- Property bindings.
- Process integration.
- Error output.
- Resource cleanup.

### 38.2 Wayland surfaces

Check:

- Surface creation.
- Layer configuration.
- Exclusive zones.
- Anchors.
- Margins.
- Keyboard interaction.
- Focus.
- Popup placement.
- Surface destruction.

### 38.3 Hyprland

Check:

- Workspace state.
- Monitor changes.
- Window events.
- Focus changes.
- Special workspaces.
- Relevant compositor interactions.

Do not assume a compositor event or IPC interface behaves identically across versions.

### 38.4 Multi-monitor

Validate:

- Monitor discovery.
- Monitor addition.
- Monitor removal.
- Resolution changes.
- Scaling changes.
- Orientation changes.
- Workspace association.
- Per-monitor state.

### 38.5 Active session safety

Never restart or replace the active compositor or shell automatically during testing.

## 39. Functional Validation by Feature Category

Use feature-specific validation plans.

### 39.1 Panels

Check:

- Visibility.
- Geometry.
- Position.
- Auto-hide.
- Interaction.
- Monitor association.
- Expansion behavior.

### 39.2 Dashboards

Check:

- Opening and closing.
- Navigation.
- Focus.
- Escape handling.
- State synchronization.
- Popup stacking.

### 39.3 Widgets

Check:

- Empty state.
- Loading state.
- Active state.
- Error state.
- Updates.
- Resource cleanup.

### 39.4 Notifications

Check:

- Delivery.
- Grouping.
- Persistence.
- Expiration.
- Hover pause.
- Actions.
- History.
- Recovery.

### 39.5 Launcher

Check:

- Filtering.
- Keyboard interaction.
- Application discovery.
- Command execution safety.
- File search boundaries.
- Calculator behavior.
- Error reporting.

### 39.6 Audio and media

Check:

- Device selection.
- Volume changes.
- Mute state.
- Playback control.
- Device removal.
- Multiple media players.
- Idle visualizer behavior.

### 39.7 Network and Bluetooth

Check:

- Discovery.
- State changes.
- Connect and disconnect.
- Pairing.
- Permission handling.
- Missing service behavior.
- Recovery.

### 39.8 Configuration and profiles

Check:

- Save.
- Load.
- Rename.
- Duplicate.
- Delete.
- Restore.
- Import.
- Export.
- Migration.

### 39.9 Installation and maintenance

Check the lifecycle requirements defined in this skill.

## 40. Validation Automation

Use hybrid automation.

### 40.1 Automatic checks

Execute relevant checks during development when practical.

### 40.2 Feature-level checks

Execute broader tests after meaningful feature completion.

### 40.3 Global checks

Reserve expensive validation for important milestones.

### 40.4 Existing scripts

If `scripts/check.sh` exists, inspect and use it when appropriate.

Do not assume its existence.

Inspect what it actually does before execution.

### 40.5 Automation safety

Never let automatic validation:

- Modify system configuration.
- Install packages.
- Change compositor configuration.
- Destroy user data.
- Terminate the active desktop.
- Modify unrelated files.

Such operations require explicit authorization.

## 41. Validation Tooling Policy

Prefer existing tools.

Before introducing new tools:

1. Identify the missing capability.
2. Inspect installed alternatives.
3. Explain the proposed tool.
4. Evaluate maintenance.
5. Evaluate compatibility.
6. Evaluate resource cost.
7. Explain expected benefits.
8. Request authorization.

Do not install packages or dependencies without approval.

Avoid redundant testing frameworks.

Prefer the simplest reliable tooling that satisfies the requirement.

## 42. Validation Autonomy

Use balanced autonomy.

The agent should:

- Select relevant tests independently.
- Increase validation depth when risk increases.
- Investigate failures.
- Collect evidence.
- Identify regressions.
- Perform safe, localized corrections when permitted.
- Report limitations.
- Propose improvements.

The agent must request approval before:

- Architectural changes.
- Destructive operations.
- Dependency installation.
- System-level modifications.
- Active desktop disruption.
- Changes to user data.
- Unrequested functional redesign.

Autonomy does not override safety or user instructions.

## 43. Architectural Change Validation

Architectural changes require a proposal and approval.

The proposal must contain:

- Current architecture.
- Problem being addressed.
- Proposed architecture.
- Affected modules.
- Dependency changes.
- Migration requirements.
- Compatibility implications.
- Performance implications.
- Security implications.
- Regression strategy.

After approval:

1. Implement incrementally.
2. Validate affected interfaces.
3. Validate dependency boundaries.
4. Run integration tests.
5. Run regression tests.
6. Update documentation.
7. Report remaining limitations.

## 44. Validation Workflow

Use the following workflow for meaningful changes.

### Step 1: Understand

Read relevant specifications and implementation.

### Step 2: Assess

Identify affected modules, risks, and dependencies.

### Step 3: Plan

Select appropriate validation methods.

### Step 4: Establish conditions

Record relevant environment details and prerequisites.

### Step 5: Execute targeted tests

Run the smallest useful set first.

### Step 6: Investigate failures

Collect evidence and identify probable causes.

### Step 7: Remediate

Apply safe corrections or request approval when necessary.

### Step 8: Revalidate

Repeat failed tests and relevant neighboring checks.

### Step 9: Check regressions

Ensure existing behavior remains intact.

### Step 10: Review documentation

Update specifications and validation records when necessary.

### Step 11: Report

Provide results, evidence, limitations, and readiness.

## 45. Definition of Done

A meaningful validation task is complete when:

- Its scope is clear.
- Relevant risks are assessed.
- Applicable tests are selected.
- Tests are executed or explicitly marked as unavailable.
- Failures are investigated.
- Safe corrections are verified.
- Regressions are checked.
- Relevant documentation is updated.
- Evidence is retained according to policy.
- Results are honestly reported.

A feature is not considered validated simply because its implementation appears complete.

## 46. Anti-Patterns

Never use the following approaches.

### 46.1 Testing only startup

Incorrect:

- Start Quickshell.
- Observe that it opens.
- Declare the entire shell validated.

Correct:

- Validate the relevant functionality, integrations, runtime behavior, and failure paths.

### 46.2 Inventing test success

Incorrect:

- Assume a test passed because no error was mentioned.

Correct:

- Record actual execution results.

### 46.3 Disabling failing tests

Incorrect:

- Remove or weaken tests to obtain a green result.

Correct:

- Investigate the underlying failure and justify any legitimate test changes.

### 46.4 Destructive runtime testing

Incorrect:

- Restart Hyprland to test a shell feature without permission.

Correct:

- Use isolated runtime testing or request authorization.

### 46.5 Unbounded performance testing

Incorrect:

- Run unlimited stress tests against the active desktop.

Correct:

- Use controlled workloads and resource limits.

### 46.6 Over-testing insignificant changes

Incorrect:

- Execute the full release suite after every color adjustment.

Correct:

- Apply proportional validation.

### 46.7 Under-testing critical changes

Incorrect:

- Test only syntax after modifying process execution.

Correct:

- Include security, integration, lifecycle, and failure testing.

### 46.8 False compatibility claims

Incorrect:

- Claim support for an alternative service without testing it.

Correct:

- Report exactly what was detected and verified.

### 46.9 Speculative performance claims

Incorrect:

- Claim an optimization improved performance without measurements.

Correct:

- Compare measurements under comparable conditions.

### 46.10 Silent recovery

Incorrect:

- Restart a failed module repeatedly without reporting the problem.

Correct:

- Use bounded recovery and structured diagnostics.

## 47. Example Validation Scenarios

### Scenario A: New QML widget

Validate:

1. QML syntax.
2. Property bindings.
3. Component behavior.
4. Keyboard interaction.
5. Focus.
6. Visual appearance.
7. Accessibility.
8. Resource lifecycle.
9. Integration with shared services.
10. Documentation.

### Scenario B: Network integration change

Validate:

1. Capability detection.
2. Provider availability.
3. Data normalization.
4. Asynchronous execution.
5. Connection state.
6. Service interruption.
7. Reconnection.
8. Invalid responses.
9. Security boundaries.
10. Regression behavior.

### Scenario C: Configuration migration

Validate:

1. Source schema.
2. Destination schema.
3. Representative fixtures.
4. Default handling.
5. User value preservation.
6. Invalid input.
7. Interrupted migration.
8. Backup recovery.
9. Compatibility.
10. Documentation.

### Scenario D: Performance regression

Validate:

1. Reproduce the workload.
2. Record current metrics.
3. Compare against a baseline.
4. Identify affected processes.
5. Inspect timers and polling.
6. Inspect resource lifecycle.
7. Identify the probable cause.
8. Correct the issue.
9. Repeat measurements.
10. Verify neighboring functionality.

### Scenario E: Release candidate

Validate:

1. Clean installation.
2. Existing installation update.
3. Configuration migration.
4. Functional coverage.
5. Integration coverage.
6. Security review.
7. Accessibility.
8. Visual consistency.
9. Performance.
10. Stability.
11. Rollback.
12. Documentation.
13. Final acceptance report.

## 48. Validation Prioritization

When time or resources are constrained, prioritize:

1. Data and configuration integrity.
2. Security-sensitive operations.
3. Application startup and recovery.
4. Process execution.
5. Shared services.
6. Wayland and Hyprland integration.
7. Critical desktop functionality.
8. Regression prevention.
9. Performance and resource lifecycle.
10. Accessibility.
11. Visual consistency.
12. Lower-risk decorative behavior.

Do not interpret prioritization as permission to permanently ignore lower-priority requirements.

Document deferred checks.

## 49. Handling Unavailable Validation Capabilities

Some validation may not be technically possible.

Examples:

- No automated screenshot capture support.
- No suitable GPU profiler.
- No isolated Wayland test environment.
- No alternative integration provider installed.
- No compatible accessibility inspection tool.

When this happens:

1. Explain the limitation.
2. Identify the affected validation.
3. Determine whether manual verification is possible.
4. Avoid unsupported claims.
5. Record the limitation.
6. Explain whether it affects readiness.

Never invent a substitute result.

## 50. Continuous Quality Improvement

Validation procedures should evolve with mao-shell.

Review:

- Recurring defects.
- Missing test coverage.
- Ineffective tests.
- False positives.
- Performance trends.
- Compatibility changes.
- Security findings.
- Documentation gaps.
- Technical debt.

Improve the validation system when evidence shows that it is insufficient.

Avoid adding complexity without a concrete benefit.

## 51. Collaboration with Other Skills

This skill works together with:

### quickshell-development

Responsible for Quickshell runtime correctness, API verification, lifecycle, performance, and implementation quality.

### qml-component

Responsible for component behavior, visual consistency, layout, accessibility, interactions, and QML-specific quality.

### system-integration

Responsible for external services, process execution, IPC, D-Bus, hardware discovery, permissions, recovery, and system integration safety.

### project-validation

Responsible for coordinating validation strategy, testing depth, evidence, regression management, performance assessment, release readiness, and quality reporting.

Do not duplicate another skill's implementation responsibilities unnecessarily.

Validate the contracts between skills and components.

## 52. Final Validation Principles

Every validation activity must reinforce the following commitments:

- Never confuse implementation with verification.
- Never confuse a successful startup with functional correctness.
- Never confuse an estimate with a measurement.
- Never confuse a hypothesis with a confirmed cause.
- Never confuse unavailable tests with passed tests.
- Never compromise the active desktop for convenience.
- Never hide failures.
- Never introduce dependencies without approval.
- Never weaken security to simplify testing.
- Never sacrifice reliability for superficial test coverage.

The final objective is to make mao-shell a reliable, secure, performant, accessible, maintainable, and production-ready desktop shell.

**Validation must provide evidence, not reassurance.**