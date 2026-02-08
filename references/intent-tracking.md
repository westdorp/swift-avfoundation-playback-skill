# Intent Tracking and UI State

Keep UI stable by deriving UI phase from separate state lanes rather than from one transport property.

## Three-Lane State Model

Model these lanes independently:

- User intent: play/pause/seek requests
- Readiness: `AVPlayerItem.status`
- Runtime transport: `AVPlayer.timeControlStatus`, `AVPlayer.reasonForWaitingToPlay`, `AVPlayer.rate`

## App-Policy Derivation Example

```swift
enum UserIntent {
    case none
    case play
    case pause
    case seek
}

enum PlaybackUIPhase {
    case idle
    case preparing
    case playing
    case paused
    case waiting
    case failed(String)
}

func derivePhase(player: AVPlayer, intent: UserIntent) -> PlaybackUIPhase {
    guard let item = player.currentItem else { return .idle }

    if item.status == .failed {
        return .failed(item.error?.localizedDescription ?? "Playback failed")
    }
    if item.status == .unknown {
        return .preparing
    }

    switch player.timeControlStatus {
    case .playing:
        return .playing
    case .paused:
        return intent == .play ? .waiting : .paused
    case .waitingToPlayAtSpecifiedRate:
        return .waiting
    @unknown default:
        return .waiting
    }
}
```

## Buffering Basics

Model buffering as a transport condition, not as user intent.

- Intent says what the user asked for.
- Readiness says whether the item can play.
- Waiting/buffering says why transport is not advancing right now.

## Do / Avoid

- Do bind controls to your derived app phase.
- Do keep a deterministic mapping for every lane combination.
- Avoid toggling UI directly from transient waiting transitions.
- Avoid conflating pause intent with temporary waiting states.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/timecontrolstatus — Defines runtime transport states.
- https://developer.apple.com/documentation/avfoundation/avplayer/reasonforwaitingtoplay — Defines waiting-state context.
- https://developer.apple.com/documentation/avfoundation/avplayer/rate — Defines the playback-rate signal used in intent modeling.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/status — Defines item readiness/failure states.
