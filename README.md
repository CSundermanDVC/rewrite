# rewrite

A Claude Code skill that rewrites your prompt into the form Claude 5 models (Opus 5.5, Fable 5.2) respond to best, shows you the rewrite, and then carries it out in the same turn.

The rules come from Anthropic's prompting guides (sources are listed in `references/rules.md`). When the skill runs on Fable 5.2, it applies an extra set of rules for that model on top (`references/fable-5-2.md`) and labels the rewrite "(Fable 5.2)".

## Install

For all your projects:

```bash
git clone https://github.com/CSundermanDVC/rewrite ~/.claude/skills/rewrite
```

For one project, run this from that project's folder:

```bash
git clone https://github.com/CSundermanDVC/rewrite .claude/skills/rewrite
```

Then type `/rewrite` followed by your prompt in Claude Code, for example:

```
/rewrite make the login tests less flaky
```

The reply first shows `Rewritten prompt:` with the rewrite, then the work itself.

## What it reads

The skill is plain text: `SKILL.md` and two rule files. It contains no program and sees only what your agent can already see.

## License

MIT. Made by Cas Sunderman (cassunderman.nl).
