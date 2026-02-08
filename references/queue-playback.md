# Queue Playback with AVQueuePlayer

`AVQueuePlayer` gives forward queue advancement; app policy should define previous/jump behavior.

## Core Model

```swift
let player = AVQueuePlayer(items: items)
player.play()
player.advanceToNextItem()
```

Use app-owned identity (`id`, index, URL) as the source of truth. Treat queue contents as a projection of that state.

## Previous and Jump Policy

A deterministic policy for previous/jump operations:

1. Update app-owned index.
2. Rebuild queue from the target item.
3. Re-attach observers/time observers.
4. Resume transport according to user intent.

## Lifecycle Checklist

- Keep one owner for queue mutations.
- Remove observer tokens before replacing queue/player objects.
- Re-register end-of-item and timeline observers after rebuild.

## Do / Avoid

- Do keep queue identity separate from `AVQueuePlayer` internals.
- Do rebuild from index for previous/jump operations.
- Avoid inferring historical queue state from player internals.
- Avoid retaining stale observer tokens across queue rebuilds.

Sources:
- https://developer.apple.com/documentation/avfoundation/avqueueplayer — Defines queue-player API and sequencing behavior.
- https://developer.apple.com/documentation/avfoundation/avqueueplayer/advancetonextitem() — Defines forward-advance queue operation.
- https://developer.apple.com/documentation/avfoundation/avplayeritem — Defines queue elements and per-item state surfaces.
- https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/02_Playback.html — Provides Apple playback architecture context for queue lifecycle design.
