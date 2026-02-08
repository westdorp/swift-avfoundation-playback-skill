# HLS Streaming

Use Apple HLS APIs for timeline control, bitrate policy, and delivery diagnostics.

## Playback Baseline

```swift
let player = AVPlayer(url: streamURL)
player.play()
```

Set playback policy with item-level knobs, then derive UI from timeline/runtime signals.

## Bitrate Policy

```swift
player.currentItem?.preferredPeakBitRate = 1_500_000 // App policy cap
player.currentItem?.preferredPeakBitRate = 0          // System-managed selection
```

Treat bitrate caps as product policy inputs.

## Live and DVR Timelines

Use `seekableTimeRanges` as the seek window for live/DVR UX.

```swift
if let range = player.currentItem?.seekableTimeRanges.first?.timeRangeValue {
    let start = range.start
    let end = range.start + range.duration
    // Clamp user seeks to [start, end]
}
```

## Diagnostics Inputs

Use `accessLog()` and `errorLog()` for delivery and failure context.

- Access log: rendition, throughput, stall counters, bitrate history
- Error log: protocol/media failure details

## Do / Avoid

- Do clamp live seeks to current seekable windows.
- Do make bitrate limits explicit app policy.
- Do triage incidents with both access and error logs.
- Avoid classifying live/VOD solely from one property.

Sources:
- https://developer.apple.com/documentation/http-live-streaming/hls-authoring-specification-for-apple-devices — Defines HLS authoring rules that affect client playback behavior.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/seekabletimeranges — Defines current seekable timeline windows for HLS live/DVR media.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/preferredpeakbitrate — Defines item-level peak bitrate preference.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/accesslog() — Defines delivery telemetry retrieval.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/errorlog() — Defines playback error telemetry retrieval.
- https://developer.apple.com/library/archive/technotes/tn2436/_index.html — Provides Apple troubleshooting workflow for HLS stream failures.
