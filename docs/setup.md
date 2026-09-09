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

- Email to **denes.grossman@henkel.com** — the primary deliverable. Full report as a
  self-contained HTML body with inline styles; never the artifact link, which is private
  and will not open for an external recipient.
- HTML artifact, font family Segoe UI, where the Artifact tool is available in the run.
- Full report in the chat response of the run's session, always.
- Nothing at all when no new issue has been published since the previous run — no
  report, no artifact, no email, just a one-line note naming the most recent issue.
- The run makes no commits and opens no pull requests.

## Prerequisites

### 1. Network access must allow the official source

By default a cloud environment uses the **Trusted** network access level, which allows
package registries and a fixed list of common domains — and nothing else. The official
Magyar Közlöny website is not on that list, so runs report HTTP 403 from the egress
proxy until the environment is changed to **Custom** with the domains below.

**How to change it** (this is an account setting; a session cannot change it itself):

1. Open https://claude.ai/code.
2. In the row above the message box, click the cloud icon showing the current
   environment's name. There is no settings page or direct link for it.
3. Hover the environment you use for this repository and click the settings (gear) icon
   on the right. The **Update cloud environment** dialog opens.
4. Set **Network access** to **Custom**.
5. In **Allowed domains**, enter one domain per line:

   ```
   magyarkozlony.hu
   *.magyarkozlony.hu
   njt.hu
   *.njt.hu
   ```

   `magyarkozlony.hu` is the official Magyar Közlöny website and the source of the
   official PDF files. `njt.hu` is the Nemzeti Jogszabálytár (National Legislation
   Database), used to verify identifiers and consolidated texts. The `*.` lines cover
   any subdomain a PDF may be served from.
6. Tick **Also include default list of common package managers**, so nothing that
   already works stops working.
7. Save the dialog.

The change applies immediately, including to sessions that are already running — the
policy is enforced per request at the egress proxy, not copied at session start-up.

Reference: https://code.claude.com/docs/en/cloud-environments#allow-specific-domains

If a host is still blocked, the run reports the blocked host and asks for the official
link rather than substituting an unofficial copy.

### 2. Required for email delivery: the Gmail connector on the Routine

A Routine created through the Claude Code Remote tools stores **no connectors**, and this
organization does not allow attaching them programmatically — `create_trigger` rejects the
`connectors` parameter with *"the connectors parameter is not available for this
organization"*. The sessions the Routine starts therefore have no Gmail tool and cannot
send the report anywhere.

To make the daily email work, recreate the Routine from the **claude.ai Routines
interface** with the **Gmail** connector attached, using the prompt in
`prompts/daily-run.md`, then delete the tool-created Routine. Until that is done, each run
reports at the top of its response that it could not send the email.

### 3. Optional: the Lawstronaut connector

Where the Lawstronaut connector is authorised for the account, the agent can use it as a
secondary route to the corpus. It is not a substitute for the official PDF: the official
Magyar Közlöny PDF remains the primary source for the issue review.

## Changing the instructions

Edit `.claude/skills/kozlony-monitor/SKILL.md`. If the change affects what the fresh
session must know without the repository, mirror it in `prompts/daily-run.md` and update
the Routine's prompt.
