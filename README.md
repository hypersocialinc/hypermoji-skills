# Hypermoji Skills

Install the Hypermoji skill with:

```bash
npx skills add hypersocialinc/hypermoji-skills --skill hypermoji
```

The skill uses the published Hypermoji CLI:

```bash
npm install -g @hypersocial/hypermoji-cli
```

Then authenticate once:

```bash
hypermoji login
```

What the skill is for:

- create a mascot from a prompt
- generate an emote set for a mascot
- inspect Hypermoji jobs and results
- download generated mascot or emote assets

Example requests:

- "Use Hypermoji to create a cheerful robot mascot for my SaaS"
- "Generate a default emote set for my mascot"
- "Show me my Hypermoji jobs"
- "Download the assets for this emote"

Repository layout:

```text
skills/
  hypermoji/
    SKILL.md
```
