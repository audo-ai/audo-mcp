# Audo MCP server and agent plugin

Audo: pro sound without the studio. Audio and video tools for you and your AI assistant: remove noise, enhance voices, transcribe, cut, and more.

This repository packages Audo's hosted MCP server at **https://audo.ai/mcp**. It uses Streamable HTTP and OAuth. The first time you connect, your assistant asks you to sign in to your Audo account. No API key is required for the OAuth connection.

## Install

### Agent Plugin clients

This repository contains a portable Agent Plugins manifest, `plugin.json`, and a root `mcp.json` pointing to the hosted server. Install the repository using your client's plugin installation workflow. Official marketplace listings are pending; this repository does not imply approval by a marketplace.

### Claude Code

```sh
claude mcp add --transport http --scope user audo https://audo.ai/mcp
```

### Codex

```sh
codex mcp add audo --url https://audo.ai/mcp
codex mcp login audo
```

### Cursor and other MCP clients

For a client's custom MCP configuration, use:

```json
{
  "mcpServers": {
    "audo": {
      "url": "https://audo.ai/mcp"
    }
  }
}
```

Connect through OAuth when the client prompts. See [setup guides](https://audo.ai/setup?utm_source=github&utm_medium=directory) for client-specific instructions.

## What it does

- **Speech cleanup:** `remove_noise` reduces background noise while preserving the recorded voice. `enhance_voice` regenerates supplied speech to restore echo, distortion, muffled recordings, and poor microphones. Both are designed for speech and can remove music or other sounds.
- **Text and captions:** `transcribe_audio` produces text, word timestamps, speaker labels, and TXT/SRT/VTT/JSON outputs. `align_script` aligns an existing script to a recording.
- **Editing:** `cut_audio`, `remove_fillers`, `remove_silence`, and `join_audio` trim recordings, remove filler words or long pauses, and combine files.
- **Finishing:** `level_speakers` balances loud and quiet voices; `mix_tracks` layers and ducks tracks; `adjust_loudness` sets a target loudness; `convert_audio` changes formats; `replace_audio` swaps a video's soundtrack.
- **Music and video:** `split_audio` separates stems; `change_speed_pitch` changes tempo and pitch; `find_key_bpm` measures key and tempo; `reverse_audio` reverses sound; `mute_video` removes a video's audio.
- **Analysis:** `analyze_audio` reports media information, loudness, peaks, clipping, and silence.

Use `add_file` for a direct HTTPS file URL or to start an upload. Supported chat clients also expose Audo's in-chat upload box. Local coding agents can use the returned upload command. Do not pass a local filesystem path as a public URL.

`list_files`, `get_file`, `get_job`, `get_transcript`, `cancel_job`, and `get_account` manage files, status, transcript reading, cancellation, and account information. Processing tools return either finished output or a job to check with `get_job`.

## Example workflows

- Remove background noise from an interview and return the cleaned recording.
- Balance a quiet guest and loud host, then normalize the result for a podcast.
- Transcribe a recording with speaker labels and return the transcript and SRT captions.
- Split vocals and instrumental audio, then change the instrumental's speed or pitch.

## Free use, limits, and privacy

Tools include a free allowance subject to per-tool daily limits. Full-file processing beyond that allowance requires sufficient existing Audo credits. Current allowances and rates are on [Audo's pricing page](https://audo.ai/pricing?utm_source=github&utm_medium=directory).

Files and results are deleted after 24 hours. Output download links are temporary; use `get_file` for a fresh link while the file is retained. Only upload recordings you are permitted to process. See [privacy](https://audo.ai/privacy) and [terms](https://audo.ai/terms).

## Documentation and support

- [MCP documentation](https://audo.ai/docs/mcp?utm_source=github&utm_medium=directory)
- [API documentation](https://audo.ai/docs/api?utm_source=github&utm_medium=directory)
- [Agent-readable tool index](https://audo.ai/llms.txt)
- [Support](https://audo.ai/support) · support@audo.ai

The repository also contains `server.json` for the official MCP Registry entry `ai.audo/audo`. The hosted service is operated by Audo AI LLC; its implementation and credentials are not included here.
