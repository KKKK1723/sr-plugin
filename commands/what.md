---
description: "Clarifies vague requests and routes them to the appropriate workflow (Fast vs. Deep)."
argument-hint: ""
---

# /what

This command acts as a **Smart Triage Interface**. It clarifies vague user requests and determines the optimal execution path (Fast Track vs. Deep Investigation) based on context.

## When to use

- **Use when:** The user's prompt is ambiguous (e.g., "fix it", "it's broken") or lacks necessary context.
- **Goal:** To convert a vague intent into a concrete **Execution Plan** or a specific **Investigation Task**.

## Actions

1.  **Step 1: Context & Complexity Assessment**

    - Read `<projectRootPath>/llmdoc/index.md` to ground the request in the project context.
    - **Assess Complexity:** Analyze the vague request.
      - Does it likely touch > 3 files?
      - Is it an architectural change?
      - Or is it a simple fix/refactor?

2.  **Step 2: Formulate Clarifying Questions (Option-Based)**

    - Ask questions to narrow down the intent AND the scope.
    - **Example:** "Are you trying to: (a) Fix a typo in the UI [Fast Track], (b) Change the login logic [Deep Track], or (c) Add a new payment method [Deep Track]?"

3.  **Step 3: Ask the User**

    - Use the `AskUserQuestion` tool to present the options.

4.  **Step 4: Route to Workflow**

    - **If the clarified task is Simple (Fast Track):**
      - Propose a direct execution plan using `worker` agents immediately.
      - *Command:* "I understand. This is a quick fix. I will run a worker to execute this now."
    - **If the clarified task is Complex (Deep Track):**
      - Formulate specific **Investigation Questions**.
      - Invoke `/withScout` with these factual questions.
      - *Command:* Calls `/withScout` with the refined inquiry (e.g., "Investigate the current payment gateway abstraction to prepare for adding Stripe.").