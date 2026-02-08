# Stall Detection and Recovery

Define a recovery ladder in app policy instead of treating every wait as a hard failure.

## Stall Signals

Use these together:

- `AVPlayer.timeControlStatus`
- `AVPlayer.reasonForWaitingToPlay`
- `AVPlayerItemAccessLogEvent.numberOfStalls`
- `AVPlayer.automaticallyWaitsToMinimizeStalling`

## Recovery Ladder (App Policy)

1. Show waiting UI when status is `.waitingToPlayAtSpecifiedRate`.
2. Keep waiting while transport can recover.
3. Escalate to bounded retry when waiting exceeds policy thresholds.
4. Replace item/player only when failures persist or status becomes `.failed`.

## Do / Avoid

- Do separate user pause intent from waiting states.
- Do correlate waiting episodes with access-log stall data.
- Do use bounded retry/backoff rules.
- Avoid infinite reload loops.

Sources:
- https://developer.apple.com/documentation/avfoundation/avplayer/timecontrolstatus — Defines paused/playing/waiting runtime transport states.
- https://developer.apple.com/documentation/avfoundation/avplayer/reasonforwaitingtoplay — Defines waiting-state reasons.
- https://developer.apple.com/documentation/avfoundation/avplayer/automaticallywaitstominimizestalling — Defines the stalling-minimization behavior switch.
- https://developer.apple.com/documentation/avfoundation/avplayeritemaccesslogevent/numberofstalls — Defines stall counter metric on access-log events.
