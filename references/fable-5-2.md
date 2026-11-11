# Fable 5.2 overlay

Apply this on top of `rules.md` when the model running the rewrite is `claude-fable-5-2` / `claude-mythos-5-2`, or the original names Fable 5.2 as its target. Where the two conflict, this file wins.

Source: F52 = https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2

Fable 5.2 follows instructions closely and literally: style rules, approval rules, cautions and ownership statements all steer it more than on earlier models. So the rewrite carries fewer rules, each with its scope and reason. Add a block below only when the task has the shape it names; a short question stays a short rewrite. Snippets are written in English here; translate them into the user's language.

## Strip from the original

- Lines asking it to think less or answer without deliberating. They no longer work at `medium` and above (effort is the only control).
- Lines asking it to delegate, use subagents, or have its work reviewed by a subagent, unless the user asked for that themselves. Never add them.
- Token budgets or countdowns written into the prompt. A large budget makes it spend more.
- "Treat that answer as done / don't revisit earlier answers". It then stops pointing out its own mistakes.
- Hard style limits ("max 20 words per sentence", "one instruction per sentence"). Followed so closely that prose turns choppy. One line on length stays.
- Absolute "never/always" rules without a reason. Rewrite them with their scope and reason, or drop them if the reason is gone.
- Any request to write its reasoning in the response (risk of a `reasoning_extraction` refusal).

## Add, by task shape

**Multistep or agentic work** (replaces rules.md rule 12). Keep the split between ordinary obstacles and deliberate blockers:

> Keep going until the work I asked for is done; carry on with anything that does not depend on my answer. Diagnose ordinary obstacles (errors, timeouts, locked files, empty results, failing tools) and work through them: retry, fix the request, use another tool or source. Leave deliberate blockers alone (a file marked not to be touched, access that was withheld, a safety guardrail), say what you found, and look for another safe route. Stop only when the work needs my input or the blocker is deliberate. Risky or destructive actions still need my confirmation.

**Approvals.** Name the exact actions that need approval and the ones already approved ("you may run the migration on staging; ask before touching production"). Write the exception into the rule itself rather than in a separate line. Name the steps that need no approval, such as retrying a call that is safe to repeat.

**Who owns the code.** If the user asks to restructure, clean up, deduplicate or refactor, add:

> This codebase is yours: what it's for and how it's built are your call, and you'll keep building on it.

Leave it out for minimal or targeted changes; keep the scope line there instead. Never use both, because a scope line does not cancel the ownership sentence. With the sentence, ask for the report to list every change, small ones included.

**Long task the user watches.**

> Before you start, say in a line what you're about to do; give brief updates while you work. Close with a short recap that stands on its own: what you found, what you did, and what's next.

**Task that ends in a report, handoff or summary for someone else.**

> Write for an intelligent reader who has not seen your tool calls or the files you opened. Put the key points in the first sentences. Use plain words and full sentences, no terms you made up during the work. Use prose where ideas connect and bullets only for separate items.

## Worked example (Fable 5.2 variant of the rules.md example)

Original: `make the login tests less flaky`

Rewrite:

```
I'm stabilising the CI for this repo; flaky login tests are blocking merges.
Find the login tests that fail intermittently and fix the root cause of the flakiness (timing, shared state, order dependence). Keep what the tests assert, and leave unrelated tests alone because other people are working on them.
Keep going until the flaky tests are fixed. Diagnose ordinary obstacles such as a slow test run or a failing tool and work through them; stop only for input only I have.
Verify by running the login test file five times in a row; all runs pass.
Report which tests were flaky, the cause, and the change, for a reader who has not seen the run. Two short paragraphs at most.
Assumptions: the repo is the current working directory; "login tests" means tests whose file or test name contains "login" or "auth".
```
