# Zhao — Animated ChatGPT Pets Companion

[中文说明](README.md)

This repository contains the custom pet asset for the built-in Pets feature in Codex / ChatGPT Work. It is not a standalone desktop-pet application or an executable program.

## Quick start

1. Download and extract `zhao-pets-upload-bundle.zip`, or download `zhao-pet-v2.png` directly.
2. In the custom-pet flow in Codex / ChatGPT Work Pets, upload `zhao-pet-v2.png`.
3. Follow the interface to name and save/select the pet.

Pets accepts a sprite-sheet PNG/WebP. The ZIP is a convenient download bundle; it cannot be uploaded directly into Pets.

## Files

- `zhao-pets-upload-bundle.zip` — the upload sheet and Chinese/English instructions.
- `zhao-pet-v2.png` — Pets v2 sprite sheet, transparent RGBA, 1536 × 2288 px, 8 columns × 11 rows, 192 × 208 px per cell.
- `animation-preview.gif` — animation preview across the states.
- `look-directions.png` — labeled review sheet with the neutral pose and all 16 look directions.

## Where the 16 look directions are

All 16 directions are already in `zhao-pet-v2.png`; they are not separate install images. Rows 10 and 11 (one-based) each contain eight frames. Row 10 runs from 000° through 157.5°, and row 11 from 180° through 337.5°, in 22.5° steps. See `look-directions.png` for the labels and per-frame preview.

![Neutral pose and all 16 look directions](look-directions.png)

## Validation

The sheet passed the Pets v2 structural preflight and quality validation. It uses transparent RGBA, with dimensions, grid, and required frames matching the v2 atlas format. SHA-256:

```text
bb7c4129ea4f415035494be44299ed0477bf65ff6e270418c726f460fda848eb
```

## Provenance and rights

This is an unofficial, AI-assisted fan-made pet asset based on character-design and in-game references supplied by the maintainer. Those reference screenshots are not redistributed here. Rights to the game, characters, names, trademarks, and related source material remain with their respective owners. This repository grants no license to that third-party material and is not affiliated with or endorsed by HoYoverse. No general-purpose open-source license is included. Please review [HoYoverse's fan-made content guidelines](https://support.hoyoverse.com/hc/en-us/articles/51005649400729-What-are-the-guidelines-for-creating-and-selling-fan-made-content).