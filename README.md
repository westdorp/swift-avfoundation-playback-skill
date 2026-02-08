# Swift AVFoundation Playback Skill

AI coding agent skill for **Swift AVFoundation playback** on **iOS, macOS, tvOS, and visionOS**.  
This repository packages practical playback guidance for `AVPlayer`, `AVPlayerItem`, HLS live/VOD timelines, AirPlay routing, audio session handling, Picture in Picture (PiP), FairPlay DRM, ad insertion, and playback error recovery.

## Why Use This Skill

- Fundamentals-first playback architecture: `AVAsset` -> `AVPlayerItem` -> `AVPlayer`
- Clear seek and live timeline guidance for HLS playback
- Production-focused platform integrations (AirPlay, Now Playing, PiP, audio session)
- Apple-cited reference files organized by playback concern

## What It Covers

- AVPlayer lifecycle and teardown safety
- Seeking semantics, tolerances, and live edge strategy
- Observation and time observation patterns
- Queue playback and coordinated multi-player control
- HLS streaming, stall recovery, and error handling
- FairPlay DRM and HLS interstitial ad insertion
- Cross-platform AVKit patterns for iOS/macOS/tvOS/visionOS

## Quick Start

1. Open [`SKILL.md`](SKILL.md).
2. Follow the read order:
   - [`references/player-item-lifecycle.md`](references/player-item-lifecycle.md)
   - [`references/seeking-and-timelines.md`](references/seeking-and-timelines.md)
   - [`references/observable-setup.md`](references/observable-setup.md)
   - [`references/time-observation.md`](references/time-observation.md)
   - [`references/intent-tracking.md`](references/intent-tracking.md)
3. Load additional reference files only for the feature you are implementing.

## Core Topics

- Playback lifecycle: [`references/player-item-lifecycle.md`](references/player-item-lifecycle.md)
- Seeking and timelines: [`references/seeking-and-timelines.md`](references/seeking-and-timelines.md)
- HLS streaming: [`references/hls-streaming.md`](references/hls-streaming.md)
- Audio session and interruptions: [`references/audio-session.md`](references/audio-session.md)
- AirPlay routing: [`references/airplay-routing.md`](references/airplay-routing.md)
- Picture in Picture: [`references/picture-in-picture.md`](references/picture-in-picture.md)
- FairPlay DRM: [`references/fairplay-drm.md`](references/fairplay-drm.md)
- Ad insertion: [`references/ad-insertion.md`](references/ad-insertion.md)
- Platform patterns: [`references/platform-patterns.md`](references/platform-patterns.md)

## Repository Layout

- `SKILL.md` - Skill trigger metadata and core usage instructions
- `references/` - Apple-cited deep dives for each playback topic
- `LICENSE` - Project license

## Keywords

Swift AVFoundation, AVPlayer, AVPlayerItem, HLS streaming, live streaming, AirPlay, AVAudioSession, Picture in Picture, FairPlay DRM, media playback, iOS video player, tvOS playback, macOS AVKit, visionOS playback
