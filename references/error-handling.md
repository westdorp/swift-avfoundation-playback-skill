# Error Handling and Diagnostics

Use a single triage path that classifies failure type before retry decisions.

## Failure Surfaces

Inspect both status and diagnostic surfaces:

- `AVPlayerItem.status` / `AVPlayer.status`
- `AVPlayerItem.error` / `AVPlayer.error`
- `AVPlayerItem.errorLog()`
- `AVPlayerItem.accessLog()`

## Access Log vs Error Log

- Access log describes delivery/runtime telemetry (bitrate, transfer, stalls).
- Error log describes protocol/media failures with status/domain/comment metadata.

## Practical Checklist

1. Confirm status failure surface (`.failed`) and capture error objects.
2. Capture latest access-log and error-log events.
3. Classify failure domain (network, authoring, key delivery, decoder/media).
4. Apply bounded retries or explicit user recovery action.

## Do / Avoid

- Do preserve the original failure context through retry logic.
- Do cap automatic retries and expose user controls.
- Avoid retry loops that discard diagnostic context.
- Avoid collapsing all failures into one generic error bucket.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/status — Defines player-level readiness/failure status.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/status — Defines item-level readiness/failure status.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/errorlog() — Defines structured playback error diagnostics.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/accesslog() — Defines structured delivery diagnostics.
- https://developer.apple.com/library/archive/technotes/tn2436/_index.html — Apple troubleshooting guide for diagnosing HLS playback issues.
