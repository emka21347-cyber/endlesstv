# ENDLESSTV

A television network built from public-domain film and broadcast history. 99 channels of old
movies, cartoons, newsreels and commercials, running on a fixed schedule whether anyone is
watching or not. You don't pick a video — you turn on the set and watch what's on.

**[www.endlesstv.ch](https://www.endlesstv.ch)**

No video on demand, no accounts, no database — the catalogue is one flat metadata file.

## How the schedule works

The schedule is a pure function:

```
(stationId, utcTimestamp, seed) → { currentItem, offset, upNext[] }
```

Same result on the server and in the browser, so every visitor sees the same thing at the same
moment without any shared state. The clock is the only input. That keeps the site static to
serve, and makes the engine testable — the tests assert that two clients can't diverge.

Joining mid-programme seeks to `now − programStart`. After a stall, it re-seeks to the live
position rather than resuming where it stopped.

## The TV

`PowerOff → PowerOn → WarmUp → Broadcast ↔ Standby → SignOff`.

Channel changes aren't instant: picture tear, static, rolling distortion, channel number, station
ident, programme. Sometimes a longer signal acquisition, colour bars, or technical difficulties.

## Media

Metadata only — video streams from the source archive, with mirror failover.

Ingest rules:

- Public domain or open licence only, verified per item; licence and provenance stored in the
  metadata, attribution on a credits page.
- Uploaded to archive.org is not a licence. Items without a verifiable one are rejected at
  ingest.
- Soundtrack rights checked separately from film rights — a public-domain film can carry an
  encumbered score.

## Assets

Station logos, bugs, idents, test patterns and CRT overlays were generated locally, with prompt,
seed and workflow saved next to each asset so they can be reproduced. Library tagging also ran
through local models, with deterministic rules on top.

None of that runs at request time. The site itself has no AI dependency.

## Numbers

| | |
|---|---|
| Items catalogued | 4,256 |
| Cleared for air | ~3,149 |
| Channels | 99 |
| Layout | 94 `era × genre` cells across 8 era-networks, plus 5 Independents |
| Tests | 25 |

Channels are derived, not hand-built: the pool is filtered by era and genre, with era-first
fallback so a thin cell doesn't go dark. Everything is flat metadata, so a much larger library
needs no structural change.

## Stack

Next.js 15.5, React 19, TypeScript, Vitest. HTML5 video with mirror failover. CRT overlay in
WebGL/canvas with a CSS fallback. Station and guide metadata are server-rendered so the site is
indexable.


## Controls

`Space` surf · `↑/↓` channel · `G` guide · `F` fullscreen · `M` mute · `Backspace` last channel ·
`P` power. All on-screen too; in landscape, tap the left or right edge to change channel.

## Code

The source code is private; this repository is the project page. The site is live at
[www.endlesstv.ch](https://www.endlesstv.ch). Library items are public domain or openly licensed, each
recorded with provenance and attribution.
