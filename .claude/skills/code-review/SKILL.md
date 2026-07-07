---
name: code-review
description: Read-only code review for staged changes or current branch.
---

Perform a read-only code review for staged changes or the current branch. Do NOT modify any files.

Determine whether the implementation is sound, sustainable long-term, and aligned with project's core goals:

- Beautiful UI/UX
- Type safety
- High performance
- Low memory footprint
- Low bundle size

Focus on:

- architecture and design of the code
- encapsulation (if a module/class exposes too much of its internal state or implementation, it should be refactored to hide it)
- modularity (if a module has too many responsibilities, it should be split)
- cohesion (if two unrelated things are in the same place, they should be split)
- coupling (if a module/class/function depends on too many other modules/classes/functions, it should be refactored to reduce dependencies)
- dependency inversion violations — flag a class that constructs its own concrete dependencies instead of depending on the abstraction and receiving them (constructor injection / factory).
- check if generic packages are aware of specific packages, eg. 
- check if implementation introduces runtime runtime failures.
- check if a fix is treating the cause of the problem instead of just the symptom, and is not a hacky workaround.
- check if code was added to a file that suffers from a large number of responsibilities and should be split into smaller files.

Ignore any deviations from the implementation plan or spec, as long as the implementation is sound and meets the core goals.

Detect if implementer was struggling with making the implementation natural/simple and decided to hack around.

Ground your feedback in specific file paths and line ranges. Provide actionable recommendations for improvement, and prioritize them based on impact.

Ignore any file changes that are not coherent with the overall feature or fix being implemented and seem out of scope.

Do not comment about what is sound about the implementation, only point out potential issues and areas for improvement.