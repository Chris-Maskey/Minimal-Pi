---
name: generate-commit
description: "Generates industry-standard conventional commit messages from staged git changes by reading the full diff content. Use this whenever the user wants you to 'generate a commit message', 'write a commit message', 'what should I commit this as', 'create a commit message for these changes', or 'review my staged changes and suggest a message'. IMPORTANT: this skill ONLY generates the message — it never stages files, never runs git commit, and never modifies the repository in any way. Its entire purpose is to analyze staged changes and output a suggested commit message for the user to review and optionally use."
---

## Purpose

Generate a conventional commit message from the user's staged changes (`git diff --cached`). This is a read-only operation — the skill analyzes changes and outputs a suggested message. It **never stages, commits, or modifies** anything in the repository.

## Workflow

### Step 1: Check for staged changes

Run:
```bash
git diff --cached --stat
```

If no staged changes are found, tell the user and stop. Suggest they stage files with `git add` first.

### Step 2: Read the full diff

Run:
```bash
git diff --cached
```

This gives you the complete picture of what changed. Always read the full diff — don't guess or summarize from the stat alone.

### Step 3: Analyze the changes

Analyze the diff to determine each component of the commit message:

**Type** — What kind of change is this?
- `feat` — A new feature
- `fix` — A bug fix
- `docs` — Documentation only changes
- `style` — Changes that don't affect meaning (formatting, whitespace, etc.)
- `refactor` — Code change that neither fixes a bug nor adds a feature
- `perf` — A code change that improves performance
- `test` — Adding or correcting tests
- `build` — Changes affecting the build system or external dependencies
- `ci` — Changes to CI configuration files and scripts
- `chore` — Other changes that don't modify src or test files
- `revert` — Reverts a previous commit

**Scope** — What area/module does it affect? Optional, inferred from the file paths and structure. Use a concise, single-word identifier. Omit if the change is broad or spans multiple unrelated areas.

**Description** — A short, imperative summary of the change, under 50 characters. This is the most important line — it should clearly convey what the change does. Use the imperative mood ("add", "fix", "update" — not "added", "fixed", "updating"). No period at the end.

**Body** — Optional, but recommended for non-trivial changes. Explain the **why** behind the change, not just the **what**. Wrap lines at 72 characters. Separate from the subject line with a blank line.

**Footer** — Optional. Use for:
- Breaking changes: `BREAKING CHANGE: <description>` (also add `!` after type/scope)
- Issue references: `Closes #123`, `Refs #456`
- Co-authored-by trailers

### Step 4: Output the message

Display the generated message clearly in the terminal. The output should be the commit message itself — clean, copyable, and ready to use.

Example for a regular change:

```
feat(auth): add JWT-based user authentication

Add JSON Web Token authentication to the login flow. Tokens are generated on
successful login and stored in httpOnly cookies for security. The auth middleware
now validates tokens on protected routes.

Closes #142
```

Example for a breaking change:

```
feat(api)!: change user endpoint to return paginated results

BREAKING CHANGE: The /api/users endpoint now returns a paginated response object
instead of a flat array. Consumers must update to handle the new response format.
```

## Key rules

1. **Never commit. Never stage.** Do not run `git commit`, `git add`, or any command that modifies the repository state. You are generating a suggestion, not executing it.
2. **Always read the full diff.** Don't infer from file names alone. Read the actual code/content changes for an accurate message.
3. **Imperative mood.** The description must use imperative tense: "add", "fix", "update", "remove" — never "added", "fixed", "updating", "adds".
4. **Description lowercase.** The description starts with a lowercase letter after the colon and space.
5. **No trailing period** on the description line.
6. **Description ≤ 50 characters.** Keep the subject line tight and meaningful.
7. **Body wrapped at 72 characters.** Keep line lengths reasonable.
8. **Be precise but concise.** A commit message should help a reader understand what changed and why, without unnecessary fluff.

## Error handling

- **No staged changes**: Inform the user no changes are staged. Suggest `git add` to stage files they want to commit.
- **Binary files**: Note that binary files are detected but their content can't be analyzed in detail. Describe what changed based on the file names and paths.
- **Empty diff**: Shouldn't occur with staged changes, but if it somehow does, inform the user.

## Related skills

This skill complements the `conventional-commit` skill — that one covers the full workflow of generating AND committing. This skill is specifically for the read-only case: generate a message for review without taking any action.
