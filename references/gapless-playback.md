# Gapless and Near-Gapless Playback

Treat gapless playback as an app objective with measurable policy targets.

## Baseline Strategy

- Use `AVQueuePlayer` for sequential playback.
- Create `AVPlayerItem` objects ahead of transition points.
- Set `preferredForwardBufferDuration` as a buffering preference.

```swift
let items = urls.map { url -> AVPlayerItem in
    let item = AVPlayerItem(url: url)
    item.preferredForwardBufferDuration = 2.0
    return item
}
let queue = AVQueuePlayer(items: items)
```

## Looping vs Playlist

- Use `AVPlayerLooper` for one-source loops.
- Use `AVQueuePlayer` plus app queue identity for playlists.

## Transition Policy

Define explicit app policy for transition tolerance (for example, acceptable audio/video delta at boundaries) and tune item preparation around that target.

## Do / Avoid

- Do prebuild items before expected transitions.
- Do treat `preferredForwardBufferDuration` as a hint.
- Do keep playlist identity in app state.
- Avoid stating a universal zero-gap guarantee.

Sources:
- https://developer.apple.com/documentation/avfoundation/avqueueplayer — Defines sequential playback queue behavior.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/preferredforwardbufferduration — Defines forward-buffer duration preference on items.
- https://developer.apple.com/documentation/avfoundation/avplayerlooper — Defines looping support for repeated playback.
- https://developer.apple.com/documentation/http-live-streaming/hls-authoring-specification-for-apple-devices — Defines HLS authoring constraints that influence transition quality.
