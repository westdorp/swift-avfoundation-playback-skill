# Player-Item Lifecycle Fundamentals

AVFoundation playback is most reliable when object ownership and teardown are explicit.

## Object Model and Responsibilities

`AVAsset` / `AVURLAsset` -> `AVPlayerItem` -> `AVPlayer` -> AVKit UI (`AVPlayerViewController` or `AVPlayerView`) / `AVPlayerLayer`

- `AVAsset` and `AVURLAsset` represent media resources.
- `AVPlayerItem` represents presentation state for one asset.
- `AVPlayer` owns transport control for the active item.
- AVKit or `AVPlayerLayer` renders and controls UI.

## Readiness and Failure Surface

Use item and player status as readiness/failure inputs:

- `AVPlayerItem.status` for `.unknown`, `.readyToPlay`, `.failed`
- `AVPlayerItem.error` and `AVPlayer.error` for failure details

Treat user intent (`play`, `pause`, `seek`) as separate app state from readiness.

## What to Observe

Observe only the signals required by your UI and policy:

- Item readiness/failure (`AVPlayerItem.status`)
- Transport runtime (`AVPlayer.timeControlStatus`, `AVPlayer.rate`)
- Timeline updates (periodic and boundary time observers)
- End-of-item transitions (`AVPlayerItem.didPlayToEndTimeNotification`)

## Teardown Safety Checklist

Run this checklist whenever replacing items, replacing players, or leaving a screen:

1. Remove all periodic and boundary time observers from the same player that added them.
2. Invalidate KVO/observation tokens and notification observers.
3. Cancel async tasks used for observation, retries, or state derivation.
4. Clear current item ownership when appropriate (`replaceCurrentItem(with: nil)`).
5. Reset app-owned playback intent and derived UI phase.

## Do / Avoid

- Do model readiness, transport, and user intent as separate state lanes.
- Do treat observer tokens and tasks as lifecycle resources.
- Do keep UI updates on the main actor.
- Avoid binding UI truth to a single AVFoundation property.
- Avoid leaving observers active after item or player replacement.

Sources:
- https://developer.apple.com/documentation/avfoundation/avasset — Defines the base media-resource object in the playback pipeline.
- https://developer.apple.com/documentation/avfoundation/avurlasset — Defines URL-backed assets commonly used for network/local media.
- https://developer.apple.com/documentation/avfoundation/avplayeritem — Defines item-level readiness, failure, and presentation state.
- https://developer.apple.com/documentation/avfoundation/avplayer — Defines transport control and runtime playback state.
- https://developer.apple.com/documentation/avfoundation/avplayeritem/didplaytoendtimenotification — Defines the item-end notification surface.
- https://developer.apple.com/documentation/avfoundation/avplayer/removetimeobserver(_:) — Defines required cleanup for time-observer tokens.
