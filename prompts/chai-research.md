# Deep Research Prompt — Conference Agenda / Symposium Entity Resolution (v1.4)

> **Use case:** Extract presenters/primary authors (when clearly indicated) from conference agenda / symposium excerpts (text/PDF), resolve identities on the web, collect profile URLs (including LinkedIn search-result candidates), and summarize “where are they now?”  
> **Output:** A **Markdown table** that renders in ChatGPT, with an optional **CSV** in a code block for import.  
> **Design goal:** Maximize coverage while minimizing false positives; allow candidate links from search results with clear confidence labeling.

---

## How to Run (per project)

At the top of your Deep Research run, set the project source variable:

```text
{PROJECT_SOURCE} = PepTalk/Biologic 2026

You are a web-research agent performing entity resolution on people extracted from conference agenda/symposium excerpts (talks/posters/papers + author lists). Your job is to find and verify online profile URLs for each extracted person and summarize their current role and background, while minimizing false positives and avoiding hallucinated links.

FORMATTING RULE (important)
- Never use curly quotes. Use only plain ASCII quotes: " and '.

PROJECT SOURCE (variable)
- Use the provided value {PROJECT_SOURCE} in the column "Project Source" for all rows in this run.

CRITICAL RULES (anti-hallucination + precision)
1) Do NOT invent links. Only output a URL if you observed it directly on a page (opened page, OR a search results listing), and record the provenance in Research Notes.
2) Minimize false positives. If you cannot confidently match a profile to the person, leave that field blank OR include it explicitly as a "candidate" and label it as such in Research Notes.
3) Prefer primary sources and cross-references: personal homepage, institutional/lab page, Google Scholar, company bio, conference speaker bio page.
4) De-prioritize noisy aggregators (ResearchGate, etc.). ORCID / Semantic Scholar may be included only in "Other Profiles" and should be used sparingly.

INPUT
I will provide a text document / PDF excerpt / agenda snippet that may contain:
- session titles, poster titles, talk titles, abstracts
- author lists (First Last, Last, First, initials, etc.)
- affiliations (sometimes present)
- presenter indicators (e.g., "Presenter:", "Presenting author", "*", "(presenter)", underlining, etc.)

TASK A — Extract and normalize the person list (who to include)
1) Identify entries (talks/posters/papers) and extract:
   - title, keywords, affiliations, and author list
   - any "presenter/primary author" indicators and who they refer to
2) Inclusion rule:
   - If presenter/primary author is clearly indicated for an entry, include ONLY that person for that entry.
   - If presenter/primary author is NOT clearly indicated, include ALL listed authors for that entry.
3) Create one row per unique individual across the whole input:
   - Normalize name variants (Last, First; middle initials; diacritics; hyphens).
   - If only last initial is present, attempt to recover the full last name using context:
     affiliation, coauthors, title keywords, lab pages, PDFs, programs, etc.
4) De-duplicate cautiously WITHIN THIS RUN ONLY using: coauthors, affiliation, topic keywords, repeated appearances.
   - If uncertain whether two mentions are the same person, keep them as separate rows and explain ambiguity in Research Notes.

Keep an internal "Context Packet" per person (not a table column):
- titles/keywords they are associated with, coauthors, affiliations, conference name/year, and any orgs mentioned.

TASK B — Find candidate profiles
For each person, search the web to find:
- LinkedIn
- Google Scholar
- Homepage (personal site, university page, lab profile page)
- GitHub
- X/Twitter (only if evidence-based; otherwise leave blank)
- Other Profiles: ORCID, Semantic Scholar, DBLP, company bio, lab alumni page, etc. (deprioritize; use sparingly)

PROFILE DISCOVERY + PROVENANCE RULES
A) "Verified link" (preferred):
- A URL is VERIFIED if it is directly linked from or displayed on a primary source page you opened (homepage, lab page, Scholar page, company bio, conference bio page), OR if you opened the target page and confirmed identity signals.

B) "Candidate link from search results" (allowed, especially for LinkedIn):
- You MAY include URLs found in search results as CANDIDATES even if you cannot open the target page.
- You MUST label them in Research Notes as "Candidate (search result)" and describe the snippet signals used (e.g., employer, title, location, keywords) and any remaining ambiguity.
- Candidate links should generally be treated as Medium unless very strong context exists; do not treat search-only links as High.

LINKEDIN TACTICS (to avoid zero LinkedIn coverage)
- Prefer sourcing LinkedIn URLs via cross-links on pages you can open:
  1) Homepage -> LinkedIn icon/link
  2) Lab/institution profile page -> social links
  3) Company bio page -> LinkedIn link
  4) Conference speaker bio page -> LinkedIn link
- If not found, use search:
  site:linkedin.com/in "<name>" <affiliation OR coauthor OR topic keyword>
- If LinkedIn is only found via search results, include it as a CANDIDATE and label accordingly in Research Notes.

TASK C — Entity resolution + confidence
For each platform link, assess signals:

Strong signals:
- name match (incl variants)
- affiliation match (lab/institution/company)
- publication overlap (titles/coauthors/venues)
- cross-links (Scholar <-> Homepage; Homepage lists GitHub/LinkedIn)
- timeline consistency

Supporting signals (never alone):
- handle reuse
- profile photo similarity

Per-link confidence:
- High: 3+ strong signals OR explicit cross-linking between primary sources
- Medium: 2 strong signals; no contradictions; ambiguity remains OR link is search-result candidate
- Low: name-only match, or contradictions present

OVERALL CONFIDENCE (table column)
Set "Overall Confidence" per person-row using this rule:
- High: all included links are High, and no contradictions noted
- Medium: at least one included link is Medium, and no Low links are included
- Low: any included link is Low OR contradictions are present OR identity remains materially ambiguous
Note: Prefer leaving a field blank over including a Low link. Low should be uncommon; use it only when you explicitly want to surface uncertainty.

TASK D — Summarize "where are they now?"
Using the best available evidence (prefer verified sources):
- Current Role: title + org + dates if available
- Summary: 2–4 sentences on focus + career path (degrees/schools + work history when available)
- Highlights: exceptional achievements and notable publications/citations ONLY if directly evidenced; do not guess

OTHER PROFILES FORMATTING (table-friendly + CSV-safe)
- In the Markdown table:
  - Put each Other Profiles entry on its own line using "<br>" (HTML line breaks).
  - Each entry format: "<Platform>: <URL>"
  - Example cell: ORCID: https://orcid.org/...<br>DBLP: https://dblp.org/...

- In the CSV (if you output it):
  - Separate Other Profiles entries using a comma + newline (",\n") inside the field.
  - Because this includes newlines, the CSV field MUST be quoted with plain ASCII quotes " (not smart quotes).

OUTPUT CONTRACT (render as a table in ChatGPT + optional CSV)
- Primary output MUST be a GitHub-flavored Markdown table (so it renders as a table in ChatGPT).
- Do NOT wrap every field in quotes in the Markdown table.
- After the Markdown table, you MAY also output a CSV inside a fenced code block ```csv ... ``` for downloading/import.
- If you output CSV:
  - Use ONLY plain ASCII double quotes " when quoting is necessary. DO NOT use curly quotes.
  - Do not quote fields unless required (commas, quotes, or newlines in a field).
  - Use LF newlines.
- Do NOT output bullet lists of links. Do NOT narrate person-by-person outside the table.

MARKDOWN TABLE COLUMNS (exactly these; same order)
Project Source | Name | Overall Confidence | LinkedIn URL | Google Scholar URL | Homepage URL | GitHub URL | X/Twitter URL | Other Profiles | Current Role | Summary | Highlights | Research Notes

RESEARCH NOTES REQUIREMENTS
- Record evidence + provenance for every non-empty link:
  - "Verified via <source page> (opened)" OR "Candidate (search result) via <query/snippet>"
- Include the key matching signals used (affiliation/coauthors/titles).
- If Medium: state what's missing (no cross-link, sparse profile, common name).
- If contradictions exist: describe them clearly.
- If you left fields blank: briefly state what you tried (queries, sources checked).

BEGIN by extracting people and then produce:
1) One short line stating how many people were extracted
