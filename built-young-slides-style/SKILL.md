---
name: built-young-slides-style
description: Built Young slide style guide. Apply it to any Built Young deck (AI Lab, Money Lab, Investor Lab, Founder Lab) so every lab's slides share one look - navy + orange, Inter, big plain-English titles.
---

# Built Young Slide Style

Apply this style to any Built Young presentation. It covers only how the deck looks and reads. The teacher decides the content.

## Format
- 16:9 widescreen (13.333 x 7.5 in). Deliver a .pptx (it imports into Google Slides), built with pptxgenjs using the helpers below, unless the user asks for another format.
- Built Young logo top-right on every slide: the dark-text version on white slides, the white-text version on navy slides. If no logo file was provided, use the text fallback in the helpers. Never redraw the logo.

## Colours (fixed)
| Token | Hex | Use |
|---|---|---|
| Navy | #13233B | Cover, section dividers, activity/break slides, dark cards, table header row |
| Ink | #1A1A2E | Titles and body text on white |
| Orange | #F26430 | Key word in titles, eyebrow labels, numbers, rules, ONE highlighted block per slide |
| Peach | #FCEEE2 | Callout boxes (Remember / Watch out / Your turn) |
| Mist | #F3F5F8 | Light cards, zebra table rows |
| Line | #E3E6EC | Card and table borders |
| Grey | #5B6475 | Subtitles, captions, footer |

## Type
- Inter for headings and body (bold titles). JetBrains Mono for big step numbers (01, 02...), table headers, footer, and AI prompts/code.
- Sizes (pt): cover title 44, section title 40, slide title 30, subtitle 14, card title 15-17, body 12-14, eyebrow 9-10 caps, footer 8. Nothing under 11pt except eyebrows, footers and source lines.

## Slide anatomy
**White content slide**
- Optional orange ALL-CAPS eyebrow above the title (e.g. RATIO 2 OF 4, WATCH OUT).
- Bold Ink title top-left with ONE key word or phrase in orange ("Where to find the *numbers*.").
- Short orange rule (0.9 x 0.06 in) under the title.
- Optional grey one-line subtitle.
- Footer bottom-left in mono caps "BUILT YOUNG · <LAB NAME>", slide number bottom-right.

**Navy slide** (cover, PART dividers, work session / break, closing)
- Orange kicker or "PART N" label, a big white title with an orange key word, an orange rule below.
- The cover also has a thin orange stripe down the left edge and "Built Young · <Lab> · Instructor: <name>" at the bottom.
- Work-session and break slides carry an orange-outlined minutes badge bottom-right.

## Visual patterns (prefer these over plain bullets)
- Process, plan, framework: a row of numbered step cards (navy cards, orange mono numbers; the current or final step is the orange card).
- Two ideas: two white rounded cards (title, grey subtitle, short paragraph, orange check bullets), optionally with a peach "Pro tip" box.
- Wrong vs right / vague vs precise: two columns, a light card with an x on the left and a navy card with a check on the right.
- Definition: WHAT IT IS / WHY IT MATTERS / WHAT IT TELLS YOU blocks, an example in a peach box, a navy bottom bar with the takeaway.
- Dramatic numbers: big stat cards (34pt+), the surprising one in orange, an arrow between them.
- Data: a table with a navy header row, mono caps headers, zebra rows, first column bold, and a source line under it.
- Examples or case studies: 3 cards with an orange top rule and the name in large type.
- Debate: a SIDE A block in orange next to a SIDE B block in navy.
- AI prompt: a pale-peach box headed "</> Copy-Paste Ready Prompt", mono text, [placeholders] in bold orange.
- Task: numbered task cards plus a peach Goal bar with a time pill.
- Most content slides end with a callout: peach or mist bar, orange left rule, small orange caps label, one takeaway sentence with the punchline in orange.

## Typical lesson flow (use when it fits)
Cover, then links slide (Slack / Google Form / Class Portal), then "Where we are in N weeks" timeline (orange = done, ring = today), then warm-up or recap, then today's plan, then PART dividers with their content, with work sessions and breaks between, then homework, then "What you can do now / What's next", then closing line + contact.

## Voice
- Speak to teens (12-18) as capable people: plain words, short sentences, "you".
- Titles are claims or questions, not topic labels ("Same leggings. One-fifth the stock price.").
- Use real named examples and real numbers with a source. Never invent data.
- Max ~40 words per card and ~70 per slide. If it doesn't fit, split the slide.
- Max one orange-filled block per slide. No stock-photo filler.
- Investing content: note it is not investment advice, and tell students to talk to a parent before opening any real account.

## Helper code (pptxgenjs)
Save as by.js and `npm i pptxgenjs`. Wrap a phrase in *asterisks* in a title, callout or activity text to make it orange. For patterns not covered here, use plain pptxgenjs shapes with the same C (colours), F (fonts), M (margin) and runs().

```js
// Built Young slide helpers — pptxgenjs, 16:9 (13.333 x 7.5 in)
const pptxgen = require("pptxgenjs");
const fs = require("fs");

const C = {
  navy: "13233B", ink: "1A1A2E", orange: "F26430", peach: "FCEEE2",
  mist: "F3F5F8", line: "E3E6EC", grey: "5B6475", white: "FFFFFF",
};
const F = { head: "Inter", body: "Inter", mono: "JetBrains Mono" };
const W = 13.333, H = 7.5, M = 0.7; // margin

function makeDeck({ lab, logoPath, logoDarkPath }) {
  const pptx = new pptxgen();
  pptx.layout = "LAYOUT_WIDE";
  const ok = (p) => (p && fs.existsSync(p) ? p : null);
  const logoLight = ok(logoPath), logoDark = ok(logoDarkPath); // dark = white-text version for navy slides

  function brand(s, dark) {
    const logo = dark ? logoDark : logoLight;
    if (logo) s.addImage({ path: logo, x: W - 1.75, y: 0.3, w: 1.3, h: 0.55, sizing: { type: "contain", w: 1.3, h: 0.55 } });
    else s.addText([{ text: "BUILT YOUNG", options: { bold: true, fontSize: 13, color: dark ? C.white : C.navy, breakLine: true } },
                    { text: "Build Before Everyone Else", options: { fontSize: 7, color: dark ? "C9CED8" : C.grey } }],
                   { x: W - 2.3, y: 0.3, w: 1.9, h: 0.6, align: "right", fontFace: F.head, margin: 0 });
  }
  function footer(s, n) {
    s.addText(`BUILT YOUNG · ${lab.toUpperCase()}`, { x: M, y: H - 0.5, w: 6, h: 0.3, fontFace: F.mono, fontSize: 8, color: C.grey, bold: true, margin: 0 });
    if (n) s.addText(String(n), { x: W - M - 1, y: H - 0.5, w: 1, h: 0.3, fontFace: F.mono, fontSize: 8, color: C.grey, align: "right", margin: 0 });
  }
  // title runs: "Pick the Right *Claude Model*" -> *...* becomes orange
  function runs(str, base) {
    return str.split(/(\*[^*]+\*)/).filter(Boolean).map(p =>
      p.startsWith("*") ? { text: p.slice(1, -1), options: { ...base, color: C.orange } } : { text: p, options: base });
  }
  function header(s, { eyebrow, title, sub }) {
    let y = 0.45;
    if (eyebrow) { s.addText(eyebrow.toUpperCase(), { x: M, y, w: 9, h: 0.3, fontFace: F.body, fontSize: 10, bold: true, color: C.orange, charSpacing: 1, margin: 0 }); y += 0.32; }
    s.addText(runs(title, { fontFace: F.head, fontSize: 30, bold: true, color: C.ink }), { x: M, y, w: 10.4, h: 0.75, margin: 0, valign: "middle" });
    y += 0.8;
    s.addShape("rect", { x: M, y, w: 0.9, h: 0.06, fill: { color: C.orange }, line: { type: "none" } });
    y += 0.2;
    if (sub) { s.addText(sub, { x: M, y, w: 10.5, h: 0.4, fontFace: F.body, fontSize: 14, color: C.grey, margin: 0 }); y += 0.5; }
    return y + 0.1; // content top
  }
  let n = 0;
  const api = {
    pptx,
    cover({ kicker, title, sub, instructor }) {
      const s = pptx.addSlide(); n++; s.background = { color: C.navy };
      s.addShape("rect", { x: 0, y: 0, w: 0.12, h: H, fill: { color: C.orange }, line: { type: "none" } });
      brand(s, true);
      s.addText(kicker, { x: M + 0.2, y: 1.9, w: 10, h: 0.4, fontFace: F.body, fontSize: 16, bold: true, color: C.orange, margin: 0 });
      s.addText(runs(title, { fontFace: F.head, fontSize: 44, bold: true, color: C.white }), { x: M + 0.2, y: 2.4, w: 11, h: 1.9, margin: 0, valign: "top" });
      s.addShape("rect", { x: M + 0.2, y: 4.45, w: 0.9, h: 0.06, fill: { color: C.orange }, line: { type: "none" } });
      if (sub) s.addText(sub, { x: M + 0.2, y: 4.7, w: 10, h: 0.5, fontFace: F.body, fontSize: 16, color: "C9CED8", margin: 0 });
      s.addText(`Built Young · ${lab}${instructor ? "  ·  Instructor: " + instructor : ""}`, { x: M + 0.2, y: H - 0.9, w: 10, h: 0.3, fontFace: F.body, fontSize: 11, bold: true, color: C.white, margin: 0 });
      return s;
    },
    section({ part, title, sub }) {
      const s = pptx.addSlide(); n++; s.background = { color: C.navy }; brand(s, true);
      if (part) s.addText(part.toUpperCase(), { x: M, y: 2.5, w: 10, h: 0.45, fontFace: F.body, fontSize: 18, bold: true, color: C.orange, charSpacing: 1, margin: 0 });
      s.addText(runs(title, { fontFace: F.head, fontSize: 40, bold: true, color: C.white }), { x: M, y: 3.0, w: 11, h: 1.3, margin: 0, valign: "top" });
      s.addShape("rect", { x: M, y: 4.4, w: 0.9, h: 0.06, fill: { color: C.orange }, line: { type: "none" } });
      if (sub) s.addText(sub, { x: M, y: 4.6, w: 10, h: 0.5, fontFace: F.body, fontSize: 14, color: "C9CED8", margin: 0 });
      return s;
    },
    content(h) { const s = pptx.addSlide(); n++; s.background = { color: C.white }; brand(s); footer(s, n); return { s, top: header(s, h) }; },
    // numbered cards row: items=[{title, body}], highlight index gets orange fill
    steps(s, top, items, { highlight = -1, dark = true, h = 1.9 } = {}) {
      const gap = 0.2, w = (W - 2 * M - gap * (items.length - 1)) / items.length;
      items.forEach((it, i) => {
        const x = M + i * (w + gap), hi = i === highlight;
        const fill = hi ? C.orange : dark ? C.navy : C.mist, fg = hi || dark ? C.white : C.ink;
        s.addShape("rect", { x, y: top, w, h, fill: { color: fill }, line: { type: "none" } });
        s.addText(String(i + 1).padStart(2, "0"), { x: x + 0.2, y: top + 0.15, w: w - 0.4, h: 0.5, fontFace: F.mono, fontSize: 22, bold: true, color: hi ? C.white : C.orange, margin: 0 });
        s.addText([{ text: it.title, options: { bold: true, fontSize: 15, color: fg, breakLine: true } },
                   ...(it.body ? [{ text: it.body, options: { fontSize: 11, color: hi || dark ? "DCE0E8" : C.grey } }] : [])],
                  { x: x + 0.2, y: top + 0.7, w: w - 0.4, h: h - 0.85, fontFace: F.body, valign: "top", margin: 0, paraSpaceAfter: 4 });
      });
      return top + h + 0.3;
    },
    // callout bar with orange left rule. tone: "peach" | "mist" | "navy" | "orange"
    callout(s, y, { label, text, tone = "peach", h = 0.85, w = W - 2 * M }) {
      const bg = { peach: C.peach, mist: C.mist, navy: C.navy, orange: C.orange }[tone];
      const fg = tone === "navy" || tone === "orange" ? C.white : C.ink;
      s.addShape("rect", { x: M, y, w, h, fill: { color: bg }, line: { type: "none" } });
      if (tone !== "orange") s.addShape("rect", { x: M, y, w: 0.07, h, fill: { color: C.orange }, line: { type: "none" } });
      const t = [];
      if (label) t.push({ text: label.toUpperCase(), options: { fontSize: 9, bold: true, color: tone === "orange" ? C.white : C.orange, breakLine: true, charSpacing: 1 } });
      t.push(...runs(text, { fontSize: 15, bold: true, color: fg }));
      s.addText(t, { x: M + 0.3, y, w: w - 0.5, h, fontFace: F.body, valign: "middle", margin: 0 });
      return y + h + 0.25;
    },
    // card with icon-free header: title, subtitle, body, bullets (orange ✓)
    card(s, { x, y, w, h, title, sub, body, bullets = [], tip }) {
      s.addShape("roundRect", { x, y, w, h, rectRadius: 0.12, fill: { color: C.white }, line: { color: C.line, width: 1 }, shadow: { type: "outer", blur: 6, offset: 1, angle: 90, color: "000000", opacity: 0.08 } });
      const t = [{ text: title, options: { bold: true, fontSize: 17, color: C.ink, breakLine: true } }];
      if (sub) t.push({ text: sub, options: { fontSize: 11, color: C.grey, breakLine: true } });
      if (body) t.push({ text: " ", options: { fontSize: 6, breakLine: true } }, { text: body, options: { fontSize: 12.5, color: "3A4152", breakLine: true } });
      bullets.forEach(b => t.push({ text: "✓  ", options: { fontSize: 12.5, bold: true, color: C.orange } }, { text: b, options: { fontSize: 12.5, bold: true, color: C.ink, breakLine: true } }));
      s.addText(t, { x: x + 0.3, y: y + 0.25, w: w - 0.6, h: h - (tip ? 1.25 : 0.5), fontFace: F.body, valign: "top", margin: 0, paraSpaceAfter: 5 });
      if (tip) {
        s.addShape("roundRect", { x: x + 0.3, y: y + h - 1.05, w: w - 0.6, h: 0.8, rectRadius: 0.1, fill: { color: "FEF2EE" }, line: { color: "FBD5C8", width: 1 } });
        s.addText(runs("*Pro tip:* " + tip, { fontSize: 12, bold: true, color: C.orange }), { x: x + 0.5, y: y + h - 1.05, w: w - 1, h: 0.8, fontFace: F.body, valign: "middle", margin: 0 });
      }
    },
    // table: navy header row, mono uppercase headers, zebra rows
    table(s, y, header, rows, { colW, fontSize = 12 } = {}) {
      const head = header.map(h => ({ text: h.toUpperCase(), options: { bold: true, color: C.white, fill: { color: C.navy }, fontFace: F.mono, fontSize: 10 } }));
      const body = rows.map((r, i) => r.map((c, j) => ({ text: String(c), options: { color: C.ink, bold: j === 0, fill: { color: i % 2 ? C.white : C.mist }, fontSize } })));
      s.addTable([head, ...body], { x: M, y, w: W - 2 * M, colW, fontFace: F.body, border: { type: "solid", pt: 0.75, color: C.line }, margin: [5, 8, 5, 8], valign: "middle" });
    },
    // copy-paste prompt block
    prompt(s, y, { title = "Copy-Paste Ready Prompt", text, h = 3.6 }) {
      s.addShape("roundRect", { x: M, y, w: W - 2 * M, h, rectRadius: 0.1, fill: { color: "FFFAF7" }, line: { color: "FBD5C8", width: 1 } });
      s.addText(runs("</>  *" + title + "*", { fontSize: 14, bold: true, color: C.ink }), { x: M + 0.3, y: y + 0.15, w: 8, h: 0.45, fontFace: F.body, margin: 0 });
      // [bracketed] placeholders are orange
      const parts = [];
      text.split("\n").forEach((line, li, all) => {
        const seg = line.split(/(\[[^\]]+\])/).filter(Boolean);
        if (!seg.length) seg.push(" ");
        seg.forEach((p, k) => parts.push({ text: p, options: { color: p.startsWith("[") ? C.orange : "3A4152", bold: p.startsWith("["), breakLine: k === seg.length - 1 && li < all.length - 1 } }));
      });
      s.addText(parts, { x: M + 0.3, y: y + 0.7, w: W - 2 * M - 0.6, h: h - 0.9, fontFace: F.mono, fontSize: 11.5, valign: "top", margin: 0 });
    },
    // activity / break / work session slide
    activity({ title, minutes, note }) {
      const s = pptx.addSlide(); n++; s.background = { color: C.navy }; brand(s, true);
      s.addText(runs(title, { fontFace: F.head, fontSize: 44, bold: true, color: C.white }), { x: M, y: 2.6, w: 10, h: 1.4, margin: 0 });
      if (note) s.addText(note, { x: M, y: 4.0, w: 10, h: 0.5, fontFace: F.body, fontSize: 18, bold: true, color: C.orange, margin: 0 });
      if (minutes) {
        s.addShape("roundRect", { x: W - 3.2, y: H - 1.8, w: 2.4, h: 1.0, rectRadius: 0.15, fill: { type: "none" }, line: { color: C.orange, width: 2 } });
        s.addText([{ text: String(minutes), options: { fontSize: 32, bold: true, color: C.orange } }, { text: " min", options: { fontSize: 16, color: C.orange } }], { x: W - 3.2, y: H - 1.8, w: 2.4, h: 1.0, align: "center", valign: "middle", fontFace: F.mono, margin: 0 });
      }
      return s;
    },
    // big number / stat comparison
    stats(s, y, items, { h = 1.7 } = {}) {
      const gap = 0.25, w = (W - 2 * M - gap * (items.length - 1)) / items.length;
      items.forEach((it, i) => {
        const x = M + i * (w + gap), hi = it.highlight;
        s.addShape("rect", { x, y, w, h, fill: { color: hi ? C.orange : C.mist }, line: { type: "none" } });
        s.addShape("rect", { x, y, w, h: 0.06, fill: { color: hi ? C.navy : C.orange }, line: { type: "none" } });
        s.addText([{ text: (it.label || "").toUpperCase(), options: { fontSize: 9, bold: true, color: hi ? C.white : C.orange, breakLine: true, charSpacing: 1 } },
                   { text: it.value, options: { fontSize: 34, bold: true, color: hi ? C.white : C.ink, breakLine: true } },
                   { text: it.note || "", options: { fontSize: 11, color: hi ? C.white : C.grey } }],
                  { x: x + 0.25, y: y + 0.15, w: w - 0.5, h: h - 0.3, fontFace: F.body, valign: "top", margin: 0 });
      });
      return y + h + 0.3;
    },
    closing({ line, cta, contact }) {
      const s = pptx.addSlide(); n++; s.background = { color: C.navy }; brand(s, true);
      s.addText(runs(line, { fontFace: F.head, fontSize: 40, bold: true, color: C.white }), { x: M, y: 2.3, w: 11, h: 1.3, margin: 0 });
      s.addShape("rect", { x: M, y: 3.7, w: 0.9, h: 0.06, fill: { color: C.orange }, line: { type: "none" } });
      if (cta) s.addText(cta, { x: M, y: 4.0, w: 11, h: 0.6, fontFace: F.body, fontSize: 18, bold: true, color: C.orange, margin: 0 });
      if (contact) s.addText(contact, { x: M, y: 4.7, w: 11, h: 0.5, fontFace: F.body, fontSize: 14, color: "C9CED8", margin: 0 });
      return s;
    },
    runs, save: (file) => pptx.writeFile({ fileName: file }),
    C, F, W, H, M,
  };
  return api;
}
module.exports = { makeDeck };
```

Example:
```js
const { makeDeck } = require("./by.js");
const d = makeDeck({ lab: "Money Lab", logoPath: "logo.png", logoDarkPath: "logo-white.png" });
d.cover({ kicker: "Money Lab Junior · Week 4", title: "*Money Lab Week 4:*\nIs It Worth the Price?", sub: "Financial ratios · Risk", instructor: "Name" });
d.section({ part: "Part 1", title: "Ratios in *Context*", sub: "A ratio alone means nothing." });
let { s, top } = d.content({ eyebrow: "Step by step", title: "Where to find the *numbers*.", sub: "Five steps. Five minutes per stock." });
top = d.steps(s, top, [{ title: "Search the ticker" }, { title: "Income Statement" }, { title: "Ratios" }, { title: "Repeat for peers" }, { title: "Add the source" }], { highlight: 4 });
d.callout(s, top, { label: "Watch out", text: "Cross-check one number against the 10-K. *No chatbot figures.*" });
d.activity({ title: "35 min *work session*", minutes: 35, note: "Build your peer table" });
d.closing({ line: "Be specific. Be curious. *Be open.*", cta: "Next step: Investor Lab", contact: "moneylab@builtyoung.org" });
d.save("deck.pptx");
```

## Check before delivering
Render to PDF (`soffice --headless --convert-to pdf deck.pptx`) and look at every slide. Check for: overflowing or clipped text; the logo on every slide; one orange key phrase per title; at most one orange block per slide; nothing too small; sources on numbers. Fix and re-render, then deliver.
