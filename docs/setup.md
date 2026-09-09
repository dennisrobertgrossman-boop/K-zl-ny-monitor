# Setup

## Schedule

The agent runs as a Claude Routine that starts a **fresh session on each firing** and
sends it the message in `prompts/daily-run.md`.

- Cron: `3 5 * * *` — evaluated in Coordinated Universal Time (UTC).
- That is **07:03 Budapest time during Central European Summer Time** and **06:03
  Budapest time during Central European Time**. Cron schedules are stored in UTC and do
  not follow the Hungarian daylight-saving transition; to keep a fixed local hour all
  year, update the Routine's cron expression at each transition (`3 5 * * *` in summer,
  `3 6 * * *` in winter).
- It runs every day. Magyar Közlöny is normally published on working days; on a day with
  no new issue the agent reports that and stops.

Completion notifications (push and email) are enabled on the Routine, so each morning's
result reaches the owner's phone and inbox.

## Delivery

- HTML artifact, font family Segoe UI, where the Artifact tool is available in the run.
- Full report in the chat response of the run's session, always.
- The run makes no commits and opens no pull requests.

## Prerequisites

### 1. Network egress must allow the official source

The environment's network egress policy must allow:

- `magyarkozlony.hu` — the official Magyar Közlöny website and the official PDF files.

Recommended to allow as well, for verifying identifiers and consolidated texts:

- `njt.hu` — Nemzeti Jogszabálytár (National Legislation Database).

If a host is blocked, the run reports the blocked host and asks for the official link
rather than substituting an unofficial copy. Network policy is set on the environment,
see https://code.claude.com/docs/en/claude-code-on-the-web.

### 2. Optional: the Lawstronaut connector

Where the Lawstronaut connector is authorised for the account, the agent can use it as a
secondary route to the corpus. It is not a substitute for the official PDF: the official
Magyar Közlöny PDF remains the primary source for the issue review.

## Changing the instructions

Edit `.claude/skills/kozlony-monitor/SKILL.md`. If the change affects what the fresh
session must know without the repository, mirror it in `prompts/daily-run.md` and update
the Routine's prompt.
