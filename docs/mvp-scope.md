# MVP Product Scope

## Purpose
Define the first-release (MVP) boundaries for Ktebli, an AI-powered universal voice dictation assistant.

## Core MVP Workflow
1. User presses a global shortcut.
2. App starts voice recording.
3. Audio is transcribed using Groq.
4. Final transcribed text is inserted into the currently active application.

## MVP Features (In Scope)
- Global hotkey to start/stop dictation.
- Voice recording capture for a single dictation session.
- Groq-based transcription integration.
- Universal text insertion into the active input context/app.
- Basic error feedback for failed recording/transcription/insertion.

## Non-MVP / Post-MVP Features (Out of Scope)
- Streaming/live partial transcription (post-MVP).
- AI rewriting (post-MVP).
- Grammar correction (post-MVP).
- Translation (post-MVP).
- Voice commands and action automation (post-MVP).
- Advanced formatting controls (post-MVP).

## Supported Platforms (MVP)
- **Primary target:** Windows (Windows-first development priority).
- **Secondary targets:** macOS and Linux are planned after MVP stabilization.

## Initial MVP Limitations
- Single transcription provider: Groq only.
- No streaming transcript updates during recording.
- No assistant-style text transformation (rewrite/translate/correct).
- Limited settings and customization in first release.
- Reliability and edge-case handling are focused on Windows first.

## MVP Acceptance Criteria
- MVP scope is documented in `docs/mvp-scope.md`.
- MVP includes global hotkey, recording, Groq transcription, and text insertion.
- Streaming transcription is explicitly marked as post-MVP.
- AI rewriting, grammar correction, translation, and voice commands are explicitly marked as future roadmap features.
- Windows-first development priority is explicitly documented.
