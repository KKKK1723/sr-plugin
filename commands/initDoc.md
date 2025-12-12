---
description: "Bootstraps a high-density, LLM-optimized documentation system (`/llmdoc`)."
argument-hint: ""
---

# /initDoc

This command creates the initial "Retrieval Map" for the project. It focuses on structure and density over volume.

## Actions

0.  **Step 0: Discovery**
    - Read `README.md`, config files, and file tree.

1.  **Step 1: Deep Scouting**
    - Launch `scout` agents to understand the *actual* architecture (which often differs from the README).
    - **Output:** structured reports defining the "Critical Paths".

2.  **Step 2: User Alignment**
    - Propose a list of detected Core Concepts (e.g., "Auth", "Payment", "Data Pipeline").
    - Ask user to confirm which ones to document immediately.

3.  **Step 3: Foundation Layer (Concurrent)**
    - **Recorder A (Project Identity):** Create `overview/project-overview.md`. Focus on "What is this?" and "Key Constraints".
    - **Recorder B (Map Maker):** Create `architecture/system-map.md`. Define the high-level module interaction.
    - **Recorder C (Rules):** Create `reference/conventions.md`. Extract strict rules from configs.

4.  **Step 4: Concept Layer (The Meat)**
    - For each selected concept, invoke a `recorder`.
    - **Strict Instruction:**
      "Create the `/architecture/<concept>.md` file.
      **CRITICAL:** You MUST include a 'Critical Paths' section listing exactly how data flows through files (file:line -> file:line).
      Do not write generic descriptions. Write a map."

5.  **Step 5: Final Linkage**
    - Generate `/llmdoc/index.md` as the entry point.
    - Clean up temporary scout reports.