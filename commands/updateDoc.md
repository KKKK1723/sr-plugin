---
description: "Updates the documentation based on recent code changes, ensuring consistency and removing staleness."
argument-hint: "[Optional: specific update instructions]"
---

# /updateDoc

This command synchronizes the `/llmdoc` "Retrieval Map" with the latest code reality. It handles additions, modifications, and deletions of concepts.

## When to use

- **Use when:** Code changes have been committed, and the docs need to reflect the new truth.
- **Suggest when:** A `worker` completes a task successfully.

## Actions

1.  **Step 1: Impact Analysis**
    - Run `git diff HEAD` (or read the user's description).
    - **Concept Extraction:** Identify which *Core Concepts*, *Architectural Flows*, or *APIs* were touched. Ignore trivial formatting changes.
    - **Staleness Check:** Identify which existing documents might now be obsolete or incorrect.

2.  **Step 2: Concurrent Updates (Content-Only Mode)**
    - Invoke `recorder` agents for each impacted concept.
    - **Prompt:**
      "**Task:** Sync documentation for concept **`<concept_name>`**.
      **1. Analyze Changes:** Read the diff and understanding the *new* behavior.
      **2. Update/Prune:** Update relevant docs. **CRITICAL:** If a feature was removed, DELETE the corresponding documentation or sections.
      **3. Format:** Strictly follow the `<ContentFormat_...>` structures.
      **4. Minimality:** Be concise. This is a map for LLMs, not a novel.
      **5. Mode:** Operate in `content-only` mode."

3.  **Step 3: Global Indexing (Full Mode)**
    - Invoke a single `recorder` agent.
    - **Task:** "Re-scan `/llmdoc`. Update `index.md` to remove broken links to deleted files and add new files. Ensure the map is traversable. Operate in `full` mode."