---
name: make-knowledge-cards
description: Turn a user-pasted article or a local Markdown/TXT file into 5-8 concise, source-faithful knowledge cards. Use when the user wants to extract key ideas, turn an article or notes into study cards, make knowledge cards, or summarize text as cards; trigger phrases include "知识卡片", "提炼要点", "做成卡片", "study cards", and "knowledge cards". Supports only pasted text and local .md/.txt files; do not use for web scraping, PDFs, Anki exports, or graphical card UIs.
---

# Make Knowledge Cards

Convert source material into a small set of knowledge cards that help a reader
understand and recall it. Every card teaches exactly one idea and contains four
parts: a title, the core knowledge, a short explanation, and an example or a
self-test question.

The goal is selective understanding, not exhaustive compression. A useful card
set reflects what matters most in the source and is fully traceable to it.

## Scope

Supported inputs:

- Text pasted directly into the conversation.
- A local `.md` or `.txt` file the user references.

Unsupported inputs and outputs:

- Web pages or URLs; do not fetch them.
- PDFs or other document formats; do not attempt extraction or conversion.
- Anki decks or Anki export files.
- Graphical card UIs, slide decks, or images.

When asked for an unsupported format, state plainly that it is not supported and
ask the user to paste the text or provide a `.md` / `.txt` file. Do not silently
switch to another source or fabricate content.

Read a local source file in full before extracting from it. If the file is
missing, unreadable, or empty, say so and ask for the text. Do not create cards
from a topic alone: without source material, there is nothing faithful to
extract.

## Workflow

### 1. Read and map

Read the complete source, ignoring navigation, ads, repeated boilerplate, and
copyright notices. Build a private inventory of candidate points, noting where
each appears:

- The central claim, question, or purpose.
- Definitions and key distinctions.
- Mechanisms: why or how something happens.
- Processes: ordered steps, prerequisites, and warnings.
- Evidence, examples, conditions, exceptions, trade-offs, and caveats.
- Claims that recur in different words.

Do not show this inventory to the user.

### 2. Select what matters

Keep only points that are central, explanatory, actionable, or necessary for
understanding the source. Drop trivia, decorative anecdotes, rhetorical asides,
and details that do not support a key point.

- Prefer the author's main claims and the reasoning behind them over background
  information the reader can safely skip.
- For a procedure, preserve the source's step order, step numbering, constraints, and warnings.
- For an argument, keep the thesis, the supporting reasons, and the stated
  scope separate from the conclusion.
- For an explainer, keep definitions, mechanisms, and important conditions.

Make each card atomic at a useful level: one claim, step, mechanism, or
recommendation. Closely related sub-actions may stay together when they jointly
implement one recommendation and the title names the whole procedure. Avoid both
bundling unrelated points and fragmenting one idea into several cards.

All four fields must develop that same idea, and the explanation must not
introduce a second rule or takeaway that could stand alone. Use this check: the
example or self-test should exercise the card's main claim as a whole, not just
one half of it. If two candidate points answer the same question, merge them
into one card. Remove semantic duplicates even when the source repeats them in
different words.

Do not create a card for a closing restatement, a generic remark such as "tools
are only aids", or a meta-comment about the article's method or importance,
unless it adds a distinct substantive rule. Do not rename a later step "the
first step" or otherwise change the source's sequence. If a card combines steps,
combine only steps that are parts of the same action.

### 3. Set the card count

Target 5-8 cards when the source contains at least that many distinct, important
points. Prefer the smallest number that still captures the source's central
knowledge; eight is a ceiling, not a quota. Select the most important points
rather than splitting one point to reach the range.

If the source has fewer than five distinct key points, output only the genuine
points and add one short line before the cards explaining that information was
insufficient. Do not pad with repetition, background knowledge, speculation, or
new examples. If the source is too thin to support even one meaningful card, say
so and ask for more material instead of producing empty cards.

Never exceed eight cards. If the source is rich, choose the points with the
greatest explanatory or practical value.

### 4. Write the cards

Write in the source's language by default. If the user requests another language,
translate faithfully and add no new information. Localize the labels while
keeping the same four fields.

For each card:

- **Title**: A specific, compact label for the single idea. Avoid vague section
  names such as "Introduction" or "More details".
- **Core knowledge**: Exactly one sentence stating the single most important,
  transferable claim.
- **Explanation**: Two to four sentences in plain language. Explain why or how,
  using only context and definitions the source provides. Preserve the source's
  conditions and degree of certainty; do not import textbook definitions or
  outside facts.
- **Example or self-test**: Use an example from the source when one illustrates
  the point. An example must be a concrete case, observation, experiment, or
  illustration actually present in the source; a recommendation or rule is not an
  example. Otherwise write one short self-test question followed by a short
  answer. A self-test may apply the source's facts to a simple scenario, but every
  element needed to answer it must come from the source. Never present a
  hypothesized scenario as something the source reported, and never add outside
  facts, new causes, or unsupported numbers.

Place the cards in a learning order, from foundational to advanced. Each card
must make sense on its own without requiring the previous card, while still
forming a coherent set.

Use this format, localizing the labels and note for the output language:

```markdown
**Source:** <file name or "pasted text"> · **Cards:** <N>

## 1. <specific title>

**Core knowledge:** <one sentence>

**Explanation:** <2-4 sentences>

**Example / self-test:** <source example, or question + short answer>
```

For Chinese output, use labels such as `核心知识`, `简明解释`, and
`例子或自测`, and a note such as `来源：...｜卡片数：N`.

### 5. Run the quality gate

Before responding, check every item and fix problems:

- Every card has all four required parts.
- Each card teaches one idea; all four fields develop that idea, the self-test
  exercises the whole claim, and no field introduces a second independent
  takeaway. No two cards substantially overlap.
- The set covers the source's central knowledge and omits minor material,
  including closing restatements and generic remarks.
- Every claim is supported by the source. There are no invented facts, examples,
  quantities, causal links, quotations, definitions, or outside knowledge.
- Attribution and scope are preserved. A single team, anecdote, or opinion is not
  presented as a universal fact. Hedges and limits remain; possibility is not
  presented as certainty, and correlation is not presented as causation.
- Step order and numbering match the source.
- Each example actually appears in the source, or each self-test can be answered
  from the source and includes a correct answer. A recommendation is not labeled
  as an example, and no hypothetical scenario is presented as reported fact.
- The count is 5-8, or an explicit insufficient-information note explains a
  smaller count. The output is selective rather than padded.
- The output language and Markdown formatting are consistent.

Return the cards in the conversation by default. Write a file only when the user
asks for one, and never overwrite an existing file without permission. Do not add
an extra article summary, generic advice, or an Anki/GUI deliverable.
