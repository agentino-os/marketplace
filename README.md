# dagoSte/marketplace — Agentino skill registry (provisional)

This repo is an **interim registry** for Agentino marketplace skills while the canonical [`agentino/marketplace`](https://github.com/agentino) organisation registry is still being set up.

## What lives here

`index.json` — a flat list of publicly-installable Agentino skills, each pointing at its canonical `dagoSte/agentino-skill-<name>` repo. The shape matches what the Agentino client expects in `agentino.skills.github.fetch_index()`:

```json
{
  "version": 1,
  "updated_at": "ISO-8601 timestamp",
  "skills": [
    {
      "name": "video-brief",
      "repo": "dagoSte/agentino-skill-video-brief",
      "description": "...",
      "author": "Agentino",
      "tags": ["video", "media", "ffmpeg", "llm-input", "marketplace"],
      "license": "MIT",
      "latest_version": "1.0.0",
      "versions": ["1.0.0"]
    }
  ]
}
```

## How consumers use it

Point Agentino at this registry instead of the default:

```bash
agentino marketplace search video --registry dagoSte/marketplace
agentino marketplace install dagoSte/agentino-skill-video-brief --registry dagoSte/marketplace
```

Once `agentino/marketplace` goes live, the `--registry` flag can be dropped and every entry is expected to migrate there verbatim.

## Why provisional

The Agentino CLI code (`agentino/skills/github.py`) defaults to `agentino/marketplace` but gracefully degrades to an empty skill list when that repo doesn't exist. This provisional registry unblocks `agentino marketplace search` today without waiting on the canonical setup.

## Keeping `index.json` in sync

Source of truth is each skill's `skill.yaml` `metadata` block. To add or update an entry, run `agentino marketplace publish --path <skill.yaml>` in the skill repo, copy the emitted JSON object, and drop it into the `skills` array here. The `updated_at` timestamp gets bumped on every commit.
