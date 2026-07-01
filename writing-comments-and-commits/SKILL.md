---
name: writing-comments-and-commits
description: Use when writing or editing a code comment, commit subject or body, or other forms of communication with upstream reviewers around a concrete code change. Use before proposing prose reviewers will read. Symptoms you should invoke this: reaching for a metaphor, hedging, restating what the code says, or drafting a commit body longer than a few sentences.
---

# Writing comments and commits

## Overview

**Core principle: say exactly what the code does — no more, no less — in the codebase's own words, for a reader seeing the final state cold.**

This skill is the sentence- and word-level companion to `narrative-coherence`
(which works at the diff/story level). It governs the prose around a change:
code comments, commit subjects and bodies, and communications with reviewers.

**Word choice is correctness here, not style.** A word's connotation is a claim.
"Fall back" claims a ranked alternative; "drive" claims agency; "round-trips"
claims a guarantee; "honor" claims an obligation. When the code makes no such
claim, the word is a bug, however well it reads. This is why the corrections
below look like nitpicks but are not: choosing the word whose meaning matches the
mechanism — and reaches no wider — is the whole task.

## The three principles everything reduces to

1. **Claim exactly what the code does.** A comment, message, or reply must be as
   strong and as scoped as the code it describes — no more. Over-broad
   generalizations, implied uniqueness, and too-specific causes are all defects
   *even when they read well*.
   - "it is limited to the state where that is allowed" → "which is not allowed
     during `kFinalizingTree` (see `AXObject::SetAncestorsHaveDirtyDescendants()`)"
     — state the exclusion, don't imply uniqueness.
   - "when its underlying node is removed from the tree" → "can synchronously
     remove the AX object wrapped by this proxy" — the node isn't actually
     removed; don't name a narrower cause than the real one.
   - An unscoped reviewer claim ("Notifications … are processed in FIFO order")
     invites "that's not true of X". Scope it to the path that holds.

2. **Write for a reader seeing the final state cold.** The reviewer never saw
   the history. Nothing is "re-enabled", "restored", "trimmed", or "now" done
   differently. Cut every word that only makes sense relative to a prior state.
   - "Re-enables the fuzzer reproducer…" → "Adds gtest regressions…"
   - "Now only advance to a scroll-button…" → "Advance only to a scroll-button…"
   - Counterfactuals ("duplicate", "suppress the redundant", "skip") describe
     what changed — they belong in the commit message, not in a code comment a
     future reader meets with no memory of this CL.

3. **Use the codebase's own words; don't coin.** Reuse the exact vocabulary
   already in the code, the test comments on HEAD, and the spec. One concept,
   one term, everywhere (code, comment, test, message, reply).
   - Coined metaphors get rejected: "sidecar", "re-wrap", "round-trips",
     "drive" (of a data structure), "stylesheet-parsing path" ("that's
     Claude-speak").
   - Use the spec's term of art precisely: keep "simplifies" (CSS Values 4);
     don't swap to "resolves"/"computes", which name different phases.
   - Don't alternate synonyms: pick "diverge" *or* "drift" and use it throughout.

## Comments

| Instead of | Write | Why |
|---|---|---|
| "Chooses the element … the caller will recreate `\|ancestors\|`" | "Returns the element into which `\|ancestors\|` will be recreated" | Say what the function yields; don't narrate who calls it or when |
| "so fall back to a `<div>` whenever any ancestor is non-phrasing" | "so a `<div>` is used when any ancestor is non-phrasing" | "Fall back" and "whenever" import a ranked alternative and a recurrence the code doesn't have |
| "Normally this is the default paragraph element" | "Usually this is the default paragraph element" | "Normally" implies abnormal cases exist; "usually" is just frequency. Use the exact noun, not a synonym |
| "Drop the manager when basic a11y is disabled" | "When basic a11y is disabled, tear down the `BrowserAccessibilityManager`" | Replace the vague verb and the pronoun with the real operation and the real type |
| "awaiting confirmation or replacement by the next `UpdateAccessibilityMode()`" | "Consumed by the next `UpdateAccessibilityMode()`" | Name the mechanism; "awaiting confirmation" ascribes an intent the code doesn't have |
| "The sanitizer correctly leaves the attribute alone." | "the sanitizer does not strip the attribute." | Name the exact operation, not a colloquial paraphrase of it |
| "With the updated … algorithm, `"xlink:href:x"` will resolve to…" | "Per the DOM 'validate and extract' algorithm, `"xlink:href:x"` resolves to…" | Present tense states current behavior; "Per the …" avoids implying the spec itself changed |
| "The computed integer to serialize." (field doc) | `// Empty for reversed(name) without an integer.` | A field comment should add the non-obvious invariant, not repeat the type |
| "Only meaningful for counter-reset." | `// Used by counter-reset.` | State the concrete relationship; "only meaningful" says nothing a reader can check |

More comment rules:

- **Lead with the one non-obvious fact** the comment exists to convey; strip
  enumerations and restatements the code or spec already carries. A long
  whitespace-set enumeration became: `// U+000B (VT) is not ASCII whitespace
  per <url>, even though base::kWhitespaceASCII includes it.`
- **A function comment says what it does, then why — never only a caveat.**
  "Avoid `base::TRIM_WHITESPACE` here: it includes VT." → "Splits `input` on CSP
  ASCII whitespace and returns the non-empty tokens. Wrapping the split avoids
  `base::TRIM_WHITESPACE`, which would treat VT as whitespace."
- **Keep only load-bearing comments.** Delete what the symbol name already
  carries; keep the note that guards a non-obvious decision a maintainer would
  otherwise "fix". Trim, don't blanket-remove.
- **Test comments narrate setup → trigger → expectation** in the code's own
  vocabulary, each sentence mapping to a block below it — not a restatement of
  the property being set up.
- **One line per case**; don't run several cases into one long comment.
- **Preserve spec URLs when trimming.** Cut prose, not references — the link is
  load-bearing.

## Commit subjects

- **Imperative mood**, not declarative: `CSP: stop treating U+000B (VT) as ASCII
  whitespace`, not "CSP stops treating…".
- **Plain, sharp verbs**: Fix / Support / Match / Distinguish / Carry / Defer.
  Never "honor". Don't settle for a bare "Fix"/"Update" when a sharper verb fits.
- **`scope: Capitalized verb`** — lowercase the scope prefix, capitalize the
  verb after the colon: `dom: Split qualified names on the first colon`.
- **Backtick identifiers and CSS values**: `Distinguish \`italic\`, \`oblique\`,
  and \`oblique 14deg\` in computed font-style`.
- **Name the actual operation**; drop framing nouns. "Use a `<div>` wrapper
  when…" → "Use a `<div>` when any ancestor is non-phrasing" (the code
  substitutes, it doesn't wrap). Match the subject's closing clause to the
  in-code comment's closing clause verbatim.

## Commit bodies

**Structure (expository, not chronological):**

1. The governing rule, or the concrete outcome — open with the spec/algorithm
   ("Per CSS Fonts 4…") or the failing tests this fixes ("This CL fixes two
   failing WPT tests related to…").
2. The defect, at symbol level, with real before→after values (`"f:o:o"`
   returned `"o"` instead of `"o:o"`). Name the **root-cause** symbols
   (`base::kWhitespaceASCII`, `base::IsAsciiWhitespace`), not the surface
   symptom — and only symbols the diff actually touches.
3. The fix, in one short imperative sentence.
4. Bound the blast radius *when a reviewer might fear a side effect* ("does not
   affect font selection nor rendering", "leaving the default path unchanged").
   Do **not** narrate code paths the change never touches.
5. Pre-existing issues, flagged as such so they aren't attributed to this CL.

**Prose rules:**

- **Concise. Prose paragraphs, not bullet points.** Cut preambles,
  meta-commentary ("Why this framing"), and re-derivation of spec logic. A
  ~15-line body with numbered footnotes became four sentences.
- **Short declarative sentences, one idea each.** Don't comma-splice with
  "however" — use a period ("The set excludes U+000B (VT). The network-service
  parser used…"). Vary causal connectives instead of repeating "so"; keep the
  link explicit rather than clipping into fragments.
- **Expand contractions**: "did not", not "didn't".
- **Don't anthropomorphize data structures**: not "`CounterDirectiveMap`
  drives layout" but "`list_item_ordinal.cc` reads it at layout time".
- **Proper spec names and casing**: "Infra", not "INFRA"; "matches ASCII
  whitespace from the Infra standard", not "matches the definition of…".
- **Cut fillers and doublings**: "reflects it accordingly", "existing, more
  limited", stacked prepositions, jargon ("all-pass" → "now pass"; "gate on" →
  "limit … by checking"). Don't inflate with "all".
- **Don't overclaim**: "symmetric with parsing", not "round-trips".
- **Don't churn the message** for incidental changes, reverts, or renames of
  private helpers the message never named. Edit it only to keep it accurate to
  the final diff and intent.

## Reviewer replies

- **Short, plain, binary questions.** Drop hypothetical "e.g." scenarios and
  parenthetical scope caveats. "Was this an intentional change? If it was
  accidental, I will put up a small CL removing that line."
- **State the "why" in one plain clause**, not an implementation-mechanism
  sentence naming internal calls.
- **Don't assert intent you can't verify** ("accidentally"); state the
  observable fact and let the question carry the doubt.
- **Scope every claim to the paths it holds for**, and keep it reconciled with
  the commit message. Name the specific test that pins a claim
  (`QueuedChildrenChangedFlattensReentrantDispatch`), not vague "unit tests".
- **Terse; say each idea once.** "I'd be happy to follow up with a new one for
  any follow-up work." → "Happy to address any follow-ups in a separate CL."
- **Present indicative, drop hedges**: "about the correct scope" → "the right
  scope"; "inside of it" → "inside it".
- Openings: "Thanks in advance for the review"; prefer "no longer reproduces"
  over "does not happen any more".

## Naming (when a change introduces identifiers)

- **Symmetric pairs**: `kImplicitAngle` / `kExplicitAngle` (shared suffix,
  contrasting prefix), not `kImplicitAngle` / `kObliqueAngle`.
- **Shorter identifiers**; trim redundant words —
  `waitForPageLoadAndInitialWindowContentChangedEvent` →
  `waitForPageLoadAndInitialContentChange`.

## A workflow rule, not a wording one

You are drafting text for someone else to commit. **Propose** comment and
commit-message wording; do not run `git commit` or `git commit --amend`, and do
not alter commits they have already made.

## The one-line self-check

Before proposing any of this prose, ask: *does it claim more than the code does
(watch the connotations), use a word the codebase wouldn't, or assume a reader
who saw the history?* If any, cut it back.
