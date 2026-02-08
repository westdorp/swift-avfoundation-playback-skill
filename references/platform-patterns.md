# Platform Playback Patterns

Use one shared playback core and thin platform adapters for AVKit surfaces and platform UX.

## Shared Core

Keep these concerns platform-agnostic:

- Asset/item/player lifecycle
- Seeking and timeline policy
- Intent/readiness/runtime state derivation
- Retry and failure classification

## Platform Adapters

Use platform-native AVKit surfaces:

- iOS/tvOS: `AVPlayerViewController`
- macOS: `AVPlayerView`
- visionOS: AVKit playback surfaces appropriate to window/immersive design

Route picker, remote commands, and PiP integration belong in adapter-level policy.

## Do / Avoid

- Do keep transport and timeline rules in shared code.
- Do isolate UI container differences in platform adapters.
- Avoid duplicating playback state machines per platform.
- Avoid platform-specific forks for core lifecycle logic.

Sources:
- https://developer.apple.com/documentation/avkit/avplayerviewcontroller — Defines AVKit controller used on iOS/tvOS-style playback surfaces.
- https://developer.apple.com/documentation/avkit/avplayerview — Defines AppKit-native playback view for macOS.
- https://developer.apple.com/documentation/avkit/avroutepickerview — Defines system route-picker UI for external playback routing.
- https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/MediaPlaybackGuide/Contents/Resources/en.lproj/UsingAVKitPlatformFeatures/UsingAVKitPlatformFeatures.html — Apple conceptual guidance for AVKit feature usage across platforms.
