# Flagel — Guess the Flag 🏳️

A browser-based flag guessing game with a twist: instead of showing hints as text, **guessing a country reveals whatever part of the hidden flag visually overlaps with that country's own flag** — same color, same position. Guess enough countries and the hidden flag declassifies itself piece by piece.

Single self-contained `.html` file. No build step, no server, no internet connection required — just open it in a browser.

## How it works

- Every flag is rasterized onto a fixed-size grid.
- When you guess a country, its flag is compared cell-by-cell against the hidden target flag.
- Any cell where the colors match (within a tolerance, since real flag colors aren't pixel-identical across sources) is revealed on the target.
- No inversion, no repositioning — a match only counts if the color lines up in the *same spot*. Guessing Indonesia against Poland reveals nothing, since red-over-white and white-over-red don't overlap; guessing Indonesia against Monaco reveals almost everything.
- Keep guessing until you know the answer, then type it in to win.

## Features

- **197 recognized flags** (193 UN member states + 2 UN observers + Taiwan + Kosovo)
- **Three difficulty tiers**, each with its own grid resolution — Easy (150 cells), Medium (294 cells), Hard (486 cells) — so harder tiers reveal in finer, harder-to-read pieces
- **Autocomplete guess input** with keyboard navigation (arrow keys + Enter)
- **Guess log** showing each attempt, how many new cells it revealed, and running completion %
- **Give up / Skip** controls
- No dependencies, no network calls, no tracking — everything (including flag images) is embedded directly in the HTML file

## Running it

Just open `flag-recon.html` in any modern browser (Chrome, Firefox, Edge, Safari). Double-clicking the file works fine — no local server needed, since all assets are embedded as base64 data URIs.

## Project structure

This repo intentionally ships as a **single HTML file** for easy distribution — you can drop it anywhere, email it, or serve it as a static file with zero configuration.

If you want to modify or rebuild it from source, the generation pipeline looked like this:

```
build.js            # rasterizes flag-icons SVGs → base64 PNGs, tagged with difficulty
game_template.html  # game markup/CSS/JS with a __FLAGS_JSON__ placeholder
assemble script      # injects flags_data.json into the template → flag-recon.html
```

(The build scripts aren't included in this distribution — only the final assembled file. Recreate them if you want to swap in new flags, art, or a different difficulty split.)

## Customization ideas

- **Grid density** — tune `TIER_GRID` in the `<script>` block to make reveals coarser or finer per tier.
- **Color tolerance** — `TOLERANCE` controls how close two colors need to be to count as a match.
- **Difficulty tiers** — each flag carries a `diff` tag (1 = easy, 2 = medium, 3 = hard) in the embedded flag data; reassign as you see fit.
- **Background art** — the page background is a single base64-encoded image baked into the CSS; swap it for your own by re-encoding an image and replacing the data URI.

## License

Flag artwork sourced from the [flag-icons](https://github.com/lipis/flag-icons) project (MIT licensed). Game code is free to use, modify, and redistribute.
