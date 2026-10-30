# 🔬 RESEARCH REPORT — دو GitHub اُمیدواروں کا ثبوت پر مبنی آڈٹ
**تاریخ:** 2026-10-30 · **مشن:** صرف تحقیق و رپورٹ — کوئی انضمام، تبدیلی، Build یا پبلشنگ نہیں ہوئی۔
**ہمارے اصل پروجیکٹس** (`offline-smart-voice-prototype/` اور `raheem-live-tv/`) **صرف پڑھے گئے، چھوئے نہیں گئے۔**

**ثبوت کی اقسام:** `SOURCE VERIFIED` = سورس کوڈ میں فائل/جگہ کے ساتھ موجود · `APK VERIFIED` = اصل ریلیز APK ڈاؤن لوڈ کر کے aapt2/apksigner سے جانچی گئی · `RUN VERIFIED` = چلا کر دیکھا (اس VM پر Android emulator نہیں، اس لیے **ایپ رن ٹائم کی کوئی جانچ ممکن نہیں ہوئی**) · `PHONE TEST NOT DONE` = اصل موبائل پر ٹیسٹ باقی۔
**README کے دعوے کہیں بھی ثبوت نہیں مانے گئے۔**

---

## 1. پروجیکٹس اور کلون انوینٹری

| | پروجیکٹ 1 — Keyboard | پروجیکٹ 2 — Live TV |
|---|---|---|
| GitHub | https://github.com/HeliBorg/HeliBoard | https://github.com/ahXN00/OwnTV_Mobile |
| مقامی فولڈر | `research-2026-10-30/heliboard/` | `research-2026-10-30/owntv-mobile/` |
| HEAD commit | `44ddb1ae` — 2026-10-25 «update translations» | `9a04fa79` — 2026-10-30 (آج ہی کا commit — زندہ پروجیکٹ) |
| سائز / فائلیں | 349 MB (بشمول .git) / 2,207 فائلیں | 29 MB / 305 فائلیں |
| سورس میں ورژن | 4.3-beta1 (versionCode 4300) | ایپ شیل؛ ورژن CI ٹیگ سے آتا ہے |

> ⚠️ **OwnTV کی ساخت واری اہم دریافت:** OwnTV_Mobile صرف **فون/ٹیبلیٹ کا UI شیل** ہے۔ اصل انجن — ڈیٹا بیس، EPG، پلیئر، پلے لسٹ پارسر — ایک **الگ ریپو** میں ہے: **OwnTV_Core** (`tv.own.owntv:core` + `:player-core`، ورژن 1.0.69) اور MPV انجن تیسرے ریپو **OwnTV_libmpv** سے آتا ہے۔ ایماندارانہ آڈٹ کے لیے Core بھی کلون کیا گیا: `research-2026-10-30/owntv-core/` (HEAD `236cecaa`، 2026-10-30، 31 MB / 1,182 فائلیں)۔ نیچے OwnTV کے فیچر شواہد میں جہاں «Core» لکھا ہے، فائل اسی ریپو کی ہے۔

---

## 2. تازہ APK ریلیزز — APK VERIFIED حقائق

### HeliBoard
- **تازہ مستحکم ریلیز:** v4.2 (2026-10-12) — https://github.com/HeliBorg/HeliBoard/releases/download/v4.2/HeliBoard_4.2-release.apk
- تازہ ترین (بیٹا): v4.3-beta1 (2026-10-18) — اسی پیٹرن پر `-release` / `-nouserlib` / `-debug` تین فائلیں۔
- **جانچی گئی فائل:** `downloads/HeliBoard_4.2-release.apk` — 22,431,797 بائٹس، MD5 `b15341a544f4296166d450a468a60004`
- package `helium314.keyboard` · versionCode 4201 / 4.2 · **minSdk 21 (Android 5.0)** · targetSdk 37
- **ایک ہی Universal APK** — چاروں ABI: `arm64-v8a, armeabi-v7a, x86, x86_64` (`libjni_latinime.so` ہر ABI میں) — APK VERIFIED
- دستخط: اصل ریلیز سرٹیفکیٹ `CN=Helium314, O=HeliBoard` (apksigner تصدیق شدہ) — ڈیبگ کی نہیں
- **Permissions (APK VERIFIED):** `READ/WRITE_USER_DICTIONARY, RECEIVE_BOOT_COMPLETED, VIBRATE, READ_CONTACTS` — **نہ INTERNET، نہ RECORD_AUDIO** (یہ دو غیاب §5 کے فیصلے کی بنیاد ہیں)

### OwnTV Mobile
- **تازہ ریلیز:** v1.1.1 (2026-10-21) — دو APK فائلیں:
  - ✅ درست فائل: https://github.com/ahXN00/OwnTV_Mobile/releases/download/v1.1.1/OwnTV-Mobile-v1.1.1.apk — 58,573,867 بائٹس، MD5 `82bb2aa732825e7492a3c2035c0bb1cd`
  - ⚠️ دوسری فائل: `OwnTV-Mobile-x86_64-v1.1.1.apk` — 41,620,534 بائٹس (§8 دیکھیں)
- **جانچی گئی standard فائل:** package `tv.own.owntv.mobile` · versionCode 10101 / 1.1.1 · **minSdk 26 (Android 8.0)** · targetSdk 36 · compileSdk 37
- **ایک APK میں دونوں ARM ABI:** `arm64-v8a + armeabi-v7a` (15 native libs ہر ایک میں؛ سب سے بڑی `libavcodec.so` 10.4 MB، `libmpv.so` 5.3 MB) — APK VERIFIED
- دستخط: اصل ذاتی ریلیز سرٹیفکیٹ `CN=Ashiq Hasan, Düsseldorf, DE` (apksigner تصدیق شدہ) — ٹیسٹ/ڈیبگ کی **نہیں**
- APK میں شامل UI زبانیں (27): `ar, hi` شامل — **`ur` (اردو) شامل نہیں** — APK VERIFIED
- Permissions: INTERNET, FOREGROUND_SERVICE(_MEDIA_PLAYBACK/_DATA_SYNC), POST_NOTIFICATIONS, SCHEDULE_EXACT_ALARM, CAMERA (اختیاری فیچر کے لیے), REQUEST_INSTALL_PACKAGES (اندرونی اپڈیٹر کے لیے) وغیرہ

---

## 3. پروجیکٹ 1 — HeliBoard: فیچر بہ فیچر ثبوت

سورس: `heliboard/app/src/main/` — جدید **Jetpack Compose سیٹنگز** (پرانے PreferenceFragments نہیں)۔

| # | فیچر | فیصلہ | ثبوت (فائل/جگہ) |
|---|---|---|---|
| 1 | Keyboard UI + Settings | SOURCE VERIFIED | IME سروس `latin/LatinIME.java`؛ سیٹنگز `helium314/keyboard/settings/` — اسکرینیں: Language, Subtypes, Preferences, Appearance, Colors (+Night), Toolbar, Gesture Typing, Text Correction, Secondary Layouts, Dictionaries, Personal Dictionaries, Advanced, About |
| 2a | **اردو (ur)** | SOURCE VERIFIED | `res/xml/method.xml`: subtype `ur-PK`، لے آؤٹ `assets/layouts/main/urdu.json` + `symbols_arabic`؛ `res/xml/spellchecker.xml` میں `ur` شامل۔ **مگر ڈکشنری APK میں bundled نہیں** (§3-نیچے) |
| 2b | **سندھی (sd)** | ❌ **موجود نہیں** | `method.xml` کے 129 subtypes میں `sd` نہیں؛ کوئی سندھی لے آؤٹ فائل نہیں؛ spellchecker میں نہیں۔ صرف ڈاؤن لوڈ کے قابل ڈکشنری `main_sd` dict-repo فہرست میں ہے — بغیر subtype/لے آؤٹ کے۔ (نوٹ: `bn-khipro.mim` سندھی **نہیں** — یہ بنگالی Khipro transliteration combiner ہے، `event/BnKhiproCombiner.kt`) |
| 2c | عربی (ar) | SOURCE VERIFIED | 3 subtypes: `arabic`، `arabic_pc`، `arabic_hijai` (لے آؤٹ فائلیں موجود)؛ spellchecker شامل |
| 2d | ہندی (hi) | SOURCE VERIFIED | 3 subtypes: `hindi.json`، `hindi_compact.json`، `hindi_phonetic.json`؛ spellchecker شامل |
| 2e | ہندی Roman (hi-Latn) | SOURCE VERIFIED (جزوی) | subtype `hi-Latn` موجود، ڈیفالٹ QWERTY؛ **spellchecker نہیں**؛ ڈکشنری `main_hi_zz` ڈاؤن لوڈ فہرست میں |
| 2f | انگریزی | SOURCE VERIFIED | en-US/GB/AU/CA/IN subtypes، ڈکشنریاں **bundled** (`main_en-US.dict`, `main_en-GB.dict`)، spellchecker مکمل |
| 2g | **Roman Urdu (ur-Latn)** | ❌ **موجود نہیں** | `ur-Latn/ur_ZZ` subtype کہیں نہیں؛ Roman Urdu ڈکشنری `assets/dicts/` میں بھی نہیں اور dict-repo فہرست میں بھی نہیں۔ صارف کو یا تو انگریزی subtype (انگریزی predictions) ملے گا یا ہاتھ سے بنایا ہوا ur-PK+QWERTY subtype — جس کی ڈکشنری عربی رسم الخط والی `main_ur` ہوگی، یعنی **رسم الخط ہی نہیں ملتا** |
| 2h | پنجابی PK (pa-PK) | ❌ subtype نہیں | صرف `res/xml/locales_config.xml` میں نام؛ `method.xml`/spellchecker میں نہیں؛ عام `main_pa` ڈکشنری ڈاؤن لوڈ فہرست میں |
| 3 | Offline typing + Suggestions + Prediction | SOURCE VERIFIED | انجن **Native AOSP** (`jni/src/dictionary/` Patricia-trie + `jni/src/suggest/`، JNI `BinaryDictionary.java/DicTraverseSession.java`)؛ **Next-word (bigram) prediction موجود**: `latin/Suggest.kt:getNextWordSuggestions()`، سیٹنگ `next_word_prediction`۔ مینی فیسٹ میں **INTERNET پرمیشن ہی نہیں** اور کوڈ میں نیٹ ورک کال 0 — تجاویز سو فیصد آف لائن۔ صارف کی ٹائپنگ سے سیکھنے والا `UserHistoryDictionary` + Contacts dictionary (پرمیشن سے مشروط) بھی موجود |
| 3b | **ڈکشنریاں — اصل صورتحال** | SOURCE + APK VERIFIED | APK میں **صرف 18 ڈکشنریاں bundled**: bg, bn, de, el, en-GB, en-US, es, fr, hu, it, nl, pl, pt-BR, pt-PT, ro, ru, sv, tr۔ **ur, ar, hi, sd کی ڈکشنریاں APK میں نہیں۔** وہ الگ repo (Codeberg `aosp-dictionaries`) سے **براؤزر میں ڈاؤن لوڈ کر کے فائل پکر سے درآمد** کرنی پڑتی ہیں (`settings/FilePicker.kt`, `dialogs/NewDictionaryDialog.kt`) — ایپ کے اندر ڈاؤن لوڈر نہیں (جان بوجھ کر: ایپ آف لائن ہے)۔ درآمد کے بعد تجاویز اس زبان میں چلتی ہیں |
| 4 | Clipboard History + Pin | SOURCE VERIFIED | `latin/ClipboardHistoryManager.kt` + SQLite (`ClipboardDao.kt`)؛ **Pin موجود** (`toggleClipPinned`؛ pinned صفائی سے مستثنیٰ)؛ ڈیفالٹ retention صرف **10 منٹ** (سیٹنگ سے بڑھایا جا سکتا ہے، >120 = کبھی صاف نہیں)؛ **تصویری/فائل کلپس بھی** (≤20 MB ڈیفالٹ، `filesDir/clipboard/` میں کاپی) |
| 4b | Themes/Customization | SOURCE VERIFIED | 3 styles (Material/Holo/Rounded)، ~16 رنگ + دن/رات الگ، **کسٹم رنگ** (color picker)، کسٹم background تصویر و فونٹ، اونچائی scale 0.3–1.5، **one-handed، split keyboard، floating keyboard** — سب موجود (`AppearanceScreen.kt`, `KeyboardTheme.kt`) |
| 4c | Layouts میں ترمیم | SOURCE VERIFIED | JSON + سادہ .txt لے آؤٹس؛ اندرونی ایڈیٹر (`dialogs/LayoutEditDialog.kt`)، فائل سے درآمد؛ symbols/numpad/phone وغیرہ ثانوی لے آؤٹس الگ سیٹنگ |
| 4d | Gesture/Swipe typing | ⚠️ مشروط | کوڈ موجود (`GestureConsumer.java`) **مگر** باہر سے فراہم کردہ gesture لائبریری کے بغیر بند: یا Google کی `libjni_latinimegoogle` سسٹم میں ہو یا صارف خود checksum-تصدیق شدہ `.so` دے (`JniUtils.sHaveGestureLib`)۔ عام فون پر **out-of-box کام نہیں کرتا** |
| 4e | GIF | ❌ موجود نہیں | کوئی GIF/Tenor/Giphy کوڈ نہیں (INTERNET بھی نہیں) |
| 5 | **Voice Typing — اہم ترین فیصلہ** | ❌ **اپنا کوئی انجن نہیں** | نیچے §5 الگ سے |
| 6 | Language packs / Dictionary management | SOURCE VERIFIED | پیک سسٹم نہیں؛ ڈکشنری درآمد/حذف Dictionary اسکرین سے؛ **Personal Dictionary** میں لفظ شامل/ترمیم/حذف (`PersonalDictionaryScreen.kt` → Android UserDictionary) |
| 7 | Emoji | SOURCE VERIFIED | `assets/emoji/` Unicode 17 تک ڈیٹا؛ emoji تجاویز کے لیے CLDR ڈکشنریاں (ڈاؤن لوڈ)؛ incognito موڈ موجود (سیکھنا بند) |
| — | Settings UI کی اردو ترجمہ؟ | NOT CHECKED | اس آڈٹ میں تصدیق نہیں ہوئی |

### §5 — HeliBoard کی آواز: مکمل سچائی (SOURCE VERIFIED)
HeliBoard کے پاس **اپنی speech recognition بالکل نہیں**:
- مینی فیسٹ میں **RECORD_AUDIO پرمیشن نہیں** (APK VERIFIED) — ایپ مائیک استعمال ہی نہیں کر سکتی۔
- کوڈ میں `SpeechRecognizer`, `RecognitionService`, whisper/vosk/sherpa/onnx/tflite — **سب غائب** (مکمل grep)؛ کوئی آواز ماڈل assets میں نہیں؛ `method.xml` کے تمام subtypes `imeSubtypeMode="keyboard"` ہیں۔
- مائیک بٹن کا اصل کام (`latin/LatinIME.java:1341` → `latin/RichInputMethodManager.kt:switchToShortcutIme()`): یہ بٹن دبانے سے HeliBoard **فون کے دوسرے voice IME پر switch** کر دیتا ہے — عموماً **Google Voice Typing**۔ وہ ہو تو آن لائن گوگل آواز چلے گی؛ کوئی voice IME نہ ہو تو **بٹن چھپ جاتا ہے** (`InputAttributes.java:106`)۔
- نتیجہ: **HeliBoard میں آف لائن آواز صفر ہے — آواز اس کی فیچر ہی نہیں، صرف ایک شارٹ کٹ ہے۔**

---

## 4. پروجیکٹ 2 — OwnTV Mobile (+Core): فیچر بہ فیچر ثبوت

| # | فیچر | فیصلہ | ثبوت (فائل/جگہ) |
|---|---|---|---|
| 1 | اصل Android UI | SOURCE VERIFIED | Jetpack Compose؛ موبائل شیل اسکرینیں: Home, Live, Guide (EPG گرڈ), Library (Movies/Series), Multiview, Recordings, Downloads, Favorites, Search, Sports, Settings, Profiles, Setup Wizard — `owntv-mobile/app/src/main/java/tv/own/owntv/mobile/ui/` (220 فائلیں) |
| 2 | **ExoPlayer + MPV دونوں** | SOURCE VERIFIED | **دونوں حقیقی۔** Media3 1.11.1: `player-core/.../LivePreviewEngine.kt` (لائیو)، `ExoSubtitleEngine.kt` (VOD)۔ MPV: اپنی libmpv بلڈ `tv.own.owntv:libmpv:2026.10.4`، ریپر `player/OwnTVPlayer.kt` (`MPVLib.EventObserver`)۔ انتخاب خودکار: `player/LiveRouting.kt:decide()` — DRM ہو تو ہمیشہ ExoPlayer (MPV میں CDM نہیں)، چینل پن، سیکھا ہوا رویہ، پھر صارف سیٹنگ؛ ناکامی پر خودکار سیڑھی `LiveLadder.kt`: **EXO_HLS → EXO_TS → MPV_HLS → MPV_TS** |
| 3 | فارمیٹس | SOURCE VERIFIED | HLS (`HlsMediaSource`), DASH، MPEG-TS/progressive (Media3 extractors، TS میں CEA-608 کیپشن زبردستی فعال)؛ MPV/FFmpeg سے عملاً ہر عام فارمیٹ۔ Widevine + ClearKey DRM (Kodi `#KODIPROP` سے) — **PlayReady موجود نہیں** |
| 4 | چینل سوئچنگ / Zapping | SOURCE VERIFIED | ایک ہی ExoPlayer + surface چینلز پر **reuse** (`LivePreviewEngine.kt`)، MPV core بھی ایک ہی `loadfile` سے اگلا چینل؛ پچھلا-چینل recall بٹن (`ChannelRecall.kt`، میموری میں)۔ **Adjacent چینل pre-warm/prefetch: موجود نہیں** (مکمل grep — صرف EPG pre-warm ملا) |
| 4b | Resume Last Channel | SOURCE VERIFIED | DataStore `last_live_channel` + فی پروفائل `StartupMode {HOME, LAST_CHANNEL, LAST_CHANNEL_GUIDE, LAST_CHANNEL_LIVE, FAVORITES, SPECIFIC_CHANNEL}` (`core/settings/SettingsRepository.kt`)؛ موبائل شیل اسے کھولتا ہے (`ui/shell/ShellViewModel.kt`) |
| 5 | EPG / Now-Next / گائیڈ | SOURCE VERIFIED | مکمل XMLTV پارسر (`core/parser/XmltvParser.kt`، gzip سمیت)؛ **EpgMatcher** — نام normalize کر کے tvg-id سے خودکار میل + دستی override (`core/epg/EpgMatcher.kt`, `EpgAutoMatcher.kt`)؛ Now/Next حساب (`core/live/EpgNowNext.kt`)؛ Room میں ذخیرہ (تقریباً +48 گھنٹے window)؛ WorkManager سے تازہ کاری (`EpgSyncWorker`)۔ **ویب scraping والا EPG grabber موجود نہیں** — ذریعہ نہ ہو تو گائیڈ نہیں |
| 5b | Catch-up | SOURCE VERIFIED | `core/epg/CatchupUrl.kt` — ٹیمپلیٹ انجن: `append/shift/flussonic/xc` اقسام، `${start}/${utc}…` placeholders؛ Xtream `timeshiftUrl()`؛ اس کے علاوہ **مقامی timeshift** (اپنی ریکارڈنگ سے rewind) `core/live/LiveTimeshift.kt` |
| 6 | PiP + Floating Mini Player | SOURCE VERIFIED | **دونوں الگ الگ موجود:** اصل Android PiP (`supportsPictureInPicture="true"` مینی فیسٹ میں، `mobile/playback/PipController.kt`) + ایپ کے اندر تیرتا mini player (`mobile/ui/player/FloatingMiniPlayer.kt`, `MiniPlayer.kt`) + پس منظر پلے بیک سروس (`PlaybackService.kt`) |
| 7 | Multiview | SOURCE VERIFIED | **زیادہ سے زیادہ 4 ٹائلیں**، ہر ٹائل الگ ExoPlayer (`player/LiveEnginePool.kt`)، آواز صرف ایک ٹائل کی، دوسری ٹائلیں 720p تک محدود؛ provider کی `maxConnections` سے جڑا connection budget (`core/live/ConnectionBudget.kt`)۔ MPV ٹائلوں میں استعمال نہیں ہوتا |
| 7b | Recording (DVR) | SOURCE VERIFIED | **حقیقی DVR:** `core/recording/RecordingEngine.kt` — اپنا OkHttp انجن: HLS سیگمنٹ جوڑ کر .ts (AES-128 decrypt سمیت)، DASH کو MediaMuxer سے remux (**FFmpeg نہیں**)؛ شیڈولنگ `RecordingManager/Scheduler` (exact alarm)، **series rules**؛ DRM چینل ریکارڈ سے انکار۔ VOD ڈاؤن لوڈ الگ نظام: Range/resume کے ساتھ (`core/download/DownloadManager.kt`) — صرف فلم/قسط، لائیو نہیں |
| 7c | Sleep Timer | SOURCE VERIFIED | `player/SleepTimer.kt` — 15/30/45/60/90 منٹ یا قسط کے آخر پر؛ موبائل UI `AudioOnlyStage.kt:SleepTimerSheet` |
| 8 | Audio-only mode | SOURCE VERIFIED | دونوں انجنوں میں (`OwnTVPlayer`: mpv `vid=no`؛ `LivePreviewEngine.enterAudioOnly()`)؛ فی چینل یادداشت (`core/player/AudioOnlyStore.kt`)؛ موبائل اسکرین `ui/player/AudioOnlyStage.kt` |
| 8b | Landscape/Fullscreen/RTL | SOURCE VERIFIED | `supportsRtl="true"` (مینی فیسٹ)، `configChanges="orientation|screenSize|…"` + PiP؛ pseudolocale (ar-XB) سے RTL جانچ کا نظام debug بلڈ میں۔ **مگر APK میں اردو UI زبان موجود نہیں** (§2) |
| 9 | Playlist formats | SOURCE VERIFIED | **M3U** مکمل (`core/parser/M3uParser.kt`: tvg-id, tvg-name, tvg-logo, tvg-chno, group-title, **catchup, catchup-source, catchup-days, catchup-correction, timeshift**, #EXTVLCOPT/#EXTHTTP/#KODIPROP)؛ **Xtream Codes** مکمل کلائنٹ (`core/parser/XtreamClient.kt` — player_api.php ایکشنز، get.php لنک کی پہچان `XtreamM3uLink.kt`)؛ **Stalker/MAG portal** تیسرا پروٹوکول (`core/stalker/`)۔ JSON/PLS/XSPF: **موجود نہیں** |
| 10 | Profiles/Favorites/Search/Categories | SOURCE VERIFIED | **Profiles حقیقی:** `ProfileEntity` (نام، اوتار، kids پرچم، **PIN hash**)، فی پروفائل favorites/history/downloads/recordings الگ (profileId FK)؛ الگ **Category Lock** PIN نظام (`core/categorylock/`)۔ Favorites `FavoriteDao`؛ Search **FTS4** full-text (`core/content/SearchReader.kt`)؛ Categories: hide/rename/reorder/custom (`core/customize/`)؛ ذرائع شامل/ترمیم/حذف (`SourceRepository/SourceEditor`)۔ **ذرائع (playlists) کی ترتیب بدلنا: موجود نہیں** (صرف EPG ذرائع reorder ہوتے ہیں) |
| 10b | Backup/Restore | SOURCE VERIFIED | `.own` فارمیٹ (ZIP یا پاس فریز سے AES-GCM سیل بند)، پاس ورڈز انکرپٹ یا حذف (`core/backup/BackupManager.kt`) — ریکارڈنگ/ڈاؤن لوڈ بیک اپ میں شامل نہیں |
| 12 | Subtitles | SOURCE VERIFIED | CEA-608/708 (TS میں زبردستی)، MPV CC ٹریک، VOD میں DVB/PGS تصویری سب ٹائٹل کے لیے ExoPlayer hand-off، **OpenSubtitles سے ڈاؤن لوڈ** (`core/subtitles/OpenSubtitlesClient.kt`)۔ **Teletext decoder: موجود نہیں** (صرف اسٹائلنگ ذکر) |
| — | موبائل پر چلا کر جانچ | PHONE TEST NOT DONE | اس VM پر emulator نہیں؛ تمام OwnTV فیصلے سورس + APK ثبوت پر ہیں |

---

## 5. موازنہ — ہمارے موجودہ پروجیکٹس سے

### 5A. Keyboard: HeliBoard بمقابلہ ہمارا Raheem Keyboard 2.2
(ہمارے حقائق: `offline-smart-voice-prototype/android-kb/TEST-REPORT-FINAL.md` + `FORENSIC-2.2-CAPTURE.md` — ہماری ایپ: package `com.raheem.raheemkeyboard`, v2.2, minSdk 24, **صرف arm64**, APK **259 MB** کیونکہ Whisper ماڈل اندر ہے؛ واحد پرمیشن RECORD_AUDIO)

| پہلو | HeliBoard | ہمارا Keyboard | آگے کون |
|---|---|---|---|
| **آف لائن آواز** | ❌ صفر — بٹن دوسرے voice IME پر switch کرتا ہے | ✅ **اپنا Whisper (sherpa-onnx small) آن-ڈیوائس**، آواز بار، پیراگراف progressive commit | **ہم — فیصلہ کن فرق** |
| اردو املا/اعراب درستگی | عام autocorrect؛ اردو کے لیے کچھ خاص نہیں | ✅ مخصوص **Orthography/Corrector** پائپ لائن (مُحَلّے وغیرہ، Gate 1: 14/14 ×4 runs) | **ہم** |
| اردو ٹائپنگ + تجاویز | لے آؤٹ موجود؛ ڈکشنری **الگ ڈاؤن لوڈ** کے بعد تجاویز | لے آؤٹ + ذاتی ڈکشنری + مقامی bigram اگلے الفاظ (Gate 3 پاس) | برابر (طریقہ مختلف) |
| زبانوں کی چوڑائی | ✅ 129 subtypes: عربی×3، ہندی×3، hi-Latn | اردو + بنیادی انگریزی QWERTY | **HeliBoard** |
| سندھی / Roman Urdu | ❌ دونوں نہیں | ❌ دونوں نہیں | برابر — دونوں کو بنانا پڑے گا |
| پیش گوئی انجن کی پختگی | ✅ AOSP native انجن + user-history learning + contacts dict — برسوں کا آزمودہ | بنیادی ذاتی ڈکشنری + bigram | **HeliBoard** |
| Clipboard | ✅ Pin + تصویری کلپس + retention کنٹرول | history موجود (سادہ) | **HeliBoard** |
| Layout/Themes لچک | ✅ split/floating/one-handed، کسٹم رنگ/لے آؤٹ ایڈیٹر | 7 تھیمز، مالک کے منظور شدہ Gboard-عکس ڈیزائن | HeliBoard فیچرز میں؛ **ڈیزائن ہمارا مقفل فیصلہ ہے** |
| Gesture typing | ⚠️ لائبریری صارف دے تو | ❌ (دیفرڈ «جلد») | برابر عملی طور پر |
| APK سائز / مطابقت | ✅ 22 MB، minSdk 21، چاروں ABI | 259 MB، minSdk 24، صرف arm64 | **HeliBoard** |
| پرائیویسی | INTERNET نہیں | انٹرنیٹ استعمال نہیں ہوتا (آف لائن ماڈل) | برابر |

**قابلِ استعمال ماڈیولز (ایماندارانہ جائزہ):** HeliBoard سے **کوڈ اٹھا کر اپنے پروجیکٹ میں ڈالنا GPL-3.0 کی وجہ سے پوری ایپ کو GPL کر دے گا** (§7) — اس لیے براہ راست کوڈ نقل مناسب نہیں۔ جو چیزیں **خیال/پیٹرن** کے طور پر لے کر اپنی کوڈ میں خود لکھی جا سکتی ہیں: (1) Clipboard **pin + retention** کا ڈیزائن؛ (2) **Incognito** موڈ؛ (3) لے آؤٹ کو **JSON ڈیٹا فائل** بنانے کا طریقہ (اسی سے عربی/ہندی لے آؤٹ اور نیا **سندھی لے آؤٹ** ہمارے کی بورڈ میں شامل ہو سکتا ہے — لے آؤٹ حروف کی ترتیب ہے، ڈیٹا ہے)؛ (4) ڈکشنری **درآمد (file picker)** کا بہاؤ — صارف `.dict` فائل دے تو تجاویز فعال؛ (5) user-history سے سیکھنے کا اصول۔ عربی/ہندی کی **ڈکشنری فائلیں** (`main_ar`, `main_hi`) AOSP dictionary repo میں موجود ہیں، مگر اس repo کا لائسنس اس آڈٹ میں **NOT CHECKED** — استعمال سے پہلے تصدیق لازم۔

### 5B. Live TV: OwnTV بمقابلہ ہماری Web ایپ (Raheem Live TV)
(ہماری صورتحال: 2,049 چینلز، **صرف سرکاری ذرائع**، tuned hls.js + recovery (Feature #1 منظور شدہ)، fallback chains؛ پارسر اس وقت tvg فیلڈز چھوڑ دیتا ہے)

| پہلو | OwnTV | ہماری Web ایپ | نوٹ |
|---|---|---|---|
| پلیئر انجن | ✅✅ دو انجن Exo+MPV + خودکار fallback سیڑھی | hls.js (ایک انجن) + alternate-link fallback | OwnTV واضح آگے — MPV وہ خراب TS اسٹریمز بھی چلاتا ہے جو براؤزر نہیں چلا سکتا |
| EPG | ✅ مکمل نظام + matcher + catch-up templates | ❌ کوئی EPG نہیں (قانونی PK/IN EPG ذریعہ ہی دستیاب نہیں — Treasure رپورٹ) | فیچر OwnTV کا شاندار، مگر **ہمارے لیے ذریعہ (source) ہی موجود نہیں** |
| پلے لسٹ ماڈل | صارف اپنی M3U/Xtream/Stalker ڈالے | ہم منتخب سرکاری کیٹلاگ دیتے ہیں | **بنیادی فلسفہ مختلف** — نیچے دیکھیں |
| Resume/آخری چینل | ✅ StartupMode سمیت | Treasure فہرست میں «چھوٹی جیت» کے طور پر موجود، شامل ہونا باقی | خیال لینے کے قابل |
| Multiview/Recording/Audio-only | ✅ سب حقیقی | ❌ ویب میں نہیں | Android ایپ بنے تو متعلقہ |
| Profiles/PIN | ✅ | ❌ | ویب پورٹل کے لیے غیر متعلق |
| اردو UI | ❌ APK میں `ur` زبان ہی نہیں | ✅ اردو-اول | **ہم آگے** |
| مطابقت/تقسیم | APK سائیڈلوڈ، 58.6 MB | لنک کھولو، چل پڑی | ویب کا فائدہ برقرار |

**ہمارے لیے غیر متعلق یا ممنوع رخ (صاف لفظوں میں):** OwnTV کی طاقت کا بڑا حصہ **Xtream Codes اور Stalker portal** جیسے پروٹوکولز ہیں — یہ عموماً **ادائیگی والے غیر سرکاری IPTV ری سیلر** سسٹمز کے لیے بنے ہیں۔ ہمارا اصول «صرف سرکاری ذرائع» ہے، اس لیے یہ فیچرز ہمارے پورٹل میں **نہ شامل کیے جا سکتے ہیں، نہ ان کی موجودگی ہمارے لیے خوبی ہے**۔ اسی طرح اس کی Movies/Series لائبریری (TMDB) ہمارے ماڈل سے باہر ہے۔ OwnTV بطور **ذاتی پلیئر** (صارف کی اپنی جائز پلے لسٹ کے لیے) ایک بہترین ایپ ہے — مگر یہ ہمارے کیوریٹڈ پورٹل کا متبادل ڈیزائن نہیں۔

---

## 6. ترجیحی فہرست — لینے کے قابل بہترین چیزیں

**Keyboard (ہماری ایپ میں، اپنے کوڈ سے):**
1. **JSON-لے آؤٹ نظام** → عربی + ہندی لے آؤٹ شامل، اور **سندھی لے آؤٹ خود تحریر** (ڈیٹا کام — سب سے سستا بڑا فائدہ)
2. **Clipboard Pin + retention کنٹرول**
3. **Incognito موڈ** (آواز/ٹائپنگ سیکھنا بند)
4. **ڈکشنری درآمد کا بہاؤ** (`.dict` فائل پکر) — لائسنس تصدیق کے بعد `main_ar/main_hi/main_ur` فائلیں صارف کو دی جا سکیں
5. One-handed / split موڈز (بعد میں)

**Live TV (ہماری ویب ایپ / مستقبل کی Android ایپ کے لیے خیالات):**
1. **EPG Matcher کا ڈیزائن** (normalize + fuzzy میل) — *جب* کوئی قانونی EPG ذریعہ ملے
2. **Catch-up URL ٹیمپلیٹ** کا پیٹرن (سرکاری ذریعہ catch-up دے تو)
3. **Resume last channel + Startup modes** (چھوٹا، فوری فائدہ)
4. Multiview کا **connection-budget** اصول اور Audio-only موڈ کا خیال

---

## 7. لائسنس اور قانونی شرائط (اصل فائلوں سے)

### HeliBoard — GPL-3.0-only
- ثبوت: ریپو میں `LICENSE` = مکمل GPLv3 متن؛ GitHub API بھی `GPL-3.0`؛ فائل ہیڈرز `SPDX: Apache-2.0 AND GPL-3.0-only` (بنیاد AOSP LatinIME **Apache-2.0** ہے — `LICENSE-Apache-2.0` ساتھ موجود؛ HeliBoard کی اپنی تبدیلیاں GPL-3.0)؛ آئیکن/کچھ assets **CC-BY-SA-4.0** (`LICENSE-CC-BY-SA-4.0`)۔
- **عملی شرائط (GPLv3 کے operative نکات):** استعمال، مطالعہ، تبدیلی — بشمول تجارتی — آزاد۔ **مگر** اس کوڈ پر مبنی APK **تقسیم** کرتے ہی: (a) پوری ایپ **GPLv3 کے تحت** لائسنس کرنا ہوگی، (b) **مکمل متعلقہ سورس کوڈ** وصول کنندہ کو دستیاب کرنا ہوگا، (c) کاپی رائٹ نوٹسز محفوظ، تبدیلیاں ظاہر کرنا ہوں گی، (d) patent grant شامل؛ کوئی warranty نہیں۔ یعنی ہماری بند/ذاتی برانڈنگ والی تقسیم اس کوڈ کے ساتھ **ممکن نہیں** — سورس کھولنا لازمی شرط ہے۔
- تھرڈ پارٹی: AndroidX/Kotlin/Compose — Apache-2.0 (APK کے `META-INF/**/LICENSE.txt` سے تصدیق)؛ `colorpicker-compose` (JitPack سے) — پروجیکٹ کا اعلان کردہ لائسنس اس آڈٹ میں درخت-سطح پر NOT CHECKED؛ کوئی نیٹ ورک/ایڈز/analytics لائبریری نہیں۔ **غیر تجارتی (non-commercial) شرط کہیں نہیں ملی۔**

### OwnTV — GPLv3 (ایک اہم بے قاعدگی سمیت)
- **OwnTV_Core:** `LICENSE` فائل میں GPLv3 متن موجود؛ API بھی GPL-3.0۔
- **OwnTV_Mobile:** ⚠️ **ریپو میں LICENSE فائل موجود ہی نہیں** (API: `license: null`) — مگر `README.md` §License میں واضح اعلان ہے: «Released under GPL v3.0, the same as the rest of OwnTV» + GPLv3 بیج۔ یعنی مصنف کی نیت/اعلان GPLv3 ہے، لیکن فائل کی عدم موجودگی ایک قانونی ابہام ہے جسے بنیاد بنانے سے پہلے مصنف سے وضاحت/فائل کا کہنا دانشمندی ہوگا۔
- **OwnTV_libmpv** ریپو (بلڈ اسکرپٹس): API کے مطابق **MIT** — **مگر** اس سے بنی اصل لائبریریاں mpv + FFmpeg ہیں: mpv = LGPLv2.1+/GPLv2+ آمیزہ، FFmpeg = بلڈ فلیگز پر منحصر LGPLv2.1+ یا **GPL** (`--enable-gpl` ہو تو)۔ APK کے اندر ان native لائبریریوں کی الگ لائسنس فائل **نہیں ملی** — تقسیم سے پہلے FFmpeg بلڈ کنفگریشن کی تصدیق **لازم** ہوگی (NOT VERIFIED اس آڈٹ میں)۔
- تھرڈ پارٹی (Core + Mobile catalogs سے): Media3, Room, WorkManager, CameraX, Compose — Apache-2.0؛ OkHttp 5 — Apache-2.0؛ Coil 3 — Apache-2.0؛ Koin — Apache-2.0؛ ZXing — Apache-2.0؛ **juniversalchardet — MPL-1.1** (کمزور copyleft: اس لائبریری کی فائلوں کا سورس دینا ہوگا، پوری ایپ پر اثر نہیں)؛ **Play Services Cast Framework — Google کی ملکیتی شرائط** (اوپن سورس نہیں)؛ SQLite — public domain۔
- مجموعی نتیجہ: **دونوں پروجیکٹ GPL خاندان کے ہیں — استعمال آزاد ہے، مگر ان کے کوڈ پر بنی تقسیم شدہ APK کا سورس کھولنا قانونی شرط ہے۔** کوئی non-commercial پابندی کسی میں نہیں۔

---

## 8. موبائل مطابقت اور انسٹال مسائل — مالک کے خصوصی سوال کا ثبوت پر مبنی جواب

**سوال:** کیا OwnTV Mobile کی APK عام Android فون پر انسٹال ہونے میں واقعی کوئی مسئلہ رکھتی ہے؟

**جواب (APK VERIFIED): اصل standard فائل صاف ہے — مسئلہ فائل کے انتخاب میں ہے:**

| جانچ | نتیجہ |
|---|---|
| `OwnTV-Mobile-v1.1.1.apk` (58.6 MB) | native-code = `arm64-v8a + armeabi-v7a` — 64-bit اور پرانے 32-bit ARM **دونوں ایک فائل میں**؛ minSdk 26 (Android 8.0+)؛ دستخط اصل ریلیز کی — **عام ARM فون پر انسٹال ہونی چاہیے** |
| `OwnTV-Mobile-x86_64-v1.1.1.apk` (41.6 MB) | native-code = **صرف `x86_64`** (aapt2 سے تصدیق) — یہ emulator/PC کے لیے ہے۔ **کسی بھی اصل ARM فون پر یہ فائل انسٹال نہیں ہوگی** — Android «App not installed» دے گا (کوئی مطابق native لائبریری نہیں) |
| الجھن کی وجہ | دونوں فائلوں کا package ایک (`tv.own.owntv.mobile`)، versionCode ایک (10101)، نام تقریباً ایک — ریلیز صفحے پر ساتھ ساتھ پڑی ہیں؛ غلط فائل اٹھانا آسان ہے |
| دوسری حد | Android **8.0 سے پرانے** فون پر دونوں فائلیں نہیں چلیں گی (minSdk 26) |
| تقسیم کا رخ | ایپ Play Store پر نہیں — GitHub سے سائیڈلوڈ؛ «unknown apps» اجازت اور Play Protect وارننگ رگڑ ہے، فائل کی خرابی نہیں |

**HeliBoard مطابقت (APK VERIFIED):** ایک Universal APK (چاروں ABI)، minSdk 21 (**Android 5.0+**)، 22.4 MB، اصل دستخط — انسٹال مطابقت دونوں میں سب سے بہتر۔ `nouserlib` ویرینٹ وہی ایپ ہے، فرق صرف یہ کہ اس میں صارف کی فراہم کردہ gesture لائبریری لوڈ کرنے کا راستہ بند ہے۔

---

## 9. حقیقتاً قابلِ عمل کیا ہے

1. **آج ہی، صفر کوڈ:** مالک کے فون پر دونوں ریلیز APK آزمائی جا سکتی ہیں — HeliBoard (22 MB) ٹائپنگ کے لیے ہمارے وائس کی بورڈ کے **ساتھ ساتھ** (دونوں IME ایک فون پر رہ سکتے ہیں، ضرورت پر switch)؛ OwnTV کی **standard** فائل (x86_64 والی ہرگز نہیں) ذاتی جائز پلے لسٹ چلانے کے لیے۔
2. **ہماری کی بورڈ میں اپنے کوڈ سے شامل کرنا (درمیانی محنت):** JSON لے آؤٹ نظام اپنا کر عربی/ہندی لے آؤٹ + **نیا سندھی لے آؤٹ**؛ clipboard pin؛ incognito۔ یہ سب ڈیٹا/چھوٹے فیچر کام ہیں، ہماری آواز والی روح برقرار رہتی ہے۔
3. **ڈکشنری درآمد کا بہاؤ** — لائسنس تصدیق کے بعد — اردو/عربی/ہندی تجاویز کی اصل طاقت کھولے گا۔
4. **Live TV ویب میں:** resume-last-channel جیسی چھوٹی جیتیں اور (قانونی ذریعہ ملنے پر) EPG matcher ڈیزائن۔
5. **Roman Urdu:** دونوں پروجیکٹس میں موجود نہیں — اس کے لیے Roman Urdu لفظ فہرست **خود بنانی** پڑے گی (corpus سے)؛ یہ الگ چھوٹا تحقیقی کام ہے، ناممکن نہیں۔

## 10. جو مناسب نہیں (صاف لفظوں میں)

1. **HeliBoard کو بنیاد بنا کر ہماری کی بورڈ اس پر دوبارہ بنانا — نامناسب۔** وجوہات: (a) اس میں آواز **ہے ہی نہیں** — ہماری ایپ کی اصل پہچان (آف لائن اردو Whisper) وہاں بھی ہمیں خود ہی لگانی پڑے گی، یعنی اصل مشکل کام وہیں کا وہیں؛ (b) مالک کا مقفل کردہ **Gboard-عکس اردو-اول ڈیزائن** HeliBoard کے اپنے ڈیزائن سے بدل جائے گا؛ (c) پوری پروڈکٹ **GPL-3.0** ہو جائے گی — سورس کھولے بغیر تقسیم ممکن نہیں۔
2. **OwnTV کو ہماری Live TV ایپ کی بنیاد بنانا — نامناسب۔** وجوہات: (a) اس کا ماڈل «صارف اپنی پلے لسٹ ڈالے (M3U/Xtream/Stalker)» ہے، ہمارا «منتخب سرکاری کیٹلاگ» — بنیادی تصادم؛ (b) تین ریپوز کا GPL اسٹیک + اپنی libmpv بلڈ چین — اس VM/ہماری موجودہ صلاحیت سے بھاری؛ (c) اس میں **اردو UI ہی نہیں** — اردو-اول پروڈکٹ کے لیے شروع سے ترجمہ کرنا پڑے گا؛ (d) Mobile ریپو میں LICENSE فائل کی عدم موجودگی (§7)۔
3. **کسی بھی پروجیکٹ سے «مکمل پاس» کی توقع** — دونوں میں اہم چیزیں غائب ہیں (HeliBoard: آواز، سندھی، Roman Urdu؛ OwnTV: اردو، قانونی-ذریعہ ماڈل، playlist reorder، teletext)۔ رپورٹ میں ہر غیاب درج ہے۔
4. **RUN VERIFIED کا دعویٰ** — اس VM پر emulator نہیں؛ رن ٹائم سلوک (HeliBoard کی تجاویز کی روانی، OwnTV کی zapping رفتار/EPG میل کی درستی) **فون ٹیسٹ کے بغیر غیر تصدیق شدہ** ہے۔

---

## ⭐ حتمی فیصلے (Verdicts)

### Keyboard Verdict — HeliBoard
**بنیاد نہیں — منتخب فیچرز/خیالات لینے کے قابل، اور ساتھ چلانے کے قابل ایک مضبوط ٹائپنگ کی بورڈ۔**
HeliBoard ٹائپنگ کی دنیا میں ہم سے آگے ہے (129 subtypes، پختہ native prediction، clipboard pin، split/floating، 22 MB universal APK) — مگر ہماری کی بورڈ کی روح **آف لائن اردو آواز** ہے جو اس میں **صفر** ہے، اور سندھی و Roman Urdu بھی دونوں میں نہیں۔ بنیاد بدلنے سے ہم اپنا مقفل ڈیزائن اور اپنی واحد انوکھی طاقت دونوں کھو کر بھی آواز کا کام نئے سرے سے کریں گے — یہ تجارت خسارے کی ہے۔ **درست راستہ:** ہماری ایپ میں (1) JSON لے آؤٹس سے عربی/ہندی + نیا سندھی لے آؤٹ، (2) clipboard pin، (3) incognito، (4) ڈکشنری درآمد — اپنے کوڈ سے شامل کرنا؛ اور چاہیں تو HeliBoard کو فون پر **ساتھی ٹائپنگ کی بورڈ** کے طور پر رکھنا۔

### Live TV Verdict — OwnTV Mobile
**بنیاد نہیں — فیچر ڈیزائن کی کان، اور ذاتی استعمال کے لیے بہترین تیار پلیئر۔**
OwnTV تکنیکی طور پر اس آڈٹ کی سب سے متاثر کن چیز ہے: دو انجن + خودکار fallback، حقیقی EPG/matcher/catch-up، DVR ریکارڈنگ، multiview، profiles — یہ سب **سورس میں ثابت** ہے۔ مگر یہ **کسی اور مقصد** کی ایپ ہے: صارف کی اپنی (عموماً Xtream/Stalker) پلے لسٹ چلانے کے لیے۔ ہمارا مقصد منتخب **سرکاری** کیٹلاگ ہے؛ اوپر سے GPL اسٹیک، تین ریپوز، libmpv بلڈ چین اور اردو کی عدم موجودگی اسے ہماری بنیاد بننے سے روکتے ہیں۔ **درست استعمال:** اس سے EPG matcher، catch-up template، resume/startup-mode اور connection-budget جیسے **ڈیزائن خیالات** لینا؛ اور مالک چاہیں تو اپنی جائز پلے لسٹ کے لیے **standard APK** (x86_64 والی نہیں) فون پر ذاتی پلیئر کے طور پر آزمانا۔

---
**آڈٹ دیانت نوٹ:** جو چیک نہیں ہو سکا وہ چھپایا نہیں گیا — HeliBoard کی Settings کا اردو ترجمہ: NOT CHECKED · AOSP dictionaries repo کا لائسنس: NOT CHECKED · FFmpeg بلڈ فلیگز (LGPL/GPL تعین): NOT VERIFIED · دونوں ایپوں کا رن ٹائم رویہ: PHONE TEST NOT DONE (VM پر emulator نہیں)۔ ہمارے کسی اصل پروجیکٹ کی کوئی فائل تبدیل نہیں ہوئی؛ کوئی build، commit، push یا publish نہیں ہوا۔
