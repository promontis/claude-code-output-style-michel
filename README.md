# Michel: an output style for Claude Code

An output style for readers who want the full depth, but not all at once.

Claude Code's answers can overload you in two opposite ways. Either everything arrives in one block (five open questions, two tables to compare, a dozen numbers), or, when you ask for something simpler, the depth disappears. This style keeps the depth and changes the shape:

- **Context first.** Every answer opens with what we are working on and where we are, even when you did not ask.
- **One question at a time.** "This is question 2 of 5." The recommendation is inside the question, so "yes" accepts it.
- **Built up in steps.** Each step has a bold numbered heading on its own line ("**2 - What the code does now**") and adds exactly one new thing.
- **Worked examples.** One running case with real numbers and the arithmetic shown.
- **The real thing on screen.** Actual code with `file:lines`, actual table rows, links instead of pasted content.
- **Every claim labelled.** From the code, the data, the docs, own knowledge, or a guess, and guesses get checked first when the advice rests on them.

The style keeps Claude Code's built-in software engineering instructions (`keep-coding-instructions: true`). It changes how Claude explains, not how it codes.

## What an answer looks like

> We are working on the invoicing module. The VAT amount on credit notes is wrong. A credit note is an invoice that pays money back to the customer. Question 1 is still open. This is question 2 of 5.
>
> **1 - Which VAT rate applies**
>
> The rate lives in the table `vat_rate`, the table with the VAT rate per country and per period:
>
> | country | rate | valid_from | valid_until |
> |---|---|---|---|
> | NL | 0.21 | 2019-01-01 | |
>
> **2 - What the code does now**
>
> …
>
> **5 - What argues against it**
>
> …
>
> **Question 2 of 5**
>
> I propose that the VAT line on a credit note becomes negative, like its net amount. Agreed?

The style answers in your language, headings included. A Dutch reader gets "Wat ertegen pleit" and "Akkoord?".

## Install

### As a plugin

In Claude Code:

```
/plugin marketplace add promontis/claude-code-output-style-michel
/plugin install michel@claude-code-output-style-michel
```

Then select the style with `/output-style michel:Michel`, or run `/config` and pick it under **Output style**.

### As a single file

Copy [`output-styles/michel.md`](output-styles/michel.md) to `~/.claude/output-styles/` (all projects) or to `.claude/output-styles/` in a repository (one project):

```bash
mkdir -p ~/.claude/output-styles
curl -fsSL https://raw.githubusercontent.com/promontis/claude-code-output-style-michel/main/output-styles/michel.md \
  -o ~/.claude/output-styles/michel.md
```

Then select it with `/output-style Michel`, or make it the default in `~/.claude/settings.json`:

```json
{
  "outputStyle": "Michel"
}
```

Claude Code reads style files when it starts, so restart it after installing or editing the file.

## The rules

The full text is in [`output-styles/michel.md`](output-styles/michel.md). In short:

| Section | What it covers |
|---|---|
| 0. Size the reply to the question | Short replies for facts, status and small decisions; the full structure only when the question needs it |
| **Every reply** | |
| 1. Language and guardrails | The reader's language, fixed phrases translated, identifiers and numbers kept exact |
| 2. Label where each claim comes from | Source per claim, made-up numbers labelled, verify before calling it a guess, relay subagent results per item |
| 3. Explain every new term | Project terms explained on first use, one name per thing |
| 4. Show the real thing | Real code with `file:lines`, links instead of content, the command when a search found nothing |
| 5. Corrections and what changed | No silent corrections, what actually happened in one line (a table only when repos differ in state) |
| 6. Questions and endings | "Question n of m", recommendation inside the question, the question comes last, no loose ends, artefacts ship bare |
| **Full replies only** | |
| 7. Context and state | Grounding paragraph, state of open decisions and parked topics |
| 8. Build the explanation in steps | Heading on its own line, one new thing per step, re-explain the step that broke, flag where a simplification stops holding |
| 9. A worked example | Real numbers, arithmetic shown |
| 10. Decisions and tables | What the answer means in the code, what each option costs, what is missing, a "What argues against it" step, real rows, expected versus actual in one table |

## Credits

The rules were shaped in a long conversation about what made explanations land and what overloaded. Several ideas come from other output styles and skills. The wording in this repository is its own; the ideas below are credited to where they came from.

| Idea | Source | License |
|---|---|---|
| Do not ask the reader to remember; restate state each turn; first and last line check; keep hedges that carry real uncertainty | [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) | MIT |
| One question at a time; one name per thing; unverified claims are unfinished work | [alexgreensh/attention-span](https://github.com/alexgreensh/attention-span) | AGPL-3.0 (ideas only, no text) |
| Context first; re-pitch when the reader is lost; deeper levels never contradict simpler ones; anchor in a concrete example | [smixs/awesome-claude-output-styles](https://github.com/smixs/awesome-claude-output-styles) (Wait What, Feynman, Ladder, ADHD) | not specified (ideas only, no text) |
| One name per thing; expected versus actual side by side; no loose ends at the end; state what has actually happened; relay subagent results per item | [Marksooxx/plain-speak](https://github.com/Marksooxx/plain-speak) | MIT |
| The strongest argument against the recommendation; say which way you lean and what would settle it; corrections reopen decisions | [OpenScribbler/Katharsis](https://github.com/OpenScribbler/Katharsis) | MIT |
| Style rules also apply when relaying subagent results | [snflkd/fluent-korean](https://github.com/snflkd/fluent-korean) | MIT |
| Recommendation inside the question so "yes" accepts it | [mattpocock/skills](https://github.com/mattpocock/skills) (grilling) | MIT |

## License

[MIT](LICENSE)
