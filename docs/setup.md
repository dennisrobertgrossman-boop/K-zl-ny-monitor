# Setup

Both monitors run as Claude Routines created in the claude.ai Routines interface. Each
firing starts a **fresh session** and sends it the Routine's prompt, which is the text
below the horizontal rule in the matching file in `prompts/`.

| Routine | Prompt | Schedule (cron, UTC) | Connectors it needs |
| --- | --- | --- | --- |
| **Közlöny monitor email feladat** | `prompts/daily-run.md` | `30 6 * * *` — every day | Gmail, Google Drive, Google Docs |
| **GVH figyelő email feladat** | `prompts/gvh-run.md` | `0 7 * * 2-4` — Tuesday to Thursday | Gmail, Google Drive, Google Docs, Legal Data Hunter (optional) |

## Schedule and time zone

A cron expression without a prefix is evaluated in Coordinated Universal Time (UTC), so
`0 7` means **09:00 Budapest time during Central European Summer Time** but **08:00
during Central European Time** (and `30 6` means 08:30 and 07:30), which starts on **25 October 2026**. To keep the
Budapest hour all year (08:30 for the Közlöny monitor, 09:00 for the GVH monitor), set
the schedules to:

```
CRON_TZ=Europe/Budapest 30 8 * * *
CRON_TZ=Europe/Budapest 0 9 * * 2-4
```

The scheduler accepts the `CRON_TZ=` prefix; other Routines on this account already use
it. The scheduler may start a run a few minutes after the set minute.

Magyar Közlöny is normally published on working days; on a day with no new issue the
Közlöny run produces nothing and stops after a one-line note. Push notifications are
enabled on both Routines; the reports themselves arrive by email.

## Editing a Routine

A Routine created in the Routines interface can be edited only there: an agent cannot
update or fire it (`update_trigger` and `fire_trigger` refuse it with *"Agents can only
update routines they created"*), and in this organization an agent cannot attach
connectors to a Routine either (`create_trigger` rejects the `connectors` parameter). To
change the instructions:

1. Edit the prompt in `prompts/` — it is authoritative — and mirror the change in the
   matching `SKILL.md`.
2. Paste everything below the horizontal rule into the Routine's prompt field, replacing
   the old text.
3. To test, use **Run now**. The Közlöny run sends nothing when no new issue has
   appeared since its last report, so a test of that Routine needs a temporary test
   paragraph at the top of the prompt, removed afterwards.

## Delivery

- **Közlöny monitor:** one email per new issue to **denes.grossman@henkel.com** and
  **ferenc.sarkozi@henkel.com**, both in To — the primary deliverable. Nothing at all
  when no new issue has been published since the previous run.
- **GVH monitor:** one email to **denes.grossman@henkel.com**, only when at least one new
  relevant item appeared; otherwise nothing is sent.
- The body is the full report as a self-contained HTML (HyperText Markup Language)
  document; never the artifact link, which is private and will not open for an external
  recipient. The runs also publish the report as an HTML artifact where the Artifact tool
  is available, and give it in full in the run's chat response.
- The emails are sent from the Gmail account connected to the Routines,
  dennisrobertgrossman@gmail.com.
- The runs make no commits and open no pull requests.

## Prerequisites

### 1. Network access must allow the official sources

By default a cloud environment uses the **Trusted** network access level, which allows
package registries and a fixed list of common domains — and nothing else. The official
sources are not on that list, so runs get an HTTP (Hypertext Transfer Protocol) 403
refusal from the egress proxy until the environment is changed to **Custom** with the
domains below. Both Routines use the "Közlöny monitor" environment.

**How to change it** (this is an account setting; a session cannot change it itself):

1. Open https://claude.ai/code.
2. In the row above the message box, click the cloud icon showing the current
   environment's name. There is no settings page or direct link for it.
3. Hover the environment and click the settings (gear) icon on the right. The **Update
   cloud environment** dialog opens.
4. Set **Network access** to **Custom**.
5. In **Allowed domains**, enter one domain per line:

   ```
   magyarkozlony.hu
   *.magyarkozlony.hu
   gvh.hu
   *.gvh.hu
   njt.hu
   *.njt.hu
   ```

   `magyarkozlony.hu` is the official Magyar Közlöny website and the source of the
   official PDF (Portable Document Format) files; `gvh.hu` is the official website of
   the Hungarian Competition Authority (Gazdasági Versenyhivatal). The `*.` lines cover
   any subdomain a file may be served from. `njt.hu`, the Nemzeti Jogszabálytár
   (National Legislation Database), is allowed but neither prompt uses it: on 2026-09-29
   it returned an empty reply to requests from this environment, so nothing may depend
   on it.
6. Tick **Also include default list of common package managers**, so nothing that
   already works stops working.
7. Save the dialog.

The change applies immediately, including to sessions that are already running — the
policy is enforced per request at the egress proxy, not copied at session start-up.

Reference: https://code.claude.com/docs/en/cloud-environments#allow-specific-domains

`WebFetch` does not consult this allowlist and is refused even for an allowed domain;
the prompts therefore fetch every page with `curl` and read PDF files with `pdftotext`.
If a host is still blocked, the run reports the blocked host and asks for the official
link rather than substituting an unofficial copy.

### 2. Connectors on the Routines

Attach connectors in the Routines interface (agents cannot, see above). Each Routine
needs only the connectors in the table at the top:

- **Gmail** — to send the report, and to search and label the feedback messages.
- **Google Drive** — to find, read and create the calibration logs and, for the GVH
  monitor, the log of processed items.
- **Google Docs** — to append lines to those logs. The Google Drive connector can only
  create a file or change its title and location, not add text to an existing document:
  on 2026-09-30 the GVH run therefore wrote its lines into a separate "(kiegészítés)"
  document. Without Google Docs the prompts fall back to a new dated document each time,
  and every run reads all documents whose title starts with the log's name.
- **Legal Data Hunter** (GVH monitor only, optional) — supplementary context. On its free
  plan the daily quota runs out ("You've used today's quota on your Free plan" on
  2026-09-29), so it can never be the primary source of a scheduled job.

Any other connector widens what an unattended run can reach without being used by the
prompt; remove it.

### 3. The feedback loop

Readers re-categorize items by clicking a button in the email, which opens a pre-filled
message to dennisrobertgrossman@gmail.com — the only mailbox the Routines can read.
Subject prefixes are `[KV]` for the Közlöny monitor and `[GVH]` for the GVH monitor. Each
run reads unprocessed feedback, logs it to a Google Drive document ("Közlöny kalibrációs
napló" or "GVH kalibrációs napló"), labels the messages `KV-feldolgozott` or
`GVH-feldolgozott`, and uses the log as precedent when rating. A feedback message counts
as new only if its Gmail message identifier (or, for older lines, its date, item and
re-categorization) is not yet in the log, so a missed label cannot log it twice — up to
2026-10-01 no run had created the labels. Feedback is accepted only
from denes.grossman@henkel.com, dennisrobertgrossman@gmail.com and, for the Közlöny
monitor, ferenc.sarkozi@henkel.com.

## Verified

- **Közlöny monitor:** exercised end to end on 2026-09-09 against Magyar Közlöny 2026.
  évi 127. szám — the scheduled Routine reached magyarkozlony.hu, read the full text of
  the official PDF, produced the Hungarian report and emailed it without supervision. A
  second run on the same day correctly produced nothing, because no newer issue had been
  published.
- **GVH monitor:** gvh.hu verified reachable on 2026-09-29, including a decision PDF
  downloaded from its `/pfile/file` link and read with `pdftotext`. The first run on
  2026-09-29 recorded its baseline and emailed the report; a simulated run over
  2026-06-05 to 2026-06-08 caught the 2026-06-05 press release "336 milliós GVH-bírság
  tiltott ármegkötés miatt".
- **Report layout:** both the email samples built from the prompts' building blocks pass
  the prompts' pre-send check and render correctly in light and dark colour schemes.
