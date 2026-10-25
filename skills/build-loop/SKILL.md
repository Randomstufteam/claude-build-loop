---
name: build-loop
description: Runs the plan, build, test, review, polish loop one slice at a time using the planner, builder, helper and reviewer agents. Use when the user asks to build or add a feature, or types /build-loop.
---

# Build loop

Work on ONE slice at a time. Never start the next slice until the user confirms the current one works.

## First time only: plan
1. If no `PLAN.md` exists, use the **planner** agent to write one. Ask the user clarifying questions first if the request is vague.
2. `PLAN.md` must list small slices in order, each with a goal and a way to test it. Show it to the user and wait for approval.

## For each slice
1. **Build:** use the **builder** agent (Sonnet) to implement only the current slice, including the UI for it. It reports files changed and anything untested.
2. **Chores:** use the **helper** agent (Haiku) for boilerplate, simple unit tests for the logic, renaming and comments. Never give it design decisions.
3. **Run checks:** build the project and run the tests. If they fail, send the errors back to the builder. Stop after 3 failed attempts and ask the user.
4. **Review:** use the **reviewer** agent (Opus). It fixes small issues directly and lists larger ones for the builder to apply.
5. **User test:** tell the user exactly what to click or try. Ask them to paste screenshots of the UI so the reviewer can polish it based on what it actually looks like. Wait for their answer.
6. **UI polish:** if the user has feedback or screenshots, send them to the reviewer for touch-ups.
7. **Commit:** when the user says the slice works, commit it to git with a clear message, mark the slice done in `PLAN.md`, then ask whether to continue.

## Rules
- Do not skip the user test step. Do not claim something works unless it was run or the user confirmed it.
- Keep each agent's task small and specific. Do not pass the whole conversation to agents.
- If the plan seems wrong mid-build, stop and send it back to the planner.
- When the user wants to add their own feature, treat it as a new slice and run it through the same loop.
