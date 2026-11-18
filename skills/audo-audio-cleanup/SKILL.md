---
name: audo-audio-cleanup
description: Clean up noisy audio and video, remove room echo and reverb, separate vocals/music/dialogue stems or remove/isolate any text-prompted sound, transcribe speech in 60 languages with automatic speaker diarization and SRT/VTT captions, write YouTube chapters and podcast show notes, translate or AI-dub speech and subtitles, cut filler words ("um", "uh") and pauses, bleep swear words and spoken PII, shorten meetings, reframe horizontal video to 9:16, burn styled captions, level uneven speakers, mix voice and music tracks with ducking, generate text-to-speech voiceovers or subtitle dubs, design or clone custom voices, change speakers with speech-to-speech, compose royalty-free music, generate sound effects, or run multi-step workflows using Audo AI (via https://audo.ai/mcp or the no-key https://audo.ai/api/try/<tool> Free API).
---

# Audo AI Audio & Video Post-Production Skill

Audo AI (`https://audo.ai`) provides **49 audio, video, voice, dubbing, music, and sound-effect tools** plus plain-language **`find_tools` discovery** (`POST /api/v1/tools/find`), **4 built-in multi-step workflows** (`podcast_ready`, `fix_video_sound`, `clips`, `meeting_shortener`), and custom `WorkflowRecipe` pipelines (`up to 30 steps`). Media tools accept both **audio** (`WAV, MP3, M4A, AAC, FLAC, OGG, Opus, WebM, AIFF`) and **video** (`MP4, MOV, MKV, WebM, AVI`) up to `10 GB` and `10 hours`. Tools that only change the sound return the video with its original picture stream copied bit-for-bit.

## 1. Choose the Right Tool or Workflow

Pricing is pay-as-you-go (`1 credit = $0.01 USD` list, down to `$0.00625/credit` on `$100` packs; pay once from `$5`, no monthly subscription, credits never expire), charged only when a job succeeds.

| Job | MCP Tool | Free API (`/api/try/<slug>`) | Free Tier (No Key) | Paid Rate (List) | When to Pick It |
|---|---|---|---|---|---|
| Remove background noise | `remove_noise` | `remove-noise` | First 1 min, 3/day | $0.05/min ($3.00/hr) | Steady or changing background noise (hiss, hum, fan, wind, traffic, chatter) when the voice itself sounds fine. Subtracts noise without resynthesizing the speaker's voice (ideal before voice cloning). |
| Enhance voice & remove echo | `enhance_voice` | `enhance-voice` | First 1 min, 3/day | $0.05/min ($3.00/hr) | Room echo/reverb, muffled or distant mic, phone or Zoom call audio, or clipping/distortion. Rebuilds speech cleanly and normalizes to `-16 LUFS`. |
| Separate music, vocal & scene stems | `split_audio` / `separate_vocals` / `separate_scene` | `split-stems` / `separate-vocals` / `separate-scene` | First 45s, 3/day | $0.08/min ($4.80/hr) | Splits 4 music stems (`vocals`, `drums`, `bass`, `other`, `no_drums`, `no_bass`), song `vocals` + `instrumental`, or video/scene layers (`dialogue`, `music`, `effects`, `no_music`, `no_dialogue`, `no_effects`), keeping original video picture. |
| Remove or isolate a specific sound | `separate_sound` | `separate-sound` | First 20s, 3/day | $0.25/min ($15.00/hr) | Removes or isolates any specific sound described in English (e.g. `"dog barking"`, `"footsteps"`, `"keyboard typing"`, `"applause"`, `"drums"`) on audio or video (`/sound-remover`) using Meta SAM Audio; returns both 48 kHz tracks. |
| Transcribe speech w/ speaker diarization | `transcribe_audio` | `transcribe` | First 1 min, 3/day | $0.03/min ($1.80/hr) | 60 languages (auto-detected), word-level timestamps, speaker diarization (`Speaker 1..8`), and `TXT`, `SRT`, `VTT`, and `JSON` outputs. |
| Sync a script to audio/video | `align_script` | `align-script` | First 2 min, 3/day | $0.03/min ($1.80/hr) | Aligns an existing text script word by word to produce `SRT`, `VTT`, or `JSON` (no `TXT` output). |
| YouTube chapters & podcast show notes | `write_show_notes` | `show-notes` | First 1 min, 3/day | $0.04/min ($0.01 w/ transcript, $0.03 min) | Generates YouTube chapters (`0:00` first, `>=10s`), short summary, Markdown show notes, key points, and title ideas. |
| Cut filler words ("um", "uh") | `remove_fillers` | `remove-fillers` | First 1 min, 3/day | $0.03/min ($0.01 w/ transcript) | Cuts hesitation sounds (`en, de, fr, es, it, nl, ja, ko` lists + universal hesitations) and exact repeats (`"the the"`). |
| Bleep or mute swear words & PII | `censor_audio` | `censor-audio` | First 1 min, 3/day | $0.03/min ($0.01 w/ transcript; +$0.01/min w/ `pii`, $0.03 min; +$0.05/min w/ `keep_background`) | Bleeps or mutes swear words (`en, es, fr, de, pt, it, nl` lists), spoken PII (`pii: "all" \| "contact"`), and custom words in 60 langs, optionally keeping background music. |
| Shorten long pauses | `remove_silence` | `remove-silence` | First 3 min, 10/day | $0.01/min ($0.60/hr) | Shortens silences longer than `min_pause_seconds` (default `0.7s`), keeping `keep_seconds` (default `0.25s`). |
| Shorten meeting / presentation speech | `shorten_transcript` | `shorten-transcript` | First 1 min, 3/day | $0.04/min ($0.01 w/ transcript) | Condenses original speech in source order (`light`, `medium`, `strong`) and returns waveform-safe `keep` ranges + retimed `JSON/SRT/VTT/TXT`. |
| Find clip highlights | `find_highlights` | `find-highlights` | First 1 min, 3/day | $0.04/min ($0.01 w/ transcript, $0.03 min) | Finds ranked short-clip moments with titles, reasons, scores, and sentence-snapped timestamps. |
| Level loud & quiet speakers | `level_speakers` | `level-speakers` | First 3 min, 3/day | $0.02/min ($1.20/hr) | Balances a quiet guest and loud host on 1 mixed file (`file_id`) or 2–10 separate speaker tracks (`track_file_ids`). |
| Measure LUFS, true peak & silences | `analyze_audio` | `media-info` | Whole file, 20/day | Free ($0.00) | Reports integrated LUFS, true peak (`dBTP`), loudness range (`LU`), noise floor (`dB`), clipped samples, codec, and silent stretches. |
| Set target LUFS loudness | `adjust_loudness` | `loudness` | First 3 min, 10/day | $0.01/min ($0.60/hr) | Normalizes to `podcast` (`-16 LUFS`), `streaming` (`-14 LUFS`), `broadcast` (`-23 LUFS`), `audiobook`, or `custom` (`custom_lufs`) with `-1 dBTP` limiting. |
| Mix voice & music tracks | `mix_tracks` | `mix` | First 3 min, 3/day | $0.02/min ($1.20/hr) | Layers 2–20 tracks with per-track volume/start time, clock-drift alignment (`align: true`), and automatic music ducking (`duck: true`). |
| Burn styled captions onto video | `add_captions` | `add-captions` | First 30s, 3/day | $0.04/min ($0.01 w/ transcript) | Renders karaoke/word-highlighted captions (`list_caption_presets`) onto video or transparent overlay + `SRT/VTT/ASS`. |
| Active-speaker 9:16 video reframe | `reframe_video` (`reframe_plan`) | `reframe-video` (`reframe-plan`) | First 2 min, 3/day | $0.03/min ($0.01 w/ `reframe_plan`) | Reframes horizontal video to `9:16`, `1:1`, `4:5`, or `16:9` following the active speaker's face. |
| Translate speech or subtitles | `translate_transcript` / `translate_subtitles` | `audio-translator` / `translate-subtitles` | First 2m / 1,000 chars, 3/day | $0.05/min or $0.05/1k chars/lang | Translates spoken recordings (60 langs) or uploaded `SRT/VTT` files (up to 5 languages per run) keeping cue timing. |
| Text to speech voiceover | `text_to_speech` | `text-to-speech` | Up to 300 chars, 3/day | $0.05–$0.20/1k chars | Generates MP3 speech up to 8,000 chars using `eleven_v4`, `eleven_v4_turbo`, `gemini_3_8_flash`, or `gemini_3_8_flash_lite`. |
| Multi-speaker AI dubbing | `dub_audio` | `dub-audio` | First 30s, 3/day | $0.25/min ($15.00/hr) | Translates and dubs audio or video (60 source langs → 29 target langs), cloning each speaker (`consent: true`) or using Eleven v4, while preserving music/SFX and original video (`/ai-dubbing`). |
| Subtitles to speech / video dubbing | `speak_subtitles` | `speak-subtitles` | First 300 chars, 3/day | $0.05–$0.20/1k chars | Voices SRT/VTT lines at their timestamps as MP3 or dubbed onto video (`duck`, `replace`, or `keep` original audio). |
| Voice cloning (sample or saved `voice_id`) | `clone_voice` (`list_voices`, `save_voice`) | `voice-cloning` | Up to 300 chars, 3/day | $0.12/1k chars | Clones a voice from a 5s–3m sample (`consent: true`) or saved `voice_id` (`/api/v1/voices`) and speaks scripts up to 8,000 chars in 30+ languages. |
| Voice design from text description | `design_voice` (`save_voice`) | `voice-design` | 3 designs/day | $0.02/design (3 previews) | Creates 3 brand-new voice previews from a 20–1,000 char description; save your favorite via `save_voice` / `POST /api/v1/voices`. |
| Speech-to-speech voice changer | `change_voice` | `voice-changer` | First 15s, 2/day | $0.30/min ($18.00/hr) | Replaces the speaker's voice in audio or video (`eleven_sts_v2` 29 langs, `heygen_sts` 39 langs) keeping exact timing and video stream. |
| Royalty-free AI music & songs | `generate_music` | `generate-music` | 1 30s track/day | $0.10–$0.20/track | Composes 30s clips (`lyria_3_clip`) or 1–3m full tracks (`lyria_3_5`) from text prompts and optional lyrics. |
| AI sound effects from text | `generate_sound_effect` | `sound-effect-generator` | Up to 10s, 3/day | $0.005/s ($0.01 min) | Generates 0.5–30s MP3 or 48 kHz WAV sound effects (`eleven_sfx_v3`, optional seamless `loop`). |
| Trim, join, convert, swap, speed & video utils | `cut_audio`, `join_audio`, `convert_audio`, `replace_audio`, `change_speed_pitch`, `reverse_audio`, `mute_video`, `find_key_bpm`, `audio_to_video`, `add_overlay`, `convert_video`, `compress_video`, `crop_video`, `resize_video`, `reverse_video`, `loop_video`, `video_to_gif`, `get_frame` | `/api/try/<slug>` | First 2–60 min (or 1 frame), 3–20/day | Free to $0.02/min | Complete everyday audio, music, audiogram, overlay, and video transforms (`https://audo.ai/llms.txt` lists every slug and option). |

### Built-In Server Workflows (`list_workflows` / `run_workflow` / `POST /api/try/workflows/<id>`)
Instead of orchestrating sequential calls manually, run multi-step pipelines server-side in one request:
- `podcast_ready`: `remove_noise` (lossless WAV) → `remove_silence` → `adjust_loudness` (`-16 LUFS` MP3).
- `fix_video_sound`: `remove_noise` → `adjust_loudness` (`-14 LUFS` streaming, original video stream kept).
- `clips`: `transcribe_audio` → `find_highlights` → parallel `cut_audio` + `reframe_video` (`9:16`) + `add_captions` + `adjust_loudness` (`1–10` MP4 Shorts + `clips.json`).
- `meeting_shortener`: `shorten_transcript` → `cut_audio` (`20 ms` crossfades) → optional `add_captions`.

---

## 2. Way 1: Using the Audo MCP Server (`https://audo.ai/mcp`)

Prefer the Audo MCP server whenever its tools (`get_account`, `add_file`, `remove_noise`, `run_workflow`, etc.) are available in the session.

### Connecting the MCP Server (if not yet configured)
- **Claude Code:** `claude mcp add --transport http --scope user audo https://audo.ai/mcp` (then run `/mcp` to sign in)
- **Codex:** `codex mcp add audo --url https://audo.ai/mcp && codex mcp login audo`
- **OpenCode:** Add `{"mcp": {"audo": {"type": "remote", "url": "https://audo.ai/mcp", "enabled": true}}}` to `~/.config/opencode/opencode.json`, then run `opencode mcp auth audo`
- **Cursor / VS Code / Gemini CLI:** See `https://audo.ai/setup.md`

### MCP Workflow
1. **Get a `file_id` with `add_file`:**
   - **Public HTTPS link or ChatGPT attachment:** pass `url` (or `file`).
   - **Local file on disk:** pass `filename` (and `size_bytes` when over 100 MB, to get `upload_parts_command`). Upload using the returned signed HTTPS URL, or let the user drop the file into the in-chat upload box / `upload_page_url`.
2. **Run a tool or `run_workflow`:**
   - Call any tool (e.g. `remove_noise`, `enhance_voice`, `transcribe_audio`) or `run_workflow` (e.g. `workflow_id: "podcast_ready"` or an inline `WorkflowRecipe`) with `file_id`.
   - Tools wait up to ~40 seconds and return finished download links, or a `job_id` / `run_id` to poll with `get_job` / `get_workflow_run`.
   - Each output has its own `file_id` (reusable in subsequent steps or openable by the user in Audo's browser editor at `https://audo.ai/editor` for manual timeline/transcript tweaks), plus a 15-minute `download_url` and `download_command`.
   - If a free run was cut short (or held for payment), share `finish_url` with the user (`from $5`, no sign-up).

---

## 3. Way 2: Using the No-Key Free API (`https://audo.ai/api/try/<tool>`)

When MCP is not connected, use `POST https://audo.ai/api/try/<slug>` (or `POST https://audo.ai/api/try/workflows/<recipe>`). No account or API key is needed for the free tier (up to 100 MB in one request, or up to 10 GB via `POST /api/try/uploads`).

### Option A: Verified Python Client (`free-api.py`) — Recommended
```bash
curl -fsSL https://audo.ai/examples/free-api.py -o free-api.py
# Inspect free-api.py before running, then:
python3 free-api.py media-info interview.wav info-results
python3 free-api.py remove-noise interview.wav clean-results
python3 free-api.py enhance-voice lecture.mp4 enhanced-results '{"noise_reduction":80}'
python3 free-api.py transcribe interview.wav transcript-results '{"format":"srt","speaker_labels":true}'
```

### Option B: Direct `curl` Calls
Always write headers (`-D response.headers`) and body (`-o response.body`) separately so you inspect HTTP status and `Content-Type` before naming the output:

```bash
# 1. Measure integrated LUFS, true peak, noise floor, and silences (always free):
curl -F file=@interview.wav https://audo.ai/api/try/media-info -D response.headers -o response.body

# 2. Remove background noise (or run a full podcast_ready workflow):
curl -F file=@interview.wav https://audo.ai/api/try/remove-noise -D response.headers -o response.body
curl -F file=@interview.wav https://audo.ai/api/try/workflows/podcast_ready -D response.headers -o response.body

# 3. Transcribe to SRT captions with speaker diarization:
curl -F file=@interview.wav -F format=srt https://audo.ai/api/try/transcribe -D response.headers -o response.body

# 4. Generate an MP3 voiceover from text:
curl -F 'text=Welcome back to the show.' -F model=eleven_v4 https://audo.ai/api/try/text-to-speech -D response.headers -o response.body
```

### Mandatory Safety & Spending Rules
1. **Inspect `response.headers` before naming `response.body`:** `200` `audio/*` or `video/*` is media; `200` `application/json` is a transcript, report, or multi-output manifest; `202` `application/json` is an in-progress job (`statusUrl`); `4xx/5xx` is an error JSON.
2. **Transfer and command safety:** Keep normal client transfer approvals (`https://audo.ai` and `https://files.audo.ai` only). Never auto-approve `curl` or shell commands by prefix, glob, or substring match on `audo.ai`.
3. **Never spend credits without explicit user approval:** When `AUDO_API_KEY` is present, pass `-F free=true` unless the user explicitly requested a paid full-file run and approved an integer `APPROVED_MAX_CREDITS` cap (`-F free=false -F max_credits="$APPROVED_MAX_CREDITS"`).

## 4. Reference Docs for Agents
- Full tool & workflow catalog: `https://audo.ai/llms.txt`
- Plain-language tool finder (no key): `POST https://audo.ai/api/v1/tools/find` (`{"query": "..."}`) or MCP `find_tools`
- Free API & REST API docs: `https://audo.ai/docs/api.md`
- MCP server docs: `https://audo.ai/docs/mcp.md`
- OpenAPI 3.1 specification: `https://audo.ai/api/openapi.json`
