---
description: "Conducts an automated, multi-perspective review of a Pull Request with structured output."
argument-hint: "[PR number or URL]"
---

# /reviewPR

This command performs a "Virtual Tech Lead" review, checking for code quality, architectural integrity, and test coverage.

## When to use

- **Use when:** User asks to review a PR or a branch before merging.

## Actions

1.  **Step 1: Fetch & Prepare**
    - Get PR details (`gh pr diff`, `gh pr view`).
    - **Context:** Read `/llmdoc/architecture` to understand the intended design patterns.

2.  **Step 2: Parallel Structured Analysis**
    - Deploy `investigator` agents with specific personas. **CRITICAL:** Each investigator MUST output an `<Issues>` list and an `<Assessment>` score.

    - **Investigator A (Code & Safety):**
      - Check: Error handling, strict typing, security risks, leftover debug code.
    - **Investigator B (Architecture & Pattern):**
      - Check: Does this match the `/llmdoc` patterns? Separation of concerns?
    - **Investigator C (Completeness):**
      - Check: Are there tests? Are docs updated?

3.  **Step 3: Synthesize Report**
    - Merge findings. Filter out duplicates.
    - **Classify:**
      - 🔴 **Blocking:** Security risks, major bugs, architectural violations.
      - 🟡 **Warning:** Missing tests, confusing naming, edge cases.
      - 🟢 **Nitpick:** Formatting, typos.

4.  **Step 4: Interactive Submission**
    - Present the structured report to the user.
    - Options: "Post as Comment", "Request Changes", or "Approve".
    - Execute the `gh pr review` command based on choice.