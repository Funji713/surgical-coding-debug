# Surgical Coding Debug

An evidence-first coding and debugging workflow for focused implementation, bug fixes, and test failures. It reduces unnecessary repository exploration, retries, scope expansion, and token usage while retaining targeted verification.

## Install

Run this in PowerShell to install directly into your local Codex skills directory:

```powershell
git clone https://github.com/Funji713/surgical-coding-debug.git "$env:USERPROFILE\.codex\skills\surgical-coding-debug"
```

To update an existing installation:

```powershell
git -C "$env:USERPROFILE\.codex\skills\surgical-coding-debug" pull --ff-only
```

Start a new Codex task after installation if the current task does not discover the skill.

## Use

```text
$surgical-coding-debug Reproduce this failing API test, isolate the root cause, make the smallest correct patch, and verify it.
```

The workflow acquires only the context needed to reproduce and explain the failure, separates execution problems from reasoning problems, patches the proven cause, and validates at the relevant boundary.

See [SKILL.md](SKILL.md) for the full workflow.
