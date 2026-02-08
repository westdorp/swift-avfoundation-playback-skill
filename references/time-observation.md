# Time Observation for Playback UI

Time observers are lifecycle resources: attach intentionally, remove deterministically.

## Periodic Time Observer

Use periodic callbacks for elapsed-time UI and analytics cadence.

```swift
let token = player.addPeriodicTimeObserver(
    forInterval: CMTime(seconds: 0.5, preferredTimescale: 600),
    queue: .main
) { currentTime in
    // Update app progress model.
}
```

## Boundary Time Observer

Use boundary callbacks for milestones such as chapter markers and interstitial boundaries.

```swift
let markers = [
    NSValue(time: CMTime(seconds: 30, preferredTimescale: 600)),
    NSValue(time: CMTime(seconds: 60, preferredTimescale: 600))
]

let boundaryToken = player.addBoundaryTimeObserver(forTimes: markers, queue: .main) {
    // Handle marker transition.
}
```

## Progress Model Policy

Keep timeline inputs separate:

- Elapsed position from periodic observer/current time
- Bounds from duration and/or seekable ranges

This keeps VOD and live UI policies explicit.

## Cleanup Rule

- Store each token.
- Remove each token through `removeTimeObserver(_:)` on the same player.
- Clear tokens during teardown before player replacement.

## Do / Avoid

- Do choose callback queues intentionally.
- Do treat token removal as required cleanup.
- Avoid recreating observers without removing prior tokens.
- Avoid deriving live bounds from elapsed time alone.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/addperiodictimeobserver(forinterval:queue:using:) — Defines periodic callback registration and queue behavior.
- https://developer.apple.com/documentation/avfoundation/avplayer/addboundarytimeobserver(fortimes:queue:using:) — Defines boundary callback registration for time markers.
- https://developer.apple.com/documentation/avfoundation/avplayer/removetimeobserver(_:) — Defines mandatory observer-token cleanup.
- https://developer.apple.com/documentation/avfoundation/avplayer/currenttime() — Defines direct current-time sampling.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/seekabletimeranges — Defines timeline windows used with time-based UI policies.
