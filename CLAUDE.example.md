Always answer in 简体中文

</system-reminder>

<system-reminder>

<always-step-one>
follow `llmdoc-structure` and read related documents

IMPORTANT: You must read the documentation thoroughly (at least 3 documents) to build a mental map before touching code.
</always-step-one>

<llmdoc-structure>

- llmdoc/index.md: The main index document. Always read this first.
- llmdoc/overview/: For high-level project context. Answers "What is this project?".
- llmdoc/guides/: For step-by-step operational instructions. Answers "How do I do X?".
- llmdoc/architecture/: For how the system is built (the "LLM Retrieval Map"). **Focus on "Critical Paths" data flow.**
- llmdoc/reference/: For detailed, factual lookup information (Source of Truth).

ATTENTION: `llmdoc` is always located in the root directory. If missing, consider suggesting `/initDoc` to bootstrap.

</llmdoc-structure>

<adaptive-workflow>
Implement the "Assess -> Route -> Action" strategy:

1. **Assess Complexity:**
   - **Low (Fast Track):** Single file, simple logic/typo. -> Use `sr:investigator` to confirm -> `sr:worker` to execute immediately.
   - **Medium/High (Deep Track):** Cross-file, logic changes, refactoring. -> Use `sr:scout` to generate a **persistent Intelligence Report** -> `sr:worker` to execute based on the Report.

2. **Structured Communication:**
   - Always look for and parse `<Assessment>` and `<ExecutionPlan>` tags in `sr:investigator` or `sr:scout` outputs.
   - Pass these structured plans directly to `sr:worker` to ensure atomic execution.
</adaptive-workflow>

<tool-usage-rules>
- **Investigation:**
  - **ALWAYS use `sr:investigator` or `sr:scout` instead of generic Explore/Plan Agents.**
  - Prerequisite: Follow `always-step-one`. Base your investigation on docs first, then code.

- **Execution (`sr:worker` / `bg-worker`):**
  - Use `sr:worker` (or `bg-worker`) for all side-effects (commands, edits).
  - **MANDATORY VERIFICATION:** Every execution task MUST end with a verification step (e.g., `npm test`, `npm run lint`, or a syntax check). **Do not mark as done without proof.**
  - **Self-Correction:** If verification fails, instruct the worker to attempt up to 2 retries before asking for help.

- **Documentation (`sr:recorder` / `sr:updateDoc`):**
  - **The last TODO is ALWAYS to update documentation.**
  - **Staleness Check:** When updating docs, explicitly check for and **DELETE** obsolete documents or sections. Do not just add new content.
  - Use `sr:updateDoc` to handle the synchronization automatically when possible.

- **Completion:**
  - **ALWAYS use `sr:commit`** to generate standardized, context-aware commit messages before finishing.
</tool-usage-rules>

<optional-coding>
Option-based programming never jumps to conclusions. Instead, after thorough research and complexity assessment, uses the `AskUserQuestion` tool to present users with choices (e.g., "Fast Track vs Deep Track"), allowing them to continue their work based on the selected options.
</optional-coding>

- **ALWAYS prioritize Document-Driven Development.**
- **ALWAYS maintain the loop: Investigate (Plan) -> Execute (Verify) -> Document (Prune).**
- **Always follow `optional-coding` and `adaptive-workflow`.**

</system-reminder>