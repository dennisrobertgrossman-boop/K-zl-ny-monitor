# Közlöny Monitor

Daily legal triage of the newest **Magyar Közlöny** issue for the in-house legal team of
**Henkel Magyarország Kft.**

The agent reads the full text of the official Magyar Közlöny PDF **in place** — it does
not download the file and does not produce an annotated copy — and returns a
prioritised, page-referenced review report.

## Contents

| Path | What it is |
| --- | --- |
| `.claude/skills/kozlony-monitor/SKILL.md` | The agent's instructions. Invocable in a session as `/kozlony-monitor`. |
| `prompts/daily-run.md` | The standalone message the scheduled Routine sends each morning. |
| `docs/setup.md` | Schedule, delivery, and the network and connector prerequisites. |

## What it produces

One report per run:

- Issue number, publication date and the direct link to the official PDF.
- A statement on whether immediate legal attention is needed.
- Items ranked High / Medium / Low, each with the legal act, why it matters to Henkel
  Magyarország Kft., effective date, deadline, likely owner, recommended next action and
  a page reference into the official PDF.
- A **Group coordination** section for matters needing alignment with Henkel Germany.
- A **Watchlist** section for uncertain or lower-priority items.

The report is published as an HTML artifact (font family: Segoe UI) where the Artifact
tool is available, and always given in full in the chat response.

## What it is not

The report is internal legal triage, not formal legal advice, and it is not the official
publication. Every report carries the label *"Belső munkapéldány — az észrevételek nem
részei a hivatalos közzétételnek."*

## Running it manually

In a session in this repository: `/kozlony-monitor`
