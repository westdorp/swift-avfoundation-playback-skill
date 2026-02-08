# Picture in Picture

PiP support is reliable when capability checks, controller ownership, and restore callbacks are explicit.

## Supported Integration Paths

- `AVPictureInPictureController` with `AVPlayerLayer` for custom playback UI
- `AVPlayerViewController` when AVKit-managed controls fit product needs

```swift
guard AVPictureInPictureController.isPictureInPictureSupported() else { return }
let controller = AVPictureInPictureController(playerLayer: playerLayer)
```

## Runtime Gating

Gate PiP affordances using runtime capability and state (`isPictureInPicturePossible`, active state).

## Restore Flow

Implement delegate restore callbacks so app UI restores when PiP stops.

```swift
func pictureInPictureController(
    _ controller: AVPictureInPictureController,
    restoreUserInterfaceForPictureInPictureStopWithCompletionHandler completionHandler: @escaping (Bool) -> Void
) {
    // Restore playback UI
    completionHandler(true)
}
```

## Background Policy

Align PiP behavior with your audio session and background capability policy.

## Do / Avoid

- Do treat PiP controller lifecycle as an owned resource.
- Do implement restore callbacks for predictable re-entry.
- Avoid showing PiP controls when runtime capability is unavailable.
- Avoid assuming identical PiP behavior on every device class.

Sources:
- https://developer.apple.com/documentation/avkit/avpictureinpicturecontroller — Defines PiP controller lifecycle and capability checks.
- https://developer.apple.com/documentation/avkit/avpictureinpicturecontrollerdelegate — Defines delegate callbacks for PiP transitions and UI restore.
- https://developer.apple.com/documentation/avkit/avplayerviewcontroller — Defines AVKit-managed playback UI path used with PiP-capable setups.
- https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/MediaPlaybackGuide/Contents/Resources/en.lproj/UsingAVKitPlatformFeatures/UsingAVKitPlatformFeatures.html — Provides Apple conceptual guidance for AVKit platform features including PiP.
- https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html — Documents background-mode keys related to playback app behavior.
