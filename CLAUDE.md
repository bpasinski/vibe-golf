# Claude

This is a vibe coding example repository.

Follow this strictly:

- **Do not create, edit, or delete any file until the user's latest message clearly authorizes the specific change you are about to make.** Authorization is a direct instruction to act now — to write, change, or remove something concrete. It is NOT: a question ("would X work?", "is this better?"), a critique or correction of a proposal, a request to compare or weigh options, or general discussion — none of these grant permission, no matter how positive. Agreeing that an idea is good is not the same as asking for it to be done. When the intent for you to act *now* is anything less than unambiguous, stay in proposal mode and ask: "Apply this?" This gate overrides Auto Mode and any "execute immediately" directive.
- never make architectural decisions alone (package boundaries, dependency direction, where code lives), propose best fit and ask to choose from options
- **NEVER run `git commit`, `git commit --amend`, `git push`, or any other command that creates, rewrites, or publishes a commit — not even to make a check pass or "fix" state.**
- implementation plans should live in the package they affect the most, eg. `<app-or-package>/plans/some-plan.md`.
- implementation plans must not have any pending decisions to make.
- never add underscores to unused function arguments
- never look at commit history
- never use git stash
- never fix linting problems, and never mention it
- keep code comments very concise and add them only when necessary; prefer self-documenting code and module documentation
- if you cannot implement a feature in a way it was established or planned to be implemented, propose a new approach and ask for approval before implementing it
- never implement logic that swallows errors
- never use `cat` nor `sed` to read files — read them directly
- never use `AskUserQuestion` tool, ask questions directly
- never modify files in `dist` directories
- UI primitives must use `radix-ui`; if a needed component isn't installed yet, install it via shadcn before rolling your own
- Icons must come from `lucide-react` (already a dep). No inline `<svg>` paths.
- keep your communication very concise and to the point; avoid unnecessary preambles, or apologies. Focus on the task at hand and the specific changes being made. If you need to explain a complex decision, do so.

## Keep CLAUDE.md up to date

Sync this file after any significant architectural change.
