---
name: hypermoji
description: Use when a user wants to create or manage Hypermoji mascots or emotes through the Hypermoji CLI, including interactive browsing, mascot generation, mascot import from image URL, emote batch generation, job tracking, and asset download. Requires the `hypermoji` CLI to be installed and authenticated first.
---

# Hypermoji

Use this skill when the user wants to create mascots, generate emote sets, import a character, inspect jobs, browse outputs interactively, or download assets through the Hypermoji CLI.

## Prerequisites

- `hypermoji` should be installed and available on `PATH`
- the user should be authenticated with `hypermoji login`

## When To Use This Skill

Use this skill when the user asks to:

- create a mascot through Hypermoji
- generate one emote or an emote set
- import a character into Hypermoji
- inspect Hypermoji jobs, mascots, or emotes
- download Hypermoji-generated assets

Typical requests:

- "Create a mascot with Hypermoji"
- "Make an emote pack for this mascot"
- "Use Hypermoji CLI to generate a character"
- "Check my Hypermoji jobs"
- "Download this Hypermoji emote"

## Environment Check

Check whether the CLI exists first:

```bash
command -v hypermoji
```

If it is missing, stop and tell the user to install it:

```bash
npm install -g @hypersocial/hypermoji-cli
```

The CLI can also install the public Hypermoji skill:

```bash
hypermoji skills install
```

Check auth first:

```bash
hypermoji --json whoami
```

If that fails, stop and tell the user to run:

```bash
hypermoji login
```

## Core Rules

- Prefer `--json` for commands when you need structured output
- For long-running operations, create the job, then wait for it
- Use batch emote generation instead of multiple single emote commands
- Do not try to automate the browser login flow from this skill
- For human-guided browsing, prefer the interactive shell before raw list commands

## Interactive Shell

For exploration and asset picking, start with:

```bash
hypermoji
```

The interactive shell supports:

- browsing mascots, styles, jobs, and emotes
- visible tabs like `Search`, `Popular`, `Recent`, and `Best`
- type-to-search from any browse list
- `Enter` / `o` to open the selected asset when available
- `d` to download mascot or emote assets locally

Use the interactive shell when the user wants to browse, compare, search, or pick assets manually. Use direct commands with `--json` when you need structured output for follow-up automation.

## Mascot Creation Workflow

1. Check auth
2. Optionally inspect styles:

```bash
hypermoji --json styles list
```

3. Create the mascot:

```bash
hypermoji --json mascots create "<prompt>"
```

4. Wait for the job:

```bash
hypermoji --json jobs wait <job-id>
```

5. Review the returned variations and pick the best one
6. Select the chosen mascot:

```bash
hypermoji --json mascots select <mascot-id>
```

7. Fetch the final mascot details:

```bash
hypermoji --json mascots get <mascot-id>
```

## Emote Generation Workflow

Use this when the user wants an emote pack or emote set.

Suggested default set:

- waving hello
- thinking
- celebrating
- typing
- surprised
- sad

Generate in parallel:

```bash
hypermoji --json emotes batch <mascot-id> "waving hello" "thinking" "celebrating" "typing" "surprised" "sad"
```

Then inspect or wait on the returned jobs:

```bash
hypermoji --json jobs list
hypermoji --json jobs wait <job-id>
```

List emotes for the mascot:

```bash
hypermoji --json emotes list <mascot-id>
```

## Import Workflow

Import a mascot from a public image URL:

```bash
hypermoji --json mascots import <image-url>
```

Then wait for the job and inspect the resulting mascot.

If the installed CLI does not support `mascots import`, tell the user their CLI is older than the skill expects and ask them to upgrade.

## Downloads

Fetch details first, then download what the user needs:

```bash
hypermoji --json mascots get <mascot-id>
hypermoji --json emotes get <emote-id>
hypermoji emotes download <emote-id> --format all
```

In the interactive shell, `d` downloads assets into a local `hypermoji-downloads/` folder under the current working directory.

## Prompt Guidance

Use prompts that describe the result directly:

- character identity
- expression or action
- color palette
- outfit or props
- animation or framing intent

Prefer “inspired by” wording over exact copyrighted character names or exact studio mimicry. Safer examples:

- “a manic green-faced trickster inspired by 90s slapstick comedy”
- “classic family-friendly feature animation look”

Avoid asking for an exact living artist’s style or an exact copyrighted character recreation when a looser prompt will achieve the same goal.

## Output Handling

When using `--json`, parse these common response fields:

- mascot creation: `jobId`, `mascotIds`
- emote creation: `jobId`, `emoteId`
- batch emotes: `emotes[]`
- jobs wait/status: `status`, `variations`, `emoteId`

For user-facing responses:

- summarize what was created
- include the selected mascot ID or emote IDs
- note any remaining manual step only if one actually remains
