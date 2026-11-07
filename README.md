# Pocket Agent (customized from Lucy Screen Agent)

This is a lightweight personal fork of the uploaded Lucy Screen Agent source.

## What was changed

- App branding changed to **Pocket Agent** in the visible Android app UI and notifications.
- Maximum agent loop reduced from **50 to 12 steps per command** to cap latency, API usage and runaway actions.
- The model response parser accepts **only the first action** per observation. The screen is observed again before another action is requested.
- Screenshot uploads reduced from a 1280px longest edge / JPEG quality 72 to **1024px / quality 65** to reduce upload size and processing.
- Accessibility tree capped at **120 elements / depth 14** (previously 200 / depth 20).
- Accessibility text and descriptions are clipped to 160 characters.
- Password-field text is omitted from the accessibility tree.
- Added a local guard that blocks taps on controls whose accessible labels match common high-impact actions (send, pay, purchase, delete, transfer, submit, publish, call, etc.).
- Added prompt guidance treating on-screen content as untrusted data and requiring visual verification before claiming success.

## Performance approach

No on-device language or vision model is bundled. The AI reasoning remains cloud-based through the configured Gemini API. Phone-side work is limited to speech recognition/TTS, screenshot capture/downscaling, accessibility-tree extraction and UI actions. A dedicated phone GPU is not required.

The existing Gemini model selection has been retained from upstream (`gemini-3.1-flash-lite-preview`) rather than silently changing model compatibility.

## Build requirements

- Flutter / Dart SDK compatible with the upstream `pubspec.yaml` (`^3.11.1`).
- Android Studio / Android SDK and JDK 17.
- Connect an Android phone with USB debugging enabled, then run:

```bash
flutter pub get
flutter analyze
flutter test
flutter build apk --release
```

The original package ID and Kotlin channel/service package names were deliberately retained to avoid breaking native Flutter channel integration.

## Important limitations

- This source package was statically inspected and modified, but **not compiled or run** in this environment because Flutter and Dart SDKs are not installed here.
- The sensitive-action guard is a best-effort label match, not a complete safety boundary. Some apps expose no accessible labels, and labels vary by language. Review every action yourself.
- Screenshots and accessibility-tree content are sent to the configured Gemini API for reasoning. Do not use this with banking, password, private-message, medical or other sensitive screens unless you understand and accept that data flow.
- The app's existing API-key storage and upstream dependencies have not been redesigned.
- Accessibility and screen-capture permissions are powerful. Enable them only when you intentionally want the agent to control your device, and use the persistent stop control.

## Upstream

Customized from the user-provided `lucy-screen-agent-main` archive. Preserve the upstream license and notices in this repository.
