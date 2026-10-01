# Közlöny Monitor

Two scheduled legal-triage agents for the in-house legal team of **a Hungarian
subsidiary of a multinational FMCG (fast-moving consumer goods) group**. The company's
name is deliberately not used in the reports or the emails.

- **Magyar Közlöny monitor** — every morning, reviews each new Magyar Közlöny issue.
  It reads the full text of the official PDF (Portable Document Format) **in place** —
  it does not download the file and does not produce an annotated copy — and returns a
  prioritised, page-referenced review report.
- **GVH monitor** — on Tuesday, Wednesday and Thursday mornings, reviews new
  publications of the Hungarian Competition Authority (Gazdasági Versenyhivatal, GVH) on
  gvh.hu, and emails a report only when a new relevant item appears.

## Contents

| Path | What it is |
| --- | --- |
| `prompts/daily-run.md` | The standalone message the Magyar Közlöny Routine sends each morning. |
| `prompts/gvh-run.md` | The standalone message the GVH Routine sends on Tuesday–Thursday mornings. |
| `.claude/skills/kozlony-monitor/SKILL.md` | English reference for the Magyar Közlöny monitor. Invocable in a session as `/kozlony-monitor`. |
| `.claude/skills/gvh-monitor/SKILL.md` | English summary of the GVH monitor. Invocable as `/gvh-monitor`. |
| `docs/setup.md` | Schedules, delivery, and the network and connector prerequisites. |

The prompts are authoritative: they are what the Routines actually run, and they hold
the exact HTML (HyperText Markup Language) building blocks of the email.

## What it produces

One report per new Magyar Közlöny issue, and one per GVH run that finds a relevant
item, in Hungarian:

- A header with the issue (or, for the GVH monitor, the period reviewed) and a link to
  the official source.
- A one-line statement on whether immediate legal action is needed, and counters of
  Magas / Közepes / Alacsony / Kizárva (High / Medium / Low / Excluded) items.
- One card per relevant item, always in the same order: a coloured category band, the
  title, a factual summary, the relevance assessment (marked as the reviewer's own), and
  a details table — legal effect, entry into force or status, source, and a verbatim
  quotation with its page reference.
- A screening ledger covering every item, relevant or not, with buttons that open a
  pre-filled email to re-categorize an item; the next runs treat that feedback as
  precedent.

The report is emailed as the primary deliverable, published as an HTML artifact (font
family: Segoe UI) where the Artifact tool is available, and always given in full in the
chat response of the run.

## What it is not

The report is internal legal triage, not formal legal advice, and it is not the official
publication. Every report carries the label *"Belső munkapéldány — az észrevételek nem
részei a hivatalos közzétételnek."*

## Running it manually

In a session in this repository: `/kozlony-monitor` or `/gvh-monitor`
