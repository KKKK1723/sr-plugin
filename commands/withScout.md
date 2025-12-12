---
description: "Handles a task by first assessing complexity, then routing to either Fast Execution or Deep Investigation."
argument-hint: "[A goal or task]"
---

# /withScout

This command handles tasks by first performing a **Complexity Assessment** and then choosing the appropriate workflow: **Fast Execution** (for simple tasks) or **Deep Investigation** (for complex tasks).

## When to use

- **Use when:** The user has a request that is not trivial (requires reading >1 file, understanding system logic, or making changes).
- **Suggest when:** A user's request cannot be fulfilled without first gathering information.

## Actions

This command follows an **Assess → Route → Action** workflow.

### **Step 1. Assess & Route **

1.  **Complexity Assessment (CRITICAL STEP):**
    - **Goal:** Immediately determine the task's complexity **(Low, Medium, or High)**.
    - **Decision:**
        - **If Complexity is Low (Fast Track):** Proceed directly to **Step 3 (Fast Execution)**.
        - **If Complexity is Medium or High (Deep Track):** Proceed to **Step 2 (Deep Investigation)**.

### **Step 2. Deep Investigation - For Medium/High Complexity **

1.  **Deconstruct & Plan:**
    - Assign questions to `investigator` or `scout` agents.
2.  **Parallel Investigation:**
    - Launch agents. **CRITICAL:** Each agent MUST return a report including a structured `<ExecutionPlan>` tag.
3.  **Synthesize & Evaluate:**
    - Merge reports into a master plan.
4.  **Execute:**
    - Proceed to **Step 4 (Deep Execution)**.

### **Step 3. Fast Execution - For Low Complexity **

1.  **Action Phase (Quick Plan):**
    - The `worker` agent creates a brief plan and executes it directly.
    - **Verification:** The worker **MUST** perform a validation check (e.g., `npm run lint`, syntax check) after the change.
    - Skip Step 4.

### **Step 4. Deep Execution - For Medium/High Complexity **

1.  **Action Phase (Execute Deep Plan):**
    - Based on the structured `<ExecutionPlan>` from Step 2, use `worker` agents to execute the request.
    - **Verification:** The worker **MUST** perform a validation check (e.g., `npm test`, build check) after the change.

### **Step 5. Final Report **

1.  **Summarize:** State the complexity assessment, findings, actions taken, and verification results.