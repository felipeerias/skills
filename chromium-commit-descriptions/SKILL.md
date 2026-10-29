---
name: chromium-commit-descriptions
description: This skill should be used when writing or revising a commit description (CL description) for Chromium.
---

# Chromium commit descriptions

A description explains what the change does and why. The diff shows how; explain an implementation detail only when it justifies the change. Reviewers read the description first. Later readers of git log and git blame read it without the review thread.

## Subject

- One line, imperative, no final period, about 70 characters at most. It completes "If applied, this commit will ...".
- Follow the recent commits on the touched files for an area tag ("[a11y]", "[svg]") and for capitalization. Do not invent a tag.

## Body

Leave a blank line after the subject. Wrap prose at 72 columns; do not break URLs or identifiers. Use the order below as a default. Combine related points or change their order when that makes the explanation clearer, and omit anything that does not apply.

1. Context: the spec or the expected behaviour, with the spec link on its own line.
2. Problem: the behaviour before this CL and its user-visible effect, in the present tense.
3. Cause: the function or condition that is responsible, with the non-obvious mechanism or evidence that justifies the fix.
4. Change: what the CL does, in plain English and in the imperative ("Make SizeMayChange() return true for ...", not "This CL makes ..." or "I changed ...").
5. Tests: the tests that cover the change, and the baselines that were removed or updated.
6. Scope, costs and alternatives: any limit on the scope, the costs of the change, and a rejected alternative when it explains the choice.

- Name the actual functions, flags, tests and files, with the same vocabulary as the code and its comments. Do not list every touched symbol.
- Support performance claims with measurements, the benchmark and the test environment. Support size claims with measurements and the build configuration. Do not invent measurements.
- Make the description self-contained: summarise what the reader needs from any linked discussion. Refer to commits by at least 12 hash characters plus their subject, and to other CLs as https://crrev.com/c/NUMBER.
- Say how the CL fits into a series, and link the relevant design documents and bugs. For a reland, say what makes it safe and link the separate fix.
- Leave out the development diary and any hunk-by-hunk narration of the diff.
- Use the length that the explanation needs. Many descriptions fit in 80 to 250 words, and shorter is fine. Keep extra detail only when readers need it.

## Footers

Put the applicable footers together in the final paragraph. Bug: references the related issues. Fixed: closes an issue on landing. Test: records manual testing or gives instructions for testers; omit it when the trybots cover all testing. Add AX-Relnotes: when the change affects assistive technology.

## Final check

- One problem per CL. A description that keeps growing is a sign that the CL probably needs splitting.
- Reread the description against the final patch set before landing.
- For the prose, apply the readable-writing-style skill.
