---
name: narrative-coherence
description: Use when reviewing or preparing a code change for submission to an upstream project or external reviewers; before uploading a CL/PR, finalizing a commit message, or tidying a diff so reviewers can follow a single story.
---

# Narrative coherence

## Overview

When reviewing and preparing a change to be submitted upstream, ensure that it
is clear and understandable to external reviewers. A reviewer who reads only the
diff and the commit message should be able to recover one coherent story.

## Checklist

- Establish a single thesis an external reviewer could recover from the diff.
- Unify vocabulary across code, comments, tests, and commit message.
- Use the same vocabulary consistently to refer to the same concepts.
- Keep every hunk that serves the thesis; consider the removal or deferral of the rest.
- Introduce abstractions at the point of need.
- Align tests to narrate the same story as the implementation.
- Reconcile the commit message and the diff.
- Order changes expositorily, not chronologically.
- Explain what the change does and why. Do not explain the details of the development process.
- Prefer to edit subtractively.
- Recompile and run all relevant tests after edits.
- Iterate.
