# Ktebli
Below is a GitHub Issues-ready implementation plan organized as Epics → actionable Issues/Tasks → Acceptance Criteria.

You can copy these directly into GitHub Issues, GitHub Projects, Linear, Jira, or Azure DevOps.

GitHub Issues Plan
AI-Powered Universal Voice Dictation Assistant
Recommended GitHub Structure
Milestones
M0 — Project Foundation
M1 — Desktop Shell MVP
M2 — Dictation Core
M3 — Groq Transcription
M4 — Universal Text Insertion
M5 — Settings, History, and Privacy
M6 — Reliability, QA, and Release
M7 — Post-MVP Enhancements

Labels
type: epic
type: feature
type: task
type: bug
type: research
type: test
type: documentation

priority: critical
priority: high
priority: medium
priority: low

area: desktop
area: audio
area: hotkeys
area: providers
area: groq
area: injection
area: settings
area: security
area: history
area: ux
area: qa
area: docs

platform: windows
platform: macos
platform: linux

status: blocked
status: ready
status: in-progress

Epic 1 — Project Foundation and Architecture
Issue 1.1 — Define MVP Product Scope

Labels: type: task, priority: critical, area: docs
 Milestone: M0 — Project Foundation

Description

Define the exact MVP boundaries for the first release of the AI dictation assistant.

The MVP should focus on the core workflow:

Press global shortcut → record voice → transcribe with Groq → insert text into active app

Tasks
Define MVP feature list.
Define non-MVP features.
Define supported platforms for MVP.
Define expected user workflow.
Define initial limitations.
Write MVP acceptance criteria.
Acceptance Criteria
MVP scope is documented in docs/mvp-scope.md.
MVP includes global hotkey, recording, Groq transcription, and text insertion.
Streaming transcription is explicitly marked as post-MVP.
AI rewriting, grammar correction, translation, and voice commands are marked as future roadmap features.
Windows-first development priority is documented.
Issue 1.2 — Create Technical Architecture Document

Labels: type: task, priority: critical, area: docs, area: desktop
 Milestone: M0 — Project Foundation

Description

Create the main architecture document describing all major application layers.

Tasks
Define UI layer.
Define application core.
Define audio layer.
Define provider layer.
Define OS integration layer.
Define security layer.
Define history and storage layer.
Define data flow between modules.
Acceptance Criteria
docs/architecture.md exists.
Architecture includes a provider-independent design.
Architecture includes a dictation state machine.
Architecture includes security handling for API keys.
Architecture includes text insertion strategy.
Architecture includes clear module boundaries.
Issue 1.3 — Initialize Repository Structure

Labels: type: task, priority: critical, area: desktop
 Milestone: M0 — Project Foundation

Description

Create the initial monorepo or application repository structure.

Recommended Structure
ai-dictation-assistant/
  apps/
    desktop/
      src/
      src-tauri/
  packages/
    shared/
  docs/
  .github/
    ISSUE_TEMPLATE/
    workflows/

Tasks
Initialize repository.
Add README.
Add license.
Add contribution guide.
Add issue templates.
Add pull request template.
Add initial project folders.
Acceptance Criteria
Repository builds or installs dependencies successfully.
README explains the project vision and MVP.
Folder structure supports desktop app and shared packages.
GitHub issue templates exist for bug, feature, and task.
Pull request template exists.
Issue 1.4 — Choose and Document Desktop Technology Stack

Labels: type: research, priority: high, area: desktop, area: docs
 Milestone: M0 — Project Foundation

Description

Finalize the desktop application stack.

Recommended Decision

Use:

Tauri + Rust + React + TypeScript

Tasks
Compare Tauri and Electron.
Document decision.
Document reasons for final choice.
Identify required plugins and native crates.
Document known platform risks.
Acceptance Criteria
docs/technology-decision.md exists.
Document explains why Tauri is selected.
Document includes fallback considerations for Electron.
Document identifies risks around global hotkeys, audio capture, and text injection.
Epic 2 — Desktop Shell and Background App
Issue 2.1 — Create Initial Tauri Desktop App

Labels: type: feature, priority: critical, area: desktop
 Milestone: M1 — Desktop Shell MVP

Description

Create the base Tauri desktop app with frontend and backend integration.

Tasks
Scaffold Tauri app.
Add React/TypeScript frontend.
Add Rust backend structure.
Add shared types package if needed.
Confirm app launches on target OS.
Add basic build command.
Acceptance Criteria
App launches successfully in development mode.
App has a basic settings window placeholder.
Tauri frontend can call at least one Rust command.
Build instructions are documented in README.
Issue 2.2 — Implement System Tray Application

Labels: type: feature, priority: critical, area: desktop, area: ux
 Milestone: M1 — Desktop Shell MVP

Description

The app should run silently in the system tray.

Tasks
Add system tray icon.
Add tray menu.
Add show settings action.
Add start dictation action.
Add stop dictation action.
Add quit action.
Support hiding the main window instead of closing the app.
Acceptance Criteria
App appears in the system tray.
Closing the settings window does not quit the app.
Tray menu includes:
Start Dictation
Stop Dictation
Settings
History
Quit
Quit fully stops the app process.
Tray works after app restart.
Issue 2.3 — Implement App Lifecycle Management

Labels: type: task, priority: high, area: desktop
 Milestone: M1 — Desktop Shell MVP

Description

Define and implement background app lifecycle behavior.

Tasks
Define startup behavior.
Define window open/close behavior.
Define tray-only mode.
Define clean shutdown logic.
Add app lifecycle logging.
Acceptance Criteria
App can run without an open window.
App can open settings from tray.
App shuts down cleanly.
Background state is preserved while settings window is closed.
No orphan processes remain after quitting.
Issue 2.4 — Add Desktop Notifications

Labels: type: feature, priority: medium, area: ux, area: desktop
 Milestone: M1 — Desktop Shell MVP

Description

Add simple non-intrusive notifications for important app states.

Tasks
Add notification support.
Add notification for recording started.
Add notification for transcription failed.
Add notification for insertion fallback.
Add user preference placeholder for notifications.
Acceptance Criteria
Notification appears when recording starts.
Notification appears when provider error occurs.
Notification appears when text insertion fails and text is copied to clipboard.
Notifications are not excessive during normal operation.
Epic 3 — Dictation State Machine
Issue 3.1 — Implement Dictation Session State Machine

Labels: type: feature, priority: critical, area: desktop
 Milestone: M2 — Dictation Core

Description

Implement the core state machine that controls dictation sessions.

States
Idle
Recording
Processing
Injecting
Error

Tasks
Create state machine module.
Define allowed transitions.
Expose current state to UI and tray.
Prevent invalid state transitions.
Add logging for state transitions.
Acceptance Criteria
App starts in Idle.
Hotkey from Idle transitions to Recording.
Stop recording transitions to Processing.
Successful transcription transitions to Injecting.
Successful insertion transitions back to Idle.
Errors transition to Error.
Error recovery returns app to Idle.
Issue 3.2 — Add Dictation Session Manager

Labels: type: feature, priority: critical, area: desktop, area: audio, area: providers
 Milestone: M2 — Dictation Core

Description

Create a session manager that coordinates recording, transcription, and insertion.

Tasks
Create dictation session abstraction.
Generate unique session IDs.
Track session start and end time.
Connect session manager to state machine.
Prepare hooks for audio, provider, and insertion modules.
Acceptance Criteria
Each dictation attempt has a unique session ID.
Session manager can start a session.
Session manager can stop a session.
Session manager stores basic metadata:
start time
end time
duration
provider
language
status
Session manager correctly triggers state transitions.
Epic 4 — Global Hotkey System
Issue 4.1 — Register Default Global Hotkey

Labels: type: feature, priority: critical, area: hotkeys, area: desktop
 Milestone: M2 — Dictation Core

Description

Add a global keyboard shortcut that can start and stop dictation anywhere in the OS.

Default Shortcut
Ctrl + Alt + Space


On macOS:

Command + Shift + Space

Tasks
Register global shortcut.
Connect shortcut to dictation state machine.
Implement toggle behavior.
Add platform-specific shortcut handling.
Log shortcut activation.
Acceptance Criteria
Pressing shortcut starts recording from any application.
Pressing shortcut again stops recording.
Shortcut works while app window is hidden.
Shortcut does not require the settings window to be open.
Shortcut state is reflected in logs.
Issue 4.2 — Add Configurable Hotkey Settings

Labels: type: feature, priority: high, area: hotkeys, area: settings
 Milestone: M5 — Settings, History, and Privacy

Description

Allow users to configure their preferred global shortcut.

Tasks
Add hotkey input UI.
Capture key combinations.
Save selected shortcut.
Re-register shortcut after change.
Validate invalid combinations.
Show shortcut conflict errors where possible.
Acceptance Criteria
User can change the dictation shortcut.
New shortcut works without app restart.
Invalid shortcuts are rejected.
Previous shortcut is unregistered after change.
User can reset to default shortcut.
Issue 4.3 — Support Toggle and Push-to-Talk Modes

Labels: type: feature, priority: medium, area: hotkeys, area: ux
 Milestone: M7 — Post-MVP Enhancements

Description

Support two dictation activation modes.

Modes
Toggle mode:
Press once to start, press again to stop.

Push-to-talk mode:
Hold shortcut to record, release to stop.

Tasks
Add mode setting.
Implement toggle mode.
Implement push-to-talk key-down/key-up handling.
Add UI explanation.
Test mode switching.
Acceptance Criteria
Toggle mode works correctly.
Push-to-talk mode works correctly.
User can switch modes in settings.
The selected mode persists after restart.
Epic 5 — Audio Recording Engine
Issue 5.1 — Implement Microphone Permission Flow

Labels: type: feature, priority: critical, area: audio, area: ux
 Milestone: M2 — Dictation Core

Description

Request and handle microphone access.

Tasks
Detect microphone permission status.
Request permission during onboarding.
Handle denied permissions.
Add retry/open settings action.
Add platform-specific permission messaging.
Acceptance Criteria
App requests microphone access when needed.
User receives clear feedback if microphone permission is denied.
Recording cannot start without permission.
Settings show microphone permission status.
Issue 5.2 — Detect Available Microphone Devices

Labels: type: feature, priority: high, area: audio, area: settings
 Milestone: M2 — Dictation Core

Description

List available audio input devices.

Tasks
Enumerate input devices.
Identify default microphone.
Store selected microphone.
Refresh device list.
Handle device unplugged events.
Acceptance Criteria
Settings show available microphone devices.
Default microphone is selected automatically.
User can choose another microphone.
App handles missing selected microphone gracefully.
Issue 5.3 — Record Audio to Temporary File

Labels: type: feature, priority: critical, area: audio
 Milestone: M2 — Dictation Core

Description

Capture microphone audio and save it in a provider-compatible temporary format.

Recommended Format
WAV
Mono
16 kHz or 24 kHz

Tasks
Start recording on state transition to Recording.
Stop recording on user action or timeout.
Save audio to temporary file.
Ensure provider-compatible format.
Clean up old temporary files.
Acceptance Criteria
App creates a valid audio file after recording.
Audio file can be played locally for debugging.
Recording starts within target latency.
Temporary audio file is deleted after transcription unless debug mode is enabled.
Recording failure transitions app to Error.
Issue 5.4 — Add Recording Timeout

Labels: type: task, priority: high, area: audio
 Milestone: M2 — Dictation Core

Description

Prevent very long accidental recordings.

Tasks
Add default maximum recording duration.
Add timeout configuration.
Stop recording automatically when timeout is reached.
Notify user if recording stopped due to timeout.
Acceptance Criteria
Recording stops when maximum duration is reached.
Default timeout is documented.
Timeout can be changed in advanced settings.
App continues processing audio after timeout stop.
Issue 5.5 — Add Basic Silence Detection

Labels: type: feature, priority: medium, area: audio
 Milestone: M5 — Settings, History, and Privacy

Description

Automatically stop recording after silence.

Tasks
Detect voice activity level.
Add configurable silence duration.
Add silence threshold setting.
Stop recording after sustained silence.
Ensure silence detection does not cut users off too early.
Acceptance Criteria
Recording stops after configured silence duration.
Silence detection can be disabled.
Silence detection works for short dictation.
App avoids stopping during brief pauses.
Epic 6 — Provider Layer
Issue 6.1 — Define Speech Provider Interface

Labels: type: feature, priority: critical, area: providers
 Milestone: M3 — Groq Transcription

Description

Create a provider-independent interface for speech recognition providers.

Suggested Interface
interface SpeechProvider {
  id: string;
  name: string;
  validateApiKey(apiKey: string): Promise<boolean>;
  transcribe(input: TranscriptionRequest): Promise<TranscriptionResult>;
  getAvailableModels(): Promise<ModelInfo[]>;
}

Tasks
Define provider interface.
Define transcription request type.
Define transcription result type.
Define provider error type.
Add provider registry.
Add provider selection logic.
Acceptance Criteria
Groq provider can implement the interface.
Future providers can be added without changing app core logic.
Provider errors are normalized.
Provider result format is normalized.
Unit tests cover provider interface assumptions.
Issue 6.2 — Implement Provider Registry

Labels: type: feature, priority: high, area: providers
 Milestone: M3 — Groq Transcription

Description

Create a registry for available speech providers.

Tasks
Add provider registration mechanism.
Add active provider selection.
Add provider lookup by ID.
Add default provider configuration.
Add unsupported provider error handling.
Acceptance Criteria
Groq is registered as the default provider.
App can retrieve active provider by ID.
Missing provider ID returns clear error.
Provider registry is independent of UI.
Issue 6.3 — Add OpenAI-Compatible Provider Placeholder

Labels: type: task, priority: medium, area: providers
 Milestone: M7 — Post-MVP Enhancements

Description

Prepare structure for custom OpenAI-compatible transcription endpoints.

Tasks
Add placeholder provider folder.
Define expected configuration fields.
Document endpoint URL, model, API key, and headers.
Do not implement full functionality yet unless required.
Acceptance Criteria
Architecture supports future OpenAI-compatible providers.
Placeholder does not affect MVP behavior.
Documentation explains how future providers will be added.
Epic 7 — Groq Cloud Transcription
Issue 7.1 — Add Groq API Key Configuration

Labels: type: feature, priority: critical, area: groq, area: settings, area: security
 Milestone: M3 — Groq Transcription

Description

Allow users to enter and store a Groq API key securely.

Tasks
Add API key input UI.
Mask API key by default.
Add show/hide toggle.
Store API key in OS secure storage.
Retrieve API key for transcription requests.
Allow user to delete saved key.
Acceptance Criteria
User can save a Groq API key.
API key is not stored in plain text config files.
API key is masked in UI.
User can remove saved API key.
App blocks transcription if API key is missing.
Issue 7.2 — Validate Groq API Key

Labels: type: feature, priority: high, area: groq, area: settings
 Milestone: M3 — Groq Transcription

Description

Validate that the user’s Groq API key works.

Tasks
Add API key validation method.
Show validation status in settings.
Handle invalid key response.
Handle network failure separately.
Avoid excessive validation requests.
Acceptance Criteria
Valid key shows success state.
Invalid key shows clear error.
Network failure shows separate message.
App does not expose the API key in logs.
Validation result is stored or cached safely.
Issue 7.3 — Implement Groq Speech Provider

Labels: type: feature, priority: critical, area: groq, area: providers
 Milestone: M3 — Groq Transcription

Description

Implement the Groq provider using the common speech provider interface.

Tasks
Create GroqSpeechProvider.
Send audio file to Groq transcription endpoint.
Include model selection.
Include language hint when configured.
Parse response.
Normalize result.
Handle API errors.
Handle timeout and retry.
Acceptance Criteria
Recorded audio can be transcribed through Groq.
Provider returns normalized TranscriptionResult.
Provider handles authentication failure.
Provider handles rate limit error.
Provider handles timeout.
Provider does not leak API key in logs.
Unit/integration tests cover successful and failed responses.
Issue 7.4 — Add Groq Model Selection

Labels: type: feature, priority: medium, area: groq, area: settings
 Milestone: M5 — Settings, History, and Privacy

Description

Allow the user to select the Groq speech recognition model when multiple options are available.

Tasks
Add model setting.
Add default model.
Fetch model list if supported.
Allow manual model fallback.
Store selected model.
Acceptance Criteria
User can view selected Groq model.
User can change model if multiple are available.
Default model works without manual configuration.
Invalid model selection shows clear error.
Epic 8 — Text Normalization
Issue 8.1 — Implement Transcription Text Normalizer

Labels: type: feature, priority: high, area: providers, area: injection
 Milestone: M3 — Groq Transcription

Description

Clean and prepare transcription output before inserting it.

Tasks
Trim leading/trailing whitespace.
Preserve punctuation.
Add optional trailing space.
Normalize line breaks.
Avoid duplicate spaces.
Preserve Arabic and English text.
Acceptance Criteria
Empty transcription is handled safely.
Leading and trailing spaces are removed.
Optional trailing space setting works.
Arabic text is not corrupted.
Mixed Arabic-English text is preserved.
Normalizer has unit tests.
Issue 8.2 — Add Automatic Punctuation Preference

Labels: type: feature, priority: medium, area: settings, area: providers
 Milestone: M5 — Settings, History, and Privacy

Description

Allow users to enable or disable automatic punctuation where supported by the provider.

Tasks
Add setting for punctuation.
Pass preference to provider if supported.
Apply post-processing fallback if needed.
Document provider limitations.
Acceptance Criteria
User can enable or disable punctuation preference.
Preference is included in provider request where possible.
Unsupported providers degrade gracefully.
Setting persists after app restart.
Epic 9 — Universal Text Insertion
Issue 9.1 — Implement Clipboard-Based Text Insertion

Labels: type: feature, priority: critical, area: injection, area: desktop
 Milestone: M4 — Universal Text Insertion

Description

Insert transcribed text into the currently focused application using clipboard paste.

Process
1. Save current clipboard content.
2. Copy transcription to clipboard.
3. Send paste shortcut.
4. Restore previous clipboard content after delay.

Tasks
Read current clipboard.
Set clipboard to transcription text.
Send paste command.
Restore previous clipboard.
Add timing delay configuration.
Add error handling.
Acceptance Criteria
Transcribed text appears in currently focused text field.
Existing clipboard content is restored after insertion.
Text insertion works in common apps.
Empty transcription is not inserted.
Insertion returns success/failure status.
Failure triggers fallback behavior.
Issue 9.2 — Add Simulated Typing Fallback

Labels: type: feature, priority: high, area: injection
 Milestone: M4 — Universal Text Insertion

Description

Add fallback insertion by simulating keystrokes.

Tasks
Implement simulated typing.
Support Unicode text where possible.
Add per-platform implementation.
Add typing speed setting if needed.
Use fallback when clipboard paste fails.
Acceptance Criteria
Simulated typing can insert simple English text.
Fallback is attempted when clipboard insertion fails.
User can choose insertion method in settings.
Failure is reported clearly.
App does not crash if simulated typing is unsupported.
Issue 9.3 — Handle Platform-Specific Paste Shortcuts

Labels: type: task, priority: high, area: injection, platform: windows, platform: macos, platform: linux
 Milestone: M4 — Universal Text Insertion

Description

Implement correct paste command by platform.

Platform Rules
Windows: Ctrl + V
Linux: Ctrl + V
macOS: Command + V

Tasks
Detect platform.
Send correct paste shortcut.
Add tests or manual validation.
Handle keyboard layout edge cases.
Acceptance Criteria
Windows uses Ctrl + V.
Linux uses Ctrl + V.
macOS uses Command + V.
Paste command works while app is hidden.
Paste command does not focus the app window.
Issue 9.4 — Add Insertion Failure Fallback to Clipboard

Labels: type: feature, priority: high, area: injection, area: ux
 Milestone: M4 — Universal Text Insertion

Description

If automatic text insertion fails, keep the transcribed text available to the user.

Tasks
Detect insertion failure.
Copy transcription to clipboard.
Notify user.
Save result to history.
Return app to idle state.
Acceptance Criteria
If insertion fails, transcription remains copied to clipboard.
User sees notification: “Text copied. Press paste to insert.”
Result is saved to history.
App does not lose the transcription.
App recovers to idle state.
Epic 10 — Settings UI
Issue 10.1 — Create Settings Window

Labels: type: feature, priority: critical, area: settings, area: ux
 Milestone: M5 — Settings, History, and Privacy

Description

Build the main settings window.

Sections
General
Provider
Microphone
Hotkey
Language
Insertion
History
Privacy
Advanced

Tasks
Create settings layout.
Add navigation sections.
Load current settings.
Save changed settings.
Validate settings.
Add reset-to-default option.
Acceptance Criteria
Settings window opens from tray.
Settings are grouped clearly.
Changes persist after restart.
Invalid settings show clear errors.
Reset-to-default works.
Issue 10.2 — Add Provider Settings

Labels: type: feature, priority: critical, area: settings, area: providers, area: groq
 Milestone: M5 — Settings, History, and Privacy

Description

Create settings for provider selection and configuration.

Tasks
Add provider selector.
Add Groq API key field.
Add validation button.
Add model selector.
Add provider status display.
Acceptance Criteria
User can select Groq provider.
User can enter Groq API key.
User can validate API key.
User can select or view model.
Provider status is visible.
Issue 10.3 — Add Language Settings

Labels: type: feature, priority: high, area: settings, area: providers
 Milestone: M5 — Settings, History, and Privacy

Description

Allow users to configure transcription language behavior.

Options
Auto-detect
English
Arabic
French
Custom

Tasks
Add language selector.
Store language preference.
Pass language hint to provider.
Support auto-detect.
Add support for mixed Arabic-English workflow.
Acceptance Criteria
User can select transcription language.
Auto-detect is default.
Arabic and English options are available.
Selected language is passed to provider where supported.
Mixed Arabic-English dictation is not blocked by settings.
Issue 10.4 — Add Insertion Settings

Labels: type: feature, priority: medium, area: settings, area: injection
 Milestone: M5 — Settings, History, and Privacy

Description

Allow users to configure how transcribed text is inserted.

Options
Clipboard paste with restore
Simulated typing
Clipboard only fallback
Add trailing space

Tasks
Add insertion method selector.
Add trailing space option.
Add clipboard restore delay option.
Store settings.
Apply settings during insertion.
Acceptance Criteria
User can choose insertion method.
User can enable or disable trailing space.
Settings affect actual insertion behavior.
Settings persist after restart.
Epic 11 — Onboarding Flow
Issue 11.1 — Create First-Time Onboarding Flow

Labels: type: feature, priority: high, area: ux, area: settings
 Milestone: M5 — Settings, History, and Privacy

Description

Guide the user through first-time setup.

Steps
1. Welcome
2. Microphone permission
3. Provider API key
4. Language preference
5. Hotkey selection
6. Test dictation

Tasks
Detect first launch.
Show onboarding window.
Add welcome screen.
Add microphone permission screen.
Add Groq key setup screen.
Add hotkey setup screen.
Add completion screen.
Acceptance Criteria
Onboarding appears on first launch.
User can complete setup without manually opening settings.
User can skip optional steps.
App marks onboarding as completed.
Onboarding can be reopened from settings.
Issue 11.2 — Add Test Dictation Step

Labels: type: feature, priority: medium, area: ux, area: audio, area: providers
 Milestone: M5 — Settings, History, and Privacy

Description

Allow the user to test microphone and provider configuration during onboarding.

Tasks
Add test recording button.
Record short sample.
Send to active provider.
Display transcription result inside onboarding.
Show troubleshooting guidance if failure occurs.
Acceptance Criteria
User can perform a test dictation.
Successful transcription is shown.
Failed transcription shows clear error.
Test does not insert text into another app.
Temporary test audio is deleted.
Epic 12 — Transcription History
Issue 12.1 — Implement Local Transcription History

Labels: type: feature, priority: medium, area: history, area: privacy
 Milestone: M5 — Settings, History, and Privacy

Description

Store recent transcriptions locally.

History Entry Fields
id
timestamp
text
provider
model
language
durationMs
insertionStatus

Tasks
Create local history storage.
Save successful transcriptions.
Save failed insertion transcriptions.
Add history size limit.
Add history disabled mode.
Acceptance Criteria
Successful transcriptions are saved locally.
User can disable history.
History respects configured size limit.
History does not save temporary audio.
History storage survives app restart.
Issue 12.2 — Create History Viewer UI

Labels: type: feature, priority: medium, area: history, area: ux
 Milestone: M5 — Settings, History, and Privacy

Description

Create UI for viewing recent transcriptions.

Tasks
Add history page.
Show timestamp and text.
Add copy button.
Add delete entry button.
Add clear all button.
Add search/filter if simple.
Acceptance Criteria
User can open history from tray or settings.
User can copy a previous transcription.
User can delete a single history item.
User can clear all history.
History respects privacy settings.
Epic 13 — Security and Privacy
Issue 13.1 — Store API Keys in OS Secure Storage

Labels: type: feature, priority: critical, area: security, area: settings
 Milestone: M3 — Groq Transcription

Description

Store provider API keys securely using OS-native secure storage.

Platform Targets
Windows: Credential Manager
macOS: Keychain
Linux: Secret Service / GNOME Keyring / KWallet

Tasks
Add secure storage abstraction.
Implement platform secure storage.
Store Groq API key securely.
Retrieve key at runtime.
Delete key on user request.
Ensure key never appears in logs.
Acceptance Criteria
API key is not stored in plain text.
API key persists after app restart.
API key can be deleted.
Logs do not contain API key.
Failure to access secure storage shows clear error.
Issue 13.2 — Delete Temporary Audio After Transcription

Labels: type: task, priority: critical, area: security, area: audio, area: privacy
 Milestone: M3 — Groq Transcription

Description

Ensure temporary audio files are deleted after use.

Tasks
Track temporary audio file path.
Delete file after successful transcription.
Delete file after failed transcription.
Add debug option to keep files.
Clean old temp files on startup.
Acceptance Criteria
Temporary audio is deleted after transcription.
Temporary audio is deleted after provider error.
Old temp files are cleaned on app launch.
Debug mode is required to retain audio.
User privacy documentation explains audio handling.
Issue 13.3 — Add Privacy Settings

Labels: type: feature, priority: high, area: privacy, area: settings
 Milestone: M5 — Settings, History, and Privacy

Description

Allow users to control privacy-sensitive behavior.

Settings
Save transcription history
Show recording notifications
Delete audio immediately
Allow diagnostic logs

Tasks
Add privacy settings UI.
Persist privacy settings.
Enforce history setting.
Enforce audio deletion setting.
Document diagnostic logging.
Acceptance Criteria
User can disable transcription history.
User can control recording notifications.
Audio deletion is enabled by default.
Diagnostic logs do not include audio content or API keys.
Privacy settings persist after restart.
Epic 14 — Error Handling and Recovery
Issue 14.1 — Add Central Error Handling System

Labels: type: feature, priority: high, area: desktop, area: ux
 Milestone: M6 — Reliability, QA, and Release

Description

Create a consistent error handling system for all modules.

Tasks
Define app error types.
Normalize provider errors.
Normalize audio errors.
Normalize insertion errors.
Map errors to user-friendly messages.
Add logging for technical details.
Acceptance Criteria
Errors have stable codes.
Errors have user-facing messages.
Technical logs are available for debugging.
Sensitive values are redacted.
App does not crash on expected errors.
Issue 14.2 — Add Retry Logic for Provider Failures

Labels: type: feature, priority: medium, area: providers, area: groq
 Milestone: M6 — Reliability, QA, and Release

Description

Retry transient transcription failures.

Tasks
Retry on timeout.
Retry on temporary network failure.
Do not retry on invalid API key.
Do not retry on unsupported file format.
Add retry count setting or constant.
Acceptance Criteria
Provider timeout retries once by default.
Invalid API key does not retry.
Rate limit error shows clear message.
Retry behavior is logged.
User is notified only after final failure.
Issue 14.3 — Add Offline Handling

Labels: type: feature, priority: medium, area: providers, area: ux
 Milestone: M6 — Reliability, QA, and Release

Description

Detect and handle lack of internet connectivity.

Tasks
Detect network failure during transcription.
Show offline error.
Avoid repeated failing requests.
Preserve transcription attempt metadata.
Return app to idle state.
Acceptance Criteria
Offline transcription attempt fails gracefully.
User sees clear message.
App does not hang in processing state.
User can retry after internet returns.
Epic 15 — Performance and Latency
Issue 15.1 — Add Performance Metrics Logging

Labels: type: task, priority: high, area: desktop, area: qa
 Milestone: M6 — Reliability, QA, and Release

Description

Measure key dictation latency points.

Metrics
hotkey_to_recording_ms
recording_duration_ms
stop_to_upload_ms
provider_latency_ms
result_to_insertion_ms
total_session_ms

Tasks
Add performance timer utility.
Track metrics per session.
Store anonymized local debug metrics if enabled.
Display basic timings in debug logs.
Acceptance Criteria
Each dictation session logs timing metrics.
Metrics do not include transcription text unless debug history is enabled.
Provider latency is measured separately.
Metrics help identify latency bottlenecks.
Issue 15.2 — Optimize Recording Startup Latency

Labels: type: task, priority: high, area: audio, area: performance
 Milestone: M6 — Reliability, QA, and Release

Description

Improve speed from hotkey press to microphone recording.

Tasks
Measure current startup delay.
Keep audio system ready if possible.
Avoid unnecessary UI operations during start.
Reduce blocking calls.
Optimize device lookup.
Acceptance Criteria
Recording starts within target latency on supported systems.
Hotkey response feels immediate.
No settings window opens during dictation.
Performance improvements are documented.
Issue 15.3 — Optimize Text Insertion Latency

Labels: type: task, priority: medium, area: injection, area: performance
 Milestone: M6 — Reliability, QA, and Release

Description

Reduce delay between receiving transcription and inserting text.

Tasks
Measure insertion time.
Tune clipboard restore delay.
Avoid unnecessary processing before insertion.
Add fast path for short text.
Test across target apps.
Acceptance Criteria
Text insertion happens quickly after transcription result.
Clipboard restore does not interrupt paste.
Insertion works reliably across tested apps.
Optimized delay is documented.
Epic 16 — Cross-Platform Support
Issue 16.1 — Windows MVP Support

Labels: type: feature, priority: critical, platform: windows
 Milestone: M6 — Reliability, QA, and Release

Description

Ensure the MVP works reliably on Windows.

Tasks
Test app launch.
Test system tray.
Test global hotkey.
Test microphone recording.
Test clipboard insertion.
Test API key secure storage.
Create Windows installer.
Acceptance Criteria
Windows build installs successfully.
Global hotkey works.
Microphone recording works.
Text insertion works in common Windows apps.
API key persists securely.
App can quit cleanly.
Issue 16.2 — macOS Support

Labels: type: feature, priority: high, platform: macos
 Milestone: M7 — Post-MVP Enhancements

Description

Add macOS support after Windows MVP.

Tasks
Handle microphone permission.
Handle accessibility permission.
Implement macOS paste shortcut.
Test system tray/menu bar behavior.
Test secure keychain storage.
Package macOS app.
Acceptance Criteria
macOS app launches successfully.
Required permissions are explained clearly.
Global hotkey works after permission approval.
Text insertion works with Command + V.
API key is stored in Keychain.
App can be packaged for distribution.
Issue 16.3 — Linux Support

Labels: type: feature, priority: medium, platform: linux
 Milestone: M7 — Post-MVP Enhancements

Description

Add Linux support with documented limitations.

Tasks
Support common Linux desktops.
Test X11 behavior.
Research Wayland limitations.
Implement Linux paste shortcut.
Test microphone recording.
Test Secret Service storage.
Package AppImage or deb/rpm.
Acceptance Criteria
App works on at least one target Linux desktop.
X11 support is documented.
Wayland limitations are documented.
Linux microphone recording works.
Clipboard insertion works where supported.
Linux package can be installed.
Epic 17 — QA and Testing
Issue 17.1 — Add Unit Tests for Core Modules

Labels: type: test, priority: high, area: qa
 Milestone: M6 — Reliability, QA, and Release

Description

Add unit tests for core non-UI modules.

Test Areas
State machine
Provider interface
Text normalization
Settings persistence
History storage
Error mapping

Acceptance Criteria
State machine has transition tests.
Text normalizer has Arabic and English tests.
Provider errors are tested.
Settings load/save is tested.
Tests run in CI.
Issue 17.2 — Add Integration Tests for Dictation Flow

Labels: type: test, priority: high, area: qa, area: desktop
 Milestone: M6 — Reliability, QA, and Release

Description

Test the complete dictation pipeline where possible.

Flow
Start recording → stop recording → transcribe → normalize → insert

Tasks
Add mocked provider integration test.
Add audio fixture.
Add fake insertion target where possible.
Validate successful session.
Validate failure paths.
Acceptance Criteria
Mocked end-to-end dictation flow passes.
Provider failure path is tested.
Empty transcription path is tested.
Insertion failure fallback is tested.
Tests can run without real API key.
Issue 17.3 — Manual QA Checklist

Labels: type: test, priority: high, area: qa, area: docs
 Milestone: M6 — Reliability, QA, and Release

Description

Create a manual QA checklist for release testing.

Target Apps
Notepad
Microsoft Word
Browser text fields
Google Docs
Gmail or Outlook Web
VS Code
Teams or Slack
Search bars

Acceptance Criteria
docs/manual-qa-checklist.md exists.
Checklist covers hotkey, recording, transcription, and insertion.
Checklist covers Arabic, English, and mixed text.
Checklist covers invalid API key.
Checklist covers microphone denied.
Checklist covers offline mode.
Issue 17.4 — Arabic and English Dictation QA

Labels: type: test, priority: high, area: qa, area: providers
 Milestone: M6 — Reliability, QA, and Release

Description

Validate language support quality for Arabic and English.

Tasks
Test English dictation.
Test Arabic dictation.
Test mixed Arabic-English dictation.
Test right-to-left insertion.
Test punctuation behavior.
Document model limitations.
Acceptance Criteria
Arabic text inserts correctly.
English text inserts correctly.
Mixed Arabic-English text is preserved.
Right-to-left text is not corrupted.
Known language issues are documented.
Epic 18 — Packaging and Release
Issue 18.1 — Add Build and Release Pipeline

Labels: type: task, priority: high, area: desktop, area: qa
 Milestone: M6 — Reliability, QA, and Release

Description

Create CI workflow for building and testing the app.

Tasks
Add GitHub Actions workflow.
Run lint checks.
Run unit tests.
Build desktop app.
Store build artifacts.
Prepare release workflow.
Acceptance Criteria
CI runs on pull requests.
CI runs tests.
CI builds app successfully.
Build artifacts are generated.
Failed tests block merge.
Issue 18.2 — Create Windows Installer

Labels: type: feature, priority: critical, platform: windows, area: desktop
 Milestone: M6 — Reliability, QA, and Release

Description

Build installable Windows package for MVP users.

Tasks
Configure Windows packaging.
Include app icon.
Include app metadata.
Test install flow.
Test uninstall flow.
Sign installer if available.
Acceptance Criteria
Windows installer is generated.
App installs successfully.
App launches after install.
App appears in system tray.
App uninstalls cleanly.
Issue 18.3 — Write Release Notes for MVP

Labels: type: documentation, priority: medium, area: docs
 Milestone: M6 — Reliability, QA, and Release

Description

Prepare release notes for the first MVP release.

Acceptance Criteria
Release notes describe core functionality.
Release notes list supported platform.
Release notes list known limitations.
Release notes explain Groq API key requirement.
Release notes include privacy summary.
Epic 19 — Documentation
Issue 19.1 — Create User Setup Guide

Labels: type: documentation, priority: high, area: docs
 Milestone: M6 — Reliability, QA, and Release

Description

Write documentation for users installing and configuring the app.

Sections
Installation
First launch
Adding Groq API key
Choosing microphone
Setting global shortcut
Using dictation
Troubleshooting
Privacy

Acceptance Criteria
docs/user-guide.md exists.
User can follow the guide without developer help.
Guide explains API key setup.
Guide explains microphone permission.
Guide explains text insertion behavior.
Issue 19.2 — Create Developer Guide

Labels: type: documentation, priority: medium, area: docs
 Milestone: M6 — Reliability, QA, and Release

Description

Write documentation for contributors.

Sections
Local setup
Project structure
Architecture
Provider layer
Testing
Build process
Contribution workflow

Acceptance Criteria
docs/developer-guide.md exists.
Developer can run the app locally.
Developer can run tests.
Developer can understand provider abstraction.
Developer can build the desktop app.
Issue 19.3 — Document Provider Integration Guide

Labels: type: documentation, priority: medium, area: docs, area: providers
 Milestone: M7 — Post-MVP Enhancements

Description

Document how future speech providers should be added.

Acceptance Criteria
docs/provider-integration.md exists.
Guide explains provider interface.
Guide explains required methods.
Guide explains error normalization.
Guide includes example provider structure.
Epic 20 — Post-MVP AI Productivity Features
Issue 20.1 — Research Streaming Real-Time Dictation

Labels: type: research, priority: medium, area: providers, area: audio
 Milestone: M7 — Post-MVP Enhancements

Description

Research real-time streaming transcription support.

Research Areas
Provider streaming APIs.
Audio chunking strategy.
Partial transcript handling.
Latency targets.
UI indication for live dictation.
Acceptance Criteria
Research document exists.
Recommended streaming architecture is proposed.
Provider requirements are listed.
Risks are documented.
Issue 20.2 — Add AI Text Refinement Pipeline

Labels: type: feature, priority: medium, area: providers, area: ux
 Milestone: M7 — Post-MVP Enhancements

Description

Allow transcribed text to be refined before insertion.

Possible Modes
Clean grammar
Make professional
Summarize
Translate
Improve punctuation
Format as bullet points

Acceptance Criteria
Refinement pipeline design is documented.
Feature can be disabled.
Dictation remains fast when disabled.
Future LLM providers can plug into the pipeline.
Issue 20.3 — Research Voice Commands

Labels: type: research, priority: low, area: ux, area: providers
 Milestone: M7 — Post-MVP Enhancements

Description

Research support for voice commands such as:

new paragraph
delete last sentence
send message
format as bullet list

Acceptance Criteria
Research document exists.
Command recognition strategy is proposed.
Safety and accidental command risks are documented.
MVP dictation behavior remains unaffected.
Suggested GitHub Project Board Columns
Backlog
Ready
In Progress
In Review
Testing
Done
Blocked

Suggested MVP Release Cut

For the first usable MVP, prioritize these issues:

1.1 Define MVP Product Scope
1.2 Create Technical Architecture Document
1.3 Initialize Repository Structure
2.1 Create Initial Tauri Desktop App
2.2 Implement System Tray Application
3.1 Implement Dictation Session State Machine
3.2 Add Dictation Session Manager
4.1 Register Default Global Hotkey
5.1 Implement Microphone Permission Flow
5.2 Detect Available Microphone Devices
5.3 Record Audio to Temporary File
6.1 Define Speech Provider Interface
6.2 Implement Provider Registry
7.1 Add Groq API Key Configuration
7.2 Validate Groq API Key
7.3 Implement Groq Speech Provider
8.1 Implement Transcription Text Normalizer
9.1 Implement Clipboard-Based Text Insertion
9.4 Add Insertion Failure Fallback to Clipboard
10.1 Create Settings Window
10.2 Add Provider Settings
10.3 Add Language Settings
13.1 Store API Keys in OS Secure Storage
13.2 Delete Temporary Audio After Transcription
14.1 Add Central Error Handling System
16.1 Windows MVP Support
17.1 Add Unit Tests for Core Modules
17.3 Manual QA Checklist
18.2 Create Windows Installer
19.1 Create User Setup Guide

MVP Definition of Done

The MVP is considered complete when:

- The app installs and launches on Windows.
- The app runs in the system tray.
- User can configure a Groq API key.
- API key is stored securely.
- User can press a global shortcut to start recording.
- User can press the shortcut again to stop recording.
- Audio is sent to Groq for transcription.
- Transcribed text is inserted into the active text field.
- Clipboard insertion restores the previous clipboard content.
- Basic settings are available.
- Arabic and English transcription are manually tested.
- Temporary audio files are deleted.
- Common errors are handled gracefully.
- A Windows installer is available.

Recommended First Sprint
Sprint 1 Goal

Build the foundation for the app and prove native desktop feasibility.

Sprint 1 Issues
1.1 Define MVP Product Scope
1.2 Create Technical Architecture Document
1.3 Initialize Repository Structure
1.4 Choose and Document Desktop Technology Stack
2.1 Create Initial Tauri Desktop App
2.2 Implement System Tray Application
3.1 Implement Dictation Session State Machine
4.1 Register Default Global Hotkey

Sprint 1 Success Criteria

At the end of Sprint 1:

- App launches.
- App runs in tray.
- Global shortcut is registered.
- Pressing the shortcut changes app state from Idle to Recording placeholder.
- Architecture and MVP documents are committed.
