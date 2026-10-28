# Model Comparison Log — Step 2 of ChatGPT's free open-source voice-cloning roadmap

Started: 2026-10-28 (Asia/Karachi). Step 1 (OpenVoice V2) already proven on this VM ≈2× realtime.

## Environment
- 2 CPUs, 7.7 GB RAM total (note: only ~1.9 GB free at start — other workloads on the box; models run strictly one at a time), NO GPU, 86 GB free disk.
- Python 3.12.3 only (`/usr/bin/python3.12`). ffmpeg/ffprobe present.
- All caches (HF_HOME, TORCH_HOME, XDG) and TMPDIR redirected INSIDE this workspace; nothing installed outside it.
- TMPDIR note: /tmp is a 512 MB tmpfs → TMPDIR=<workspace>/tmp for all installs.

## Test material (identical for every model)
- Reference: `~/workspace/voice-experiments/openvoice/input/owner_ref.wav` — owner's voice, 34.60 s, 22050 Hz mono, clean Urdu reading.
- Reference transcript = target text (Urdu): scene-1 text in `~/workspace/voice-experiments/piper-urdu/scenes/scene01.txt` (3 sentences).
- Models without Urdu-script support: Devanagari transliteration via `urdu2devanagari.py` in this dir.
  APPROXIMATE, documented: the Urdu source has no vowel diacritics, so short vowels are unrecoverable by
  letter mapping alone → word-level dictionary for this text's 41 distinct words (hand-checked) +
  character-map fallback (with do-chashmi-he aspirate digraphs). Output: `scene01_devanagari.txt`.
- Baseline for comparison (Step 1): OpenVoice V2 ≈2.07× realtime conversion, output already exists from Step 1.

## Per-model records
(each model gets its own section below as work proceeds)

## Transliteration — DONE
`scene01_devanagari.txt` produced and hand-verified (all 41 words via dictionary; one corrupted
dictionary value found by codepoint audit and fixed). Text:
आप के हाथ में इस वक़्त जो काग़ज़ है, एक नोट बुक, एक अख़बार, एक किताब का सफ़हा, यह दुनिया की सब से ख़ामोश ईजाद है. न इस ने शोर मचाया, न तलवार चलाई. मगर इस ने इल्म को बादशाहों के महलों से निकाल कर आम आदमी के हाथ में दे दिया।

## 1) XTTS-v2 (coqui-tts)
- Route: fresh venv (py3.12) + pip install torch/torchaudio (CPU wheels) + `coqui-tts` (maintained Idiap fork of Coqui TTS; original `TTS` package requires Python <3.12).
- Model licence: Coqui Public Model License (CPML) — non-commercial terms for the XTTS-v2 weights. Code: MPL-2.0.

### XTTS-v2 install record
- venv OK: torch 2.14.1+cpu, coqui-tts 0.28.0 (PyPI). One documented extra needed: `coqui-tts[codec]`
  (torch >= 2.9 requires torchcodec for audio IO — ImportError message names this fix verbatim).
- Model download (tts-hub/XTTS-v2, ~1.9 GB): first attempt STALLED at ~51 MB (frozen blob, process
  at ~3% CPU — xet transport hang). Killed; retry via `hf download` with HF_HUB_DISABLE_XET=1
  resumed and is progressing. Download contention on this box is heavy (other tenants downloading
  models too) — wall-clock is slow, not a model fault.

## 2) F5-TTS
- Route: fresh venv (py3.12) + torch/torchaudio CPU wheels + `pip install f5-tts` (PyPI).
  Installed OK (venv 2.2 GB); `import f5_tts` OK. Model: F5TTS_v1_Base (SWivid/F5-TTS).
- Licence: code MIT; pretrained weights CC-BY-NC-4.0 (non-commercial) — noted for the report.
- First inference attempt FAILED before any synthesis, environment cause: this sandbox's egress
  proxy env includes bracketed IPv6 entries in NO_PROXY, and the httpx build inside the F5 venv
  crashes parsing them (httpx.InvalidURL: Invalid port ':1]') when huggingface_hub builds its
  client. Workaround: pre-download weights with the XTTS venv's (working) hf CLI into the shared
  HF cache + run F5 with a sanitised NO_PROXY (localhost,127.0.0.1 only). No proxy values recorded here.
- Language route: F5 has no Urdu/Hindi setting; conditioning is via ref_text characters.
  Attempt order: (a) Urdu script ref_text+gen_text, (b) Devanagari transliteration fallback.

### XTTS-v2 RESULT — WORKS
- Route used: Devanagari transliteration + language `hi`, speaker_wav = owner_ref.wav (full 34.6 s).
- Extra fix needed: PyPI torchcodec wheel is CUDA-linked; replaced with `torchcodec 0.17.0+cpu`
  from the PyTorch CPU index (its .so demanded libcudart/libtorch_cuda absent in CPU torch).
- Model: tts-hub/XTTS-v2 — model.pth 1,867,929,383 B + dvae.pth 210 MB. Load ~43-68 s.
- Timings (2 contended cores): s1 83.3 s→9.00 s (9.25x), s2 33.6 s→5.43 s (6.18x), s3 35.6 s→7.01 s
  (5.09x). Total synthesis 152.5 s for 21.44 s speech = ~7.1x realtime (excl. load). SLOW vs
  OpenVoice's ~2x — and this box was under heavy external load during measurement, so treat as
  an upper bound, not a clean benchmark.
- Outputs: out/xtts_scene1.wav (24 kHz, 22.24 s incl. 2x0.4 s gaps) + out/xtts_scene1.mp3 (160k).
- Objective indicators (analyze.py, librosa pyin): F0 mean 81.8 Hz / median 84.1 / std 10.9 —
  vs owner real voice 83.2 / 84.6 / 10.9 (near-exact pitch-centre and spread match; OpenVoice
  clone measured 93.1 Hz mean in Step 1, Piper base 104.7 Hz). Peak 0.00 dB (coqui full-scale
  normalisation), RMS -18.0 dB. Duration sane vs the same text in Piper (21.0 s).
- Caveats: pitch match is NOT a similarity verdict (owner's ear decides). Peak at exactly 0 dB
  leaves no headroom. CPML licence = non-commercial weights.

## 3) GPT-SoVITS — BLOCKED at the language frontend (proven, no weights downloaded)
- Route attempted: official repo cloned (RVC-Boss/GPT-SoVITS, 43 MB) for a genuine frontend test;
  full weight install (~4-6 GB) NOT performed because the blocker is structural and provable
  without it — downloading 5 GB to reproduce a guaranteed KeyError would waste the metered link.
- Evidence (all from the cloned official code + README):
  * README: cross-lingual inference supports English, Japanese, Korean, Cantonese, Chinese only.
  * GPT_SoVITS/TTS_infer_pack/TTS.py (v2): languages = [auto, auto_yue, en, zh, ja, yue, ko,
    all_zh, all_ja, all_yue, all_ko] — no hi/ur entry exists to select.
  * GPT_SoVITS/text/ ships G2P modules ONLY for chinese, cantonese, english, japanese, korean.
  * Empirical check: 0 of the 36 distinct Devanagari characters in scene01_devanagari.txt exist
    in GPT_SoVITS/text/symbols2.py's symbol table → cleaned_text_to_sequence would KeyError/drop
    every character of Hindi text; Urdu (Arabic script) is equally absent.
- Verdict: CANNOT synthesize Hindi/Urdu text in any configuration of the official release.
  Not a hardware or install failure — the language simply does not exist in the model.
  Licence note (for completeness): code MIT; pretrained weights per HF repo terms.
- Timing: n/a. Output: none possible.

### F5-TTS RESULT — runs, but fails the test (script coverage + speed)
- Install fixes needed (both documented above): sanitised NO_PROXY for its httpx client;
  weights pre-downloaded via the XTTS venv's hf CLI (SWivid/F5-TTS, safetensors 1,348,435,761 B);
  CUDA-linked torchcodec replaced with 0.17.0+cpu (same fix as XTTS).
- Urdu attempt (ref_text + gen_text in Urdu script, full owner ref): sentence 1 ONLY completed.
  * Speed: 811.6 s compute for 4.63 s of audio = **175x realtime**. A 5-minute narration would
    take ~15 hours on this VM. Impractical by two orders of magnitude.
  * Completeness: output is 4.63 s for the longest sentence (owner reads it in ~13 s; Piper ~10 s;
    XTTS 9.0 s). Root cause MEASURED in the model's own vocab.txt (2,545 entries):
    only 14/32 Urdu-script characters of the test text exist in the vocab. Missing include the
    backbone letters ا ی ے ہ ک گ پ چ ٹ ش خ ف غ ذ آ ئ ں ھ — nearly every Urdu word is mutilated
    before synthesis, so the output can only be a fragment of the sentence.
  * Indicators (analyze.py): peak ~0.0 dB, RMS -17.4 dB, F0 mean 94.0 Hz / std 5.3 (flat) —
    speech-like energy in a plausible pitch band, but it is NOT the full sentence.
- Devanagari fallback: NOT run — proven pointless in advance: 0/36 Devanagari characters exist
  in F5's vocab.txt. Every character would be dropped; the run would synthesize an empty prompt.
- Outputs (sentence-1 fragment only, labelled as such): out/f5tts_scene1.wav + .mp3 (24 kHz, 4.63 s).
- Verdict: mechanically functional, practically disqualified on this hardware + this language.

## 4) CosyVoice — documented impractical verdict (not installed)
Per the task's own allowance for this GPU-oriented model, and on this evidence:
- Language: CosyVoice/2/3 official support = Chinese (+dialects), English, Japanese, Korean
  (v2/3 add several European languages). Hindi and Urdu appear in NO official language list —
  the same structural absence proven for GPT-SoVITS, so there is no supported route to
  phonemize/tokenize our text even in cross-lingual zero-shot mode.
- Hardware: designed and documented around GPU inference (vendor/community guidance ~4-12 GB
  VRAM); this VM has no GPU, 2 CPU cores, 7.7 GB RAM. F5-TTS — a smaller 0.3B model — already
  ran at 175x realtime here; CosyVoice's LLM+flow stack would be slower still.
- Install cost: heaviest of the four (repo + submodules, ModelScope/HF multi-GB weights,
  py3.10-era pinned deps vs this box's Python 3.12) for a model that cannot address the language.
- Verdict: IMPRACTICAL on this VM for Urdu/Hindi. Not attempted beyond this documented review.
  Licence: Apache-2.0 (code) — the most permissive of the four, noted for the record.

## FINAL COMPARISON TABLE
| Model | Installed? | Licence (weights) | Language route | Speed (x realtime) | Output | Key objective note |
|---|---|---|---|---|---|---|
| OpenVoice V2 (Step 1) | yes | MIT | Piper Urdu TTS + tone transfer | ~2.07x | (Step 1 files) | F0 mean 93.1 Hz (halfway to owner) |
| XTTS-v2 | yes | CPML (non-commercial) | Devanagari transliteration, lang=hi | ~7.1x overall (152.5 s / 21.4 s) | out/xtts_scene1.wav/.mp3 (full scene, 22.24 s) | F0 81.8/84.1/10.9 vs owner 83.2/84.6/10.9 — near-exact pitch match |
| F5-TTS | yes | CC-BY-NC-4.0 | Urdu script direct (partial vocab) | 175x (sentence 1) | out/f5tts_scene1.wav/.mp3 (FRAGMENT, 4.63 s) | Urdu vocab coverage 14/32 chars; Devanagari 0/36 |
| GPT-SoVITS | repo cloned; weights not installed | MIT (code) | none exists | n/a | none possible | 0/36 Devanagari chars in symbol table; languages zh/en/ja/ko/yue only |
| CosyVoice | not installed (documented verdict) | Apache-2.0 | none for hi/ur | n/a | none | GPU-oriented; no hi/ur in any official list |

## Recommendation for the owner's ear
1. XTTS-v2 scene-1 (out/xtts_scene1.mp3) — the ONLY new candidate worth listening to: complete
   scene, direct voice cloning (not tone transfer), pitch statistics essentially on the owner's.
   Costs: ~7x realtime compute; CPML non-commercial licence is a real constraint for channel use.
2. OpenVoice V2 remains the speed/practicality baseline (~2x) with a halfway pitch shift.
3. F5-TTS fragment is kept only as evidence; GPT-SoVITS and CosyVoice are closed for Urdu/Hindi.
Similarity itself is the owner's judgement — no output here is claimed to "sound like the owner".
