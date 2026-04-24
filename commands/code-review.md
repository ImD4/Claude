# Code Review

Review the code on the current branch. Read every source file in the project and evaluate it as a senior developer would during a pull request review.

Compare what you find against the coding standards in `.claude/CLAUDE.md`. If no standards file exists, use widely accepted practices for the language and framework.

Read the actual source files — do not guess from function names or file names alone. Focus on findings that would block a merge or cause real problems. Skip formatting issues (those are handled by automated tooling).

## What to evaluate

1. **Correctness** — Does the code do what it claims to? Think about what happens at the boundaries — unusual inputs, empty data, failure conditions. Check that every code path leads somewhere intentional and that loops are guaranteed to terminate.

2. **Readability** — Could someone unfamiliar with the codebase understand this without asking questions? Check naming, structure, and complexity. Flag abbreviations, functions that do too many things, and comments that restate the code instead of explaining why. Cross-reference with `.claude/CLAUDE.md`.

3. **Error handling** — Errors should be propagated to callers who need to know — not silently replaced with default values that hide the failure. Check that error messages are specific enough to diagnose without reading the source.

4. **Scalability** — Will this still work when the data grows 10x? Look for unbounded resource usage — anything that loads an entire dataset into memory, grows without limit, or does repeated expensive work that could be done once.

5. **Consistency** — Does this code follow the same patterns as the rest of the codebase? Flag places where the code breaks conventions without good reason, or where related configuration values contradict each other. Check that types distinguish between "absent" and "empty" — callers should be able to tell whether data is missing or genuinely has no content.

6. **Tests** — Do the tests prove the code works, or just prove it runs? Check whether tests verify actual behaviour and output, whether edge cases are covered, and whether the tests would catch a real regression.

7. **Stale references** — Do comments, documentation, and error messages match the actual code? Check for file names, command names, or package names that have been renamed or removed but are still referenced.

8. **Unnecessary complexity** — Could this be simpler and still work? Flag over-engineering, premature abstractions, and features built for hypothetical future requirements.

9. **Boundaries** — Is input from users or external sources validated where it enters the system? Are internal implementation details leaking into public interfaces?

10. **Security** — Flag unsanitised input in queries or commands, hardcoded secrets, and overly broad permissions. Only flag clear, concrete issues.

11. **Debuggability** — When this breaks in production, can someone figure out what went wrong? Check that error messages carry enough context and that different failure modes are distinguishable.

12. **Cross-module duplication** — Look for duplicated logic across the entire project, not just between files that appear related. Files with different names and different responsibilities can still share implementation patterns that should be extracted. When you find shared logic, check whether a common module already exists that it should live in.

## Output format

Report findings grouped by severity:

- **Critical** — will cause data loss, crashes, or hangs in production. Must fix before merging.
- **High** — will silently produce wrong results or degrade reliability. Should fix before merging.
- **Medium** — inconsistencies, scalability risks, or brittleness that will cause problems as the project grows. Fix if straightforward, otherwise note for follow-up.
- **Low** — stale references, naming issues, minor docs drift. Fix or note for follow-up.

For each finding, cite the file path and line number. State what the problem is and why it matters.

If there are no findings at a given severity level, skip that level. If there are no findings at all, say so. Do not invent issues to fill the report.
