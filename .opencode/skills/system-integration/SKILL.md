---
name: system-integration
description: Specialized system integration guidance for mao-shell on Arch Linux and Wayland. Use automatically when discovering, monitoring, controlling or interacting with system services, hardware, devices, processes, IPC interfaces, Hyprland, audio, networking, Bluetooth, sensors or external commands.
---

# System Integration

## 1. Purpose

This skill defines how mao-shell interacts with the underlying operating system, desktop environment, hardware, services and external processes.

Its purpose is to ensure that system integrations are:

- Reliable.
- Secure.
- Asynchronous.
- Resource-efficient.
- Compatible with different Arch Linux configurations.
- Resilient to service failures and hardware changes.
- Properly isolated from the visual interface.
- Observable and diagnosable.
- Maintainable over time.

The skill focuses on the boundary between mao-shell and the external system.

It does not replace the general development rules defined in `AGENTS.md` or `quickshell-development`.

## 2. Scope

This skill applies whenever an implementation interacts with:

- Operating system services.
- Desktop environment interfaces.
- Hyprland IPC and events.
- Audio systems.
- Network management.
- Bluetooth.
- Hardware devices.
- System monitoring.
- External commands.
- Background processes.
- System configuration and user-level operations.
- Hardware discovery and hotplug events.

Typical integrations include:

- Hyprland.
- PipeWire and compatible audio tools.
- NetworkManager and alternative network managers.
- BlueZ and Bluetooth utilities.
- CPU and memory monitoring.
- GPU monitoring.
- Temperature and hardware sensors.
- Media-related system interfaces.
- Notification-related interfaces.
- Power management.
- System update information.

The list is not exhaustive.

New integrations may be introduced when justified by product requirements.

## 3. Mandatory Project Context

Before implementing or modifying an integration, read the relevant project documentation.

Required documents:

1. `AGENTS.md`
2. `ROADMAP.md`
3. `TASKS.md`
4. `COMPONENT_SPECS.md`
5. `DESIGN_PATTERNS.md`
6. The applicable Quickshell development instructions.

Also inspect existing integration modules and shared services before creating anything new.

### 3.1 Documentation authority

Resolve conflicts by topic, as defined in `AGENTS.md` ("Precedence when documents conflict"). Product behaviour comes from `COMPONENT_SPECS.md`, architecture from `DESIGN_PATTERNS.md`, and workflow and safety from `AGENTS.md`. This skill and external references never override those documents.

Technical correctness is mandatory.

Never implement an API, command, protocol or behavior that does not actually exist merely because a project document mentions it.

If documentation conflicts with the installed environment, investigate and report the discrepancy.

## 4. Integration Strategy

mao-shell uses a hybrid integration strategy.

Choose the most appropriate mechanism for each integration:

- Native Quickshell capabilities.
- Supported IPC interfaces.
- System service APIs.
- D-Bus interfaces.
- Event streams.
- External CLI utilities.

Do not force every integration through the same mechanism.

### 4.1 Selection priorities

Evaluate mechanisms in this order:

1. Reliability and compatibility.
2. Event-driven capabilities.
3. Native integration when sufficiently capable.
4. Direct service or IPC interfaces.
5. External CLI tools when justified.
6. Polling only when no reliable event mechanism exists.

This is a selection framework, not an absolute requirement to use native APIs first.

A reliable system interface can be preferable to an incomplete native abstraction.

### 4.2 Avoid unnecessary abstraction

Do not introduce generic integration frameworks solely to make every service look identical.

Use shared abstractions only when they solve a real maintenance or consistency problem.

Keep service-specific functionality accessible when normalization would discard meaningful information.

## 5. System Discovery

Never assume that a particular service, executable, device or manager exists.

Arch Linux installations can differ significantly.

Discover capabilities at runtime.

### 5.1 Initial discovery

At startup, determine which integrations are available.

Depending on the integration, inspect:

- Executable availability.
- Service availability.
- Service state.
- IPC socket availability.
- D-Bus service availability.
- Device presence.
- Permissions.
- Required protocol support.
- Version compatibility.

Discovery must not block the interface.

### 5.2 Capability model

Represent discovered capabilities explicitly.

An integration should be able to distinguish between:

- Available.
- Unavailable.
- Initializing.
- Degraded.
- Temporarily disconnected.
- Permission denied.
- Failed.
- Unsupported.

Do not represent every unavailable integration as a generic error.

The distinction helps the interface communicate useful information.

### 5.3 Runtime capability changes

Capabilities can change after startup.

Examples:

- A service starts.
- A service stops.
- A device is connected.
- A device disappears.
- An IPC socket becomes unavailable.
- A permission changes.
- A network manager restarts.

Update the integration state when relevant changes occur.

Do not require a full shell restart to recover from ordinary service or hardware changes.

## 6. Portable Arch Linux Compatibility

mao-shell targets Arch Linux and Wayland.

It should remain usable across different Arch Linux configurations.

Do not assume that every user has the same desktop utilities or service managers installed.

### 6.1 Detect alternatives

Examples of possible differences include:

- Different network managers.
- Different audio utilities.
- Different GPU monitoring tools.
- Different Bluetooth configurations.
- Different service states.
- Different hardware sensor availability.

Detect the actual environment before selecting an integration method.

### 6.2 Alternative selection

When multiple compatible implementations exist:

1. Detect available capabilities.
2. Evaluate compatibility.
3. Select a reliable implementation.
4. Record which implementation was selected.
5. Expose degraded capabilities when necessary.

Do not silently switch to an incompatible alternative.

### 6.3 Dependency installation

Never install missing packages automatically.

Never run package installation commands without explicit user authorization.

If a required dependency is missing:

- Explain which capability is affected.
- Identify the missing dependency.
- Explain whether an alternative exists.
- Report any manual action required.

## 7. Event-Driven Communication

Use a hybrid communication model.

Event-driven updates are the preferred mechanism.

Polling is a fallback for integrations without suitable events.

### 7.1 Preferred mechanisms

When available and verified, use:

- IPC event streams.
- D-Bus signals.
- Native signals.
- Service notifications.
- Device change events.
- File or socket notifications.
- Other documented event interfaces.

Avoid repeatedly querying a service when it can notify mao-shell of changes.

### 7.2 Polling fallback

Polling may be used when:

- No suitable event interface exists.
- The service does not expose all required information through events.
- Periodic verification is necessary.
- An event stream cannot reliably detect a particular condition.

Polling must be:

- Asynchronous.
- Limited to the required information.
- Configured with a reasonable interval.
- Avoidant of overlapping requests.
- Stopped when no longer required.

Do not use very short intervals without a measurable reason.

### 7.3 Hybrid synchronization

When events and polling are combined:

- Events should normally trigger immediate updates.
- Polling should provide fallback verification.
- Duplicate updates should be avoided.
- The integration must prevent concurrent redundant requests.
- State must remain consistent if events arrive out of order.

Do not assume that every event stream is complete or perfectly ordered.

## 8. Fully Asynchronous Process Execution

All external processes must execute asynchronously.

There are no synchronous exceptions.

Never block the UI thread while waiting for:

- Process startup.
- Process completion.
- Standard output.
- Standard error.
- Network operations.
- Service responses.
- Device operations.
- IPC responses.

### 8.1 Execution requirements

Every external process must have:

- Explicit executable selection.
- Separate arguments.
- Defined ownership.
- A lifecycle strategy.
- Output handling.
- Error handling.
- Timeout handling where applicable.
- Cancellation behavior where applicable.
- Exit status inspection.
- Cleanup behavior.

### 8.2 Argument safety

Never construct shell commands by concatenating untrusted values.

Prefer APIs that accept an executable and a separate argument list.

Avoid:

- Shell interpolation.
- Dynamic command strings.
- `eval`.
- Unnecessary shell wrappers.
- Unvalidated user-provided arguments.

Validate arguments according to the target command's expected format.

### 8.3 Output handling

Treat all external process output as untrusted data.

Validate:

- Expected format.
- Required fields.
- Data types.
- Numeric ranges.
- Missing values.
- Encoding.
- Unexpected output.

Never assume a successful exit code guarantees valid output.

### 8.4 Timeouts

Long-running operations must have appropriate timeouts.

Timeouts should reflect the operation.

For example, a local status query and a Bluetooth pairing operation do not necessarily require the same timeout.

When a timeout occurs:

- Report the operation as timed out.
- Cancel or terminate the process when appropriate.
- Release associated resources.
- Preserve diagnostic information.
- Avoid reporting success.

### 8.5 Cancellation

Where possible, operations should support cancellation.

Cancellation must not leave:

- Orphaned processes.
- Partially updated shared state.
- Invalid device state.
- Unreleased resources.
- Duplicate retry operations.

## 9. Process Lifecycle Management

Use a hybrid lifecycle strategy.

Some processes benefit from remaining active.

Others should only exist while an operation is running.

### 9.1 Persistent processes

Persistent processes are appropriate when they provide continuous value.

Examples include:

- Reliable event subscriptions.
- Long-running IPC listeners.
- Necessary service monitoring.
- Continuous streams that cannot be replaced by native event subscriptions.

Persistent processes must have explicit lifecycle ownership.

### 9.2 On-demand processes

Use on-demand processes for:

- Status queries.
- One-shot operations.
- Device control commands.
- Short-lived diagnostics.
- Operations that do not require a continuous connection.

Terminate them after completion.

### 9.3 Process ownership

Every process must have a clear owner.

The owner must know:

- Why the process exists.
- When it should start.
- When it should stop.
- How it reports failure.
- How it is restarted.
- Which resources it owns.

Avoid duplicate instances of the same integration process.

### 9.4 Unexpected termination

When a persistent process terminates unexpectedly:

1. Capture its exit status.
2. Record diagnostic context.
3. Update integration availability.
4. Determine whether recovery is safe.
5. Apply bounded retry behavior.
6. Notify dependent modules of the state change.

Do not restart processes indefinitely.

## 10. Hybrid Failure Recovery

Failures must be isolated and recovery must be controlled.

A failure in one integration must not unnecessarily break unrelated shell functionality.

### 10.1 Failure isolation

Examples:

- Bluetooth failure must not disable networking.
- Audio failure must not break the application launcher.
- GPU monitoring failure must not stop CPU monitoring.
- Hyprland event interruption must not terminate unrelated services.

Separate integrations according to their responsibilities.

### 10.2 Recovery sequence

When an integration fails:

1. Detect the failure.
2. Record its context.
3. Mark the integration as degraded or unavailable.
4. Notify dependent modules.
5. Determine whether the failure is recoverable.
6. Attempt controlled recovery if appropriate.
7. Validate recovery.
8. Publish the new state.

### 10.3 Retry policy

Retries must be:

- Limited.
- Delayed.
- Context-aware.
- Cancellable.
- Observable.

Use exponential backoff or another justified delay strategy.

Do not create tight retry loops.

Do not repeatedly restart a service that is intentionally stopped by the user.

### 10.4 Recovery limitations

Some failures require user intervention.

Examples include:

- Missing permissions.
- Missing packages.
- Hardware faults.
- Unsupported protocols.
- Invalid system configuration.

Do not pretend these conditions can always be repaired automatically.

## 11. Structured Diagnostics

Use the centralized mao-shell diagnostic system.

Do not create a second independent logging or diagnostics framework.

Diagnostics must provide sufficient information to understand integration behavior.

### 11.1 Required diagnostic information

Where applicable, record:

- Timestamp.
- Integration name.
- Operation.
- Severity.
- Service or executable.
- Process identifier.
- Duration.
- Result.
- Exit status.
- Retry count.
- Failure category.
- Recovery outcome.

### 11.2 Diagnostic levels

Use appropriate severity levels.

Typical categories:

- Debug.
- Info.
- Warning.
- Error.
- Critical.

Do not classify ordinary service unavailability as critical.

### 11.3 Performance diagnostics

Measure relevant values where useful:

- Operation duration.
- Response time.
- Process startup time.
- Event delivery latency.
- Polling frequency.
- Retry frequency.
- Resource consumption.
- Number of active processes.

Avoid expensive diagnostic collection that creates the performance problem being investigated.

### 11.4 Diagnostic reports

Integration diagnostics should be compatible with the general mao-shell diagnostic report.

Provide enough context to identify:

- Which integration failed.
- Which implementation was selected.
- Whether the dependency was available.
- What operation failed.
- Whether recovery was attempted.
- Whether user intervention is required.

## 12. Data Validation and Normalization

Integrations receive data from external systems.

Never assume external data is valid.

### 12.1 Input validation

Validate data before publishing it to shared services.

Check:

- Types.
- Required properties.
- Numeric ranges.
- Empty values.
- Encoding.
- Unexpected fields.
- Invalid states.
- Stale timestamps.

### 12.2 Hybrid data model

Normalize data that is shared between modules.

Preserve integration-specific information when it provides useful functionality.

Examples:

A common device representation may expose:

- Stable identifier.
- Display name.
- Availability.
- Connection state.
- Device category.

A service-specific representation may additionally expose:

- Protocol capabilities.
- Backend-specific identifiers.
- Supported operations.
- Technical metadata.

Do not discard useful information merely to force a universal model.

### 12.3 Data freshness

Distinguish between:

- Current data.
- Cached data.
- Stale data.
- Unknown data.
- Unavailable data.

Do not present stale information as current.

When a service becomes unavailable, decide explicitly whether cached information remains useful.

### 12.4 Source of truth

Every shared state must have a clear owner.

Avoid multiple integrations independently writing the same state.

Dependent modules should consume shared services rather than maintaining competing copies.

## 13. Security Requirements

System integration is a security-sensitive part of mao-shell.

Follow strict security practices.

### 13.1 Trust boundaries

Treat all external inputs as untrusted.

This includes:

- CLI output.
- IPC messages.
- D-Bus messages.
- Device names.
- Network identifiers.
- Service metadata.
- Environment variables.
- File paths.
- User-provided values.

### 13.2 Command injection prevention

Never concatenate untrusted data into executable command strings.

Use argument-safe process APIs.

Validate values before passing them to external commands.

Do not use `eval` or dynamically constructed shell scripts.

### 13.3 Sensitive information

Protect:

- Passwords.
- Tokens.
- Authentication material.
- Private network information.
- Sensitive file paths.
- User-specific device information.

Do not expose secrets in logs or diagnostic reports.

### 13.4 Output sanitization

Sanitize external output before:

- Logging.
- Displaying in UI.
- Passing between modules.
- Saving to persistent storage.
- Including in diagnostic reports.

Avoid terminal control sequences and unexpected formatting in diagnostic output.

### 13.5 Access boundaries

Modules should only receive the information they need.

Do not expose unrestricted process execution or system control through generic global interfaces.

Use explicit service interfaces.

## 14. Permissions and Privilege Management

Use a hybrid permission model.

Normal operations should run with the current user's permissions.

Polkit may be used for operations that genuinely require authorization.

### 14.1 Prohibited behavior

Never:

- Execute `sudo` from mao-shell.
- Request root privileges unnecessarily.
- Attempt to bypass system permissions.
- Modify authorization policies automatically.
- Store administrative credentials.
- Hide privileged operations from the user.

### 14.2 Controlled privileged operations

When an operation requires elevated privileges:

1. Verify that the operation genuinely requires them.
2. Check whether a supported authorization mechanism exists.
3. Use an explicit authorization flow.
4. Make the operation understandable to the user.
5. Handle authorization denial.
6. Report the result accurately.

Never assume authorization will succeed.

### 14.3 User-level operations

Prefer user-session services and interfaces whenever possible.

Do not use system-level control when user-level functionality is sufficient.

## 15. Controlled System Actions

mao-shell may perform explicit operations on devices and services.

Examples include:

- Connecting and disconnecting Bluetooth devices.
- Changing audio output devices.
- Muting and unmuting audio.
- Changing network connection state.
- Activating supported interfaces.
- Controlling supported user-level services.
- Performing normal desktop operations.

### 15.1 Action boundaries

Do not modify system configuration files automatically.

Do not silently change:

- Hyprland configuration.
- System service configuration.
- System-wide settings.
- Package configuration.
- Boot configuration.
- System authorization policies.

Changes to these areas require explicit user authorization and must follow the project's installation and maintenance rules.

### 15.2 User intent

Distinguish between viewing information and performing an action.

A hover interaction must not accidentally execute a system operation.

Potentially disruptive operations must require an explicit interaction.

### 15.3 Action results

For every action:

- Report success only after confirmation.
- Report failures accurately.
- Preserve the previous state when an operation fails where possible.
- Avoid duplicate actions.
- Handle concurrent requests safely.

## 16. Hardware Discovery and Hotplug

Use hybrid hardware detection.

Perform initial discovery and continue monitoring relevant hardware changes.

### 16.1 Initial discovery

At startup, discover the hardware relevant to enabled integrations.

Examples include:

- Audio devices.
- Network interfaces.
- Bluetooth adapters.
- GPUs.
- Sensors.
- Monitors.
- Input devices.

Do not assume a specific device exists.

### 16.2 Dynamic changes

Monitor relevant changes such as:

- Device connection.
- Device disconnection.
- Device replacement.
- Device state changes.
- Adapter availability.
- Monitor changes.
- Service changes affecting device access.

### 16.3 Hotplug handling

When a device appears:

1. Detect it.
2. Validate its information.
3. Identify its capabilities.
4. Register it with the appropriate integration.
5. Publish its availability.
6. Notify dependent modules.

When a device disappears:

1. Detect removal.
2. Invalidate active references.
3. Cancel operations targeting the device.
4. Update shared state.
5. Notify dependent modules.
6. Release resources.

### 16.4 Device identity

Do not rely exclusively on display names.

Names may change or be duplicated.

Use stable identifiers where available.

Handle devices whose identifiers are only temporarily available.

## 17. Hyprland Integration

Hyprland is the target compositor.

Use supported IPC mechanisms and documented interfaces for interaction.

### 17.1 API verification

Before implementing an integration:

- Inspect the installed Hyprland version.
- Verify the relevant IPC mechanism.
- Verify event formats.
- Confirm command behavior.
- Check version-specific differences.

Do not assume an IPC command remains identical across all versions.

### 17.2 Event handling

When using Hyprland events:

- Parse messages defensively.
- Handle malformed messages.
- Handle connection interruptions.
- Avoid duplicate subscriptions.
- Reconnect safely.
- Update shared state consistently.

### 17.3 Desktop state

Depending on product requirements, integrations may expose:

- Workspaces.
- Active windows.
- Focus changes.
- Monitor information.
- Special workspaces.
- Relevant compositor state.

Keep compositor state in a dedicated integration service.

Visual components must not independently implement their own Hyprland IPC connections.

### 17.4 Configuration safety

Do not automatically rewrite Hyprland configuration.

Do not silently change:

- Startup commands.
- Keybindings.
- Window rules.
- Monitor configuration.
- Layer rules.

Follow the project installation and migration requirements.

## 18. Audio Integration

Audio integration should use a hybrid approach.

Prefer reliable native or event-based interfaces when available.

Use compatible system utilities when required.

### 18.1 Capability discovery

Detect:

- Audio service availability.
- Default output.
- Available outputs.
- Available inputs.
- Device identifiers.
- Volume state.
- Mute state.
- Relevant device capabilities.

Do not assume PipeWire is available without verification.

### 18.2 State synchronization

Keep audio state synchronized with the actual system.

Handle changes initiated outside mao-shell.

Examples:

- Physical keyboard volume controls.
- Desktop environment controls.
- Application volume changes.
- Device disconnection.
- Default device changes.

### 18.3 Operations

Support explicit operations such as:

- Set volume.
- Mute.
- Unmute.
- Select output.
- Select input.
- Change default device.

Validate values and capabilities before executing operations.

### 18.4 Audio failures

If an audio service becomes unavailable:

- Mark the integration unavailable.
- Preserve useful cached information when appropriate.
- Stop unnecessary polling.
- Attempt controlled reconnection.
- Notify dependent modules.

Do not allow audio failure to affect unrelated shell functionality.

## 19. Network Integration

Network integration must not assume NetworkManager.

Detect the actual network management capabilities.

### 19.1 Discovery

Where supported, identify:

- Network manager.
- Available interfaces.
- Connection state.
- Wi-Fi capability.
- Ethernet capability.
- VPN capability.
- Current connection.
- Relevant signal information.

### 19.2 Operations

Support only operations exposed by the detected backend.

Examples:

- Connect.
- Disconnect.
- Enable or disable an interface.
- Select a Wi-Fi network.
- Inspect connection information.

Do not expose unsupported operations as if they were available.

### 19.3 Sensitive information

Network data may contain sensitive information.

Avoid unnecessarily logging:

- SSIDs.
- Private IP addresses.
- Network identifiers.
- Connection metadata.

Never log credentials.

### 19.4 Network changes

Handle:

- Connection loss.
- Reconnection.
- Interface disappearance.
- Manager restart.
- VPN changes.
- Wi-Fi availability changes.

Network monitoring must not produce excessive background activity.

## 20. Bluetooth Integration

Bluetooth functionality should use detected system capabilities.

Do not assume BlueZ or a specific CLI utility exists.

### 20.1 Discovery

Identify:

- Adapter availability.
- Adapter state.
- Paired devices.
- Connected devices.
- Discoverability.
- Pairing capability.
- Device categories.

### 20.2 Operations

Support compatible operations such as:

- Enable or disable the adapter.
- Start or stop discovery.
- Pair.
- Connect.
- Disconnect.
- Remove a pairing.

Validate device identifiers and operation state.

### 20.3 Pairing

Pairing must be treated as an explicit operation.

Handle:

- User confirmation.
- Authentication requests.
- Timeouts.
- Pairing rejection.
- Device disappearance.
- Backend disconnection.

Do not report a device as paired merely because a pairing command was issued.

### 20.4 Device categories

Preserve useful device classification.

For example:

- Audio.
- Input.
- Other.

Do not assume every connected device supports the same operations.

## 21. CPU, Memory and GPU Monitoring

System monitoring must be efficient and capability-aware.

### 21.1 CPU

Expose useful CPU information when available.

Avoid unnecessarily frequent collection.

### 21.2 Memory

Distinguish between memory measurements with different meanings.

Do not present ambiguous values without documenting their interpretation.

### 21.3 GPU

GPU monitoring depends on hardware and driver support.

Detect the available monitoring mechanism.

Do not assume a specific vendor or utility.

Where possible, distinguish:

- GPU utilization.
- Memory utilization.
- Temperature.
- Fan information.
- Device availability.

Missing GPU metrics should not break the monitoring integration.

### 21.4 Sensors

Sensor availability varies.

Validate values and units.

Do not assume every sensor reports temperatures or fan speeds in the same format.

### 21.5 Monitoring lifecycle

Avoid unnecessary collection when the information is not needed.

Use shared monitoring services rather than starting duplicate monitoring processes for individual widgets.

## 22. Service and Process State

System integrations must maintain accurate process and service state.

Distinguish between:

- Installed.
- Available.
- Active.
- Inactive.
- Failed.
- Starting.
- Stopping.
- Unknown.

Do not infer that a service is active merely because its executable exists.

Do not assume systemd is the only possible service-management environment without verifying the target requirement.

Never manipulate system services outside the permitted user-level and authorization boundaries.

## 23. External CLI Integration

CLI utilities are valid integration mechanisms when their behavior is verified.

### 23.1 Before using a command

Inspect:

- Whether the executable exists.
- Its installed version.
- Supported arguments.
- Expected output.
- Exit codes.
- Error behavior.
- Whether it supports machine-readable output.

Prefer structured output formats over parsing human-oriented terminal text.

### 23.2 Parsing output

When parsing CLI output:

- Handle missing fields.
- Handle unexpected fields.
- Handle localization.
- Handle whitespace.
- Handle version differences.
- Reject malformed data.
- Avoid fragile positional assumptions.

### 23.3 Command compatibility

Do not assume output remains identical across utility versions.

When possible, detect the version and select an appropriate parser.

### 23.4 Avoid command duplication

Centralize ownership of an integration.

Do not let multiple UI components execute the same status command independently.

## 24. Shared Integration Services

Integrations should expose clear interfaces to the rest of mao-shell.

### 24.1 Responsibilities

A shared integration service should own:

- Discovery.
- External communication.
- Data validation.
- State synchronization.
- Operation execution.
- Error handling.
- Recovery.
- Diagnostics.

### 24.2 UI separation

Visual components should not:

- Execute arbitrary system commands.
- Parse raw IPC streams.
- Maintain independent service connections.
- Implement their own retry loops.
- Own persistent integration processes.

They should consume exposed state and invoke supported operations.

### 24.3 Service interfaces

Interfaces should be explicit and documented.

Expose only what dependent modules need.

Avoid leaking implementation-specific internals.

### 24.4 Communication

Follow the hybrid communication model defined by the project:

- Shared services own shared state.
- QML signals communicate relevant events.
- Public interfaces expose supported operations.
- Internal implementation details remain private.

Avoid circular dependencies.

## 25. Concurrency and State Consistency

External operations can overlap.

Design integrations to handle concurrency explicitly.

### 25.1 Concurrent operations

Consider what happens when:

- Two requests target the same device.
- A device disappears during an operation.
- A service restarts during a query.
- A new request arrives before an older one completes.
- Events arrive while a command is running.

### 25.2 Operation ownership

Associate operations with their target and lifecycle.

Cancel or invalidate operations when their target is no longer available.

### 25.3 Stale results

Do not allow an older operation to overwrite newer state.

Use request identity, generation tracking or another justified consistency mechanism.

### 25.4 Duplicate actions

Prevent accidental duplicate operations.

For example, repeated clicks should not launch unlimited identical connection requests.

## 26. Resource Management

System integrations run continuously or are activated frequently.

Resource usage matters.

### 26.1 Resource ownership

Track ownership of:

- Processes.
- IPC connections.
- Event subscriptions.
- Timers.
- File descriptors.
- Temporary files.
- Device references.
- Cached data.

### 26.2 Cleanup

Release resources when:

- An integration is disabled.
- A process terminates.
- A connection becomes invalid.
- A device disappears.
- A module is destroyed.
- A subscription is no longer required.

### 26.3 Resource duplication

Avoid:

- Duplicate event listeners.
- Duplicate polling loops.
- Multiple processes collecting the same metrics.
- Redundant service connections.
- Unbounded output buffers.
- Unnecessary cached history.

### 26.4 Continuous operation

Assume mao-shell may run for long desktop sessions.

Do not design integrations that only work correctly during short tests.

## 27. Performance Requirements

Performance is a strict requirement.

### 27.1 General principles

- Never block the UI thread.
- Avoid unnecessary polling.
- Avoid duplicate processes.
- Avoid repeated expensive discovery.
- Avoid excessive IPC traffic.
- Avoid unnecessary data parsing.
- Avoid unbounded buffers.
- Avoid uncontrolled retries.

### 27.2 Data collection frequency

Choose update frequencies according to actual usefulness.

Different metrics may require different intervals.

Do not update every metric at the same frequency without justification.

### 27.3 Demand-aware monitoring

Where appropriate, reduce collection activity when dependent functionality is not active.

However, do not interrupt event subscriptions required for correct state synchronization.

### 27.4 Performance regression

When modifying an integration, consider:

- CPU usage.
- Memory usage.
- Process count.
- Event volume.
- IPC traffic.
- Startup latency.
- Recovery overhead.

## 28. Testing Strategy

Use hybrid testing.

Combine automated tests, mocks and real environment validation.

### 28.1 Unit tests

Where feasible, test:

- Parsers.
- Data normalization.
- Validation.
- State transitions.
- Retry decisions.
- Error classification.
- Capability detection logic.

### 28.2 Mock testing

Use mocks to simulate:

- Missing services.
- Invalid output.
- Delayed responses.
- Timeouts.
- Unexpected termination.
- Device changes.
- Permission denial.

Mocks should represent realistic behavior.

Do not use mocks as proof that a real integration works.

### 28.3 Integration testing

Test against the actual installed service or utility when practical.

Verify:

- Executable availability.
- Command compatibility.
- Output parsing.
- Event behavior.
- State synchronization.
- Operation results.

### 28.4 Wayland validation

Validate behavior in the real Wayland session.

A unit test cannot fully prove correct desktop integration.

Test the actual interaction with Hyprland and relevant desktop services.

### 28.5 Failure testing

Include:

- Service stopped.
- Service restarted.
- Process killed.
- IPC connection interrupted.
- Device disconnected.
- Permission denied.
- Invalid response.
- Operation timeout.

### 28.6 Test safety

Do not perform disruptive operations on the user's active desktop without authorization.

Do not disconnect active network or audio devices merely to test recovery.

Use controlled test devices or mocks when necessary.

## 29. Development Environment Safety

Protect the active desktop session.

### 29.1 Isolated development

Use the project's development mode and isolated runtime when available.

Do not assume that launching another Quickshell instance automatically creates isolation.

Inspect:

- IPC endpoints.
- Socket paths.
- Configuration paths.
- Runtime directories.
- Process ownership.
- Service connections.

### 29.2 Production protection

Do not accidentally:

- Terminate the production shell.
- Replace active configuration.
- Override existing IPC endpoints.
- Modify running service state.
- Change Hyprland startup behavior.

### 29.3 Verification

Before starting or stopping development processes, verify their identity and ownership.

Never terminate arbitrary processes based only on a broad process name match.

## 30. Logging and Sensitive Data

Diagnostics must be useful without exposing private information.

### 30.1 Log useful context

Record enough information to identify the operation and failure.

Avoid recording entire raw responses when a small sanitized summary is sufficient.

### 30.2 Redaction

Redact sensitive values before logging.

This includes credentials and tokens, and may include private network information or device metadata.

### 30.3 Error messages

Errors should explain:

- What operation failed.
- Which integration was involved.
- Whether the issue is temporary.
- Whether recovery was attempted.
- Whether user intervention is required.

Do not expose sensitive implementation details unnecessarily.

## 31. Documentation Requirements

Every integration must be documented thoroughly.

Documentation must remain synchronized with implementation.

### 31.1 Required integration documentation

Document:

1. Purpose.
2. Supported capabilities.
3. Architecture.
4. Selected backend.
5. Alternative backends.
6. Dependencies.
7. Detection process.
8. Runtime behavior.
9. IPC or command interfaces.
10. Data models.
11. State ownership.
12. Events and updates.
13. Process lifecycle.
14. Permissions.
15. Security considerations.
16. Error handling.
17. Recovery behavior.
18. Resource cleanup.
19. Tests.
20. Known limitations.

### 31.2 Interface documentation

Document public properties, signals, operations and data structures.

Do not document private implementation details as stable interfaces.

### 31.3 Decision records

When an integration involves a significant architectural choice, document:

- The problem.
- Considered alternatives.
- Selected approach.
- Reasons.
- Limitations.
- Consequences.

### 31.4 Maintenance

Update documentation whenever behavior, dependencies, protocols or interfaces change.

Outdated documentation is a maintenance defect.

## 32. External References and API Verification

Use official documentation as the primary technical reference.

Useful sources include:

- Quickshell documentation.
- Hyprland documentation.
- Official D-Bus documentation.
- PipeWire documentation.
- BlueZ documentation.
- Network manager documentation.
- Official utility manuals.
- Installed command help and manual pages.

External repositories may provide implementation examples.

They are references, not authoritative API definitions.

### 32.1 Version verification

Before implementing unfamiliar functionality:

1. Identify the installed version.
2. Locate the official documentation.
3. Confirm the supported interface.
4. Verify command syntax.
5. Inspect version-specific behavior.
6. Test the actual implementation.

### 32.2 No speculative interfaces

Never invent:

- CLI arguments.
- IPC events.
- D-Bus methods.
- Service names.
- JSON fields.
- Quickshell APIs.
- Device capabilities.

If the interface cannot be verified, investigate further or explain the uncertainty.

## 33. Dependency Management

Keep dependencies minimal.

Prefer:

- Existing system utilities.
- Existing project services.
- Native capabilities.
- Official interfaces.
- Already-installed tools.

Before introducing a dependency, evaluate:

- Necessity.
- Maintenance status.
- Compatibility.
- Security.
- Resource usage.
- Alternative implementations.

Do not install packages without explicit authorization.

Do not introduce a daemon when an existing service already provides the required functionality.

## 34. Working Autonomy

The agent has balanced autonomy.

It may decide implementation details when they do not alter product behavior or project architecture.

It should consult the user before:

- Introducing significant dependencies.
- Adding persistent background services.
- Changing integration architecture.
- Modifying system configuration.
- Performing destructive or disruptive operations.
- Changing permission requirements.
- Introducing major security implications.

Do not ask for approval for every minor technical decision.

Explain important trade-offs clearly.

## 35. Collaboration With Other Skills

This skill works alongside the other mao-shell development skills.

### 35.1 quickshell-development

Responsible for:

- Quickshell development rules.
- QML and JavaScript implementation.
- Reactive state.
- Application lifecycle.
- General architecture.
- Runtime behavior.

`system-integration` defines how external system capabilities are accessed and managed.

### 35.2 qml-component

Responsible for:

- Reusable visual components.
- Visual behavior.
- Interaction states.
- Accessibility.
- Design system.

System integrations must expose clean interfaces to visual components.

They must not implement presentation logic.

### 35.3 project-validation

Responsible for:

- Project-level validation.
- Validation workflows.
- General quality checks.
- Final project verification.

System integration testing must provide relevant evidence to the validation workflow.

## 36. Integration Development Workflow

Follow this workflow when creating or modifying an integration.

### Step 1: Understand the requirement

Identify:

- The product feature.
- Required system capabilities.
- Expected user actions.
- Data requirements.
- Failure behavior.
- Security implications.

Do not start by selecting a command or library.

### Step 2: Inspect the existing project

Read the required documentation.

Inspect existing services and integrations.

Identify reusable functionality.

Avoid creating duplicate services.

### Step 3: Inspect the environment

Determine:

- Installed tools.
- Available services.
- Versions.
- Permissions.
- Hardware.
- Existing IPC interfaces.

Do not assume a particular configuration.

### Step 4: Select the integration mechanism

Compare available approaches.

Consider native APIs, IPC, D-Bus, events and CLI tools.

Select the most reliable compatible option.

### Step 5: Define the interface

Define:

- Public state.
- Operations.
- Events.
- Error states.
- Data structures.
- Lifecycle ownership.

Ensure that dependent modules do not need implementation-specific details.

### Step 6: Implement discovery

Detect availability and capabilities.

Support unavailable and degraded states.

### Step 7: Implement communication

Use asynchronous communication.

Prefer event-driven updates.

Use polling only when justified.

### Step 8: Implement state management

Validate data.

Normalize shared state.

Preserve useful backend-specific information.

Prevent stale results from overwriting current state.

### Step 9: Implement operations

Validate requests.

Use argument-safe process execution.

Handle concurrency, cancellation and timeouts.

### Step 10: Implement failure handling

Isolate failures.

Add controlled recovery.

Limit retries.

Preserve unrelated functionality.

### Step 11: Implement diagnostics

Integrate with the centralized diagnostic system.

Expose useful operational information.

Redact sensitive data.

### Step 12: Test incrementally

Test each part before proceeding.

Use mocks where useful.

Validate against the real environment when safe.

### Step 13: Document

Document interfaces, dependencies, lifecycle and limitations.

Update relevant project documentation.

### Step 14: Final verification

Review correctness, security, performance and cleanup.

Report tests performed and any remaining limitations.

## 37. Correct Patterns

The following patterns illustrate the expected engineering approach.

### 37.1 Asynchronous execution

Correct:

- Start the process asynchronously.
- Provide explicit arguments.
- Capture output.
- Handle errors.
- Apply a timeout where appropriate.
- Update state only after validating the result.

Incorrect:

- Run a blocking process from a UI interaction.
- Wait synchronously for a service response.
- Assume every command finishes immediately.

### 37.2 Event-driven state

Correct:

- Subscribe to relevant events.
- Maintain one authoritative state owner.
- Update dependent modules when state changes.
- Reconnect safely after interruption.

Incorrect:

- Start a new polling loop for every widget.
- Create duplicate event subscriptions.
- Maintain separate inconsistent copies of service state.

### 37.3 Capability detection

Correct:

- Detect available services.
- Identify supported operations.
- Expose unavailable capabilities.
- Use compatible alternatives.

Incorrect:

- Assume a package is installed.
- Assume a service is active.
- Execute a command without checking its availability.

### 37.4 Controlled recovery

Correct:

- Detect failure.
- Record the reason.
- Apply bounded retries.
- Stop retrying when recovery is not possible.
- Expose the degraded state.

Incorrect:

- Restart a process indefinitely.
- Hide repeated failures.
- Report availability before confirming recovery.

### 37.5 Shared integration ownership

Correct:

- One service owns an integration.
- Consumers use its public interface.
- Operations are coordinated centrally.

Incorrect:

- Every component starts its own process.
- Multiple modules independently modify the same system state.
- Components access private integration internals.

## 38. Anti-Patterns

Avoid the following approaches.

### 38.1 Blocking commands

Do not execute long-running commands synchronously.

### 38.2 Shell command concatenation

Do not construct commands using untrusted strings.

### 38.3 Blind dependency assumptions

Do not assume NetworkManager, PipeWire, BlueZ or any other service is installed.

### 38.4 Excessive polling

Do not repeatedly query the same information without a clear reason.

### 38.5 Duplicate processes

Do not launch multiple monitoring processes for the same resource.

### 38.6 Unbounded retries

Do not retry failed operations indefinitely.

### 38.7 Silent failure

Do not swallow errors or expose misleading default values as real system state.

### 38.8 Fragile parsing

Do not rely on undocumented output formatting.

### 38.9 Global process execution

Do not expose unrestricted command execution to arbitrary UI components.

### 38.10 Uncontrolled system modification

Do not modify system configuration or privileged settings without explicit authorization.

### 38.11 Stale state

Do not allow outdated process results to overwrite newer device or service state.

### 38.12 Unmanaged resources

Do not leave subscriptions, processes or timers running after their owner is destroyed.

## 39. Definition of Done

An integration is not complete merely because its main operation works.

Before considering it complete, verify the following.

### Discovery

- [ ] Required capabilities are detected.
- [ ] Missing dependencies are handled.
- [ ] Unsupported capabilities are exposed correctly.
- [ ] Alternative implementations are handled where applicable.

### Execution

- [ ] All external processes execute asynchronously.
- [ ] Arguments are passed safely.
- [ ] Output is validated.
- [ ] Errors and exit codes are handled.
- [ ] Timeouts and cancellation are supported where applicable.

### State

- [ ] State has a clear owner.
- [ ] Shared data is normalized appropriately.
- [ ] Backend-specific data is preserved where useful.
- [ ] Stale results cannot overwrite current state.
- [ ] External changes are reflected correctly.

### Lifecycle

- [ ] Persistent processes have explicit ownership.
- [ ] On-demand processes terminate correctly.
- [ ] Duplicate processes are avoided.
- [ ] Resources are released.
- [ ] Unexpected termination is handled.

### Recovery

- [ ] Failures are isolated.
- [ ] Recovery is controlled.
- [ ] Retries are bounded.
- [ ] Unrecoverable conditions are reported.
- [ ] Unrelated modules continue functioning.

### Security

- [ ] External data is treated as untrusted.
- [ ] Command injection is prevented.
- [ ] Sensitive information is protected.
- [ ] Permissions are respected.
- [ ] Privileged operations are explicitly authorized.
- [ ] No unauthorized system configuration changes occur.

### Performance

- [ ] No UI-blocking operations exist.
- [ ] Polling is justified.
- [ ] Resource usage is reasonable.
- [ ] No unnecessary processes or subscriptions remain.
- [ ] Long-running behavior has been considered.

### Testing

- [ ] Relevant automated tests pass.
- [ ] Mocks cover important failure conditions.
- [ ] Real integration behavior has been checked when safe.
- [ ] Wayland behavior has been validated where relevant.
- [ ] Recovery behavior has been tested.

### Documentation

- [ ] Public interfaces are documented.
- [ ] Dependencies are documented.
- [ ] Lifecycle behavior is documented.
- [ ] Error and recovery behavior is documented.
- [ ] Limitations are documented.
- [ ] Documentation matches implementation.

## 40. Final Engineering Principles

System integration must make mao-shell feel like a cohesive part of the desktop without compromising the operating system.

Always prioritize:

1. Reliability over convenience.
2. Verified interfaces over assumptions.
3. Asynchronous execution over blocking operations.
4. Events over unnecessary polling.
5. Capability detection over hardcoded dependencies.
6. Clear ownership over duplicated processes.
7. Controlled recovery over infinite retries.
8. Explicit permissions over privilege escalation.
9. Data validation over blind trust.
10. Observable behavior over silent failures.
11. Minimal dependencies over unnecessary infrastructure.
12. Maintainability over premature abstraction.

The final objective is a responsive, secure and resilient desktop shell that integrates deeply with Arch Linux and Wayland while remaining respectful of the user's system, resources and control.