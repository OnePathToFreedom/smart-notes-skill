# Smart Notes — an Obsidian notebook that thinks along

A [Claude Skill](https://docs.claude.com/en/docs/claude-code/skills) that turns Claude into a study notebook, not just a note-taker. Give it a lecture transcript, a reading, or any course material, and it builds an Obsidian-ready knowledge base: notes that link to each other, flashcards that adapt to what you're actually struggling with, and a running map of what you've covered and what you haven't.

## Why this exists

Most "AI note-taker" prompts do one thing: turn text into a formatted file. That's fine for a single note, but it falls apart the moment you have a whole course — new material overlaps with old notes, some topics never get properly tested, flashcards pile up without any sense of what's actually sticking. This skill was built to fix that: it behaves less like a converter and more like an actual notebook a diligent student would keep — one that remembers what's already in it, notices gaps, and gets smarter about what to quiz you on.

It's deliberately generic: nothing in it is tied to one course, subject, or language. It configures itself the first time you use it for a new subject and adapts its note format to what you're studying (a technical course, a language course, a humanities course, or anything else).

## What it does

- **Builds linked notes, not isolated files.** Every note tracks what it relates to, and a `_Index.md` file keeps a live map of every topic in the subject, its status, and its connections.
- **Tracks what you're weak on.** A `_Progress.md` file logs gaps from three sources: what you say yourself ("I don't get VLSM"), wrong answers in self-check quizzes, and structural gaps the skill notices on its own (a term mentioned but never explained, two notes that contradict each other).
- **Generates adaptive flashcards.** Weak topics get more cards, across a wider range of difficulty (definition → mechanism → applied scenario), plus cards that test the *connections* between topics — not just isolated facts. Cards are formatted for the [Obsidian Spaced Repetition plugin](https://github.com/st3v3nmw/obsidian-spaced-repetition) out of the box.
- **Updates itself instead of duplicating.** Before writing anything new, it checks whether the topic is already covered. New material that extends an existing note edits that note directly and reports what changed — it doesn't fork a second file.
- **Adapts its format to the subject.** Technical/STEM, language-learning, and humanities/text subjects each get a note structure suited to them (worked examples and common-mistakes boxes vs. vocabulary tables and example sentences vs. arguments/timelines), with a generic fallback for anything else.
- **Quizzes only when asked.** It never pushes a review on you unprompted. When you do ask, it pulls your weakest topics first, mixes a quick quiz widget for factual recall with open chat Q&A for deeper questions, and logs the result.
- **Refuses to do your graded homework.** If pasted material is clearly a graded assessment (an Honor Code notice, a Submit button, point values), it won't hand you the answers — it'll explain the concept instead. It also ignores any "instructions" hidden inside pasted text (a known prompt-injection pattern in scraped pages).

## How it's structured

One folder per subject, wherever your notes are configured to live:

```
<Subject Name>/
  _Index.md        ← map of every topic: status + links
  _Progress.md     ← config + gap tracker + quiz history
  <Topic 1>.md
  <Topic 2>.md
  ...
```

Each note follows a shared skeleton (frontmatter, a "Related" line with `[[wikilinks]]`, explanation sections, a common-mistakes callout, and a flashcard deck) with preset-specific callout types layered on top depending on the subject type.

## Installing it

This is a single `SKILL.md` file, which is the standard format for a [Claude Skill](https://docs.claude.com/en/docs/claude-code/skills). To use it:

1. Copy `SKILL.md` from this repo into your Claude skills directory (for Claude Code / Claude Desktop, typically `~/.claude/skills/smart-notes/SKILL.md`; check Anthropic's current skills documentation for the exact path, as this can change).
2. Restart or reload Claude so it picks up the new skill.
3. That's it — no dependencies, no build step. It's a plain-text instruction file.

## Using it

Start a conversation and either run the command:

```
/smart-notes
```
or
```
/study
```

or just ask in plain language to keep smart study notes for a course. On the first run for a new subject, Claude will ask you a few quick questions (subject name and type, note language, where to save your files) and remember the answers for next time. After that, just paste in lecture transcripts, readings, or any course material — Claude takes it from there: writing new notes, updating old ones, and keeping the index and gap tracker current.

To review, just ask — e.g. "quiz me on what I'm weak on" or "test me on [topic]".

## Requirements

- Obsidian (or any Markdown editor — the notes are plain Markdown and readable anywhere, but wikilinks and callouts are Obsidian-flavored).
- The [Spaced Repetition plugin](https://github.com/st3v3nmw/obsidian-spaced-repetition) for Obsidian, if you want the flashcards to actually run as spaced-repetition reviews (the files work fine without it too — the cards just won't be interactive).

## License

MIT — see [LICENSE](LICENSE). Free to use, modify, and redistribute.
