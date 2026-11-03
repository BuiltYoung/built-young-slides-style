# Built Young Slide Style

A skill that keeps every [Built Young](https://builtyoung.org) deck on brand — AI Lab, Money Lab, Investor Lab, Founder Lab — so a student moving between labs sees one consistent course, not four instructors' PowerPoint habits.

It covers how a deck looks and reads. The teacher decides what goes in it.

## What it specifies

- **Colour** — a fixed seven-token palette: navy `#13233B`, orange `#F26430`, peach, mist, ink, line, grey. At most one orange-filled block per slide, so the highlight always means something.
- **Type** — Inter for headings and body, JetBrains Mono for step numbers, table headers and prompts. Fixed sizes from 44pt cover titles down to 8pt footers, with a floor at 11pt for anything a student has to read.
- **Anatomy** — white content slides (eyebrow, title with one orange key phrase, orange rule, subtitle, footer) and navy slides (cover, part dividers, work sessions, breaks, closing).
- **Patterns** — eleven layouts to reach for instead of bullet lists: numbered step cards, wrong-vs-right columns, stat comparisons, definition blocks, copy-paste prompt boxes, data tables with sources.
- **Voice** — speak to 12–18 year olds as capable people. Titles are claims, not topic labels. Real examples with real numbers and a source. ~40 words per card, ~70 per slide.

## What ships with it

A `pptxgenjs` helper module, inline in `SKILL.md`, that implements the whole system — `cover()`, `section()`, `content()`, `steps()`, `callout()`, `card()`, `table()`, `prompt()`, `activity()`, `stats()`, `closing()`. Wrap a phrase in `*asterisks*` in any title and it comes out orange.

Output is a 16:9 `.pptx`, which imports cleanly into Google Slides.

```
built-young-slides-style/
└── SKILL.md    style rules + pptxgenjs helpers
```

## Using it

**Claude.ai / Claude Desktop** — [download `built-young-slides-style.zip`](https://github.com/BuiltYoung/built-young-slides-style/raw/main/built-young-slides-style.zip), then: Settings → Capabilities (make sure *Code execution and file creation* is on) → Customize → Skills → **+** → **Create skill** → upload the zip.

That zip is packaged with the skill folder as its root, which is what the uploader expects. GitHub's own **Code → Download ZIP** wraps everything in an extra `-main/` folder and will fail — use the link above.

**Claude Code** — drop it into your skills directory:

```bash
git clone https://github.com/BuiltYoung/built-young-slides-style.git
cp -r built-young-slides-style/built-young-slides-style ~/.claude/skills/
```

**Without an assistant** — `SKILL.md` reads as a plain style guide. The colour table, type scale and slide patterns work for anyone building a deck by hand.

## Before you ship a deck

The skill ends with a render check, and it matters more than any rule above it:

```bash
soffice --headless --convert-to pdf deck.pptx
```

Look at every slide. Overflowing text, a missing logo, two orange blocks competing, an unsourced number. Fix and re-render before it reaches a classroom.

## Notes

The logo is never redrawn in code — pass a real file via `logoPath` and `logoDarkPath` (the white-text version for navy slides). Without one, the helpers fall back to set text.

Investing content carries a not-investment-advice line and tells students to talk to a parent before opening a real account. That stays in.

## License

MIT. See [LICENSE](LICENSE).
