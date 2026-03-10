---
name: commit-but
description: Commit staged changes with GitButler CLI, push, and create a GitHub PR. Uses concise commit messages (feat/fix/chore prefix).
user_invocable: true
---

This skill commits staged code using GitButler's CLI (`but`), pushes, and creates a GitHub PR.

## Steps

1. Run `but status` to see workspace state, then `but diff` to see uncommitted changes. If nothing is changed, tell the user and stop.

2. Analyze the staged diff. Write a concise commit message with a prefix (feat/fix/chore). Examples:
   - `feat: send email to agents on lead view`
   - `fix: messenger height max 500px`
   - `chore: shorter qualification text`

3. Generate a short branch name starting with `obed/`. Do NOT include feat/fix/chore in the branch name. Keep it 2-4 words max, kebab-case. Examples:
   - `obed/email-agents-lead-view`
   - `obed/messenger-height`
   - `obed/qualification-text`

4. Create the branch and commit using GitButler CLI:
   ```
   but branch new <branch-name>
   but commit <branch-name> -m "<commit message>"
   ```
   If changes aren't already staged to this branch, use `but stage <file> <branch-name>` first.

5. Push the branch:
   ```
   but push <branch-name>
   ```

6. Create a GitHub PR using `gh`:
   ```
   gh pr create --head <branch-name> --base dev --title "<commit message>" --body ""
   ```
   Only add a body/description if strictly necessary (e.g. breaking change, non-obvious context). Default to empty body.

7. Return the PR URL to the user.

8. Run a code review on the PR using the Skill tool with `skill: "review"` and pass the PR URL as args. Only report issues and potential bugs — no overview, no summaries, no praise. If no issues found, say so in one line.

## Important
- Do NOT use git to create branches. Always use `but branch new`.
- Do NOT add Claude as co-author.
- Keep everything concise — message, branch name, PR title, PR body.
- The PR base branch is `dev`.
