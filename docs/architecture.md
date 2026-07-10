# Technical Architecture

## Purpose
Define the MVP technical architecture for Ktebli, a Windows-first desktop dictation app that captures speech, transcribes it via Groq, and inserts text into the active application.

## Architecture Principles
- Provider-independent transcription pipeline.
- Clear separation between UI, application core, infrastructure, and OS integrations.
- Security-first handling of secrets and temporary audio.
- Fail-safe insertion behavior that preserves user clipboard data.

## High-Level Module Boundaries

### 1) UI Layer
**Responsibility:** User interaction and app visibility.

**Modules:**
- System tray menu
- Settings window
- Status/notification surface

**Boundary:** UI reads/writes state only through application core commands and events; UI does not call providers, OS APIs, or secure storage directly.

### 2) Application Core
**Responsibility:** Orchestration and business rules.

**Modules:**
- Dictation session manager
- Dictation state machine
- Command/event bus (or equivalent internal interfaces)
- Error handling coordinator

**Boundary:** Core coordinates all flows but delegates I/O to audio, provider, storage, security, and OS integration layers.

### 3) Audio Layer
**Responsibility:** Microphone discovery, permission checks, and recording lifecycle.

**Modules:**
- Microphone permission service
- Device enumeration service
- Recorder service (start/stop, temp-file output)

**Boundary:** Audio layer never decides application workflow; it only exposes capture capabilities and recording artifacts.

### 4) Provider Layer (Provider-Independent Design)
**Responsibility:** Speech-to-text abstraction and provider implementations.

**Modules:**
- `SpeechProvider` interface (transcribe contract)
- Provider registry/factory
- `GroqSpeechProvider` implementation (MVP provider)

**Boundary:** Core depends on the `SpeechProvider` interface, not concrete providers. New providers can be added without changing core dictation flow.

### 5) OS Integration Layer
**Responsibility:** Platform-native integrations.

**Modules:**
- Global hotkey registration/handling
- Text insertion adapter
- Clipboard management utilities
- System notifications

**Boundary:** OS-specific behavior is isolated behind adapters so core logic remains platform-agnostic.

### 6) Security Layer
**Responsibility:** Secret protection and privacy controls.

**Modules:**
- API key secure storage adapter (OS credential vault/keychain)
- In-memory secret access service
- Temporary audio cleanup policy

**Boundary:** Secrets are never persisted in plain-text settings or logs. Only security layer can read/write API credentials.

### 7) History and Storage Layer
**Responsibility:** Persisted app data and optional dictation history.

**Modules:**
- Settings repository
- History repository (if enabled)
- Retention/cleanup rules

**Boundary:** Storage schema and persistence details stay isolated from core logic via repository interfaces.

## Dictation State Machine

Primary states:
- `Idle`: No active dictation.
- `Recording`: Microphone capture in progress.
- `Transcribing`: Audio sent to selected provider.
- `Inserting`: Transcribed text being inserted into target app.
- `Completed`: Session finished successfully, returning to idle.
- `Error`: Session failed, error surfaced, recovery to idle.

Primary transitions:
1. `Idle -> Recording` on valid global hotkey press.
2. `Recording -> Transcribing` on stop action (hotkey or UI command) with valid captured audio.
3. `Transcribing -> Inserting` on successful transcription result.
4. `Inserting -> Completed` on successful insertion, then `Completed -> Idle`.
5. Any state -> `Error` on failure (permission, audio, provider, insertion, unexpected).
6. `Error -> Idle` after user acknowledgement or automatic recovery policy.

State guards:
- Recording can start only when microphone permission is granted.
- Transcription can start only when an API key and provider are configured.
- Insertion can start only when non-empty normalized text exists.

## End-to-End Data Flow
1. Hotkey event enters OS integration layer.
2. Core session manager receives command and moves state (`Idle -> Recording`).
3. Audio layer records microphone input to a temporary file.
4. Core resolves active provider through provider registry.
5. Provider layer transcribes audio and returns text.
6. Core normalizes/validates text and moves to insertion phase.
7. OS integration layer inserts text into active app (clipboard-assisted strategy for compatibility).
8. Security/storage layers apply cleanup (temp audio deletion) and optional history/settings updates.
9. Core emits completion/error event; UI updates status.

## API Key Security Handling
- API keys are stored only in OS secure storage (Credential Manager/Keychain/secret service).
- Core receives keys through a short-lived accessor; keys are not written to logs, history, or plain-text config.
- Redaction is applied to all error telemetry and user-facing diagnostics.
- Missing/invalid keys are handled as recoverable errors with guided UI feedback.

## Text Insertion Strategy
- Primary strategy: clipboard-assisted paste for maximum cross-application compatibility.
- Process:
  1. Capture and preserve current clipboard.
  2. Set clipboard to transcribed text.
  3. Trigger paste into active target context.
  4. Restore original clipboard content.
- Fallback behavior:
  - If direct insertion fails, keep transcription in clipboard and notify user.
  - Return actionable error status to core/UI for feedback.

## Error Handling Boundaries
- Layer-local errors are mapped to typed domain errors in core.
- Core decides user-facing severity and retry behavior.
- UI only renders error messages and recovery actions.

## MVP Extensibility
- Additional providers can be added by implementing `SpeechProvider`.
- Streaming transcription can be introduced as new states/events without breaking existing module boundaries.
- Post-MVP rewrite/translation features can be added as optional processing stages after transcription and before insertion.
