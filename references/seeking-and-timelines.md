# Seeking and Timelines Fundamentals

Seek behavior is deterministic when your app treats seek requests as intent and reconciles against AVFoundation timeline signals.

## Seek Semantics

Use `AVPlayer.seek(to:toleranceBefore:toleranceAfter:completionHandler:)` for user or programmatic seeks.

```swift
func requestSeek(_ player: AVPlayer, to target: CMTime) {
    player.seek(to: target, toleranceBefore: .zero, toleranceAfter: .zero) { finished in
        guard finished else { return }
        // Commit app state for the completed seek.
    }
}
```

If a newer seek supersedes an older seek, the older completion handler can report `finished == false`.

## Coalescing Policy

Apply a single policy for rapid scrub updates:

- Keep only the latest requested target.
- Cancel pending seeks when your scrubbing model requires explicit cancellation.
- Reconcile UI only from the most recent completed seek.

## Tolerances

Seek tolerances are precision/performance controls.

- Tight tolerances prioritize positional accuracy.
- Wider tolerances prioritize smoother transport and lower work.

Set tolerances as app policy per scenario (frame-accurate editor vs long-form streaming).

## Live and DVR Timelines

Use `seekableTimeRanges` as the primary seek window for live/DVR media.

```swift
func liveWindow(for item: AVPlayerItem) -> CMTimeRange? {
    item.seekableTimeRanges.first?.timeRangeValue
}
```

Treat `duration` as supporting context, not the only timeline classifier.

## Live-Edge Strategy

Use the end of `seekableTimeRanges` with an explicit holdback policy.

```swift
func seekNearLiveEdge(_ player: AVPlayer, holdbackSeconds: Double) {
    guard let range = player.currentItem?.seekableTimeRanges.first?.timeRangeValue else { return }
    let edge = range.start + range.duration
    let target = CMTimeSubtract(edge, CMTime(seconds: holdbackSeconds, preferredTimescale: 600))
    player.seek(to: target)
}
```

## Common Pitfalls

- Empty `seekableTimeRanges`: disable live scrub affordances until a range exists.
- Indefinite or changing duration: keep slider bounds driven by current timeline signals.
- UI drift during scrubbing: separate optimistic thumb movement from confirmed playback time.

## Do / Avoid

- Do treat seek completion as the commit point for timeline state.
- Do clamp live seeks to current seekable windows.
- Do keep a latest-intent policy for rapid scrubbing.
- Avoid undocumented shortcuts like seeking to conceptual infinity.
- Avoid using `duration` alone to drive live controls.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/seek(to:tolerancebefore:toleranceafter:completionhandler:) — Defines seek semantics, tolerances, and completion behavior.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/cancelpendingseeks() — Defines explicit cancellation of pending seek requests.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/seekabletimeranges — Defines the current seekable timeline windows.
- https://developer.apple.com/documentation/coremedia/cmtime — Defines the media-time representation used for seeks and timeline math.
- https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/MediaPlaybackGuide/Contents/Resources/en.lproj/ExploringAVFoundation/ExploringAVFoundation.html — Apple conceptual guidance for time and seeking in AVFoundation.
