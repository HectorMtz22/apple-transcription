# Live Transcribe

A SwiftUI app for iOS and macOS that performs real-time speech transcription with optional live translation. Built on Apple's on-device speech frameworks, it supports Korean, English, and Spanish, and lets you pick between the newer offline `SpeechAnalyzer` API and the classic `SFSpeechRecognizer`.

## Features

- Live transcription with a volatile "in-progress" line that finalizes into entries
- On-device translation between Korean, English, and Spanish via Apple's Translation framework
- Selectable recognition backend: `SpeechAnalyzer` (offline) or `SFSpeechRecognizer` (online)
- Waveform visualization driven by RMS audio metrics
- Silence-based finalization (1.5s) so lingering speech gets committed even without an explicit final result
- Transcript export as plain text

## Requirements

- iOS 26.0+ or macOS 26.0+
- Xcode with Swift 6.0
- [XcodeGen](https://github.com/yonaskolb/XcodeGen) to regenerate the project from `project.yml`

## Build

```sh
xcodegen generate
open LiveTranscribe.xcodeproj
```

Then build and run the `LiveTranscribe` target for iOS or macOS.

## Project Layout

- `LiveTranscribeApp.swift` — app entry point
- `ContentView.swift` — SwiftUI UI, language/backend pickers, transcript list
- `TranscriptionEngine.swift` — `@Observable` coordinator: state, result stream, translation dispatch, silence detection, deduplication
- `SpeechAnalyzerSession.swift` — offline `SpeechAnalyzer` backend
- `SpeechRecognitionSession.swift` — `SFSpeechRecognizer` backend
- `AudioBufferProcessor.swift` — RMS and gain computation feeding the waveform
- `Models.swift` — `AppLanguage`, `TranscriptEntry`, `AudioMetrics` circular buffer

## Permissions

The app requests microphone access and, when using `SFSpeechRecognizer`, speech recognition authorization. Usage strings are declared in `Info.plist`.
