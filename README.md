# agentino-os/marketplace — official Agentino skill registry

This repo is the **canonical registry** for Agentino marketplace skills. The Agentino CLI (`agentino/skills/github.py`) defaults to `agentino-os/marketplace` — users don't need to configure anything.

## What lives here

`index.json` — a flat list of publicly-installable Agentino skills, each pointing at its canonical `agentino-os/agentino-skill-<name>` repo. The shape is what the Agentino client expects in `agentino.skills.github.fetch_index()`:

```json
{
  "version": 1,
  "updated_at": "ISO-8601 timestamp",
  "skills": [
    {
      "name": "video-brief",
      "repo": "agentino-os/agentino-skill-video-brief",
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

Zero configuration — the default works:

```bash
agentino marketplace search video
agentino marketplace install agentino-os/agentino-skill-video-brief
```

## Overriding the registry source

For private forks, local testing, or alternative registries, set the `AGENTINO_MARKETPLACE_REGISTRY` env var:

```bash
export AGENTINO_MARKETPLACE_REGISTRY=some-org/my-registry
agentino marketplace search foo
```

## Contributing a skill

1. Publish your skill as a public GitHub repo following the structure in [docs/skills/marketplace-guide.md](https://github.com/agentino-os/agentino/blob/main/docs/skills/marketplace-guide.md).
2. Run `agentino marketplace publish --path skill.yaml` to generate your entry.
3. Fork this repo, add the JSON object to the `skills` array in `index.json`.
4. Open a pull request — the `updated_at` timestamp gets bumped on merge.

## Keeping `index.json` in sync

Source of truth is each skill's `skill.yaml` `metadata` block. Every entry must match its upstream `name`, `version`, `description`, `tags`, and `license`. Outdated entries are the most common source of "I installed but didn't get the latest version" bug reports.
