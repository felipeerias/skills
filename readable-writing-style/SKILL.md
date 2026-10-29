---
name: readable-writing-style
description: This skill should be used when writing or revising prose that a person will read, such as an email, a commit description, a review comment, a post, or a report.
---

# Readable writing style

Draft naturally, or start from the supplied draft, at the length that the facts and the medium require, and shorter when the facts allow. Then check the draft against the points below and change only what needs correction. Do not add filler to show compliance; add a word only when clarity or accuracy requires it, such as a missing "that".

The reader processes text one word at a time with limited memory. Include the context that this reader needs, and no more. Keep each claim's facts, scope and strength intact when rewording.

## Sentences

- Keep the verb next to its subject and the object next to the verb. Long conditions and asides go at the end or in their own sentence; a short, clear clause may sit in the middle.
- Keep "that", "which", "who", articles, "then" and "of". Never drop "that" from an object relative ("the behaviour that the old code never reached"). Recast an object relative with a full-noun subject, usually by making the noun the object of the main clause and leaving a subject relative for the rest: "the regression that the compositor change introduced blocks the release" becomes "The compositor change introduced a regression that blocks the release".
- One step or claim per clause. No participle chains ("..., exceeding the padding and growing the object") and no "-ing" tails that comment without support ("..., ensuring consistency").
- Connect ideas with because, so, when, but. Split a sentence with two main points or difficult nesting, and keep the relationship between the ideas. Let length vary; a run of five-word sentences reads as badly as one forty-word sentence.
- When the actor matters and is known, make it the subject and its action the verb ("we dropped the check because ..."). A passive or "This change ..." is fine when it keeps the topic in place or the actor is unknown.
- Comma after a fronted clause. "Only" right before the word it limits. A noun instead of a pronoun that could point to two things.

## Words

- No more than two nouns in a row unless they are a term that the reader knows. State the relationship instead: "how absolute paths are handled when backing up dotfiles", not "absolute dotfile backup path handling".
- No ad hoc noun-to-verb conversions ("calendar it") and no personification that hides what the code does or guarantees ("catches the exception and returns false", not "swallows the error"). Ordinary technical verbs are fine.
- Plain, consistent words: one term per concept, the project's own term when there is one, no coined compounds or fresh metaphors. "Is", "has", "uses", "wrote", not "serves as", "boasts", "utilizes", "authored". Avoid delve, leverage, robust, seamless, crucial, pivotal, showcase, underscore (verb), foster, enhance, streamline, meticulous, landscape (abstract), "align with". Keep identifiers, names and quotations exactly.

## Claims and tone

- Keep the concrete facts that support a claim or decision (version, file, function, number, bot, error, date). Never swap one for an evaluation such as "a significant improvement". Leave out facts that the reader does not need.
- Say what was observed, what is inferred and what is verified. Give the basis of an uncertain claim ("the failures started after the timeout change; the cause is not confirmed"). Avoid vague authority ("experts say"), boilerplate hedges ("it is important to note") and evasive apologies ("any inconvenience this may have caused"). First person only for what the sender actually did or knows.
- No inflated significance, promotional adjectives, triplets by reflex, or "not X but Y" unless correcting a real claim. No recap or uplifting line at the end of a short message.
- No openers or closers aimed at the reader's mood ("Great question", "I hope this helps", "Let me know if you have any questions"). A brief, specific thanks or apology is fine. Never invent the sender's feelings. No exclamation marks.
- In criticism, no sarcasm, shaming or impatience markers ("obviously", "as I already said").
- Cut redundant restatements and explanations of what the reader already knows. Keep brief references back and connectives when they clarify how ideas relate.

## Structure

- Lead with the point or the request. Define terms that this reader may not know. State the action, owner and date of an ask when they are known; do not invent them.
- Format to help navigation, not to decorate: lists for parallel items or steps; headings only when a message covers several separate topics; no bold lead-ins, emoji, em dashes or arrows in prose. Plain text, not Markdown, for commit messages, mailing lists and Gerrit.

## Final check

- Reread the draft as the recipient and check every factual claim against the available evidence. Density builds up late in a long session: if the draft comes at the end of one, redraft the dense passages from the facts rather than polishing them.
