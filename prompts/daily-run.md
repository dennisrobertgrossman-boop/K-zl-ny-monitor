# Daily run prompt

This is the exact standalone message the Routine sends into a fresh session each
morning. It is standalone on purpose: a fresh session starts with no conversation
history.

---

Run the daily Magyar Közlöny legal review for the in-house legal team of Henkel
Magyarország Kft.

Read `.claude/skills/kozlony-monitor/SKILL.md` in this repository and follow it exactly.
If that file is not present, follow the instructions inline below.

Key points, so nothing is lost if the file is unavailable:

1. Open the official Magyar Közlöny website (https://magyarkozlony.hu/) and identify the
   newest **Magyar Közlöny** issue — not Hivatalos Értesítő, not a supplement. Verify its
   issue number, publication date and direct official PDF link. Review every issue
   published since the previous morning's run, newest first.
2. **Do not download the PDF and do not produce an annotated copy.** Read the full text
   of the PDF in place — annexes, tables, transitional rules and entry-into-force
   provisions included — and keep track of page numbers.
3. Screen the whole issue against the Henkel relevance standard: corporate governance
   and reporting; employment, payroll, workplace safety and immigration; commercial
   contracts, procurement, distribution, competition and consumer practices; product
   compliance, chemicals, product safety, labelling, advertising, market surveillance
   and recalls; environment, waste, packaging, extended producer responsibility, energy
   and emissions; data protection, cybersecurity, digital services, artificial
   intelligence and regulatory reporting; tax, customs, sanctions, trade controls, real
   estate, disputes, administrative procedure and enforcement; and Hungarian
   implementation of European Union law needing coordination with Henkel Germany.
   Exclude ceremonial, individual appointment, local-only and public-sector-only items
   unless they create a credible business impact.
4. Rate each item Magas / Közepes / Alacsony, and keep confirmed legal text separate from
   your assessment. Do not assign owners.
5. Produce one report: a top briefing (issue number, publication date, official link,
   whether immediate legal attention is needed, ranked items with legal act, topic,
   relevance to Henkel Magyarország Kft., effective date and page reference), then a
   **Megfigyelési lista** (Watchlist) section, a screening ledger listing every item in
   the issue with its outcome, and the per-item findings anchored to page and section
   with the operative text quoted. The only per-item fields are Joghatás, Relevancia and
   Hatálybalépés — no owners, no deadlines, no recommended actions, no dependency notes,
   and no group-coordination section.
6. **Write the report in Hungarian** — headings, summaries, assessments, owner names and
   priority labels (Magas / Közepes / Alacsony) included. Publish it as an HTML artifact
   using **Segoe UI** as the font family, label it "Belső munkapéldány — az észrevételek
   nem részei a hivatalos közzétételnek.", and also give the full report in the chat
   response. Write the chat response itself in the language the user is using. If the
   Artifact tool is unavailable, the chat response alone is the deliverable — say so.
7. End with: the issue reviewed, the count of High / Medium / Low items, and any
   limitations. Never invent an obligation, deadline, legal effect, page reference or
   quotation. If the official site or PDF cannot be reached, report the blocked host and
   ask for the official link — never substitute an unofficial copy.
8. If no new issue has been published since the previous run, say exactly that, name the
   most recent issue and its date, and stop.

Do not commit anything to the repository and do not open a pull request — this run is a
read-only review.
