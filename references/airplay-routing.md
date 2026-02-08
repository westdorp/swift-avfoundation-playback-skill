# AirPlay Routing

Use system routing controls and keep output-route behavior in app policy.

## Core Routing Controls

```swift
player.allowsExternalPlayback = true
let externalActive = player.isExternalPlaybackActive
```

Set external playback permissions by content policy.

## Route Picker UI

Use `AVRoutePickerView` for route selection.

```swift
import AVKit

let picker = AVRoutePickerView()
```

## Route-Change Policy

Observe route-change notifications and recompute UI from player/session state.

```swift
NotificationCenter.default.addObserver(
    forName: AVAudioSession.routeChangeNotification,
    object: AVAudioSession.sharedInstance(),
    queue: .main
) { _ in
    // Recompute app route state.
}
```

## Do / Avoid

- Do use Apple’s route picker UI.
- Do keep route state in app model for restoration.
- Avoid custom route-selection UIs that bypass system controls.
- Avoid assuming a single route-change reason implies one UX outcome.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/allowsexternalplayback — Defines whether external playback is allowed.
- https://developer.apple.com/documentation/avfoundation/avplayer/isexternalplaybackactive — Defines runtime signal for active external playback.
- https://developer.apple.com/documentation/avkit/avroutepickerview — Defines the system route-picker control.
- https://developer.apple.com/documentation/avfoundation/supporting-airplay-in-your-app — Provides Apple guidance for AirPlay support in apps.
