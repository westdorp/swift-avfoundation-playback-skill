# Now Playing and Remote Commands

Now Playing metadata and remote controls should be managed by one active playback owner.

## Publish Metadata

```swift
import MediaPlayer

MPNowPlayingInfoCenter.default().nowPlayingInfo = [
    MPMediaItemPropertyTitle: title,
    MPMediaItemPropertyPlaybackDuration: duration,
    MPNowPlayingInfoPropertyElapsedPlaybackTime: elapsed,
    MPNowPlayingInfoPropertyPlaybackRate: rate,
]
```

Update metadata on play/pause, seeks, and item transitions.

## Register Remote Commands

```swift
let commandCenter = MPRemoteCommandCenter.shared()

commandCenter.playCommand.addTarget { _ in
    // Start playback
    return .success
}

commandCenter.pauseCommand.addTarget { _ in
    // Pause playback
    return .success
}
```

## Session Ownership

Use `MPNowPlayingSession` when coordinating multiple playback participants under one active now-playing context.

## Cleanup Policy

- Remove command handlers when playback context ends.
- Clear stale metadata when there is no active item.

## Do / Avoid

- Do keep metadata aligned with real player state.
- Do scope command handlers to active context ownership.
- Avoid leaving handlers registered after teardown.
- Avoid publishing stale elapsed/rate values.

Sources:
- https://developer.apple.com/documentation/mediaplayer/mpnowplayinginfocenter — Defines the Now Playing metadata center.
- https://developer.apple.com/documentation/mediaplayer/mpremotecommandcenter — Defines remote command registration APIs.
- https://developer.apple.com/documentation/mediaplayer/mpremotecommand — Defines command objects returned by command center properties.
- https://developer.apple.com/documentation/mediaplayer/mpnowplayingsession — Defines session-scoped ownership for now-playing state.
