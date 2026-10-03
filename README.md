# autoresearch-pro

Improve an existing skill, prompt, article, plugin, workflow, or system mechanism
through small, reversible changes and before/after comparisons.

[中文说明](README_zh.md) · [Core instructions](SKILL.md)

The skill was renamed from **cjl-autoresearch-cc** to **autoresearch-pro**.
This repository was also renamed to autoresearch-pro. The folder and frontmatter name used by
your agent should be `autoresearch-pro`.

## When to use it

Use it for an explicit iteration, a case-based comparison, or an improvement
grounded in failure records. It is not needed for a one-off rewrite, translation,
an explanation of autoresearch, or routine development without an experiment.

Example request:

> Use autoresearch-pro to improve this classification prompt. Preserve the
> original, compare both versions on a few realistic inputs, and deliver one
> useful revision. Do not use paid APIs or modify external systems.

The default is **one round**, with at most **three** when a concrete remaining
problem justifies another round. Save the baseline, test the same tasks, inspect
actual artifacts, and keep only demonstrated improvements. If runtime testing is
unavailable, deliver a separate candidate with that limitation instead of claiming
a quality gain. No mandatory generic checklist, simulated quality percentage, or
100-round loop.

## Install

No package manager dependencies, scripts, credentials, or provider configuration
are required. Clone to a stable directory named after the skill:

```sh
mkdir -p ~/.agents/skills
git clone https://github.com/0xcjl/autoresearch-pro.git ~/.agents/skills/autoresearch-pro
```

These commands assume the destination does not exist. If you already have an
installation, inspect and back it up first; do not overwrite local edits.
Record `git rev-parse HEAD` in the checkout for reproducibility. To update a clean
checkout later, use `git pull --ff-only`; review the diff before reloading clients.

### Agent-specific setup

| Agent | Entry point | Invoke | Verify discovery |
|---|---|---|---|
| Codex | `~/.agents/skills/autoresearch-pro/` | `$autoresearch-pro` or matching task intent | Skill picker; where supported, `codex debug prompt-input` |
| Claude Code | `~/.claude/skills/autoresearch-pro/` | `/autoresearch-pro` | Slash-command picker in a fresh session |
| Hermes | `$HERMES_HOME/skills/autoresearch-pro/` (default `~/.hermes/skills/`) | Ask it to use `autoresearch-pro` | `hermes skills list --source local --enabled-only`, then `skill_view` |
| OpenClaw | Personal shared source or managed `~/.openclaw/skills/autoresearch-pro/` | Ask it to use `autoresearch-pro` | `openclaw skills info autoresearch-pro --agent <id> --json` |

For Claude Code and Hermes on macOS/Linux, keep a single shared source and add
only the required entry points (existing destinations must be inspected first):

```sh
mkdir -p ~/.claude/skills "${HERMES_HOME:-$HOME/.hermes}/skills"
ln -s "$HOME/.agents/skills/autoresearch-pro" "$HOME/.claude/skills/autoresearch-pro"
ln -s "$HOME/.agents/skills/autoresearch-pro" "${HERMES_HOME:-$HOME/.hermes}/skills/autoresearch-pro"
```

For profiles with a different Hermes home, use that profile's skills directory.
Verify that the skill is enabled in the intended profile. A Claude
`additionalDirectories` permission alone is not a skill installation.

Current OpenClaw can discover the personal shared source. Check discovery first;
if it is not available in your installation, use its supported local installer:

```sh
openclaw skills install "$HOME/.agents/skills/autoresearch-pro" --global
openclaw skills info autoresearch-pro --agent <id> --json
```

Replace `<id>` with your configured agent ID. Check `eligible`, `modelVisible`,
and `commandVisible`; agent filters can differ. Installer and symlink behavior
vary by client version. Do not force an install over a different existing copy.
If symlinks are unavailable, copy `SKILL.md` and `references/` together and track
the source commit; update those copies explicitly.

The workflow itself uses standard Markdown and `name`/`description` frontmatter.
It does not depend on Codex-only tools, Claude hooks, Hermes plugins, or OpenClaw
metadata. Use each host's existing file and test tools. A host without tools can
perform a bounded text comparison but must not claim runtime validation.

### Migrate the old name

Back up your current installation outside active skill roots. Compare local edits
before replacing it. Rename active entry directories and any explicit command or
path references to `autoresearch-pro`; update shared links to the new target.
Do not leave an old active copy that can shadow or duplicate discovery. Preserve
historical records and upstream provenance under their original names.
ClawHub is released separately as `0xcjl/autoresearch-pro`, retaining its existing
registry slug. Check the exact registry version before treating a GitHub update
as a registry update. GitHub source is MIT; ClawHub distribution is additionally
authorized under the platform's MIT-0 terms.

## Verification and limits

The 2026-10-03 local comparison used five tasks: two ordinary optimization tasks,
two adjacent non-optimization tasks, and one held-out offline prompt task. The
candidate produced reviewable artifacts for the three optimization tasks where
the old workflow stopped for checklist/confirmation input. Both versions routed
the two adjacent tasks correctly; this does **not** prove higher routing accuracy.

Classification responses were generated in-session; workflow checks were textual.
The held-out case was prepared before editing but evaluated sequentially by the
same evaluator. No production, cross-model, or statistical accuracy claim is made.

Local discovery was checked in Codex, Claude Code, Hermes, and OpenClaw. Hermes
also read the main file and reference successfully. Discovery is not a full
behavior test on every host. Static SkillSpector reported SAFE with score 8 and
one EA2 finding on “Do not ask the user” in the generic-checklist instruction.
The maintainer explicitly accepted that finding; it was not suppressed.
High-impact actions still require explicit authorization in the skill.

## Sources and license

Installation references: [Codex](https://developers.openai.com/codex/skills/),
[Claude Code](https://code.claude.com/docs/en/skills),
[Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills/),
[OpenClaw](https://docs.openclaw.ai/cli/skills).

Inspired by [Karpathy/autoresearch](https://github.com/karpathy/autoresearch) and
[openclaw-autoresearch-pro](https://github.com/0xcjl/openclaw-autoresearch-pro).
[MIT license](LICENSE).
