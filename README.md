# Ktebli

Ktebli is a Windows-first desktop AI dictation assistant that captures speech, transcribes it, and inserts text into the active application.

## Project Vision
Build a reliable, privacy-conscious desktop dictation experience with a provider-independent architecture and strong OS integration.

## MVP Scope
The first release focuses on the core dictation workflow:

1. Start/stop dictation with a global hotkey.
2. Capture microphone audio for one session.
3. Transcribe audio using Groq.
4. Insert final text into the currently active app.

### In Scope
- Global hotkey start/stop.
- Single-session voice recording.
- Groq transcription integration.
- Universal text insertion.
- Basic error feedback.

### Out of Scope (Post-MVP)
- Streaming/live partial transcription.
- AI rewriting, grammar correction, translation.
- Voice commands and automation.
- Advanced formatting controls.

## Repository Structure
```text
.
├─ apps/
│  └─ desktop/
│     ├─ src/
│     └─ src-tauri/
├─ packages/
│  └─ shared/
├─ docs/
└─ .github/
   ├─ ISSUE_TEMPLATE/
   └─ workflows/
```

## Documentation
- MVP scope: `docs/mvp-scope.md`
- Technical architecture: `docs/architecture.md`

## Contributing
Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before opening an issue or pull request.

## License
This project is licensed under the MIT License. See [LICENSE](./LICENSE).
