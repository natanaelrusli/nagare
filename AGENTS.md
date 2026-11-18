# AGENTS.md

Home base for agent sessions. Run the agent from this folder.
Shared skills live in `.claude/skills/`; shared docs in `docs/`.

## Starting a session

- Start the agent from this folder, not a parent directory (a parent loads only its root pointer, not these rules).
- Name the target repo in your first message (e.g. "in <repo>, fix X"). The agent reads that repo's `AGENTS.md` first, using the paths under "Other repos".
- For a ticket or issue link, use the `dev-workflow` skill. For MR/PR review, use `mr-review-watch`.
- Work on a branch in the target repo. Push and open MRs/PRs only when asked.

## Workspace rules

After every task that changes files, runs side-effecting commands, or makes a decision (here or in any sub repo, including subagents), follow the `dev-log` skill (`.claude/skills/dev-log/SKILL.md`) and append an entry to `Dev log/YYYY-MM-DD.md`.

## Merging

Every merge is squashed and deletes its remote source branch (e.g. `glab mr merge --squash --remove-source-branch`, or `squash=true` and `should_remove_source_branch=true` via API). Applies to every repo; overrides a per-MR "merge without squash" note.

## Commits and PRs

Applies to every repo and every subagent.

- Never open a PR/MR unless the developer explicitly asked in this session.
- UI changes: attach before and after images. Motion or timing changes: attach a short video.
- Upload PR evidence (images, videos) through the PR description or a comment. Never commit assets that exist only for a PR.
- Commit titles are conventional commits in plain language, e.g. `fix(web): new threads no longer spike CPU`.
- Commit/PR body: the problem in one or two sentences, then how it was fixed. Last line names the model and harness, e.g. `Done by <model> in <harness>.`

## Subagent orchestration

For any task that splits into two or more parts (multi-file changes, multi-repo work, research plus implementation), delegate the parts to subagents instead of doing them inline.

| Role | Does | Model / effort |
|------|------|----------------|
| Orchestrator (main session) | Plans, writes briefs, dispatches, reviews, decides. Never does worker jobs itself. | Strongest available model |
| Worker | Code changes, tests, fixes | Mid-tier model, medium effort |
| Explorer | Read-only exploration of code or docs. Brief must say "read only, change no files". | Fast model, high effort |

Workers follow these workspace rules too, including the dev log.

## Other repos

Keep one line per repo: name, purpose, local path, clone URL, and whether to read its `AGENTS.md` first. Clone if missing.

```
- <name> (<purpose>): <local path>. Clone from <url> if missing. [Read its AGENTS.md first.]
```

Example:

```
- web-app (frontend): ~/code/web-app. Clone from git@example.com:org/web-app.git if missing. Read its AGENTS.md first.
```
