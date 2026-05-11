# SolQuicks Live — OBS Avatar Studio 🦊

A simple, single-page live avatar tool for the Most Famous Fox. Real-time lip sync, auto-expressions driven by mic input, and a one-click OBS Mode for clean window capture.

Built as a stripped-down sibling of [BHB Live](https://bigheadbillionaires.com/live/) — same expression engine and lip-sync algorithm, but for a single character with eye and mouth overlays only.

---

## Quick start

1. Drop these files into the repo root:
   - `index.html` — control panel + built-in OBS Mode
   - `obs.html` — optional pure-overlay page for OBS Browser Source
   - `style.css` — dark + heat (yellow/orange) theme used by `index.html`
2. Put all the `Fox *.png` files into an `assets/` subfolder (see [Files used](#files-used-must-be-in-assets-folder)).
3. Serve the folder over HTTPS or `localhost` (browsers block mic on `file://`).
   - Easiest: GitHub Pages, Vercel, or `npx serve .` from a terminal.
4. Open `index.html`, click **🎤 Start Mic**, allow mic access, and start talking.

Final layout:

```
SolQuicks-OBS/
├── index.html
├── obs.html
├── style.css
├── README.md
└── assets/
    ├── Fox Base 1.png
    ├── Fox Green.png
    ├── Fox Ahh.png
    ├── Fox Blink.png
    ├── Fox Curious.png
    ├── Fox Eee.png
    ├── Fox Ehh.png
    ├── Fox Furious.png
    ├── Fox Joy.png
    ├── Fox Mmm.png
    ├── Fox Ouchy.png
    ├── Fox Rrr.png
    ├── Fox Smile.png
    ├── Fox Sss.png
    ├── Fox Stare.png
    ├── Fox Stern.png
    └── Fox Suprised.png
```

---

## How OBS capture works

There are two ways to get the fox into OBS. Pick whichever feels easier.

### Option A — Window Capture (recommended, simplest)

1. Open `index.html` in your browser.
2. Adjust mood, lip sync style, blink interval — get it dialed in.
3. Click **▶ Enter OBS Mode** in the top right. The UI disappears; just the fox remains.
4. In OBS: **+ Source → Window Capture →** pick your browser window.
5. If you flipped **Green Screen** on, add a **Chroma Key filter** in OBS (default green works).
6. To get back to controls: hover the **top-left corner** — a faded "× Exit OBS Mode" button appears.

The faded corner box also has a **⏸ Pause Mic** button for when you need to mute mid-stream.

### Option B — Browser Source (more polished, two windows)

1. Open `index.html` as your control panel.
2. Copy the OBS URL from the **OBS Setup** section (it points to `obs.html`).
3. In OBS: **+ Source → Browser Source →** paste the URL.
4. Set Width × Height (e.g. 1000 × 1000) and check **Shutdown source when not visible**.
5. The browser source runs its own mic and reads your control-panel settings via `BroadcastChannel`.
6. Apply a Chroma Key filter if Green Screen is on.

---

## Features

| Feature | What it does |
|---|---|
| **Auto Expressions** | Mic onset detection picks an eye expression per phrase, weighted by your **Mood** (Happy / Calm / Mad / Shocked) and your speaking volume. |
| **Lip Sync** | Four-tier mic volume → mouth file. Two styles: Default (Ehh — neutral closed mouth on silence) and Smiley (smiles on silence). |
| **Auto Blink** | Periodic blink on a slider (1–12s). Also fires automatically before each new expression and during long silences. |
| **Green Screen** | Draws `Fox Green.png` as the top layer so OBS Chroma Key can cut out the background. |
| **Manual Test** | 7-button grid to lock the fox to a specific expression for testing or screenshots. Auto-mode overrides as soon as you speak. |
| **OBS Mode** | Hides the entire UI, scales the fox to fill the window. Hover top-left for a faded exit button. **Esc** also exits. |

---

## Files used (must be in `assets/` folder)

**Base + chroma key**
- `Fox Base 1.png` — default base layer
- `Fox Green.png` — green-screen variant base

**Mouth shapes (lip sync)**
- `Fox Mmm.png` (silent — default style)
- `Fox Smile.png` (silent — smiley style)
- `Fox Eee.png` (quiet)
- `Fox Ehh.png` (mid)
- `Fox Ahh.png` (loud)

*Fox Sss.png and Fox Rrr.png are not used by the current lip-sync map — they can stay in the assets folder unused.*

**Eye expressions**
- `Fox Stare.png` (idle / default)
- `Fox Blink.png` (blink frame)
- `Fox Curious.png`
- `Fox Joy.png`
- `Fox Stern.png`
- `Fox Furious.png`
- `Fox Ouchy.png`
- `Fox Suprised.png` *(filename uses the original "Suprised" spelling — don't rename it)*

---

## Mood → expression weights

Same weighted-pool system as BHB Live. Per phrase, the engine measures average mic RMS for ~350ms after speech onset, classifies it as `high/mid/low`, then picks an expression from the mood's pool (avoiding repeats):

| Mood | Loud (high) | Medium | Soft (low) |
|---|---|---|---|
| **Happy** | Joy / Surprised / Curious | Curious / Joy / Surprised | Curious / Stare / Joy |
| **Calm** | Curious / Stern / Stare | Stare / Curious / Stern | Stare / Curious / Stern |
| **Mad** | Furious / Ouchy / Stern | Stern / Furious / Ouchy | Stern / Furious / Ouchy |
| **Shocked** | Surprised / Ouchy / Joy | Surprised / Ouchy / Curious | Curious / Surprised / Ouchy |

---

## Tuning notes

If your friend's voice doesn't trigger expressions reliably:

- **Too sensitive?** Bump `SILENCE_THRESH` from `0.020` higher (e.g. `0.030`).
- **Not loud enough to hit "high"?** Lower `HIGH_THRESH` from `0.11` to ~`0.08`.
- **Wrong onset spacing?** Tune `MIN_ONSET_GAP` (seconds between new expressions).

All constants live near the top of the auto-expression engine block in both `index.html` and `obs.html` — keep them in sync if you edit.

---

## Browser support

Works in any modern Chromium browser (Chrome, Edge, Brave, Arc) and Firefox. Safari works but `getUserMedia` requires a user gesture (the **Start Mic** button handles that). Mobile Safari/Chrome work too — OBS Mode will fill the screen.

---

## Credits

Built for [SolQuicks](https://github.com/BHALEYART/SolQuicks-OBS) by [BHaleyArt](https://github.com/BHALEYART). Engine ported from BHB Live.
