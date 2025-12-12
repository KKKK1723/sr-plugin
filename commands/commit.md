---
description: "Smartly analyzes changes, performs code sanity checks, and generates context-aware conventional commit messages."
argument-hint: ""
---

# /commit

This command acts as a **Smart Commit Gateway**. It analyzes staged/unstaged changes, checks for leftover debug code, understands the task context, and generates a standardized commit message.

## When to use

- **Use when:** The user is ready to save their work.
- **Suggest when:** A `worker` has completed a task successfully, or the user says "done", "save", or "wrap up".

## Actions

This command follows a **Gather → Check → Draft → Confirm** workflow.

### **Step 1. Gather Context & Diff **

1.  **Git Status:** Run `git status`, `git diff --staged`, and `git log -n 5` (to learn style).
2.  **Task Context:**
    * Check for a recent context file (e.g., `.claude/context.md` or `llmdoc/agent/*.md`).
    * *Goal:* Understand the **Business Intent** (Why) behind the changes, not just the code syntax (What).

### **Step 2. Sanity Check (Code Hygiene) **

1.  **Scan for Debris:** Analyze the diffs for common mistakes:
    * Leftover `console.log`, `print()`, or debugging statements.
    * New `TODO` or `FIXME` comments.
    * Conflict markers (`<<<<<<<`).
2.  **Report Issues:** If any issues are found, **warn the user immediately** before proposing the message.

### **Step 3. Draft Commit Message **

Generate a commit message following the **Conventional Commits** standard:
> `<type>(<scope>): <subject>`
>
> `<body>` (Optional, for complex changes)

* **Type:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`.
* **Scope:** The module or layer affected (e.g., `auth`, `ui`).
* **Subject:** Imperative mood, concise summary (e.g., "add login validation", NOT "added login validation").
* **Body:** Explain *why* the change was made, referencing the task context if available.

### **Step 4. Propose & Execute **

1.  **Interact:** Use `AskUserQuestion` to present the drafted message and the status.
    * *Option A:* "Commit with this message"
    * *Option B:* "Edit message"
    * *Option C:* "Stage all changes and commit" (if currently unstaged)
    * *Option D:* "Cancel"
2.  **Execute:** Run the git command based on the user's choice.
3.  **Post-Commit Suggestion:**
    * After a successful commit, check if the changes might affect documentation.
    * **If yes:** Suggest: *"Changes committed. Would you like to run `/updateDoc` to keep documentation in sync?"*