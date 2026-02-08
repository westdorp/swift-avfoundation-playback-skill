# FairPlay Streaming (DRM)

FairPlay integration is deterministic when key-session flow and error handling are explicit.

## Core Key Flow

1. Create an `AVContentKeySession` with `.fairPlayStreaming`.
2. Register content recipients.
3. Handle key requests in delegate callbacks.
4. Create SPC, send to KSM, receive CKC.
5. Return `AVContentKeyResponse` or explicit key error.

```swift
let keySession = AVContentKeySession(keySystem: .fairPlayStreaming)
keySession.setDelegate(self, queue: .main)
```

## Offline Policy

For offline scenarios, handle `AVPersistableContentKeyRequest` and store persistent keys according to your security policy.

## Failure Policy

On key-processing failures:

- Return explicit content-key response errors.
- Include request identifiers and key-stage context in app diagnostics.

## Do / Avoid

- Do keep SPC/CKC exchange boundaries explicit.
- Do separate content-key failures from media-delivery failures.
- Do define persistent-key storage and eviction policy.
- Avoid treating all playback failures as DRM failures.

Sources:
- https://developer.apple.com/documentation/avfoundation/avcontentkeysession — Defines FairPlay content-key session APIs.
- https://developer.apple.com/documentation/avfoundation/avcontentkeyrequest — Defines key-request processing APIs.
- https://developer.apple.com/documentation/avfoundation/avpersistablecontentkeyrequest — Defines persistent key-request path for offline playback.
- https://developer.apple.com/streaming/fps/ — Official FairPlay Streaming resources and program documentation.
- https://developer.apple.com/library/archive/technotes/tn2454/_index.html — Apple troubleshooting guidance for FairPlay Streaming issues.
- https://developer.apple.com/videos/play/wwdc2017/503/ — Apple session guidance for AVContentKeySession workflows.
