# Style Test Report — «ایک مخلص دوست کی آخری امید» (Ek Mukhlis Dost Ki Aakhri Umeed)

**For ChatGPT's review — Roadmap Step 3: Visual Style Test**
Prepared 2026-10-28 · Film: owner's own true story, 11 scenes · Script: Revision 4 (approved)

> **How to review this package:** The GitHub connector cannot play video, so this review is based on the **six frame stills** in this repo (listed below) plus this report. The full test video itself (`style-test-scene1.mp4`, 56.5 s, 1920×1080) cannot be watched through the connector — the frame stills are the actual review surface.

---

## 1. What this style test is

This is **Step 3 of the production roadmap**: a 30–60 second visual style test of **Scene 1 only**, built to verify three things before full production:
1. the pencil-sketch style stays uniform across different images,
2. the Urdu kinetic typography renders correctly (connected letters, right-to-left) and stays legible,
3. the visuals sit in sync with the locked voice-over, with no blank or frozen stretches.

## 2. Technical specs (ffprobe-verified)

| Item | Value |
|---|---|
| Duration | **56.5 s** (4.0 s title card → 0.8 s dissolve → 52.5 s Scene 1) |
| Video | H.264, **1920×1080, 30 fps** (the locked film spec) |
| Audio | AAC, **48 kHz** — voice-over only in this test |
| File | `style-test-scene1.mp4`, 47,718,057 bytes, MD5 `c45a62ac5e7e7d4bc3a6440de55a45d1` |
| Voice | The locked film voice: **ElevenLabs "George", Eleven v4 model** (owner approved and locked 2026-10-28; full-film VO of all 11 scenes is complete, 10:39) |

Silence analysis confirms: silence only during the 4 s title card, then continuous narration to the end with only natural inter-sentence pauses (longest 0.93 s).

## 3. Structure

- **0:00–0:04** — Title card: the film title «ایک مخلص دوست کی آخری امید» in large Nastaliq on the paper background, gentle slow zoom.
- **0:04–0:56.5** — Scene 1 narration over four pencil sketches, joined by 1.2 s cross-dissolves, each with a slow Ken Burns zoom/pan so no frame is ever frozen.

## 4. The four sketches (one uniform style)

All four are graphite pencil sketches with cross-hatching on warm cream paper, monochrome, no colour, no text inside the artwork:

- **(a) Textile mill gate at dawn** — iron gates between brick pillars, long factory buildings and tall chimneys behind (scene 0:04–0:16 approx.)
- **(b) Rows of looms in a mill hall** — long perspective rows of textile machines with belts and pulleys (scene ~0:15–0:29)
- **(c) Small boy silhouette beside a towering machine wheel** — the child worker dwarfed by the machinery (scene ~0:28–0:41)
- **(d) Close study of hands threading fabric** — working hands guiding thread/fabric through rollers and gears (scene ~0:40–0:56.5)

## 5. Frame stills in this repo (the review surface)

| File | Moment | What it shows |
|---|---|---|
| `style-frame-1-title.png` | ~2.0 s | Title card — film title in Noto Nastaliq Urdu Bold on paper |
| `style-frame-2-mill-gate.png` | ~10.5 s | Sketch (a) mill gate + keyword «مزدوری» on screen |
| `style-frame-3-looms.png` | ~19.0 s | Sketch (b) looms + keyword «بارہ سال کی عمر» on screen |
| `style-frame-4-boy.png` | ~29.5 s | Sketch (c) boy beside the machine wheel (+ keyword «ہیلپر» over the machine area) |
| `style-frame-5-hands.png` | ~43.0 s | Sketch (d) hands + keyword «ہمت نہیں ہاری» on screen |
| `style-frame-6-technician.png` | ~55.5 s | Sketch (d) closer zoom + final keyword «ٹیکنیشن» on screen |

## 6. Kinetic typography — the five key words

Timings were taken from word-level ASR timestamps of the actual voice-over, so each word appears at the moment it is spoken (times below include the 4 s title offset):

| Keyword | Meaning | Appears at |
|---|---|---|
| «مزدوری» | labour / wage-work | ~0:09.5 |
| «بارہ سال کی عمر» | at the age of twelve | ~0:17.5 |
| «ہیلپر» | helper (his first job) | ~0:28.3 |
| «ہمت نہیں ہاری» | he did not lose heart | ~0:41.6 |
| «ٹیکنیشن» | technician (what he became) | ~0:54.8 |

Each keyword fades in with a gentle rise-and-settle, holds about 3 seconds, and fades out.

## 7. Urdu shaping method (verified by frame inspection)

Urdu text is **not** drawn with ffmpeg drawtext (it breaks Arabic-script shaping). Text overlays are rendered as PNGs with **Python PIL + libraqm** (`direction="rtl"`, `language="ur"` on the logical text) in **Noto Nastaliq Urdu Bold**, then composited. An earlier attempt with arabic_reshaper + python-bidi produced tofu boxes (Nastaliq has no Arabic Presentation-Forms glyphs), so the libraqm route is the verified one. All six frame stills were visually inspected: every Urdu word is fully connected, correct RTL, and legible.

## 8. Honest limitations

1. **No music in this test.** Music licensing is settled separately for the full film (soft, verified-licensed track, below the voice, fully muted in the hospital scenes 7–8 per the brief). The test isolates style + typography + sync.
2. **Keyword treatment is fade + rise-and-settle, not a letter-by-letter typewriter reveal.** A per-letter reveal would break Urdu letter joining mid-word; the chosen treatment is the safe kinetic option for Nastaliq.
3. **Test title card is ~4 s;** the full-film plan allows ~10 s. Only the test is shortened.
4. One keyword («ہیلپر», frame 4) sits over a darker machine area and slightly higher than the others (Nastaliq stacking) — still clearly legible, but placement can be standardised in full production.
5. Sketches (a) and (d) were generated 3:2 and cover-cropped to 16:9; (b) and (c) are native 16:9. Nothing of consequence was lost.
6. This is Scene 1 only, as the roadmap requires — full production (all 11 scenes, ≈10:49) starts only after this style is approved.

## 9. Questions for ChatGPT

Please answer from the six frame stills and this report:

**(a)** Does this pencil-sketch + kinetic-typography style suit this emotional true story?
**(b)** Is the sketch style uniform enough across the four images (gate, looms, boy, hands)?
**(c)** Is the Urdu typography legible and well-placed in the frames?
**(d)** Should full production proceed in exactly this style — and if not, what single change matters most?
