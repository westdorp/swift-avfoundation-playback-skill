# Audio Session, Interruptions, and Route Changes

Audio session behavior should be deterministic because category, activation, and interruption handling are app policy.

## Baseline Session Setup

```swift
import AVFAudio

let session = AVAudioSession.sharedInstance()
try session.setCategory(.playback, mode: .moviePlayback)
try session.setActive(true)
```

Select category and mode according to your product policy.

## Interruption Policy

Observe interruption notifications and apply explicit resume rules.

```swift
NotificationCenter.default.addObserver(
    forName: AVAudioSession.interruptionNotification,
    object: AVAudioSession.sharedInstance(),
    queue: .main
) { notification in
    // Read interruption type/options and apply policy.
}
```

Recommended policy:

1. On interruption begin: capture `wasPlaying`, then pause.
2. On interruption end: resume only when options/context permit.

## Route-Change Policy

Observe route changes and branch behavior by reason.

```swift
NotificationCenter.default.addObserver(
    forName: AVAudioSession.routeChangeNotification,
    object: AVAudioSession.sharedInstance(),
    queue: .main
) { notification in
    // Handle route-change reason and recompute route UI.
}
```

Recommended policy for `oldDeviceUnavailable`: pause and require explicit user re-engagement.

## Background Checklist

- Keep playback category active when background playback is part of product policy.
- Keep Now Playing metadata synchronized with transport state.
- On foreground return, recompute UI from current player and audio-session state.

## Do / Avoid

- Do centralize interruption and route logic in one owner.
- Do preserve user intent across interruption boundaries.
- Avoid implicit auto-resume rules.
- Avoid stale route/session UI after scene transitions.

Sources:
- https://developer.apple.com/documentation/avfaudio/avaudiosession — Defines categories, modes, activation, and runtime session APIs.
- https://developer.apple.com/documentation/avfaudio/avaudiosession/interruptionnotification — Defines interruption notification contract.
- https://developer.apple.com/documentation/avfaudio/avaudiosession/routechangenotification — Defines route-change notification contract.
- https://developer.apple.com/library/archive/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/AudioSessionBasics/AudioSessionBasics.html — Provides Apple conceptual guidance for audio-session behavior.
- https://developer.apple.com/library/archive/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/AudioSessionCategoriesandModes/AudioSessionCategoriesandModes.html — Describes category/mode behavior tradeoffs.
- https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html — Documents background-mode Info.plist keys used by playback apps.
