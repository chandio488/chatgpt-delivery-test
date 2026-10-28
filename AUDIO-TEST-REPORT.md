# Audio Test Report — Voice Clone Experiment (2026-10-28)

Teen files ki machine-measured report. Ye numbers hain, kisi ki sama'at (sun-ne ka tajurba) nahi.
Similarity ka hatmi faisla malik ke kaan ka hai. Ye report ChatGPT ki review ke liye hai taake
woh numbers ki bunyaad par farq aur behtari ki rai de sakay.

## Files
- **A — Base narration:** `sample_scene1_human.mp3` (Piper Urdu TTS, expressive/human-style, scene 1 of «کاغذ کی کہانی»)
- **B — Owner clone:** `owner_clone_scene1.mp3` (A par OpenVoice V2 tone transfer — target: malik ki apni awaz, 34.6s reference sample se)
- **C — Owner real voice (reference ground truth):** malik ki asal recording (34.6s, phone, khamosh kamra). Ye file repo mein NAHI rakhi gayi — sirf is ke numbers diye ja rahe hain.

## Measured results

| Measure | A (base) | B (clone) | C (owner real) |
|---|---|---|---|
| Duration | 21.01 s | 21.00 s | 34.60 s |
| Loudness (integrated) | −15.9 LUFS | −15.8 LUFS | −19.8 LUFS |
| True peak | +0.01 dBTP | −0.29 dBTP | −1.11 dBTP |
| Loudness range (LRA) | 3.7 LU | 2.5 LU | 2.8 LU |
| Pitch (F0) mean | 104.7 Hz | 93.1 Hz | 83.2 Hz |
| Pitch (F0) median | 102.3 Hz | 90.6 Hz | 84.6 Hz |
| Pitch spread (p10–p90) | 92–122 Hz | 80–115 Hz | 76–93 Hz |
| Pitch variation (std) | 12.8 Hz | 15.1 Hz | 10.9 Hz |
| Brightness (spectral centroid) | 1259 Hz | 1937 Hz | 1741 Hz |
| Spectral bandwidth | 1528 Hz | 2063 Hz | 1907 Hz |
| Zero-crossing rate (avg) | 0.058 | 0.101 | 0.077 |
| Pauses ≥ 0.3 s (count) | 7 | 5 | 0 |
| Total pause time | 4.76 s | 3.55 s | 0.0 s |
| Longest pause | 1.07 s | 0.95 s | — |
| Silence ratio | 22.6 % | 16.9 % | 0.0 % |

## Numbers kya kehte hain (plain reading)
1. **Pitch malik ki taraf gaya:** base 104.7 Hz → clone 93.1 Hz → malik ki asal 83.2 Hz. Tone transfer ne
   awaz ka markaz taqreeban aadha faasla malik ki taraf tay kiya; abhi ~10 Hz ka farq baqi hai.
2. **Clone asal awaz se thoda zyada roshan/teez hai:** brightness 1937 Hz, jabke malik ki asal awaz 1741 Hz
   hai. Zero-crossing bhi clone mein zyada hai (0.101 vs 0.077) — halki si izaafi high-frequency energy/noise ki nishani.
3. **Saans/stop wale waqfay mehfooz rahe:** base ke 7 waqfon mein se clone mein 5 waisay hi qaim hain,
   sab se lamba waqfa 0.95 s. Converter ne khamoshi ko thoda bhar diya (silence ratio 22.6% → 16.9%),
   magar saans ki saakht (structure) khatm nahi hui.
4. **Safai:** clone mein clipping nahi (true peak −0.29 dBTP), loudness base jaisi hi rakhi gayi (−15.8 LUFS).
5. **Malik ki asal reading ka apna andaaz:** un ke 34.6s sample mein 0.3s se lamba koi waqfa nahi —
   woh seedha musalsal parhte hain; narration ke waqfay base script ki design se aaye hain.

## Method / limits
- Analysis: librosa (pyin pitch tracking, spectral features) + ffmpeg (loudnorm, silencedetect −40 dB / 0.3 s), 22050 Hz mono.
- Ye report timbre ki *numbers wali* tasveer hai — "awaz malik jaisi lagti hai ya nahi" ye numbers sabit nahi kar sakte.
- Sawal ChatGPT se: in numbers ki roshni mein (1) A aur B ka asal farq kya hai? (2) Clone ko malik ki asal
  awaz (C) ke aur qareeb laane ke liye agli behtari kya honi chahiye — pitch ka baqi farq, izaafi brightness,
  aur waqfon ka naram hona — in teenon par tartib-war rai dein.
