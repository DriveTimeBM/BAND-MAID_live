# BAND-MAID_live

A searchable archive of BAND-MAID OKYUJI (live performance) videos from BAND-MAID Prime, with full setlists and timestamp-linked song playback.

## Live

Hosted at: `https://drivetimebm.github.io/BAND-MAID_live/`

## Data sources

The page fetches three JSON files:

1. **`prime.json`** — `https://drivetimebm.github.io/BAND-MAID_gpt/prime/prime.json`
   All BAND-MAID Prime videos. Only entries with `Category === "OKYUJI"` are used.
2. **`setlists.json`** — `https://drivetimebm.github.io/BAND-MAID_prime/data/setlists.json`
   Per-video setlists keyed by Prime video ID.
3. **`aliases.json`** — local file in this repo
   User-editable list of fan nicknames for shows (e.g., "Zepp Tokyo" → "BAND-MAID WORLD DOMINATION TOUR 2018【宣告】").

Data is cached in the browser's `localStorage` for 24 hours. The **⟲ Refresh** button forces a fresh fetch.

## `aliases.json` format

```json
[
  {
    "showKey": "BAND-MAID WORLD DOMINATION TOUR 2018",
    "aliases": ["Zepp Tokyo", "Zepp Tokyo 2018", "WDT 2018"]
  }
]
```

- `showKey` is matched case-insensitively against the derived show title (substring match in either direction, so you don't need to match the exact title — a partial match like `"WORLD DOMINATION TOUR 2018"` is fine)
- `aliases` are the fan-friendly terms that should match when searched

## How shows are grouped

`prime.json` has one entry per video. Multi-part shows have titles like:
- `BAND-MAID OKYUJI (Sept 9, 2022) part 1`
- `BAND-MAID OKYUJI (Sept 9, 2022) part 2`

The page strips the `part N` suffix and parenthesized date from each title to derive a show key, then groups videos sharing the same key. Parts are sorted numerically within each show. Dates and venues are pulled preferentially from `setlists.json` (which is human-curated) and fall back to parsing the prime.json title if the setlist data isn't available.

## Search

The search box matches against:
- Show titles
- Venue names
- Fan aliases (from aliases.json)
- Individual song names within setlists
- Dates (ISO format or year)

When a song name matches, the show is highlighted with the match count, and clicking it selects the show at the right part + timestamp.

## URL state

Current view is reflected in the URL query string:
- `?show={showKey}` — selected show
- `?part={index}` — which part is shown
- `?t={seconds}` — timestamp to seek to
- `?q={query}` — search filter

Share or bookmark any state.

## Known limitations

- **Iframe embedding** may be blocked by BAND-MAID Prime's server (likely `X-Frame-Options: SAMEORIGIN` or similar). If the embed doesn't play, use the **Open on Prime ↗** button to open the video in a new tab. Everything else (setlists, search, timestamps) works regardless.
- Prime requires a paid subscription. The setlist navigation works without login, but the videos themselves need authenticated access.
- Timestamp seeking depends on Prime's player honoring URL fragments (`#t=NNN`). If it doesn't, the link opens the video from the beginning.
