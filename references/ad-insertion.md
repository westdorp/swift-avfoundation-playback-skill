# Ad Insertion (CSAI, SSAI, SGAI)

Ad insertion architecture is a product and infrastructure policy decision. Keep timeline semantics explicit in client state.

## Architecture Policy

- CSAI: client app fetches and plays ad media.
- SSAI: server stitches ad and content before delivery.
- SGAI/interstitials: server guidance with client-side interstitial APIs.

## Interstitial API Surface

```swift
let event = AVPlayerInterstitialEvent(
    primaryItem: item,
    time: CMTime(seconds: 30, preferredTimescale: 600)
)
event.templateItems = [AVPlayerItem(url: adURL)]

let controller = AVPlayerInterstitialEventController(primaryPlayer: player)
controller.events = [event]
```

Use item-level interstitial ranges and integrated timeline views where available in your target OS range.

## Timeline Policy

Define policy for ad-boundary seeks and scrubber behavior:

- Whether seeks can cross ad boundaries
- How ad occupancy is represented in timeline UI
- How ad and content analytics are correlated in one coordinate space

## Do / Avoid

- Do keep ad/content timeline mapping explicit.
- Do separate ad failure handling from content failure handling.
- Do keep seek-boundary policy consistent across controls.
- Avoid mixing timeline coordinate spaces in analytics/state.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayerinterstitialevent — Defines interstitial event modeling.
- https://developer.apple.com/documentation/avfoundation/avplayerinterstitialeventcontroller — Defines interstitial event scheduling controller.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/interstitialtimeranges — Defines item-level interstitial time-range inspection.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/integratedtimeline — Defines integrated timeline API for unified playback timelines.
- https://developer.apple.com/streaming/GettingStartedWithHLSInterstitials.pdf — Official Apple HLS interstitial implementation guidance.
- https://developer.apple.com/videos/play/wwdc2022/10145/ — Apple session guidance for HLS interstitial workflows.
