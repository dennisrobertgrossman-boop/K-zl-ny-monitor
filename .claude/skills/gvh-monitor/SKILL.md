---
name: gvh-monitor
description: Monitors new publications of the Hungarian Competition Authority (Gazdasági Versenyhivatal, GVH) on gvh.hu — decisions, merger notifications, sector inquiries, market analyses, court decisions, notices, guidance and press releases — for relevance to a Hungarian FMCG (fast-moving consumer goods) subsidiary, and emails a report only when a new relevant item appears. Use when asked to review or monitor GVH publications, or when the GVH Routine fires.
---

# GVH monitor

The canonical, self-contained instructions live in `prompts/gvh-run.md` (Hungarian,
below the horizontal rule). Follow them exactly; this file summarizes the design.

## Design
- **Source:** the official gvh.hu only. Pages are fetched with `curl` (WebFetch is refused
  by the environment's egress policy); decision PDF (Portable Document Format) files come from the decision page's
  `/pfile/file?path=…&inline=true` link and are read with `pdftotext`. Legal Data Hunter
  is optional supplementary context only — on the free plan its daily quota runs out, and
  its GVH coverage and freshness are unverified.
- **Pagination:** list pages paginate via `_pageNumber/<n>` links; each run pages back
  until it reaches an already-processed item or one published before the previous run, so
  a gap (holiday, outage) cannot push items off the first page unseen. Press releases are
  also read from the yearly subpage. New proceedings are announced as press releases; the
  old "induló eljárások" section has not been updated since 2016.
- **State:** every Google Drive document whose title starts with "GVH figyelő — feldolgozott
  tételek" (the original plus any supplements) lists items
  already reviewed; runs append with the Google Docs connector (keyed by case number, or by URL — Uniform Resource Locator — where there is none). The first run
  records the current listing as a baseline and assesses only the last 7 days.
- **Relevance:** the business profile (adhesives, sealants and coatings; laundry, home care,
  hair and body care, professional hair) and its channels drive the standard: cartels and
  vertical restraints, abuse of dominance, retailer–supplier relations, unfair commercial
  practices (green, efficacy, pricing and influencer claims), mergers in the product,
  distribution and supply markets, sector inquiries, court rulings, guidance and
  consultations, press releases and dawn raids in those sectors.
- **Output:** email only when at least one new item is relevant; one message to
  denes.grossman@henkel.com, subject `GVH-figyelő — <date> (…)`. The report opens with a
  header band, a one-line "Azonnali jogi teendő: Igen/Nem" (immediate legal action
  needed: yes or no) box and counter tiles; an overview list follows when there are at
  least three relevant items.
- **Item cards:** each relevant item gets a card with the same parts in the same order —
  a category band in the category colour ("<KATEGÓRIA> RELEVANCIA · G<nn> · <type>"), the
  official title with case number and dates below it, ÖSSZEFOGLALÓ (summary, facts only),
  MIÉRT RELEVÁNS (SAJÁT ÉRTÉKELÉS) (why it is relevant — own assessment), and a RÉSZLETEK
  (details) table: Joghatás (legal effect), Állapot (status), Érintettek (undertakings
  concerned), Forrás (source), Idézet (quotation).
- **Colour and layout:** the same gold light/dark design as the Közlöny monitor. Every
  background colour sits on a `<td>` as both a `bgcolor` attribute and inline
  `background-color`, never on `<span>`, `<a>`, `<div>` or `<p>`: in the sent copies of
  both reports of 2026-09-29 the badges and buttons carried white text with no
  background colour at all. A check script in the prompt flags any such tag before
  sending.
- **Screening ledger:** four columns — Az., Tétel (title, with type, case number and date
  below), Eredmény (category or exclusion reason), Átsorolás (three re-categorization
  buttons for the other categories).
- **Feedback:** mailto buttons send `[GVH] <report date> G<nn> <old>-<new>` to
  dennisrobertgrossman@gmail.com; each run logs them to "GVH kalibrációs napló", labels
  them `GVH-feldolgozott`, and treats them as precedent. The report subject never starts
  with `[GVH]`, so reports are not mistaken for feedback. Feedback goes to the Gmail
  address because that is the only mailbox the Routine can read.
- **Company name:** the prompt names the group only so the run can recognize it among
  published party names; the report never names it in its own words, but reproduces
  official party names verbatim.
