# mao-shell — Component Specifications

**Project:** mao-shell  
**Platform:** Arch Linux  
**Display server:** Wayland  
**Window manager/compositor:** Hyprland  
**Shell framework:** Quickshell  
**Primary language:** QML / JavaScript  
**Initial languages:** Spanish and English  
**Specification status:** Initial consolidated specification

---

# 1. Project Vision

## 1.1 Objective

mao-shell is a custom, modular desktop shell designed for Arch Linux, Wayland and Hyprland, built using Quickshell.

The objective is to create a distinctive desktop environment experience with its own visual identity, interaction model and collection of integrated tools.

mao-shell should not feel like a conventional desktop panel with a collection of unrelated widgets. It should behave as a coherent interactive layer surrounding the desktop.

The shell will include:

- Four interactive perimeter bars.
- Configurable widgets and contextual dashboards.
- A global application and system launcher.
- A general dashboard and Control Center.
- A wallpaper selector.
- A notification system.
- Media controls and audio visualization.
- System monitoring.
- A clipboard history manager.
- File and trash shortcuts.
- A configurable power menu.
- A custom desktop assistant interface.
- A system health and diagnostics module.
- A custom lock screen experience.
- Configuration, profiles and automation management.

The project must be developed incrementally, with a strong emphasis on modularity, reliability, maintainability and visual consistency.

## 1.2 Visual Philosophy

The visual identity of mao-shell is one of its most important aspects.

The design should combine:

- Minimalism in everyday use.
- Experimental and expressive compositions.
- Asymmetric layouts.
- Organic and elastic animations.
- Selective transparency and blur.
- Subtle but distinctive visual details.
- A consistent visual language across all modules.

The shell should feel different from conventional desktop shells.

Caelestia may be used as a visual reference and source of inspiration, but mao-shell must develop its own identity rather than copying another project.

### Design principles

1. Minimalist interaction: avoid unnecessary controls and information.
2. Expressive composition: use asymmetric arrangements and unconventional proportions.
3. Discreet perimeter: bars should not dominate the desktop.
4. Expressive panels: dashboards and extensions can be visually prominent.
5. Organic movement: use smooth, natural and moderately elastic animations.
6. Readability first: experimentation must never compromise usability.
7. Consistency: all modules must share the same visual language.

---

# 2. Development Philosophy and Scope

## 2.1 Incremental Development

mao-shell must be built in small, verifiable stages.

The project must not attempt to implement every module simultaneously.

Recommended development order:

1. Inspect the current system and installed Quickshell capabilities.
2. Establish the project structure and core architecture.
3. Implement the visual design system.
4. Build the four perimeter bars.
5. Implement the widget system and Desktop Edit Mode.
6. Implement shared panels and dashboard infrastructure.
7. Implement core system integrations.
8. Build the Launcher.
9. Add notifications, media, monitoring and other modules.
10. Add advanced settings, profiles and automation.
11. Implement mao Assistant frontend.
12. Add maintenance, diagnostics and installation workflows.
13. Integrate the custom lock screen.
14. Perform integration testing and gradual migration.

Each stage must be independently testable.

## 2.2 MVP Strategy

The first functional version should prioritize the shell foundation rather than every advanced integration.

The initial MVP should include:

- Core Quickshell architecture.
- Four perimeter bars.
- Basic widget framework.
- Visual design system and Wallust integration.
- Desktop Edit Mode.
- Basic Launcher.
- General dashboard.
- Control Center structure.
- Wallpaper selector.
- Core audio, network and Bluetooth integration where available.
- Basic notifications using existing infrastructure.
- Configuration persistence.
- Basic settings interface.

Advanced integrations, assistant backends, automation and extensive maintenance functionality can follow in later phases.

The architecture must anticipate future modules without requiring them to be implemented in the MVP.

## 2.3 Existing Desktop Environment

The existing desktop must remain operational throughout development.

- Do not automatically uninstall Waybar or Rofi.
- Do not disable existing components without explicit authorization.
- Development must be reversible.
- Production startup should only be enabled after successful testing.
- Avoid duplicate services and conflicting desktop components.

---

# 3. Platform and Technical Architecture

## 3.1 Target Environment

- Arch Linux.
- Wayland.
- Hyprland.
- Quickshell.

Do not assume that every optional service or package is installed.

The implementation must inspect the actual environment before selecting integration methods.

## 3.2 Technology

Initial implementation:

- QML for interface and components.
- JavaScript for application logic.
- Quickshell APIs for shell surfaces and system integration.
- Existing Linux services for hardware and desktop functionality.

Avoid introducing C++ unless a demonstrated limitation makes it necessary.

## 3.3 Architectural Separation

Keep the following responsibilities separate:

- Presentation.
- Business logic.
- Configuration.
- System integrations.
- Shared services.
- Module state.
- Diagnostics.

UI components should not directly contain complex system commands or integration logic.

Use shared services for operations that need to be consumed by multiple modules.

## 3.4 System Integration Principles

1. Inspect available services before implementation.
2. Reuse existing infrastructure.
3. Avoid running competing daemons.
4. Avoid duplicating system state.
5. Use real system information rather than simulated values.
6. Handle unavailable capabilities gracefully.
7. Keep integrations replaceable.
8. Verify Quickshell API availability before using an API.
9. Avoid assuming a specific hardware configuration.

## 3.5 Hyprland Integration

Hyprland manages normal application windows.

Quickshell manages mao-shell surfaces.

The shell must:

- Respect monitor geometry.
- Respect reserved screen areas.
- Handle active workspaces and windows.
- Register global shortcuts through Hyprland.
- Integrate safely with the existing Hyprland configuration.
- Avoid overwriting existing configuration.

Any Hyprland configuration changes must be shown to the user and explicitly authorized.

---

# 4. Project Structure

Recommended initial project location:

`~/.config/quickshell/mao-shell/`

The canonical repository structure is defined in `DESIGN_PATTERNS.md` §20. The product areas in this specification map to it as follows:

- Settings interface: `modules/settings/`.
- Hardware metrics and sensors: `services/system/`.
- Typography, spacing and animation timing: tokens in `services/theme/`, not in `components/`.
- Wallpapers: the user's configured directory, not the repository.
- Packages and system updates: `services/updates/`.
- Sounds and translation files: reserved locations; their formats are pending (§34.9 and §37).

This structure may be extended when implementation requires additional separation.

Avoid creating unnecessary abstractions or excessively fragmented files.

---

# 5. Core Shell Architecture

## 5.1 Perimeter UI

mao-shell is built around four interactive perimeter bars:

- Top.
- Bottom.
- Left.
- Right.

These bars form a continuous visual frame around the desktop.

They are not collections of independent cards.

Widgets are integrated into the bar surface and can extend inward into the desktop when additional space is needed.

## 5.2 Bar Visibility

Each bar has an independent visibility setting:

- Always visible.
- Auto-hide.
- Hidden.

Auto-hide may leave a subtle indicator line to reveal the bar's position.

All four bars are enabled by default, even if no widgets are assigned.

## 5.3 Bar Dimensions

- Bar dimensions are fixed by design.
- The top bar may be slightly wider than the other bars.
- Users cannot freely resize bar thickness.
- The bars adapt to resolution, scaling and orientation.
- Extended panels may adapt their dimensions independently.

## 5.4 Reactive Cursor Behaviour

When the pointer approaches a bar:

- The bar may shift a few pixels toward the pointer.
- Elements may react subtly.
- Motion should feel organic and restrained.
- The bar returns smoothly to its resting position when the pointer moves away.

This behaviour must be configurable and respect reduced-motion settings.

## 5.5 Multi-monitor Support

Every connected monitor receives its own four perimeter bars.

Default behaviour:

- Identical visual configuration across monitors.
- Synchronized system indicators.
- Same widget distribution.
- Monitor-specific workspace information.
- Monitor-specific active-window information.
- Automatic adaptation when monitors connect or disconnect.

Respect each monitor's:

- Resolution.
- Orientation.
- Scaling.
- Fractional scaling.

### Optional independent monitor configuration

The architecture should support independent widget layouts per monitor in the future.

This is optional and not required for the MVP.

Default behaviour remains synchronized.

---

# 6. Widget Framework and Desktop Edit Mode

## 6.1 Widget Framework

Widgets are modular components that can be assigned to any perimeter bar.

The framework must support:

- Enable and disable.
- Reordering.
- Moving between bars.
- Drag-and-drop.
- Configurable spacing.
- Reasonable widget sizing.
- Widget-specific settings.
- Automatic persistence.

Widgets must not be implemented as isolated applications.

## 6.2 Default Distribution

Provide a sensible default widget arrangement.

The user should be able to modify it without editing QML files.

## 6.3 Desktop Edit Mode

Desktop Edit Mode allows the user to configure the shell visually.

When enabled:

- Widgets display editing controls.
- Widgets can be moved.
- Widgets can be resized within allowed limits.
- Widgets can be added or removed.
- Bars can be hidden or shown.
- Widget-specific settings become accessible.

When disabled:

- Editing controls disappear.
- The desktop returns to its clean appearance.

## 6.4 Layout Profiles

Support multiple saved widget distributions.

Users can:

- Save layouts.
- Rename layouts.
- Duplicate layouts.
- Delete layouts.
- Restore the default layout.
- Switch between saved layouts.

---

# 7. Panels and Dashboard Behaviour

## 7.1 General Panel System

Panels extend inward from the perimeter bars.

They must appear visually connected to the corresponding bar or indicator.

Panel behaviour:

- Floating surfaces.
- Selective blur.
- Rounded corners.
- Adaptive dimensions.
- Smooth opening and closing.
- Keyboard navigation.
- Focus management.

## 7.2 Contextual Dashboards

Hovering over supported indicators opens contextual dashboards.

Hover is informational only.

It must never trigger actions or change system state.

The pointer must be able to move from the indicator into the dashboard without closing it.

Close the dashboard when the pointer leaves both the trigger and dashboard, using a reasonable delay or tolerance.

## 7.3 Main Panel Navigation

Opening another main module may replace the current main view inside the same panel.

When possible:

- Preserve scroll position.
- Preserve control values.
- Restore previous keyboard focus.

## 7.4 Closing Behaviour

- Escape closes the active panel or subpanel.
- Clicking outside closes the active panel.
- Closing behaviour must be consistent across modules.

## 7.5 Subpanels

Subpanels open laterally from the main panel.

A maximum of two simultaneous subpanels is allowed.

When a third subpanel is opened, close the oldest one.

Provide a visible back-navigation control.

---

# 8. Visual Design System

## 8.1 Design Tokens

Use semantic design tokens rather than hardcoded colors.

Examples:

- `surface`
- `surface-elevated`
- `surface-transparent`
- `text-primary`
- `text-secondary`
- `border-subtle`
- `accent`
- `accent-muted`
- `warning`
- `error`
- `success`

## 8.2 Wallust Integration

Wallust is the primary source of automatic colors.

The shell must:

- Read the generated palette.
- Apply semantic colors consistently.
- Update colors when the wallpaper changes.
- Animate palette transitions.
- Allow per-module overrides.

Wallust availability must be checked during system inspection.

The palette is regenerated by the user's existing wallpaper-change script, which runs Wallust at the same moment the wallpaper changes. Whether mao-shell triggers regeneration itself is decided in Phase 1. Colours come only from Wallust and the wallpaper; no colour is hardcoded.

## 8.3 Custom Palettes

Users can:

- Save custom palettes.
- Create palettes.
- Rename palettes.
- Duplicate palettes.
- Delete palettes.
- Preview palettes.
- Select saved palettes manually.
- Return to automatic Wallust colours.

Palettes must be independent of wallpapers.

## 8.4 Transparency and Blur

- Default values are configurable.
- Global blur settings are available.
- Individual modules may override blur and transparency.
- Bars and extended panels have independent settings.
- Use selective blur rather than applying blur indiscriminately.

## 8.5 Window Effects

Window blur and transparency are configured through Hyprland rules.

mao-shell may provide a UI for managing these rules.

Only the following per-application effects are included:

- Blur enabled or disabled.
- Transparency level.

Other effects remain global.

The shell should detect running applications and allow adding them to custom rules.

The Hyprland window class should be identified automatically when possible.

## 8.6 Typography

Use a clear typographic hierarchy.

- Expressive headings.
- Readable body text.
- Monospaced numerals where useful.
- Consistent spacing.
- Adaptive density.

## 8.7 Iconography

The initial release uses a consistent custom icon style.

- Active and inactive variants.
- Warning and error variants.
- Wallust-compatible accent colours.
- No icon-pack selector in the initial release.

Future customization may be added.

## 8.8 Accessibility

Provide:

- Compact, normal and enlarged density modes.
- Independent font scaling.
- Independent UI element scaling.
- High contrast mode.
- Reduced motion.
- Disabled animations.
- Keyboard navigation.
- Visible focus indicators.
- Accessible labels for controls and icons.
- Screen reader compatibility where supported by QML and the Wayland environment.

---

# 9. Perimeter Bar Widget Distribution

## 9.1 Top Bar

Suggested layout:

**Left**
- System update indicator.
- Power menu.

**Center**
- Hyprland workspaces.
- Active window title.

**Right**
- CPU usage.
- RAM usage.
- GPU usage.
- Microphone and volume.
- Bluetooth.
- Network.
- Clock and calendar.
- Access to integrated panels.
- Additional configurable widgets.

mao Assistant may also be placed on a bar.

## 9.2 Bottom Bar

Suggested widgets:

- Media player.
- Audio visualizer.
- Additional configurable widgets.

## 9.3 Left and Right Bars

Both sides support configurable widgets.

The right bar also hosts:

- Notification indicator.
- Notification extensions.
- System tray.
- Additional configurable widgets.

## 9.4 Workspaces

Workspaces should use rounded pills or segments.

Display:

- Active workspace.
- Occupied workspaces.
- Empty workspaces.

Requirements:

- Smooth transitions.
- Click to switch.
- Dynamic workspace support.
- Special workspace support.
- Monitor-aware workspace information.

## 9.5 Active Window Title

- Display only when space permits.
- Truncate intelligently.
- Hide when the bar becomes crowded.

---

# 10. Resource Monitoring

## 10.1 Bar Indicators

Show compact values for:

- CPU usage.
- RAM usage.
- GPU usage.

## 10.2 Performance Dashboard

Include:

### CPU
- Overall utilization.
- Per-core utilization.
- Temperature.

### Memory
- Used memory.
- Available memory.
- Total memory.

### GPU
- Utilization.
- Temperature.
- VRAM usage.

### Additional sensors
- CPU fan RPM, if available.
- GPU fan RPM, if available.
- Other useful hardware sensors when reliable.

Unavailable metrics must be hidden rather than displaying fabricated values.

Inspect hardware and available sensor interfaces before implementation.

---

# 11. Audio and Microphone

## 11.1 Integration

Detect the current audio stack.

Prefer PipeWire and WirePlumber when available.

Adapt to PulseAudio or another installed stack when necessary.

Do not create competing audio services.

## 11.2 Bar Behaviour

- Speaker indicator.
- Microphone indicator.
- Real-time state.
- Click speaker to toggle output mute.
- Click microphone to toggle input mute.

Hover opens a combined audio dashboard.

## 11.3 Audio Dashboard

Include:

- Output volume.
- Input volume.
- Output mute.
- Microphone mute.
- Output device selection.
- Input device selection.
- Selected device names.

Use a circular volume dial rather than a conventional slider for the primary output volume control.

Hover must never change audio state.

---

# 12. Audio Visualizer

## 12.1 General Behaviour

The bottom bar includes an optional system-wide audio visualizer.

It should be inspired by CAVA but have its own visual identity.

## 12.2 Audio Source

Capture system output audio rather than relying on a specific application.

Prefer PipeWire integration when available.

Do not create competing audio capture services.

## 12.3 Visual Style

Default:

- Asymmetric layout.
- Fluid wave-like movement.
- Subtle appearance.
- Wallust-compatible colours.

Configurable:

- Visualization style.
- Sensitivity.
- Height.
- Intensity.
- Animation behaviour.

The visualizer remains idle when no sound is playing.

It can be disabled without hiding the bottom bar.

Use the native Quickshell PipeWire integration when it provides the required audio data. CAVA is used only if native capture is insufficient, verified in Phase 3. The visualizer stops capturing when it is hidden or when no audio is playing.

---

# 13. Media Player

## 13.1 Integration

Use MPRIS as the primary interface.

Support compatible players such as:

- Spotify.
- Firefox.
- VLC.
- Other MPRIS-compatible applications.

Do not implement Spotify-specific integration as the primary architecture.

## 13.2 Bar Widget

Display:

- Track title.
- Artist.
- Small cover image.
- Playback state.
- Source application icon.

## 13.3 Interaction

Click playback controls to pause or resume.

Hover opens the media dashboard.

## 13.4 Media Dashboard

Display:

- Larger cover art.
- Track title.
- Artist.
- Previous.
- Play/pause.
- Next.

When multiple players are active, allow selecting the active player.

Prioritize the most relevant active player.

The media widget and audio visualizer must operate simultaneously.

If technically supported, clicking the cover may open the source application.

---

# 14. Network

## 14.1 Detection

Inspect the installed network management infrastructure.

Do not assume NetworkManager or systemd-networkd.

Reuse the existing manager.

## 14.2 Contextual Dashboard

Display:

- Connection status.
- Connection type.
- Wi-Fi SSID.
- Local IP address.
- Wi-Fi signal strength.
- Network traffic or connection speed.

## 14.3 Control Center

Provide:

- Wi-Fi enable/disable.
- Ethernet status.
- VPN status and controls.

## 14.4 Wi-Fi View

Show:

- Available networks.
- Current connection.
- Connected network details.
- Active versus available state.

All displayed states must reflect the actual system.

---

# 15. Bluetooth

## 15.1 Integration

Reuse the existing Bluetooth infrastructure.

Prefer BlueZ when available.

Do not create another Bluetooth daemon.

## 15.2 Bar Behaviour

- Hover opens contextual information.
- Click opens the Bluetooth dashboard.
- Clicking the indicator must not directly toggle Bluetooth.

## 15.3 Contextual Dashboard

Display:

- Bluetooth enabled/disabled state.
- Connected devices.
- Device names.
- Device types.
- Connection status.
- Battery level when available.

## 15.4 Bluetooth Management

The Control Center must support:

- Enable/disable Bluetooth.
- Connect devices.
- Disconnect devices.
- Pair devices.
- Remove paired devices.
- Inspect device details.

## 15.5 Pairing

Support authentication methods provided by the device and Bluetooth stack:

- PIN.
- Passkey.
- Numeric comparison.
- Authorization.
- Confirmation.

Use shell-native pairing dialogs where infrastructure permits.

Explain the requested action clearly.

## 15.6 Device Categories

Organize devices into:

- Audio: headphones, headsets, speakers.
- Input: keyboards, mice, game controllers.
- Other: phones and miscellaneous devices.

Use category-specific icons and states.

The category model must be extensible.

---

# 16. Clock, Calendar and Weather

## 16.1 Clock

Display time in the top bar.

Use locale-aware formatting.

## 16.2 Contextual Dashboard

Include:

- Prominent clock.
- Date.
- Calendar.
- Weather card.

## 16.3 Weather Provider

Use Open-Meteo initially.

The provider must be isolated behind a replaceable service interface.

Do not require an API key for the intended non-commercial use.

## 16.4 Location Resolution

Use this priority:

1. Automatic approximate IP geolocation.
2. Last successfully resolved location if automatic detection fails.
3. Manual configuration.
4. Manual city or coordinates.

Store the resolved location locally to avoid repeated lookups.

IP geolocation is approximate and may be inaccurate when using a VPN.

Automatic IP geolocation is enabled by default. The user can disable it, in which case the manual options apply.

Do not assume a GPS or desktop geolocation service exists.

## 16.5 Weather Information

Display:

- Current temperature.
- Feels-like temperature.
- Weather condition and icon.
- Daily high and low.
- Precipitation probability.
- Hourly forecast.
- Five-day forecast.

## 16.6 Refresh and Cache

- Periodic refresh.
- No network request every time the dashboard opens.
- Cache valid results.
- Show loading and error states.
- Retain the last valid data while offline.
- Indicate when retained data is stale.

Exact refresh interval and cache lifetime should be selected during implementation.

---

# 17. System Updates

## 17.1 Sources

Check:

- Official Arch repositories.
- AUR packages through the installed helper.

The AUR helper is yay. Detect its presence before use. The periodic check does not require root. Applying updates requires root and runs only when the user starts it in a terminal. The non-root check method is verified in Phase 3.

Do not introduce another package manager.

## 17.2 Bar Indicator

Always display the package update icon.

States:

- Neutral/green: no pending updates.
- Amber: updates available.
- Red: check failed.

Display the number of pending updates.

Avoid double-counting packages.

Hover shows the age of the newest available update when reliable.

Click opens a terminal for manual review and update.

## 17.3 Update Age

For official repositories, use package build dates when reliable.

For AUR packages, use available package metadata timestamps.

Do not present unreliable timestamps as exact publication dates.

If no reliable timestamp exists, show update availability without inventing an age.

## 17.4 Behaviour

- Check every three hours.
- Store the last valid result.
- Store the last check timestamp.
- Avoid unnecessary repeated checks.
- Retain the last valid result if a check fails.
- Clearly distinguish failed checks from no pending updates.
- Never download or install OS updates automatically.

---

# 18. Power Menu

Include:

- Suspend.
- Reboot.
- Shutdown.
- Log out.

Behaviour:

- Suspend executes directly.
- Reboot requires a shell-native confirmation dialog.
- Shutdown requires a shell-native confirmation dialog.
- Logout is available as an action.
- Lock screen is a separate quick action.

Hover must never execute destructive actions.

---

# 19. Application Launcher and Global Search

## 19.1 General Design

The Launcher replaces Rofi gradually.

Appearance:

- Centered floating window.
- Initial width around 45% of the screen.
- Responsive and configurable dimensions.
- Compact initial state.
- Expand when entering specialized modes.
- Translucent background.
- Selective blur.
- Smooth animations.

Focus the search field immediately.

Keyboard navigation must be supported.

Escape closes the Launcher from every mode.

## 19.2 Application Search

Display applications immediately when opening the Launcher.

Requirements:

- Most-used applications first.
- Usage-frequency tracking.
- Live filtering.
- Application icon.
- Name.
- Description when available.
- Launch on selection.
- Close Launcher after launching.
- Empty state when no results exist.

Do not create a separate favorites section in the initial design.

## 19.3 File Search

Prefix:

`f`

Search the user's home directory and subdirectories.

Include hidden files and directories by default, such as:

- `.config`
- `.bashrc`
- `.ssh`

Use:

- Substring matching.
- Fuzzy matching.
- Relevance ordering.
- Live results.

Open files with their default application.

Provide a clear empty state.

## 19.4 Calculator Mode

Prefix:

`c`

Entering calculator mode changes the Launcher into a dedicated calculator interface.

Supported:

- Basic arithmetic.
- Parentheses.
- Percentages.
- Powers.

Show expression and result.

Enter displays the result inside the Launcher.

Do not automatically copy results to the clipboard.

Errors must be concise and must not close the Launcher.

## 19.5 Command Mode

Prefix:

`!`

Display a simplified terminal-like interface inside the Launcher.

This is not a persistent shell session.

Each command executes as an independent process.

Display:

- Standard output.
- Standard error.
- Exit code.

Support:

- Consecutive commands.
- Per-session command history.
- Up/down history navigation.
- ANSI colours where feasible.

Do not persist:

- Shell variables.
- Working directory.
- Session state between commands.

Do not require an external terminal to display command results.

### Safety

Only commands in an explicit allow-list may run. Free-form command execution is not supported. Commands outside the allow-list are rejected with an explanation. Each allowed command runs through controlled process handling.

Never concatenate arbitrary user input directly into an unsafe shell command string.

Interactive and long-running command behaviour must be defined during implementation.

## 19.6 Global Local Search

The Launcher is a local global search interface.

Search categories include:

- Installed applications.
- Files and directories.
- mao-shell settings.
- mao-shell modules.
- System actions.
- Connected devices.
- Clipboard history.
- Wallpaper and shell features.

Do not include web search results.

Internet search belongs to mao Assistant.

## 19.7 Settings Search

Search results can directly open the corresponding mao-shell settings section.

Examples:

- Bluetooth.
- Transparency.
- Animations.
- Notifications.
- Shortcuts.
- Accessibility.

## 19.8 Quick Actions

Allow direct actions through search keywords.

Examples:

- Shutdown.
- Reboot.
- Suspend.
- Lock screen.
- Empty trash.
- Toggle Do Not Disturb.
- Enable/disable Bluetooth.
- Enable/disable Wi-Fi.
- Change audio devices.
- Open a module or settings page.

Reversible actions may execute directly.

Destructive or sensitive actions require confirmation.

Operations requiring permissions must request explicit authorization.

## 19.9 Custom Commands and Actions

Allow users to create custom actions.

Each action may define:

- Name.
- Description.
- Keyword.
- Icon.
- One or more steps.
- Optional keyboard shortcut.

Support chained actions.

Example:

`gaming`

May:

- Open Steam.
- Activate a visual profile.
- Launch selected applications.
- Prepare a gaming environment.

Use restricted permissions by default.

Sensitive actions require authorization.

## 19.10 Search Behaviour

The Launcher must prioritize local results and remain fast.

No web results should be mixed with local results.

mao Assistant is responsible for external information and web searches.

---

# 20. Wallpaper Selector

## 20.1 Appearance

A large, near-fullscreen panel.

Include:

- Image grid.
- Search.
- Filenames.
- Larger hover preview.

## 20.2 Wallpaper Source

Provide one configurable root directory.

Example:

`~/Pictures/Wallpapers/`

Scan recursively through all subdirectories.

Display supported images in one unified gallery.

Avoid duplicate entries.

Invalid or inaccessible images must not prevent the selector from working.

## 20.3 Applying Wallpapers

A single click applies the selected wallpaper to all monitors.

No additional confirmation or Apply button.

The Wallust palette is regenerated by the existing wallpaper-change script at the same moment the wallpaper changes (see §8.2).

No per-monitor wallpaper selection in the initial design.

## 20.4 Thumbnail Cache

Generate and persist thumbnails.

Reuse cached thumbnails when opening the selector.

Regenerate thumbnails when the original image changes.

Hover displays a larger preview.

Hover never applies a wallpaper.

---

# 21. General Dashboard and Control Center

Both views share the same integrated panel infrastructure.

## 21.1 Access

- Dedicated button on the right bar.
- Global Hyprland shortcut.
- Exact shortcut selected after inspecting existing bindings.

## 21.2 Panel Design

- Floating lateral panel.
- Near the access button.
- Detached from screen edges.
- Stable base dimensions.
- Responsive to resolution and scaling.
- Asymmetric layout.
- Selective blur.
- Rounded corners.
- Transparency.
- Fixed left navigation sidebar.
- Main content on the right.

The panel must not resize unpredictably based on its content.

## 21.3 Navigation

- Lateral navigation between main views.
- Click opens a subpanel.
- Hover previews information only.
- Visible back arrow.
- Escape closes active panel or subpanel.
- Preserve scroll and control values where feasible.

Maximum two simultaneous subpanels.

Opening a third closes the oldest.

## 21.4 General Dashboard

Include:

### Main information
- Large clock.
- Date.
- Calendar.
- Weather.

### System information
- Performance summary.
- Network state.
- Connected devices.
- Hyprland workspaces.
- Active window.

### Quick actions
- Lock screen.
- Screenshot.
- Do Not Disturb.
- System settings shortcut.

No quick light/dark toggle.

Wallust controls the automatic palette.

## 21.5 Control Center

Include:

### Network
- Connection status.
- Wi-Fi.
- Ethernet.
- VPN.

### Audio
- Output volume.
- Microphone.
- Input/output device selection.

### Bluetooth
- Bluetooth state.
- Connected devices.
- Device management.

### Display
- Brightness when supported.

### System
- System settings shortcuts.
- Battery and power information when applicable.

Each main block may open a lateral subpanel.

## 21.6 Brightness

Provide a brightness slider when compatible hardware and system interfaces are available.

Hide unsupported controls.

## 21.7 Battery

Only show the battery block when a compatible battery exists.

Include:

- Battery percentage.
- Charging state.
- Estimated remaining time when available.
- AC connected/disconnected state.
- Low battery alerts.

Hide the block on desktops without a battery.

## 21.8 System Settings

mao-shell-specific settings must live inside mao-shell.

System-wide Linux settings should open installed native settings applications.

Do not create a replacement for every system configuration utility.

---

# 22. Notifications

## 22.1 Integration

Inspect the existing notification daemon and infrastructure.

Reuse existing notification services whenever possible.

Do not run competing notification daemons.

## 22.2 Presentation

Notifications appear at the bottom of the right perimeter bar.

They expand inward as temporary extensions of the bar.

The design must remain visually integrated with mao-shell.

## 22.3 Grouping

Display one notification card per application.

All notifications from the same application are grouped.

Cards can expand to show individual notifications.

Preserve application-provided notification actions.

## 22.4 Notification Timing

Default behaviour:

- Normal notifications: five seconds.
- Important notifications: ten seconds.
- Persistent notifications: remain until withdrawn by the application or manually closed.

Hover pauses the timer.

Priority classification must be based on reliable notification metadata.

## 22.5 Notification Indicator

Keep a permanent notification indicator in the right bar.

Include:

- Bell icon.
- Visual indicator when unread notifications exist.
- Counter for unread notifications.

Click opens the notification panel.

The indicator updates when notifications are reviewed.

## 22.6 History

Provide notification history inside the integrated panel.

Group history by application.

Allow cards to expand.

Retain history for seven days, then automatically delete expired entries.

## 22.7 Configuration

Allow:

- Enabling/disabling notification presentation.
- Configuring sound.
- Configuring notification behaviour.
- Reviewing notification history.

---

# 23. System Tray

Integrate a System Tray into the perimeter UI.

Use StatusNotifierItem/AppIndicator compatibility where available.

Support:

- Application icons.
- Contextual menus.
- Application-provided states.
- Configurable icon visibility.
- Configurable placement on any bar.

The tray must visually integrate with the perimeter bars.

Do not create a competing tray service.

---

# 24. Screenshot Integration

## 24.1 Existing Screenshot Tool

Reuse the screenshot tool already installed on the system.

The selected tool is flameshot. It is installed, as confirmed in the package database.

Do not implement a new screenshot engine unless there is a clear technical reason.

Preserve existing functionality such as:

- Region selection.
- Drawing and annotations.
- Colour tools.
- Clipboard copying.

## 24.2 Integration

Provide:

- Global shortcut.
- Dashboard access.
- Configurable screenshot destination folder.
- Shortcut to open the screenshot folder.

Allow changing the destination folder from mao-shell settings.

---

# 25. Files and Trash

Add a lightweight Files section to the dashboard.

Include:

- Recently opened or modified files when reliable system information is available.
- Trash access.
- Restore deleted files.
- Permanently delete files.
- Open file location.
- Open the default file manager.

Reuse the installed file manager and desktop infrastructure.

Do not build a complete file manager.

---

# 26. Clipboard History

## 26.1 Functionality

Provide a clipboard history manager.

Support:

- Text.
- Images.
- Preview.
- Search.
- Copy an existing item again.
- Delete individual entries.
- Clear history.
- Configurable history limit.
- Enable/disable history collection.

## 26.2 Integration

Inspect the existing clipboard infrastructure.

Avoid duplicate clipboard managers.

## 26.3 Privacy

Store history locally.

Provide controls to pause or disable recording.

Avoid collecting passwords and private content whenever supported by the clipboard infrastructure.

Sensitive data handling must be treated as an important requirement.

---

# 27. mao Assistant

## 27.1 Objective

mao Assistant is an integrated assistant interface designed to support future agents such as Hermes, OpenCode and other compatible backends.

The first implementation must focus on the frontend and architecture.

Do not couple mao-shell directly to a specific agent.

## 27.2 Initial Scope

Build:

- Chat interface.
- Conversation history.
- Text input.
- Voice input.
- Message rendering.
- Activity indicators.
- Stop/cancel controls.
- Expandable task interface.
- Backend abstraction layer.

Real agent integration is postponed.

## 27.3 Interface

The assistant opens as a floating lateral panel.

Access through:

- Widget on any configured bar.
- Configurable global shortcut.

The panel:

- Overlays existing windows.
- Does not resize application windows.
- Can remain open while working.
- Closes with Escape, outside click or close button.

## 27.4 Conversation Features

Support:

- Persistent conversation history.
- Markdown.
- Code blocks.
- Structured information.
- Streaming when supported by the backend.
- Activity indicator.
- Stop execution.

## 27.5 Voice Input

The initial voice implementation is input-only.

Behaviour:

1. Activate microphone using a global shortcut or voice button.
2. Capture speech.
3. Transcribe speech into text.
4. Automatically submit the transcription to the agent when capture ends.
5. Display the transcription in conversation history.

Allow the user to stop or cancel capture.

The transcription engine must be independent of the agent backend.

## 27.6 Voice Output

No text-to-speech response generation in the initial release.

The assistant responds through text.

## 27.7 Backend Architecture

Define an internal API abstraction and adapter interface.

Future integrations may support:

- Hermes.
- OpenCode.
- Other compatible agents.

Backend connection details, endpoints, authentication and transport protocols will be decided in a later development phase.

## 27.8 Future Capabilities

The architecture should allow future agents to:

- Open applications.
- Search files.
- Execute commands.
- Inspect system information.
- Modify system settings.
- Control mao-shell modules.
- Use external tools.

## 27.9 Future Security Requirements

- Restricted permissions by default.
- Sensitive actions require explicit authorization.
- Show actions in progress.
- Allow stopping tasks.
- Authenticated connections.
- Distinguish proposed, authorized and executed actions.

These capabilities are not part of the initial frontend implementation.

---

# 28. Profiles and Automations

## 28.1 Profile Categories

Support:

### Visual profiles
Examples:

- Minimal.
- Glass.
- Balanced.
- Compact.
- Custom.

### Bar distribution profiles

Save:

- Active widgets.
- Widget ordering.
- Widget placement.
- Bar visibility.
- Spacing and layout.

### Complete desktop profiles

Combine:

- Visual appearance.
- Bar layout.
- Enabled modules.
- Behaviour.
- Dashboard configuration.
- Other supported preferences.

## 28.2 Profile Management

Allow:

- Create.
- Rename.
- Duplicate.
- Delete.
- Switch.
- Restore defaults.

Profiles must not overwrite one another unexpectedly.

## 28.3 Activity Profiles

Example profiles:

- Gaming.
- Work.
- Study.
- Relax.

Profiles are user-defined configurations.

Switching profiles is initially manual.

## 28.4 Application-based Automation

The architecture should support optional automatic profile switching based on running applications.

Examples:

- Steam activates Gaming.
- VS Code activates Development or Work.

Users configure:

- Trigger application.
- Target profile.
- Whether to restore the previous profile after the application closes.
- Rule priority.
- Enabled/disabled state.

Automatic switching must be disabled by default.

The user must explicitly configure and enable each rule.

---

# 29. Global Shortcuts

## 29.1 Integration

Hyprland registers global shortcuts.

mao-shell provides a visual shortcut manager.

## 29.2 Management

Users can:

- View shortcuts.
- Change shortcuts.
- Detect conflicts.
- Review existing bindings.

Do not silently modify Hyprland configuration.

Show proposed changes and request authorization.

## 29.3 Suggested Initial Shortcuts

These are proposals, not final bindings:

| Action | Suggested shortcut |
|---|---|
| Launcher | Super + Space |
| mao Assistant | Super + A |
| Control Center | Super + C |
| Wallpaper selector | Super + W |
| Power menu | Super + X |
| Desktop Edit Mode | Super + Shift + E |

Inspect the actual Hyprland configuration before selecting final bindings.

Additional shortcuts may include screenshot capture, voice capture and notification history.

---

# 30. Lock Screen

## 30.1 Objective

Provide a custom lock screen experience matching mao-shell's visual identity.

Do not confuse the login manager with the session locker.

SDDM remains the login manager.

Use an appropriate secure Wayland/Hyprland-compatible locking mechanism.

Do not implement authentication security from scratch.

## 30.2 Design

The lock screen uses one fixed design initially.

Minimalist appearance:

- Large clock.
- Date.
- Current weather.
- Current wallpaper.
- Configurable wallpaper blur.
- Password entry.
- Unlock action.
- Discreet battery and system indicators where appropriate.

No advanced layout customization in the initial version.

## 30.3 Security

The locking mechanism must securely prevent interaction with the desktop.

Reuse the system authentication mechanism.

The shell must never expose the desktop while authentication is pending.

## 30.4 SDDM

Keep the existing SDDM configuration initially.

Visual refinements to match mao-shell may be considered later.

This is low priority.

---

# 31. Sound and Do Not Disturb

## 31.1 Interface Sounds

The only initial interface sound is an optional notification sound.

Do not add sounds for:

- Opening menus.
- Launcher interaction.
- Changing profiles.
- Widget interaction.
- Navigation.

Notification sound must be configurable and fully disableable.

Different sounds by application or priority may be considered later.

## 31.2 Do Not Disturb

Support:

- Manual enable/disable.
- Activation from bar, dashboard and Launcher.
- Scheduled hours.
- Temporary activation for a selected duration.
- Visible active-state indicator.
- Persistent configuration.

Allow configurable exceptions for applications or important notifications.

---

# 32. System Health and Diagnostics

## 32.1 Health Indicator

Display a discreet system health indicator.

Notify the user about relevant issues.

Examples:

- Required service stopped.
- Insufficient disk space.
- Repeated module failures.
- Broken integration.
- Quickshell errors.
- Problems with Hyprland or other required components.

## 32.2 Diagnostics Dashboard

Display:

- Module status.
- Detected services.
- Installed and missing dependencies.
- Recent errors.
- Quickshell version.
- mao-shell version.
- Integration status.
- Diagnostic checks.

Allow:

- Manual checks.
- Viewing logs.
- Exporting diagnostic reports.
- Restarting individual modules when feasible.

## 32.3 Safety

The health system is primarily observational.

It must not automatically restart services or change system configuration without authorization.

---

# 33. Usage Statistics

Record local usage statistics to improve the experience.

Possible information:

- Most-used applications.
- Frequently opened modules.
- Frequently used widgets.
- Frequent actions and commands.

Use statistics to improve Launcher ordering.

Provide a statistics view with configurable periods.

Allow:

- Disabling collection.
- Clearing history.
- Reviewing recorded information.

All statistics remain local.

No telemetry or external data transmission.

---

# 34. Settings Architecture

## 34.1 Centralized Configuration

Use JSON as the main configuration format.

Keep configuration separate from presentation and logic.

Configuration must be:

- Versioned.
- Validated.
- Automatically persisted.
- Organized by responsibility.

Use separate files or logical sections for modules and profiles.

## 34.2 Basic Settings

Keep common options accessible.

Examples:

- Appearance.
- Profiles.
- Bars and widgets.
- Notifications.
- Sound.
- Shortcuts.
- Language.
- Accessibility.
- Wallpaper.
- Palettes.

## 34.3 Advanced Settings

Include:

- Detailed module configuration.
- System service integrations.
- Per-application window rules.
- Automations.
- Custom commands.
- Diagnostics.
- Logs.
- Maintenance.
- Backup management.
- Import/export.

## 34.4 Auto-save

Preferences should be saved automatically.

Avoid requiring users to manually save every change.

## 34.5 Profiles

Support visual, bar distribution and complete desktop profiles.

## 34.6 Backup and Restore

Provide:

- Automatic periodic backups.
- Backup before important changes.
- Configuration history.
- Restore previous versions.
- Import/export JSON.
- Backup before importing.
- Validation before restoration.

Allow configuring the maximum number of stored backups.

## 34.7 Theme Configuration

Allow:

- Wallust automatic palette.
- Custom saved palettes.
- Per-module colour overrides.
- Transparency.
- Blur.
- Corners.
- Borders.
- Shadows.
- Animations.
- Spacing.
- Density.

## 34.8 Accessibility Configuration

Provide:

- Compact.
- Normal.
- Enlarged.
- Independent font scaling.
- Independent element scaling.
- High contrast.
- Reduced motion.
- Disabled animations.

## 34.9 Internationalization

Initial languages:

- Spanish.
- English.

Behaviour:

1. Detect system language.
2. Use it if supported.
3. If unsupported, use Spanish.
4. Allow manual selection.
5. Persist user preference.

Keep text separate from QML.

Use JSON translation files loaded through the configuration service. Compiled translation formats are not used (AGENTS §18).

Support locale-aware dates, times and regional formatting.

Architecture must allow future languages.

---

# 35. Installation and Maintenance

## 35.1 Scripts

Provide:

- `install.sh`
- `uninstall.sh`
- `update.sh`
- `check.sh`

## 35.2 Installation Principles

- User-scope installation by default.
- Avoid unnecessary root access.
- Create backups before important changes.
- Never silently overwrite Hyprland configuration.
- Safe and idempotent.
- Dependency and capability checks.
- Useful dry-run support.
- Validate before activation.
- Avoid inconsistent states after failure.

## 35.3 First-run Setup Assistant

The first-run assistant runs entirely in the terminal.

It must:

1. Check dependencies.
2. Detect services.
3. Detect hardware and integrations.
4. Select an initial visual profile.
5. Configure weather location.
6. Review shortcuts.
7. Generate initial configuration.
8. Validate before activation.

## 35.4 Re-running Setup

The assistant can be executed again later.

It should be accessible through advanced settings.

Allow:

- Full setup review.
- Selective integration checks.
- Hardware re-detection.
- Dependency checks.
- Configuration review.

Before modifying existing settings, show the planned changes and request authorization.

## 35.5 Development Mode

Development mode should provide:

- Detailed logs.
- Fast reload.
- Debugging support.
- Development-specific checks.

## 35.6 Production Mode

Production mode should provide:

- Normal session startup.
- Stable behaviour.
- Restrained logs.
- No unnecessary debugging overhead.

## 35.7 Startup

mao-shell should eventually start automatically with Hyprland.

During development, it runs manually.

Do not enable automatic startup until validated.

Avoid duplicate instances.

Keep Waybar and Rofi available until migration is approved.

## 35.8 Shell Updates

Check for new mao-shell versions every 24 hours.

When a new version is available:

- Show a discreet notification.
- Display installed version.
- Display available version.
- Show changelog.
- Provide version history.
- Allow manual update.

Never automatically download or install shell updates.

## 35.9 Safe Update Process

Before updating:

1. Create a backup.
2. Preserve user configuration.
3. Validate dependencies.
4. Validate updated files.
5. Track installed version.
6. Provide rollback if update fails.

OS package updates are separate and checked every three hours.

---

# 36. Migration Strategy

The existing desktop environment must remain usable throughout development.

Migration should be gradual:

1. Develop mao-shell independently.
2. Run it manually alongside existing components.
3. Test modules individually.
4. Validate system integrations.
5. Test multi-monitor behaviour.
6. Enable startup only after validation.
7. Replace existing components progressively.
8. Preserve a rollback path.

Never uninstall Waybar or Rofi automatically.

---

# 37. Pending Technical Decisions

The following details must be investigated or selected during implementation.

They should not block the initial architectural work. Decisions already taken are recorded in §41 and are no longer pending.

## Weather
- Exact dashboard layout.
- Refresh interval.
- IP geolocation provider.
- Cache lifetime and storage.

## Updates
- Terminal selection.
- Exact official repository and AUR check implementation.
- Reliable metadata sources.

## Launcher
- File type filtering and metadata.
- Calculator edge cases.
- Interactive and long-running command behaviour.

## Wallpapers
- Invalid image handling.
- Thumbnail cache implementation.

## Dashboards
- Exact contextual dashboard content.
- Exact Control Center subpanel hierarchy.
- Native system settings shortcuts.

## Notifications
- Priority classification.
- Exact expansion behaviour.
- Existing daemon integration.

## System integration
- Hardware metrics source.
- Network manager.
- Audio stack.
- Bluetooth infrastructure.
- Notification daemon.
- Clipboard manager.
- Screenshot tool.
- Existing Hyprland configuration.

## Focus and Wayland
- Focus restoration.
- Reserved areas.
- Surface interaction.
- Lock screen integration.
- Multi-monitor behaviour.

## Assistant
- Actual backend connection.
- Authentication.
- API transport.
- Transcription engine.

## Profiles and automation
- Exact automation rule structure.
- Conflict resolution.
- Profile restoration behaviour.

---

# 38. Acceptance Criteria

The implementation is considered successful when the following requirements are met.

## Core architecture
1. The project runs on Arch Linux, Wayland, Hyprland and Quickshell.
2. Modules are independently organized.
3. Module failures do not bring down the entire shell.
4. Services are reused rather than duplicated.
5. The shell supports development and production modes.
6. Configuration is persistent and versioned.

## Perimeter bars
7. Four perimeter bars are available.
8. All bars are enabled by default.
9. Bars support independent visibility settings.
10. Bar dimensions remain fixed.
11. Cursor-reactive animation is configurable.
12. Widgets can move between bars.
13. Widgets can be reordered.
14. Desktop Edit Mode provides editing controls.
15. Layout profiles can be saved and restored.
16. Multi-monitor support works.
17. Workspaces and active windows are monitor-aware.
18. Scaling and fractional scaling are respected.

## Panels and dashboards
19. Panels extend inward from perimeter bars.
20. Panels use consistent animations and visual language.
21. Escape closes active panels.
22. Clicking outside closes active panels.
23. Hover dashboards do not trigger actions.
24. The pointer can move from triggers into dashboards.
25. Main views share panel infrastructure.
26. Subpanels support back navigation.
27. Maximum two subpanels can coexist.
28. Opening a third closes the oldest.
29. Panel state is preserved where feasible.

## Visual identity
30. Wallust palette integration works.
31. Custom palettes can be saved.
32. Per-module visual overrides are supported.
33. Blur and transparency are configurable.
34. Reduced-motion mode is supported.
35. High contrast is supported.
36. Iconography is consistent.
37. The shell retains its distinctive asymmetric design.

## Monitoring and integrations
38. CPU, RAM and GPU indicators show real values.
39. Detailed monitoring displays supported temperatures and sensors.
40. Audio and microphone states are accurate.
41. Audio devices can be selected.
42. Network information reflects the actual connection.
43. Bluetooth devices can be inspected and managed.
44. Unsupported hardware features are hidden.
45. Battery information is conditional on hardware availability.

## Audio and media
46. The audio visualizer reacts to system-wide output.
47. The visualizer can be disabled independently.
48. MPRIS-compatible media players are supported.
49. Multiple active media players can be selected.
50. Media controls and visualizer work simultaneously.

## Launcher
51. Applications appear immediately.
52. Applications are ordered using local usage statistics.
53. Live filtering works.
54. File search includes hidden files.
55. File search supports fuzzy matching.
56. Calculator mode has a dedicated interface.
57. Calculator errors do not close the Launcher.
58. Command mode displays stdout, stderr and exit status.
59. Command history works within the session.
60. Global search includes local settings and actions.
61. Web search is excluded.
62. Quick actions use appropriate authorization.
63. Custom commands can be created and executed.

## Dashboard
64. General dashboard and Control Center share panel infrastructure.
65. Navigation remains consistent.
66. Quick actions are available.
67. Unsupported hardware blocks are hidden.
68. Brightness controls appear when supported.
69. System settings open appropriate native applications.

## Notifications and tray
70. Notifications appear from the bottom of the right bar.
71. Notifications are grouped by application.
72. Cards can expand.
73. Notification timing follows configured priorities.
74. Hover pauses notification timers.
75. Notification history lasts seven days.
76. Notification indicator remains accessible.
77. System Tray supports compatible applications.

## Wallpapers, screenshots and files
78. Wallpaper selector recursively scans its configured directory.
79. Thumbnails are persistently cached.
80. One click applies wallpaper to all monitors.
81. Wallust updates after wallpaper changes.
82. Screenshot integration reuses the existing tool.
83. Screenshot destination is configurable.
84. Screenshot folder has a dashboard shortcut.
85. Recent files and trash can be accessed from the dashboard.

## Clipboard
86. Clipboard history supports text and images.
87. Search and preview work.
88. Entries can be copied again.
89. History can be cleared or disabled.
90. Clipboard history remains local.

## Assistant
91. mao Assistant frontend works independently of a real backend.
92. Conversation history persists.
93. Text input works.
94. Voice transcription works as input-only functionality.
95. Transcriptions appear in conversation history.
96. Capture can be stopped or cancelled.
97. Backend architecture is replaceable.
98. No text-to-speech is required initially.

## Profiles and automation
99. Visual profiles can be saved.
100. Bar distribution profiles can be saved.
101. Complete desktop profiles can be saved.
102. Profiles can be duplicated and restored.
103. Automatic profile switching is disabled by default.
104. User-configured application rules can activate profiles.

## Lock screen and power
105. Lock screen uses a secure existing Wayland-compatible mechanism.
106. Lock screen displays clock, date and weather.
107. Lock screen has one initial fixed design.
108. Reboot and shutdown require confirmation.
109. Suspend executes directly.
110. Lock screen is separate from the power menu.

## Settings, maintenance and diagnostics
111. Basic and advanced settings are separated.
112. Configuration is automatically persisted.
113. Backups can be created and restored.
114. Import validates configuration.
115. First-run setup can be executed again.
116. Diagnostics display module and service status.
117. System health warnings are available.
118. Usage statistics remain local.
119. Shell update checks run every 24 hours.
120. Changelog is displayed before manual updates.
121. Updates include backup and rollback.
122. OS update checks run every three hours.
123. No automatic package installation occurs.
124. Spanish and English are supported.
125. System language detection works.
126. Accessibility features are available.
127. Help and shortcut documentation is available.

---

# 39. Instructions for the Coding Agent

The agent must follow these rules throughout development.

## Before implementation

1. Read this specification completely.
2. Read `AGENTS.md`.
3. Inspect the existing system.
4. Inspect the installed Quickshell version and available APIs.
5. Inspect the existing Hyprland configuration.
6. Detect installed services and relevant applications.
7. Identify existing infrastructure that should be reused.
8. Report relevant findings before making significant architectural decisions.

## During implementation

1. Work incrementally.
2. Implement one logical module or feature at a time.
3. Preserve existing working functionality.
4. Separate presentation from logic.
5. Avoid hardcoded system assumptions.
6. Avoid unnecessary dependencies.
7. Do not duplicate existing system services.
8. Use actual system state.
9. Validate APIs before using them.
10. Keep configuration backward-compatible when possible.
11. Handle missing capabilities gracefully.
12. Add useful error handling.
13. Test changes before considering a module complete.

## System modifications

Never silently:

- Overwrite Hyprland configuration.
- Disable existing services.
- Uninstall existing applications.
- Replace existing notification daemons.
- Change system-wide configuration.
- Execute destructive operations.

Show planned modifications and request authorization.

## Completion reports

After completing each development stage, report:

- What was implemented.
- Files created or modified.
- Dependencies required.
- System integrations used.
- Tests performed.
- Known limitations.
- Remaining work.

Do not claim functionality is complete without testing it.

## Source of truth

This document defines the functional and architectural requirements of mao-shell.

If an implementation decision conflicts with this specification, explain the conflict before changing the design.

Implementation details may evolve as technical constraints are discovered, but existing functional decisions must not be discarded silently.

---

# 40. Final Project Direction

mao-shell should become a coherent, expressive and modular desktop shell rather than a collection of independent widgets.

Its defining characteristics are:

- A distinctive four-sided perimeter interface.
- Organic and asymmetric visual composition.
- Minimal everyday interaction.
- Powerful contextual dashboards.
- Deep integration with the Linux desktop.
- Flexible configuration and profiles.
- Safe system interaction.
- Modular architecture.
- Incremental and verifiable development.

The first priority is to build a reliable visual and architectural foundation.

Advanced functionality should be added progressively without compromising the shell's performance, stability or identity.

---

# 41. Recorded Decisions

| Topic | Decision |
|---|---|
| Wallust trigger | The existing wallpaper-change script runs Wallust at the same moment the wallpaper changes (§8.2). |
| Colour appearance | Dark only, derived from Wallust and the wallpaper. No light mode is required (DESIGN_PATTERNS §1). |
| Notification daemon | dunst is kept. Its appearance will be customised later (§22.1). |
| Screenshot tool | flameshot (§24.1). |
| Clipboard manager | Pending. No clipboard manager is installed (§26.2). |
| AUR helper and updates | yay. The check runs without root; applying updates requires root and is started by the user in a terminal (§17). |
| Update check interval | Every 24 hours for mao-shell versions (§35.8). OS checks remain every 3 hours (§17.4). |
| Weather geolocation | Automatic IP geolocation enabled by default, and the user can disable it (§16.4). |
| Lock screen | hyprlock, installed (§30). |
| Translations | JSON files loaded through the configuration service (§34.9). |
| Audio visualizer | Native Quickshell PipeWire first; CAVA only if native capture is insufficient (§12.3). |
| Command mode | Only commands in an explicit allow-list may run (§19.5). |
| Hyprland configuration | mao-shell may generate changes. Each change is shown for authorization and backed up before it is applied (§3.5, §9 of AGENTS.md). |
| Development order | Core system integrations come before the Launcher, aligned with ROADMAP Phase 3 and Phase 4 (§2.1). |
| Development path | Run from the repository during development. Installation to `~/.config/quickshell/mao-shell/` comes later (§4). |
| Evidence storage | Outside the repository in `~/.local/share/mao-shell/evidence/` (DESIGN_PATTERNS §20). |
| Quickshell | 0.3.2 installed. APIs are verified against this version (AGENTS §6). |
| Licence | MIT (`LICENSE`). |

Still pending: clipboard manager, versioning scheme (§37), and performance budgets, which are set by measurement in Phase 6 and not invented in advance (project-validation §12.2).