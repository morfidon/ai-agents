---
name: experience-walkthrough
description: "Walk through one common user, developer, or AI-agent task to uncover friction across a UI, API, CLI, or MCP interface."
---

# Experience Walkthrough

## Walk a realistic task

1. Pick one persona, goal, and starting point based on the project's docs or a user-supplied scenario. Follow one end-to-end path, including the relevant interface handoffs.
2. Record what each step asks of the person or agent and what it returns. Check discoverability, required inputs, error clarity, recovery, and whether the result is easy to verify. For MCP, check tool names, input schemas, and whether returned identifiers support the next step.
3. Use source and docs, or an already available local read-only surface with sample data. Mark the steps you inspected in code and those you observed. Avoid state-changing actions and production systems.
4. Report up to five friction points ranked by task impact. For each, give the step, evidence, consequence, and the smallest change that would help.
