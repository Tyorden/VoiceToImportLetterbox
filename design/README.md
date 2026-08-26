# Design

All design work for the Voice Logger app, generated from [`docs/DESIGN_BRIEF.md`](../docs/DESIGN_BRIEF.md).

## v1 — Claude Design export (Aug 25, 2026)
- `Voice Logger Artboards.dc.html` — open in a browser (needs `support.js` alongside). Artboards 1a–1d desktop, 1e mobile, 1f component sheet.
- `Letterboxd Voice Logger.zip` — original export, re-importable into Claude Design.
- `preview.png` — thumbnail.

## v2 — Iterations (`iterations/`)
Editable canvas: https://claude.ai/code/artifact/72b3b276-981f-46ea-891b-997dae6a0a4b
(`iterations/voice-logger-iterations.html` is the same canvas as a standalone file — open it in a browser.)

Working artboard sources (edit these, then re-seed):

| File | What |
|------|------|
| `Main.dc.html` | **Lead** — refined split view: transcript + mic dock left, staging table with inline disambiguation and match badges right |
| `MobileSession.dc.html` | Lead — mobile (390×844), voice-first, table collapses to cards |
| `ReviewExport.dc.html` | Lead — review & export: editable table, validation warning, CSV preview, download CTA |
| `MarqueeWarm.dc.html` | Direction B — warm amber, serif "marquee" titles, ticket rows |
| `PaperLedger.dc.html` | Direction C — light paper/index-card ledger, mono, max scannability |
| `TranscriptFirst.dc.html` | Direction D — chat is the product; films appear as inline cards |
| `V1Original.dc.html` | v1 export, untouched, on its own page for comparison |
| `canvas.json` | Canvas layout, pages, sticky notes |
| `site-v1-backup.html` | The website before it was restyled to the lead design system |

## Design tokens (lead direction)
```
bg #0c0a10 · surface #131018 · surface-2 #171320 · border #262031
text #ece9f1 · muted #9b93a8 · faint #6f6880
accent #e061ab (ink #180a12) · accent-2 #7a4be0 · ok #5fd39a · warn #f0b35f · fuzzy #9d8df0
Sora (display) · IBM Plex Sans (body) · IBM Plex Mono (data)
```
The website (`index.html`) and the app (`app.html`) use these tokens. `app.html` implements the lead direction (Main / MobileSession / ReviewExport artboards).
