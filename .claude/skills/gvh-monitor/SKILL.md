---
name: gvh-monitor
description: Monitors new publications of the Hungarian Competition Authority (Gazdasági Versenyhivatal, GVH) on gvh.hu — decisions, merger notifications, sector inquiries, market analyses, court decisions, notices, guidance and press releases — for relevance to a Hungarian FMCG subsidiary, and emails a report only when a new relevant item appears. Use when asked to review or monitor GVH publications, or when the GVH Routine fires.
---

# GVH monitor

The canonical, self-contained instructions live in `prompts/gvh-run.md` (Hungarian,
below the horizontal rule). Follow them exactly; this file summarizes the design.

## Design
- **Source:** the official gvh.hu only. Pages are fetched with `curl` (WebFetch is refused
  by the environment's egress policy); decision PDFs come from the decision page's
  `/pfile/file?path=…&inline=true` link and are read with `pdftotext`. Legal Data Hunter
  is optional supplementary context only — on the free plan its daily quota runs out, and
  its GVH coverage and freshness are unverified.
- **State:** Google Drive document "GVH figyelő — feldolgozott tételek" lists every item
  already reviewed (keyed by case number, or URL where there is none). The first run
  records the current listing as a baseline and assesses only the last 7 days.
- **Relevance:** the business profile (adhesives, sealants and coatings; laundry, home care,
  hair and body care, professional hair) and its channels drive the standard: cartels and
  vertical restraints, abuse of dominance, retailer–supplier relations, unfair commercial
  practices (green, efficacy, pricing and influencer claims), mergers in the product,
  distribution and supply markets, sector inquiries, court rulings, guidance and
  consultations, press releases and dawn raids in those sectors.
- **Output:** email only when at least one new item is relevant; one message to
  denes.grossman@henkel.com, subject `GVH-figyelő — <date> (…)`. Same gold light/dark
  design, three per-item fields (Joghatás, Relevancia, Állapot), screening ledger with
  re-categorization buttons.
- **Feedback:** mailto buttons send `[GVH] <report date> G<nn> <old>-<new>` to
  dennisrobertgrossman@gmail.com; each run logs them to "GVH kalibrációs napló", labels
  them `GVH-feldolgozott`, and treats them as precedent. The report subject never starts
  with `[GVH]`, so reports are not mistaken for feedback.
- **Company name:** the prompt names the group only so the run can recognize it among
  published party names; the report never names it in its own words, but reproduces
  official party names verbatim.
