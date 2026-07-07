---
name: plan-review
description: Read-only review of an implementation plan.
argument-hint: Selected file
---

Perform a read-only review of an implementation plan. Do NOT modify any files (including the plan itself).

Determine whether the proposed plan is sound, sustainable long-term, and aligned with project's core goals:

- Beautiful UI/UX
- Type safety
- High performance
- Low memory footprint
- Low bundle size

Focus on:

- architecture and design of the proposed solution
- package boundaries, dependency direction, and where code lives
- encapsulation, modularity, cohesion, and coupling implied by the plan
- scope creep, missing steps, unstated assumptions, and risks
- whether the plan contains redundant text that doesn't add value for implementers

Never mention:

- minor implementation details that don't affect the overall soundness of the plan
- gaps that can be easily filled in during implementation without affecting the overall design

Ground your feedback in specific sections of the plan and, where relevant, specific file paths and line ranges in the codebase the plan affects. Provide actionable recommendations for improvement, and prioritize them based on impact and effort.

**Important**: Find alternative solutions or well known generic patterns that can be applied to the problem at hand, and suggest them if they would be a better fit than the proposed solution.

Provide only important feedback that would significantly improve the plan. Avoid nitpicks or minor style issues unless they have a meaningful impact on readability or maintainability.

Do not summarize what is sound about the plan.

List top 3 real issues in the plan.
Always be sure about the issues you list. If you are not sure whether something is an issue then investigate more.
Keep the list of issues very concise.
Never point to any source code or line numbers in the codebase. Focus on behaviour and design issues in the plan itself, not on implementation details.
