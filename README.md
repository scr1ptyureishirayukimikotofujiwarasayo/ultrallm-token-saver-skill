# max-15-words

An agent skill that caps every response at **15 words or fewer**, unless you ask for more.

No preamble. No filler. No "Great question!" Just the answer.

Tested and used in [opencode](https://opencode.ai), where it worked extremely well.

## Why

Output tokens cost money and time. Most of what an AI agent says is bloat: greetings, restated questions, hedges, recaps, closing offers. This skill removes that by giving the model one hard rule it can count.

A word limit works better than a vague "be concise" because the model can check it before sending.

## Example

| Prompt | Without skill | With skill |
|---|---|---|
| What's the capital of Australia? | "Great question! The capital of Australia is Canberra, not Sydney or Melbourne as many people assume." | `Canberra.` |
| Why is the sky blue? | A paragraph on Rayleigh scattering | `Air scatters blue sunlight more than red, so blue reaches your eyes.` |
| Did the tests pass? | "I ran the full suite and I'm happy to report that all tests passed successfully." | `Yes, 42/42 pass.` |

## How it works

- Every reply is 15 words or fewer. The model counts before sending.
- The answer comes first. Greetings, restating the question, hedges, and closing offers are dropped.
- If the full answer won't fit, the model gives the most important part and stops.
- Headers, bullets, and tables are skipped unless they are shorter.

### Exceptions

The model goes over 15 words only when you explicitly ask, for example:

- "explain in detail", "elaborate", "step by step"
- a stated length or format, like "write 200 words" or "a full report"

That applies to that one reply. The next reply returns to 15 words.

### Not counted

Code blocks, file contents, JSON, and tool call arguments don't count toward the limit, because cutting them breaks them. The prose around them still has to fit in 15 words.

## Install

The skill is a single file: [`SKILL.md`](./SKILL.md).

1. Create a folder named `max-15-words` in your agent's skills directory.
2. Put `SKILL.md` inside it.
3. Restart or reload your agent.

Skills directory locations vary by tool. Check your agent's documentation for the exact path. Common patterns:

```
~/.config/opencode/skill/max-15-words/SKILL.md   # opencode, global
.opencode/skill/max-15-words/SKILL.md            # opencode, per project
~/.claude/skills/max-15-words/SKILL.md           # Claude Code, global
.claude/skills/max-15-words/SKILL.md             # Claude Code, per project
```

It also works as a `.skill` package in Claude.ai under Settings, Capabilities, Skills.

## Customize

Want a different cap? Edit `SKILL.md` and change every `15` to your number. The limit appears in the description, the rules, the exceptions, and the examples. Keep the examples under your new limit, since models copy them.

A 30-word version is a good middle ground if 15 feels too tight for your tasks.

## Trade-offs

- Complex answers get cut to the single most important point. Ask for detail when you need it.
- A hard cap can hide nuance. For anything risky, ask a follow-up or request a fuller answer.
- Results depend on the model following instructions. Stronger models follow the limit more reliably.

## License

MIT. See [LICENSE](./LICENSE).
