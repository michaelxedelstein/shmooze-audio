# Shmooze prayer recordings

Hebrew and English readings of the 52 prayers in the Shmooze app, rendered with ElevenLabs and re-encoded to 64 kbps mono. The manifest lives in the app at `src/data/prayerAudio.ts`.

## Who reads

The voice follows the prayer's gender tag in the app (`src/data/prayers.ts`):

| | Hebrew (eleven_v3) | English (eleven_multilingual_v2) |
|---|---|---|
| **Prayers women say**: the three candle lightings, Birkat HaChodesh | Miriam | Jenifer |
| **Everything else**: men's prayers and prayers said by all | Jason | Seth |

The English is a plain, conversational read. v1's English, on the more theatrical v3 model, came across like a storyteller. Hebrew stays on v3 because v2 has no Hebrew.

`voices.json` records the voices and which prayers take a woman's. The app's `tests/prayerAudio.test.ts` fails if the manifest and the tags ever disagree.

## Versions

- `v1` (release assets): all 52 prayers read by Miriam (Hebrew) and Rachel (English).
- `v2/` (this folder): as above. Miriam's Hebrew for the women's prayers is v1's file unchanged; everything else is new. The app pins each URL to the commit that added it, so a phone never plays a stale copy.

## Remaking a recording

1. Take the prayer's lines from the app, drop parenthesised stage directions (the app's `spokenLines`), join them with line breaks, and for Hebrew read the Name as Adonai (`יְהוָה` and `יְיָ` become `אֲדֹנָי`).
2. Render it in the voice and model `voices.json` gives it, one take.
3. `FFMPEG=… python tools/build.py encode <raw dir> v2` re-encodes to 64 kbps mono.
4. Commit, then `python tools/build.py manifest v2 <commit sha>` prints the app's `src/data/prayerAudio.ts`.
