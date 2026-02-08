# Observation Setup for AVPlayer

Use one observation architecture per feature and keep lifecycle ownership explicit.

## Startup Configuration

Enable AVPlayer observation before creating players that your app will observe.

```swift
import AVFoundation

func configurePlaybackObservation() {
    if !AVPlayer.isObservationEnabled {
        AVPlayer.isObservationEnabled = true
    }
}
```

## Observation Surfaces

Observe the minimum signal set needed for UI and policy:

- `AVPlayerItem.status` for readiness and failure
- `AVPlayer.timeControlStatus` for runtime transport phase
- `AVPlayer.reasonForWaitingToPlay` for waiting context
- `AVPlayer.rate` for playback-rate intent alignment

For Swift Observation models, use `withObservationTracking` and route mutations to the main actor.

## Threading Rule

- If an API accepts a queue, choose `.main` when updating UI directly.
- If callbacks can arrive off-main, marshal UI state updates back to the main actor.

## Teardown Rule

On item or player replacement:

1. Cancel observation tasks.
2. Invalidate notification/KVO-style observers.
3. Remove time-observer tokens.

## Do / Avoid

- Do keep observation startup and teardown in one owner object.
- Do separate transport signals from user intent in app state.
- Avoid updating UI from unspecified callback threads.
- Avoid mixing multiple observation pipelines without clear ownership.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/isobservationenabled — Defines startup opt-in for AVPlayer observation.
- https://developer.apple.com/documentation/observation/withobservationtracking(_:onchange:) — Defines Swift Observation change tracking.
- https://developer.apple.com/documentation/avfoundation/avplayer/timecontrolstatus — Defines runtime transport states for observation models.
- https://developer.apple.com/documentation/avfoundation/avplayer/reasonforwaitingtoplay — Defines waiting reason context for transport state.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/status — Defines readiness and failure state on player items.
