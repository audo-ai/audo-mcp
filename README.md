# Audo MCP server

Audo is a set of audio and video tools for you and your AI assistant: remove noise, enhance voices, transcribe, cut, and more. Pro sound, without the studio.

This repository documents Audo's hosted MCP server at `https://audo.ai/mcp`. It works over streamable HTTP. Sign-in is OAuth: the first time you use it, your assistant asks you to sign in at audo.ai. The repository also holds the `server.json` entry for the official MCP Registry, `ai.audo/audo`.

## Add it

**Claude Code**

```bash
claude mcp add --transport http --scope user audo https://audo.ai/mcp
```

**Any MCP client**

```json
{
  "mcpServers": {
    "audo": {
      "url": "https://audo.ai/mcp"
    }
  }
}
```

Setup guides for ChatGPT, Claude, and other assistants: https://audo.ai/setup

## Tools

| Tool | What it does |
| --- | --- |
| `remove_noise` | Clears the background, keeps your voice. |
| `enhance_voice` | Makes any recording sound studio-quality: echo, muffled, and phone voices. |
| `transcribe_audio` | Speech to text, who said what: 60 languages, SRT and VTT captions, word times. |
| `align_script` | Syncs a script to audio, for captions. |
| `remove_fillers` | Cuts the ums and uhs. |
| `remove_silence` | Cuts out long pauses. |
| `cut_audio` | Trims the start, middle, or end. |
| `join_audio` | Merges files into one. |
| `mix_tracks` | Layers voices and music. |
| `replace_audio` | New sound, same picture. |
| `adjust_loudness` | Makes quiet audio louder. |
| `level_speakers` | Evens out loud and quiet voices. |
| `convert_audio` | Audio to MP3, WAV, and more. |
| `analyze_audio` | Length, format, and more. |

Files and jobs: `create_upload`, `import_file` (fetches a direct file URL), `list_files`, `get_file`, `get_job`, `get_download`, `get_transcript`, `cancel_job`, and `get_account`. Uploaded files are deleted after 24 hours.

## Free use and pricing

Every tool has a free part each day, with no payment. Beyond it, you pay per minute of audio, and only for jobs that succeed. Prices: https://audo.ai/pricing

## Example prompts

- Use Audo to remove the background noise from interview.wav.
- Use Audo to transcribe meeting.mp4 with speaker labels and give me SRT captions.
- Use Audo to make voice-note.m4a sound like it was recorded in a studio.
- Use Audo to cut the first 30 seconds from podcast.wav.

## Links

- MCP docs: https://audo.ai/docs/mcp
- API docs: https://audo.ai/docs/api
- Every tool, for agents: https://audo.ai/llms.txt
- Setup guides: https://audo.ai/setup

## Support

support@audo.ai
