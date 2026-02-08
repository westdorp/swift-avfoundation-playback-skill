# Coordinated Playback

When controlling multiple players, keep participation and command fan-out explicit in app state.

## Coordination Model

- Give each player a stable identity.
- Keep participation (`active`, `suspended`) in app policy state.
- Broadcast transport commands only to active members.

```swift
func playAll(_ players: [UUID: AVPlayer], activeIDs: Set<UUID>) {
    for (id, player) in players where activeIDs.contains(id) {
        player.play()
    }
}
```

## Audio Leadership Policy

For multi-video layouts, define one audio leader and mute non-leaders.

```swift
func applyLeader(_ leader: UUID?, players: [UUID: AVPlayer]) {
    for (id, player) in players {
        player.isMuted = (id != leader)
    }
}
```

## Do / Avoid

- Do keep a deterministic registry of players.
- Do apply idempotent group commands.
- Avoid deriving group membership from view hierarchy state.
- Avoid leaving removed players with active observers/tasks.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer — Defines per-player transport controls used for group coordination.
- https://developer.apple.com/documentation/avfoundation/avplayer/ismuted — Defines mute control used for audio-leader policy.
- https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/02_Playback.html — Provides Apple playback model context for multi-player orchestration.
