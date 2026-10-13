# GitHub Copilot app workshop: facilitator guide

## Workshop at a glance

**Audience:** beginners who have GitHub accounts and Copilot access
**Duration:** 90 minutes, plus a separate 20-minute participant preflight call
**Working artifact:** the existing `samples/book-app-web` React app
**Presentation:** [copilot-app-book-app-workshop.pptx](./copilot-app-book-app-workshop.pptx)
**Learner outcome:** implement and verify a small feature while using the Copilot app to coordinate isolated sessions, reusable skills, a custom agent, and a canvas.

The lesson is about supervising work in the app, not about building a production application from scratch. Keep the feature small and use the canonical Book App in this course repository.

## Before inviting learners

1. Ensure `.github/agents/book-app-reviewer.agent.md` and `.github/skills/book-app-reviewer/SKILL.md` are committed to this repository's default branch before learners open their projects.
2. The repository is public, so learners can clone it with read access. Forking is not required; grant Write access only if learners should push their own branches.
3. Keep `main` protected and have each learner work from their own checkout in a separate session/worktree and branch. Never ask the whole class to edit or push directly to `main`.
4. Confirm the GitHub Copilot app is available to learners and permitted by organization policy.
5. Run the sample's tests and build, then open its browser preview. Do not begin class with an unverified starting point.
6. Open the slide deck and keep this guide available. Present slides 1–5 during the Wednesday preflight call; use slides 6–24 during the workshop.
7. If the Canvas extension or the prepared custom agent is unavailable in the app version learners use, use the fallback activities below rather than losing the implementation and verification time.

## Wednesday preflight call (20 minutes)

Use slides 3–5. The goal is to resolve setup blockers before the live lesson.

| Time | Facilitator action |
| --- | --- |
| 0–3 min | Explain that learners will open their own checkout of this public repo as an app project, change the Book App on a separate branch/worktree, and verify the result. They do not need to fork or use GitHub CLI. |
| 3–8 min | Ask everyone to install and sign in to the GitHub Copilot app. Confirm Copilot access and any organization restrictions. |
| 8–13 min | Confirm every learner can clone/open this repo as a project in the app. Have them create a separate session/worktree from `main`; do not have learners share a working directory or edit `main`. |
| 13–18 min | Confirm Git is available (`git --version`) and Node.js LTS/npm are installed (`node --version`, `npm --version`). In `samples/book-app-web`, install dependencies and run the tests/build. |
| 18–20 min | Ask learners to open the app's Browser preview. Note any setup blockers and arrange help before class. |

### Prerequisite checklist and commands

- GitHub account with Copilot access; organization policy permits the app and access to this repository.
- Git available to the app for repository/worktree operations. Learners usually do not need to install it separately if it is already present.
- Node.js LTS and npm for the app's local preview and validation. The sample supports Node `^20.19.0 || >=22.12.0`; CI uses Node 22. Use Node 22 LTS for a consistent class environment.
- This repository added as a project in the Copilot app; no fork is required.
- A separate local session/worktree for each learner. Learners need Write permission only if they are expected to push their branches; otherwise local work and verification do not require it.
- GitHub CLI is **not** needed.

From the repository root, run in the app's terminal:

```bash
cd samples/book-app-web
npm ci
npm test -- --run
npm run build
npm run dev -- --host 127.0.0.1 --port 5173
```

Confirm the preview at `http://127.0.0.1:5173`. If `npm ci` fails because the lockfile is absent or out of sync, use `npm install` and report the setup issue; don't keep learners blocked on the preflight call.

## Scope and acceptance criteria

The Book App lets a reader search books by title/author, filter by genre and reading status, and see stats for the visible results. Learners add **Clear filters**:

- The control appears only when a search, genre, or reading-status filter is active.
- Selecting it resets all three filters to their defaults.
- The full list and stats for the full list return.
- Existing search, filtering, stats, accessibility, and empty-state behavior remain intact.
- Keep the change small. No new dependencies, backend, database, external API, unrelated redesign, merge, or push.

## 90-minute facilitator run of show

| Time | Slides | What to do |
| --- | --- | --- |
| 0–10 | 6–8, 11–12 | Frame the goal and the GitHub/Copilot/app distinction. Show the project/session list and main session surface, then launch the Book App preview. Demonstrate search, genre/status filters, updated stats, and an empty result; ask what currently takes extra steps. |
| 10–20 | 13–15 | Introduce Clear filters and read the acceptance criteria together. Explain the Describe → Plan → Build → Prove loop; ask learners to improve the prompt by naming the user need, behavior, boundaries, and verification. |
| 20–32 | 16–17 | Create Session A in a new worktree from the default branch and request a plan in Plan mode. Review it against the criteria. Start Session B independently in a second worktree; select the prepared reviewer agent and request a read-only checklist using the existing skill. |
| 32–42 | 9–10, 18 | Mini-teach skills versus agents and the GitHub work loop. Explain sessions versus worktrees, Plan versus Interactive, and where reusable guidance/agents are managed in Customize. Point out that Session B cannot automatically see Session A's uncommitted changes. |
| 42–62 | 16–17 | Return to Session A; approve or revise the plan, switch to Interactive, and implement. Let Session B finish its checklist independently. Coach learners to review proposed changes rather than accept a broad rewrite. |
| 62–78 | 19–21 | Open a Canvas and track the proposal, build, reviewer checklist, and validation. Inspect Session A's diff. Run tests/build and validate behavior in Browser. Mark evidence only after observing it. |
| 78–90 | 22–24 | Compare Session A's actual diff and checks with Session B's checklist. Invite learners to name one useful app feature and one thing they still had to verify themselves. Close with the takeaways. |

The ranges above total 90 minutes. Keep the app demonstration short; the hands-on implementation and verification are the core learner work.

## Learner prompts

### Session A: plan, then build

```text
Add a small Clear filters control to the Book App in samples/book-app-web.

Acceptance criteria:
- Show it only when search, genre, or reading status is not at its default.
- Selecting it resets search, genre, and reading status.
- The full book list and its reading stats return.
- Preserve existing behavior and accessible labels.
- Make no unrelated changes and add no dependencies.

First, inspect the relevant code and propose a short implementation and validation plan. Do not edit files until I approve the plan.
```

After reviewing the plan, approve it only if it covers all three filters and the tests/behavior to check. Then ask the agent to implement the approved plan. If it skipped a case, request a plan revision rather than repairing the feature yourself.

### Session B: read-only checklist

```text
Use the book-app-reviewer skill to inspect the current Book App and prepare a concise, read-only acceptance checklist for the Clear filters feature:
- hidden when no filters are active
- visible when search, genre, or reading status is active
- resets all three filters
- restores the full list and corresponding stats
- preserves existing behavior and accessibility

Do not edit files or run commands. This session is independent and cannot see another worktree's uncommitted changes. State what evidence I should inspect later; do not claim the feature has passed.
```

The checklist is deliberately produced from the baseline rather than from Session A's branch. At the end, bring the checklist to Session A (or read it aloud) and compare it with the actual diff and test/browser evidence. Do not imply that isolated worktrees share uncommitted files.

## Canvas activity

Create or open a simple workshop board in the app's Canvas side panel. Use four items:

1. **Feature proposal** — acceptance criteria, status: ready.
2. **Session A — build** — plan approval and implementation status.
3. **Session B — review checklist** — checklist findings, status: ready.
4. **Verification evidence** — diff inspected, tests, build, browser checks; status reflects what actually ran.

Keep the board small and visible while moving between sessions. Explain that it summarizes task state; it does not merge worktrees or prove that the code is correct.

## Verification checklist

From Session A's `samples/book-app-web` directory:

```bash
npm test -- --run
npm run build
```

In the Browser preview, check each of the following:

1. With no active filters, Clear filters is absent and the complete list/stats are shown.
2. Enter a search term; Clear filters appears.
3. Select a genre; Clear filters appears.
4. Select a reading status; Clear filters appears.
5. With more than one filter active, selecting Clear filters resets all three, restores the full list, and updates stats.
6. Review the diff for scope and accessible control text. Confirm the existing empty-state and filter behavior remain intact.

If tests, build, or preview do not run, record them as **not run/blocked**, not as passed. Use the Canvas to make the distinction visible.

## Facilitation and recovery

- **Git/worktree setup fails:** keep learners in their current project session and have them observe the instructor demo; do not switch branches or use shared files to simulate isolation.
- **Custom agent is missing:** verify its profile is on the repository's default branch and reload/reopen the project. If still unavailable, use the default agent with the same read-only prompt; explain that the distinct agent role is prepared configuration, not magic.
- **Skill is not offered automatically:** explicitly mention/use the `book-app-reviewer` skill in the prompt. A skill provides reusable task instructions; it does not itself enforce a read-only permission boundary.
- **Canvas unavailable:** make a four-row status board in a plain-text note or on a shared whiteboard, and continue the exercise. Do not claim that this fallback is the Copilot Canvas.
- **Implementation takes longer:** preserve at least 10 minutes for diff review and one verification path. Use the prepared reviewer checklist and instructor's tested baseline as support, but distinguish demonstrated checks from the learner's own.
- **A learner's feature does not work:** ask them to reproduce one acceptance criterion, inspect the exact diff, and revise the prompt/plan. The lesson is about the work loop, not racing to a perfect demo.

## Materials in this repository

- `samples/book-app-web`: working, beginner-sized frontend artifact.
- `.github/skills/book-app-reviewer/SKILL.md`: reusable review guidance.
- `.github/agents/book-app-reviewer.agent.md`: prepared read-only reviewer role for the parallel session.
