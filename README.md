# Set Builder

Paste a client's song list → the app finds each track in your library → creates Serato crates per moment (Melodies, Formality, Slow dance, Cake cutting…), plus a report you can send or print.

## Run

```bash
npm install
npm start          # launch the app
npm test           # 24 tests for the core logic
npm run dist       # build a .dmg (macOS) / installer (Windows)
```

## How to use

1. **Setup** – name the event, add your music folder(s), click *Scan library* (first scan reads tags; later scans are cached and fast). Confirm the Serato folder (default `~/Music/_Serato_`).
2. **Paste the list.** Each moment is a header ending in `:`; songs are one per line.

   ```
   Melodies:
   Ed Sheeran - Perfect
   Formality:
   Clair de Lune - Debussy
   Slow dance:
   Kiss Me by Sixpence None The Richer
   Cake cutting:
   Sugar | play when the knife goes in
   ```
   `Artist - Title`, `Title by Artist`, or just `Title` all work. Text after `|` is a per-song note (shown in the report).
3. **Review.** Green = confident match, amber = check it (several candidates or a partial match), red = not in your library. Use the dropdown to pick another version, ▶ to preview, or type in the row's search box + Enter to search manually.
4. **Create Serato crates** – **close Serato DJ first**, then reopen it. You get a parent crate named after the event with one sub-crate per moment. Re-running backs up the previous crate as `.crate.bak`.
   - **Export report** – CSV, HTML, and one `.m3u8` playlist per moment.
   - **Copy tracks into folders** – numbered copies of the tracks, one folder per moment.

## Notes

- Matching handles accents, `feat.`, typos, tag-less files (uses the filename), duplicate copies of a song, and Arabic script (hamza/alef variants).
- Tracks on an external drive are written to that drive's `_Serato_` folder (Serato requires this); the drive needs to have been opened in Serato once.
- A crate can't hold per-track notes, so notes live in the crate name (the moment) and in the report/CSV.
- The renderer is sandboxed (context isolation, CSP), and the main process only accepts track paths that are inside your scanned folders.

## Verify once on your machine

The crate file is written to the community-documented Serato format and round-trips in the tests, but I could not open Serato from here. Before a real event, create a test crate and confirm it appears with the right tracks. If Windows paths look wrong in Serato, the fix is in `core/serato.js` (`cratePath`).
