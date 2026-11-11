---
name: rewrite
description: Use when the user invokes /rewrite with a task, or asks to optimize, improve, or restructure a prompt for Claude Opus 5.5, Fable 5.2, or the Claude 5 family before running it.
argument-hint: <your prompt>
---

# Rewrite, then run

Rewrite the prompt below into the form Claude 5 models (Opus 5.5, Fable 5.2) respond to best, then carry out the rewritten prompt in this same turn. The user has already approved this flow by invoking the skill, so the rewrite runs without a confirmation step.

<original_prompt>
$ARGUMENTS
</original_prompt>

If the original prompt is empty, ask the user what task to rewrite and stop there.

## Rewrite

Read `~/.claude/skills/rewrite/references/rules.md` and apply it. The rewrite keeps the original intent and every concrete detail, invents nothing, states any chosen reading on an `Assumptions:` line, and stays in the user's language.

Then check the model. If your own model ID (from your system prompt) is `claude-fable-5-2` or `claude-mythos-5-2`, or the original prompt names Fable 5.2 as its target model, also read `~/.claude/skills/rewrite/references/fable-5-2.md` and apply it on top; where it conflicts with `rules.md`, it wins. On any other model, `rules.md` alone applies.

## Response shape

The user chose to see the rewrite before it runs, so they can learn how their prompts change. Your reply therefore has exactly two parts, in this order:

1. The line `Rewritten prompt:` (or `Rewritten prompt (Fable 5.2):` when the Fable 5.2 overlay was applied) followed by the rewrite in a fenced block. Write this as visible text before your first tool call, even when another style rule asks for code first or minimal prose. Those rules govern part 2.
2. The execution: treat the rewritten prompt as the user's actual instruction and complete it, using tools, skills, and subagents as the task requires, following the assumptions stated in the rewrite. Then report the outcome as for any task.

A reply that contains the outcome but not part 1 is incomplete.
