---
description: "Interactively clarifies vague requests and constructs a precise prompt for /withScout."
argument-hint: "[Optional: The vague request]"
---

# /what

This command acts as a **Requirement Analyst**. Its sole purpose is to turn a vague user intent (e.g., "fix it") into a precise, context-aware instruction that can be handled by the execution engine.

## When to use

- **Use when:** The user's input is ambiguous, lacks context, or is too short (e.g., "cleanup code", "it doesn't work").
- **Goal:** To produce a **Refined Prompt** that serves as the perfect input for `/withScout`.

## Actions

1.  **Step 1: Grounding (Context Check)**

    - **Check Documentation:**
        - IF `<projectRootPath>/llmdoc/index.md` exists: Read it to understand the domain language.
        - IF NOT exists: Briefly scan the root directory file names to guess the project type.
    - **Analyze Input:** Identify missing parameters (e.g., "Which file?", "Which error?", "What represents 'clean'?").

2.  **Step 2: Interactive Clarification**

    - Use the `AskUserQuestion` tool to disambiguate.
    - **Strategy:** Offer concrete scenarios based on the context.
    - **Example:**
        - *Vague:* "Fix the bug."
        - *Clarification:* "I see a few recent changes. Are you referring to: (A) The TypeScript error in `auth.ts`? (B) The layout issue in `Header.tsx`? or (C) Something else?"

3.  **Step 3: Prompt Engineering (The Hand-off)**

    - Once the user responds, synthesize their intent into a **Structured Prompt**.
    - **Format:** `[Action Verb] [Target Component] [Specific Constraint] [Context]`.

4.  **Step 4: Execute via Scout**

    - **Do not execute the worker directly.** (Let the system handle routing).
    - Instead, invoke `/withScout` with your refined prompt.
    - *Command:* "Based on your clarification, I am running: `/withScout [Refined Prompt]`"

## Example Flow

1.  **User:** `/what fix auth`
2.  **Agent:** "I see `src/auth/` has JWT logic and OAuth. Are you trying to (A) Fix a specific bug in login? or (B) Refactor the token refresh flow?"
3.  **User:** "B, the refresh flow is outdated."
4.  **Agent:** "Understood. Invoking: `/withScout Refactor the JWT token refresh flow in src/auth to match modern security standards.`"
    *(Then /withScout takes over, assesses complexity as High, and starts Investigators.)*