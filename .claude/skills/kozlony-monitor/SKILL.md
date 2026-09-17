---
name: kozlony-monitor
description: Daily legal triage of the newest Magyar Közlöny issue for the in-house legal team of a Hungarian FMCG (fast-moving consumer goods) subsidiary. Reads the full text of the official PDF in place (no download, no annotated copy) and produces a page-referenced, prioritised written report. Use when asked to review, screen, or monitor Magyar Közlöny, or when the daily Közlöny Routine fires.
---

# Magyar Közlöny daily review — FMCG subsidiary legal team

## Purpose

Find the newest official **Magyar Közlöny** issue, read its entire text, and turn it
into a practical legal-review package for the in-house legal team of **a Hungarian
subsidiary of a multinational FMCG (fast-moving consumer goods) group**. The company's
name is deliberately not used anywhere in the report or the email that carries it.

## Output mode (read-only review)

This agent does **not** download the PDF and does **not** produce an annotated copy.
It reads the full text of the official PDF in place and delivers a written report.

**Deliverable:** one page-referenced review report, containing:

1. The verified identification of the issue (number, publication date) and the direct
   link to the official PDF on the official Magyar Közlöny website.
2. The top-of-report briefing (Section 3 below).
3. The per-item findings, each anchored to the page and section where the underlying
   text appears in the official PDF (Section 4 below).

**The report is written in Hungarian.** Every part of it — headings, summaries,
assessments, owner names, priority labels, the method and limitations section — is in
Hungarian. The chat response that accompanies it is written in the language the user is
using in the conversation.

Publish the report as an HTML artifact when the Artifact tool is available in the
session, and also give the full report in the chat response. Use **Segoe UI** as the
font family in any HTML output. If the Artifact tool is unavailable, the chat response
alone is the deliverable — say so.

**Every report is emailed to denes.grossman@henkel.com.** Use a Gmail or other email
tool where one is available in the run.

- Subject: `Magyar Közlöny <év>. évi <szám>. szám — napi jogi átvilágítás (<n> magas /
  <n> közepes / <n> alacsony)`
- Body: the full report as a self-contained HTML body (`htmlBody`), inline styles on
  every element, Segoe UI as the font family. No external stylesheet and no CSS
  variables. Use `<table>` layout rather than flex or grid. Supply a plain-text `body`
  alternative too. The one permitted `<style>` block is the dark-mode override block
  described below — Outlook's desktop renderer strips it and falls back to the inline
  (light) styles, which is the intended behaviour there.
- **Visual design — warm gold theme, light and dark.** Structure the email as a full
  HTML document (`<html><head>…</head><body>…</body></html>`), not a bare fragment, so
  the head can carry the meta tags below. Apply the same palette to the HTML artifact
  and the email. Give every themed element both an inline style (the light default,
  read by every client including Outlook) and a class name (read only by clients that
  support the dark override):
  - In `<head>`: `<meta name="color-scheme" content="light dark">` and
    `<meta name="supported-color-schemes" content="light dark">`, plus one `<style>`
    block containing only:
    ```
    @media (prefers-color-scheme: dark) {
      .bg-page   { background-color: #15110B !important; }
      .bg-card   { background-color: #241C12 !important; border-color: #B8963E !important; }
      .text-body { color: #EDE3CC !important; }
      .text-head { color: #E9C46A !important; }
      .header-band  { background-color: #241C12 !important; }
      .header-title { color: #E9C46A !important; }
      .tile-bg   { background-color: #241C12 !important; }
      .tile-label{ color: #EDE3CC !important; }
    }
    ```
    No CSS variables inside it — only these class rules, guarded by the media query and
    `!important` so they can override the inline light styles where the client honours
    the query, and are otherwise inert.
  - Light values (the inline defaults, also what Outlook always shows): header band
    `#1F1710` background with `#F0D999` title text, bold; page/body background
    `#FFFFFF` or `#FFFDF8` with `#2B2118` body text; card and table borders solid
    1–2px `#D4B96A`; section headings and links `#A67C00`, bold; counter tiles on a
    white or cream tile with a `#D4B96A` top border, label `#2B2118`.
  - Dark values (applied only via the override block above, on top of the same
    structure): page/body background `#15110B`, body text `#EDE3CC`; card background
    `#241C12` with `#B8963E` border; header band background `#241C12` with `#E9C46A`
    title text; headings and links `#E9C46A`; counter tiles on `#241C12` with
    `#EDE3CC` label text.
  - Priority badges keep the same solid fill and white bold text in both themes — they
    carry their own background, so the surrounding theme does not affect their
    contrast: **Magas** `#8B1E1E`, **Közepes** `#A67C00`, **Alacsony** `#6B5A2E`,
    **Kizárva** (screening-ledger only) `#5B5346`.
  - Never rely on a pale tint with dark text for anything — that is what read as
    washed out in Outlook's light theme. Avoid rounded corners, gradients, and
    box-shadows as anything meaning depends on; Outlook's desktop renderer handles
    them unpredictably. Plain rectangles and solid fills are the safe default in
    either theme.
- Pass the HTML **directly, in full, as the `htmlBody` parameter**. Never reference it by
  file path and never use shell substitution such as `$(cat body.html)`: the tool's
  parameter is not a shell, the substitution does not run, and the recipient receives the
  literal `$(cat ...)` text. If the report was written to a file first, read it back and
  paste the content into the parameter.
- One report, one email. Verify the body is the finished HTML before sending. If a broken
  message did go out, send the correction as a reply in the same thread
  (`replyThreadId`), not as a new thread.
- Never send the artifact link as the deliverable: the artifact is private and will not
  open for an external recipient.
- If no email tool is available in the run, say so plainly at the top of the chat
  response and name what is missing. Never skip the email silently.

Never present the report as the official publication, and never restate it as if it
were the text of the issue. Label it: **"Belső munkapéldány — az észrevételek nem
részei a hivatalos közzétételnek."** ("Internal working copy — annotations are not
part of the official publication.")

## Relevance standard

Treat an item as relevant only when it may plausibly affect the company, its
employees, products, contracts, operations, management, compliance duties, or
group reporting, including:

- Corporate governance, registrations, reporting, and intra-group arrangements.
- Employment, benefits, workplace safety, immigration, payroll obligations, and
  workforce policies.
- Commercial contracts, procurement, distribution, payment terms, competition, and
  consumer-facing practices.
- Product compliance, chemicals, product safety, labelling, advertising, market
  surveillance, and recalls.
- Environmental permits, waste, packaging, extended producer responsibility,
  sustainability, energy, and emissions.
- Data protection, cybersecurity, digital services, artificial intelligence, records,
  and regulatory reporting.
- Tax, customs, sanctions, trade controls, real estate, disputes, administrative
  procedure, and enforcement.
- Hungarian implementation of European Union law requiring coordination with regional
  or group functions abroad.

Exclude ceremonial, individual appointment, local-only, and public-sector-only items
unless they create a credible business impact.

## Step 1 — Locate and verify the newest issue

**Goal:** select the correct official publication.

1. Open the official Magyar Közlöny website (https://magyarkozlony.hu/) from the
   configured knowledge source.
2. Identify the most recently published **Magyar Közlöny** issue — not a different
   official gazette (such as Hivatalos Értesítő) and not a supplement.
3. Verify its issue number, publication date, title, and direct official PDF link.
4. Do **not** download or store the PDF. Record the direct official link.
5. Compare against the previous run's covered issue where that is known. If more than
   one issue was published since the previous run, review each of them, newest first,
   and report them in one package.

Continue only after the issue and the official PDF link are verified. If verification
fails, follow Error handling.

## Step 2 — Read the complete text of the PDF

**Goal:** identify every provision that meets the relevance standard.

1. Read the full text of the PDF at the official link, including annexes, tables,
   transitional rules, and entry-into-force provisions. Read it in place — stream the
   text; do not save a copy of the file.

   What works in this environment: `curl` reaches magyarkozlony.hu once the domain is
   allowed, but **WebFetch does not** — it is refused as `EGRESS_BLOCKED` because it
   fetches through a different path that does not consult the environment's allowlist.
   Stream the issue straight into a text extractor instead:

   ```
   curl -sSL "<official PDF link>" | pdftotext -layout - issue.txt
   ```

   `pdftotext` comes from `poppler-utils`; install it with `apt-get update && apt-get
   install -y poppler-utils` if the command is missing. Extract the homepage listing the
   same way (`curl -sSL https://magyarkozlony.hu/`) rather than with WebFetch.
2. Keep track of the page number of every passage you rely on, so each finding can be
   cited back to the issue.
3. For each potentially relevant legal act or provision, verify its official title,
   identifier, affected legislation, effective date, deadlines, and transitional rules.
4. Separate confirmed legal text from interpretation. Quote or closely paraphrase the
   operative text, then keep your assessment in a clearly marked separate field.
5. Rate each item:
   - **High** — likely action, deadline, material exposure, or immediate escalation.
   - **Medium** — assessment, monitoring, or business-owner confirmation is needed.
   - **Low** — remote or contextual relevance worth recording.
If the whole text cannot be read in one pass, read it in ordered segments and confirm
in the report that the entire issue was covered, page range by page range.

## Step 3 — Top-of-report briefing

**Goal:** make the most important findings visible immediately.

Put this at the very beginning of the report:

- Issue number, publication date, and the official source link.
- A clear statement on whether immediate legal attention is needed.
- A ranked list of relevant items, highest priority first.
- For each item: legal act, topic, why it matters to the company, effective date, and
  page reference.
- A short **Watchlist** section (Megfigyelési lista) for uncertain or lower-priority
  items.
- A screening ledger listing every item in the issue with its outcome.

Do not include likely owners, deadlines, recommended next actions, dependency notes, or
a group-coordination section. The report states what the law says and when it takes
effect; deciding who acts on it is the reader's.

Use the priority colours from the visual design section consistently: solid `#8B1E1E`
for **Magas** (High), `#A67C00` for **Közepes** (Medium), `#6B5A2E` for **Alacsony**
(Low). In HTML output use colour plus a text label; in plain text use the text label
alone. Meaning must never depend on colour alone.

## Step 4 — Per-item findings anchored to the text

**Goal:** put the explanation next to the provision it explains, by reference.

For each relevant item, in priority order, give:

1. **Anchor** — page number (and where useful, section/§ number and the opening words
   of the passage) of the first substantive appearance of the provision in the official
   PDF. Cite the narrowest passage that supports the finding, not a whole page.
2. **Quoted operative text** — the confirmed legal text, verbatim in Hungarian.
3. **Joghatás** — the legal effect in plain language.
4. **Relevancia** — specific relevance to the company.
5. **Hatálybalépés** — the effective date.
6. **Priority.**

These are the only per-item fields. For amendments, explain the legal effect rather than
merely repeating the amending text. Preserve official Hungarian titles and identifiers
exactly.

## Step 5 — Validate and deliver

1. Confirm the issue reviewed is the newest Magyar Közlöny issue and the link points to
   the official PDF.
2. Confirm every briefing item cites the correct page and matches its per-item finding.
3. Confirm every summary is grounded in text actually visible in the issue.
4. Deliver the report (artifact link where available, plus the full report in chat).
5. State in the chat response: the issue reviewed, the number of High / Medium / Low
   items, and any limitations.
6. If no new issue has been published since the previous run, **produce no report** — no
   artifact, no briefing, no ledger, and no email. Reply with one line naming the most
   recent issue and its date, and stop there.

## General guidelines

- Write the report itself in Hungarian, preserving official Hungarian titles and
  identifiers exactly as published. Write the accompanying chat response in the language
  the user is using.
- Use only the official Magyar Közlöny PDF as the primary source for the issue review.
- Prioritise legal accuracy and traceability over visual polish.
- Do not infer business facts that are not known; where applicability turns on a
  business fact this review cannot establish, say so and leave the point open.
- Treat the output as internal legal triage, not formal legal advice.
- Use every technical term strictly within the confines of its legal definition, and
  write out any abbreviation in full at its first use.

## Error handling and limitations

- If the newest issue or the direct official PDF link cannot be verified, do not
  substitute an unofficial copy. Report what failed and ask for the official link.
- If the official website cannot be reached because of the session's network egress
  policy, report the blocked host verbatim, do not route around it, and ask for the
  host to be allowed in the environment's network settings or for the link to be
  supplied.
- If the PDF is a scan or text extraction is unreliable, attempt optical character
  recognition (OCR) and visibly flag every passage that needs manual verification.
- If the full text cannot be read, still report the issue number, date and official
  link, list what was read, and state precisely which pages were not covered.
- Never invent an obligation, deadline, legal effect, page reference, or quotation.
