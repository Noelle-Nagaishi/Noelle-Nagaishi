# AI conventions

## About this repository
This is Noelle Nagaishi's professional and academic portfolio repository, documenting selected coursework, projects, professional experience, and developing skills.
Canonical file: AGENTS.md. CLAUDE.md points here.

## Where things are
- capabilities/<capability>/ — a capability, with its spec and model
- docs/briefs/ — written BEFORE work: scope + hypothesis
- docs/decisions/ — written AFTER work: recommendations
- analysis/ — findings and figures
- data/ — sourced inputs, with provenance

## Naming
- The directory matters most. If you are not certain which folder a file belongs in, ask me before you write it — do not choose for me.
- Graded files use the exact filename the stage brief gives — lowercase, hyphens, no spaces. Some courses date-stamp (YYYY-MM-DD-lastname-slug.md); the stage page says so when they do.
- Slugs name the engagement, never the week, the course, or the assignment number.
- Never invent a path or filename. I will give you the exact one.

## How I work
- Explain concepts clearly and show the reasoning or process rather than only giving me the final answer.
- For quantitative work, show formulas, inputs, and calculations so I can verify the result.
- For academic work, prioritize the assignment instructions, course materials, and sources I provide.
- Keep study notes concise and focused on concepts, applications, examples, and material useful for exams or class discussion.
- Critique my work directly: identify errors, missing requirements, unsupported claims, and weaknesses rather than simply agreeing with me.
- When uncertain, say so and explain what would resolve the uncertainty.
- Do not invent facts, citations, sources, requirements, or calculations.
- Every statistic or factual claim you give me is a draft until I verify it against a source.

## What you may and may not draft
- You MAY explain, critique, debug, quiz me, check calculations, organize information, and draft mechanical files.
- You MAY help me revise work that I drafted by identifying weaknesses and suggesting specific improvements.
- You MAY NOT write my briefs, analyses, memos, reflections, or other work intended to demonstrate my independent judgment.
- When an artifact is evidence of my judgment, I draft it first, and AI reviews it.

## Documentation
When work changes, update the document that describes it in the same commit.
A capability's README names the engagements that exercised it — keep that current.

## Scope
Do the work I asked for. If you notice something worth doing that I did not ask for, tell me instead of doing it.

## Commits
Use descriptive commit messages stating what changed and why. Never use vague messages such as "update" or "stuff."

## Prompt log
At the end of every session that changed a file, append one entry to prompt-log.md containing:
- the date
- what I asked
- what you produced
- what was wrong, if anything
- how the error was caught

Never backfill earlier sessions and never edit a past entry.

## Never include
No credentials, API keys, tokens, personal data about other people, licensed or copyrighted material, or confidential employer information.
If I provide something that fits one of these categories, stop and tell me rather than committing it.

## Mistakes to avoid
Record errors here as they happen so the same error does not repeat.
- (empty — add the first one when it happens)
