# claude-babysit-pr-skill

A Claude Code skill for monitoring a GitHub pull request through CI checks, reviews, comments, and merge-readiness changes.

## Overview

This skill helps track the state of a pull request while work is waiting on automated checks or human review. It uses a dedicated watcher workflow rather than encouraging ad hoc polling loops.

The watcher reports meaningful changes. A notification is not itself a merge approval: the pull request must still be checked against the repository's actual merge requirements.

## Main Capabilities

- Monitor required CI checks.
- Detect failed and completed checks.
- Track review activity and comments.
- Identify unresolved review threads.
- Detect base-branch changes and merge conflicts.
- Support different completion conditions.
- Maintain watcher state between runs.

## Completion Modes

| Mode | Meaning |
|---|---|
| `ready` | Required checks pass, qualifying review conditions are met, and no unresolved review threads remain |
| `green` | Required checks pass |
| `any-change` | Stop after the first new event |

The exact merge gate depends on repository branch protection and review policy.

## Requirements

- GitHub CLI (`gh`) installed and authenticated.
- Python 3.10 or later.
- Permission to read the pull request and its check and review status.
- Access to the repository containing the pull request.

## Installation

Clone the repository into the skills directory:

```bash
git clone https://github.com/Brajesh31/claude-babysit-pr-skill.git ~/.claude/skills/babysit-pr
```

Ensure the helper script is executable:

```bash
chmod +x ~/.claude/skills/babysit-pr/scripts/pr-wait
```

## Usage

Invoke the skill when asked to monitor a pull request. The helper can also be run directly:

```bash
~/.claude/skills/babysit-pr/scripts/pr-wait <pr> [--repo owner/name] [--until ready|green|any-change]
```

Use the repository option when the current working directory does not identify the target repository.

## Recommended Workflow

1. Inspect the current PR status and existing review feedback.
2. Confirm the repository and pull request identifier.
3. Start one watcher for the target PR.
4. Wait for a meaningful event rather than repeatedly polling.
5. Re-check the current checks, reviews, comments, and merge state after notification.
6. Address required failures or unresolved feedback.
7. Confirm the actual merge gate before declaring the PR ready.

## State and Resource Management

The helper stores watcher state under:

```text
~/.local/state/pr-wait/
```

Use one watcher per pull request. Avoid running duplicate watchers for the same PR because they consume additional API capacity and can produce redundant notifications.

## Repository Contents

- `SKILL.md`: Pull request monitoring workflow.
- `scripts/pr-wait`: Helper for monitoring pull request events.
- `README.md`: Installation, completion modes, and usage.

## Safety Notes

- Monitor only pull requests that are within the user's authorized work.
- Do not treat a passing CI run as proof that the change is correct.
- Re-check the latest head commit after new pushes.
- Confirm required reviews and unresolved threads.
- Never merge automatically unless the user explicitly authorized that action and the repository policy permits it.

## Troubleshooting

If the watcher cannot access a PR, verify GitHub CLI authentication and repository permissions. If the watcher reports no change, inspect the PR directly and confirm the selected completion mode and required checks.
