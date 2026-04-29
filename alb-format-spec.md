# .alb Format Specification
### Version 0.1

---

## Overview

The `.alb` format is an open container format for distributing albums as a single self-contained file. It is designed to preserve the album as an intentional artistic unit: audio, artwork, liner notes, and metadata together, in a way that is portable, permanent, and platform-independent.

An `.alb` file is a renamed ZIP archive. Any tool that can open a ZIP can inspect its contents. No proprietary compression. No DRM. No encoding lock-in.

The format is released into the public domain. Anyone may implement, extend, or build tooling around `.alb` without restriction or permission.

---

## File Structure

```
myalbum.alb
├── album.json          : required, metadata and track manifest
├── audio/
│   ├── 01-track.mp3    : required, audio files prefixed with track number
│   ├── 02-track.mp3
│   └── ...
├── artwork/
│   ├── cover.jpg       ← required: front cover, minimum 1400x1400px
│   ├── back.jpg        : optional
│   └── ...             : optional: insert scans, photos, etc.
└── docs/
    ├── liner-notes.pdf : optional but encouraged
    └── ...             : optional: lyrics sheet, credits, booklet pages
```

---

## album.json Schema

```json
{
  "format": "alb",
  "version": "0.1",

  "album": {
    "title": "Album Title",
    "artist": "Artist Name",
    "label": "Label Name",
    "catalog_number": "LABEL-001",
    "release_year": 2025,
    "genre": ["Indie Rock"],
    "cover": "artwork/cover.jpg"
  },

  "tracks": [
    {
      "number": 1,
      "title": "Track Title",
      "file": "audio/01-track-title.mp3",
      "duration_seconds": 214,
      "credits": "Written by Artist Name. Produced by Producer Name.",
      "lyrics": "Optional lyrics text.\nLine breaks preserved."
    }
  ],

  "credits": {
    "produced_by": "Producer Name",
    "recorded_at": "Studio Name, City",
    "recorded_year": 2025,
    "mixed_by": "Name",
    "mastered_by": "Name",
    "artwork_by": "Name",
    "additional": "Any freeform credits text."
  },

  "links": {
    "label_url": "https://yourlabel.com",
    "forum_url": "https://yourlabel.com/forum/albums/label-001",
    "purchase_url": "https://yourlabel.com/albums/album-title"
  }
}
```

All fields except `format`, `version`, `album.title`, and `tracks` are optional. Fields not defined in this spec version should be ignored by compliant players rather than causing errors.

---

## Metadata Authority

The `album.json` file is the authoritative source of all metadata for an `.alb` release. Embedded file metadata (ID3 tags, Vorbis comments, EXIF data, or any other format-level tags) is ignored by compliant players. This ensures consistency across all tracks and all players, regardless of how the source audio files were tagged.

This means `.alb` files can be built from untagged or incorrectly tagged audio files without issue. The maker defines the record. The files are the payload.

---

## Audio File Requirements

- **Accepted formats:** MP3 (minimum 320kbps), WAV, FLAC, AIFF, M4A, OGG
- **Naming convention:** `[zero-padded-number]-[slug].ext`, e.g. `01-opening-track.mp3`
- **Track numbers:** Zero-padded for proper alphabetical sorting: `01`, `02`, not `1`, `2`
- **Multiple formats:** A future spec version may define a `/audio/lossless/` subdirectory for including both a compressed and lossless version of each track.

---

## Artwork Requirements

- `cover.jpg` is required. Minimum 1400x1400px. JPEG or PNG accepted.
- Additional image files may be included in `artwork/` and referenced in `album.json` or displayed at the player's discretion.

---

## Docs Directory

The `docs/` directory is freeform. Suggested contents:

- `liner-notes.pdf`: full liner notes as designed
- `lyrics.pdf`: lyrics sheet, if not embedded per-track in the JSON
- Additional PDFs, plain text files, or images at the artist's discretion

A compliant player should make any file in `docs/` accessible to the listener.

---

## Player Compliance

A compliant `.alb` player must:

- Open `.alb` files locally without requiring network access
- Parse `album.json` and construct the track listing from it
- Play audio files in declared order with standard transport controls (play, pause, skip, seek)
- Display cover artwork
- Display per-track credits and lyrics if present in the JSON
- Make files in the `docs/` directory accessible to the listener

A compliant `.alb` player must not:

- Require an account or authentication to open a local file
- Require internet access to play a file
- Modify the contents of an `.alb` file during playback
- Read or apply embedded audio file metadata in preference to `album.json`

---

## Extensibility

Fields not defined in this spec should be ignored gracefully. Future versions of this spec may add:

- `/video/`: optional music video or short film
- `/stems/`: isolated stems for remix or archival use
- Purchase token support for unlocking bonus content
- Cryptographic signatures for verifying release authenticity

---

## Reference Implementation

The reference player and maker are available at:

- **ALB Player**: opens and plays `.alb` files in the browser
- **ALB Maker**: packages audio, artwork, and liner notes into a `.alb` file

Both are open source, browser-based, and require no installation. Source and downloads at [github.com/quiettheory/alb](https://github.com/quiettheory/alb).

---

## File Extension Notes

The `.alb` extension may conflict with other software on some operating systems. Players should register the extension at install time using standard OS mechanisms. A future spec update will include platform-specific file association guidance for macOS, Windows, and Linux.

---

## License

This specification is released into the public domain under the [Creative Commons Zero (CC0)](https://creativecommons.org/publicdomain/zero/1.0/) dedication. No rights reserved.

---

Version 0.1. Published 2026. Originated by Quiet Theory Records.
Feedback and contributions welcome at [forum.quiettheory.com](https://forum.quiettheory.com).
