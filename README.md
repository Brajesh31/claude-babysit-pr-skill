# Babysit PR

Monitor a GitHub pull request through CI checks, reviews, comments, and merge-readiness changes.

## Features

- Tracks required CI checks and reports failures.
- Monitors review activity and unresolved review threads.
- Detects base-branch changes and merge-state problems.
- Supports `ready`, `green`, and `any-change` completion modes.
- Uses a single watcher per pull request and records reported events between runs.

## Requirements

- GitHub CLI (`gh`) installed and authenticated.
- Python 3.10 or later.
- Access to the repository containing the pull request.

## Installation

Copy this skill directory into your agent's skills folder. Ensure the helper script is executable:

```bash
chmod +x .claude/skills/babysit-pr/scripts/pr-wait
```

## Usage

The skill instructions are in `SKILL.md`. The helper can be run directly:

```bash
.claude/skills/babysit-pr/scripts/pr-wait <pr> [--repo owner/name] [--until ready|green|any-change]
```

- `ready`: required checks pass, a qualifying review bot has reviewed the current head, and no review threads remain unresolved.
- `green`: required checks pass.
- `any-change`: stop after the first new event.

The script uses the GitHub CLI and stores watcher state under `~/.local/state/pr-wait/`.

## Repository Contents

- `SKILL.md`: Pull request monitoring instructions.
- `scripts/pr-wait`: Python helper that watches a pull request and reports meaningful events.
- `README.md`: Installation and usage guidance.
- `LICENSE`: MIT License notices.

## License

MIT License. See [LICENSE](LICENSE).
