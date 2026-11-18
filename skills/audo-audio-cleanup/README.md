# Audo AI Audio & Video Cleanup Skill (`audo-audio-cleanup`)

A standalone Agent Skill for **Claude Code**, **OpenCode**, **Codex**, **Cursor**, and **Gemini CLI** that gives your coding agent all **44 Audo AI audio, video, voice, music, and sound-effect tools** plus plain-language `find_tools` discovery and **4 built-in multi-step workflows**: clean up noisy audio and video, remove room echo, split 4-stem music or video dialogue/music/effects, transcribe speech in 60 languages with speaker diarization and SRT/VTT subtitles, cut filler words (`um`, `uh`) and silences, bleep swear words, reframe horizontal video to 9:16, burn captions, balance speakers, normalize LUFS loudness, mix tracks with ducking, generate text-to-speech, dub SRT/VTT subtitles, design or clone voices, run speech-to-speech voice conversion, generate sound effects, and compose royalty-free AI music.

Works in two modes:
1. **Zero-setup Free API (`https://audo.ai/api/try/<tool>`)** — No account or API key required for the free portion of each file (and whole-file LUFS / media analysis is always free, up to 20 files a day).
2. **Audo MCP Server (`https://audo.ai/mcp`)** — Connect once via OAuth or `AUDO_API_KEY` to chain tools by `file_id` and process full-length recordings up to 10 hours / 10 GB.

---

## Quick Install

### 1. Install the Skill
Copy this folder (`skills/audo-audio-cleanup`) into your agent's skills directory:

```bash
# Project-scoped (Claude Code, OpenCode, Codex, Cursor):
mkdir -p .claude/skills/audo-audio-cleanup
cp skills/audo-audio-cleanup/SKILL.md .claude/skills/audo-audio-cleanup/SKILL.md

# Or user-scoped across all projects:
mkdir -p ~/.claude/skills/audo-audio-cleanup
cp skills/audo-audio-cleanup/SKILL.md ~/.claude/skills/audo-audio-cleanup/SKILL.md
```

### 2. Optional: Connect the Audo MCP Server (`https://audo.ai/mcp`)
For full-file processing and multi-step tool chaining without re-uploading between steps:

- **Claude Code:**
  ```bash
  claude mcp add --transport http --scope user audo https://audo.ai/mcp
  ```
- **Codex:**
  ```bash
  codex mcp add audo --url https://audo.ai/mcp
  codex mcp login audo
  ```
- **OpenCode (`~/.config/opencode/opencode.json`):**
  ```json
  {
    "$schema": "https://opencode.ai/config.json",
    "mcp": {
      "audo": {
        "type": "remote",
        "url": "https://audo.ai/mcp",
        "enabled": true
      }
    }
  }
  ```
- **Gemini CLI:**
  ```bash
  gemini mcp add --transport http --scope user audo https://audo.ai/mcp
  ```

---

## Tools Included

| Tool | MCP Name | Free API Endpoint | Free Tier | Paid Rate (List) |
|---|---|---|---|---|
| Remove noise | `remove_noise` | `POST /api/try/remove-noise` | First 1 min, 3/day | $0.05/min ($3.00/hr) |
| Enhance voice (echo & mic fix) | `enhance_voice` | `POST /api/try/enhance-voice` | First 1 min, 3/day | $0.05/min ($3.00/hr) |
| Split vocal, music & video stems | `split_audio` | `POST /api/try/split-stems` | First 45s, 3/day | $0.08/min ($4.80/hr) |
| Transcribe (60 languages, speakers, SRT) | `transcribe_audio` | `POST /api/try/transcribe` | First 2 min, 3/day | $0.03/min ($1.80/hr) |
| Align script to audio/video | `align_script` | `POST /api/try/align-script` | First 2 min, 3/day | $0.03/min ($1.80/hr) |
| Remove filler words (`um`, `uh`, repeats) | `remove_fillers` | `POST /api/try/remove-fillers` | First 2 min, 3/day | $0.03/min ($0.01 w/ transcript) |
| Remove silence (shorten pauses) | `remove_silence` | `POST /api/try/remove-silence` | First 3 min, 10/day | $0.01/min ($0.60/hr) |
| Level speakers (host & guest balance) | `level_speakers` | `POST /api/try/level-speakers` | First 3 min, 3/day | $0.02/min ($1.20/hr) |
| Media info & LUFS meter | `analyze_audio` | `POST /api/try/media-info` | Whole file, 20/day | Always free |
| Set loudness (`-16` / `-14` / `-23` LUFS) | `adjust_loudness` | `POST /api/try/loudness` | First 3 min, 10/day | $0.01/min ($0.60/hr) |
| Mix tracks (align & duck music) | `mix_tracks` | `POST /api/try/mix` | First 3 min, 3/day | $0.02/min ($1.20/hr) |
| Burn styled captions | `add_captions` | `POST /api/try/add-captions` | First 2 min, 3/day | $0.04/min ($0.01 w/ transcript) |
| Active-speaker 9:16 video reframe | `reframe_video` | `POST /api/try/reframe-video` | First 2 min, 3/day | $0.03/min ($0.01 w/ plan) |
| Text to speech (`eleven_v4`, `gemini_3_8_flash`) | `text_to_speech` | `POST /api/try/text-to-speech` | Up to 300 chars, 3/day | $0.05–$0.20/1k chars |
| Voice cloning (`list_voices`, `save_voice`) | `clone_voice` | `POST /api/try/voice-cloning` | Up to 300 chars, 3/day | $0.12/1k chars |
| Voice design from description | `design_voice` | `POST /api/try/voice-design` | 3 designs/day | $0.02/design |
| Voice changer (`eleven_sts_v2`, `heygen_sts`) | `change_voice` | `POST /api/try/voice-changer` | First 30s, 3/day | $0.30/min ($18.00/hr) |
| AI music & sound effects (`lyria_3_5`, `eleven_sfx_v3`) | `generate_music`, `generate_sound_effect` | `POST /api/try/generate-music`, `POST /api/try/sound-effect-generator` | 2 tracks / up to 10s SFX | $0.10–$0.20/track, $0.005/s |
| Bleep censor & SRT to speech | `censor_audio`, `speak_subtitles` | `POST /api/try/censor-audio`, `POST /api/try/speak-subtitles` | First 2m / 300 chars, 3/day | $0.03/min (+$0.05 bg), $0.05–$0.20/1k chars |
| Everyday audio & video transforms (22 tools) | `cut_audio`, `join_audio`, `convert_audio`, `replace_audio`, `change_speed_pitch`, `find_key_bpm`, `reverse_audio`, `mute_video`, `translate_transcript`, `translate_subtitles`, `shorten_transcript`, `find_highlights`, `audio_to_video`, `add_overlay`, `convert_video`, `compress_video`, `crop_video`, `resize_video`, `reverse_video`, `loop_video`, `video_to_gif`, `get_frame` | `POST /api/try/<slug>` | Free allowance on every tool | Free to $0.05/min (or $0.01/job, $0.05/1k chars per language) |

---

## Example Prompts

- *"Clean up `episode-raw.wav`: check its LUFS loudness first, remove the background noise, cut the ums and uhs, shorten pauses over 0.8 seconds, and save the result at podcast loudness (-16 LUFS)."*
- *"Remove the room echo from `lecture.mp4` while keeping the video untouched, and generate an `.srt` subtitle file."*
- *"Transcribe `interview.m4a` with speaker labels and save both the plain text transcript and SRT captions."*
- *"Even out the quiet guest and loud host in `podcast-interview.wav`, then mix `intro-music.mp3` underneath with automatic ducking."*

---

## Links

- **Website:** [https://audo.ai](https://audo.ai)
- **MCP Setup Guide:** [https://audo.ai/setup](https://audo.ai/setup)
- **API & Free API Docs:** [https://audo.ai/docs/api](https://audo.ai/docs/api)
- **MCP Server Docs:** [https://audo.ai/docs/mcp](https://audo.ai/docs/mcp)
- **Agent Index (`llms.txt`):** [https://audo.ai/llms.txt](https://audo.ai/llms.txt)
- **Support:** [https://audo.ai/support](https://audo.ai/support)
