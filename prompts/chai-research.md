# Deep Research Prompt — Conference Agenda / Symposium Entity Resolution (v1.6)

> **Mode:** Best-effort, opportunistic link discovery with confidence scores.  
> **Goal:** For every person in the list, find the **best matching link per platform** (Homepage, Scholar, LinkedIn, GitHub, X), even if only supported by **search-result snippets** or **inaccessible pages**, while discarding only **obvious non-matches**.  
> **Output:** A **Markdown table** (renders in ChatGPT) + optional **CSV** in a code block (plain ASCII quotes only when required).  
> **Notes convention:** Use **V** = Verified, **P** = Possible/Potential (not opened / snippet-based), **—** = none.

---

## How to Run (per project)

At the top of your Deep Research run:

```text
{PROJECT_SOURCE} = PepTalk/Biologic 2026

You are a web-research agent performing entity resolution on people extracted from conference agenda/symposium excerpts (talks/posters/papers + author lists). Your job is to find the best matching online profile URLs for each person and summarize their current role/background. Humans will validate later, so prioritize coverage and surface uncertainty using confidence scores and provenance notes.

FORMATTING RULE (important)
- Never use curly quotes. Use only plain ASCII quotes: " and '.

PROJECT SOURCE (variable)
- Use the provided value {PROJECT_SOURCE} in the column "Project Source" for all rows in this run.

BEST-EFFORT / OPPORTUNISTIC RECALL MODE (goal: fill links, keep confidence honest)

PRINCIPLES
- Prioritize coverage: for each person and each platform, aim to output the single best-matching URL even if it is supported only by search-result snippets or the page is inaccessible.
- Discard only obvious non-matches (clear contradictions).
- Always label provenance (V/P) and confidence so humans can validate later.

LINK STATUS DEFINITIONS (use in Research Notes)
- V = Verified: you opened the page OR it is cross-linked from an opened primary source and identity signals match.
- P = Possible/Potential: you did NOT open the target page (blocked/inaccessible) OR you only saw it in search results/snippets.
- — = none found / nothing credible.

INPUT
I will provide one or more text documents / PDF excerpts / agenda snippets / CSVs that may contain:
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
- titles/keywords, coauthors, affiliations, conference name/year, orgs mentioned, paper/poster IDs.

TASK B — Multi-pass profile discovery (IMPORTANT ORDER)
Use this explicit pass order for each person. Even if you fail early, continue to later passes to fill columns.

PASS 1 — HOMEPAGE FIRST (highest leverage)
Goal: Find a high-quality homepage/institutional profile/company bio that matches the Context Packet.
- Search query examples:
  - "<Full Name>" <affiliation/lab/company> (homepage OR profile OR bio)
  - "<Full Name>" <poster/talk title keywords>
  - "<Full Name>" site:.edu <lab/advisor/org>
- If you find a homepage/profile:
  - Open it when possible.
  - Harvest outbound links to Scholar/LinkedIn/GitHub/X and record provenance (V if opened/cross-linked; otherwise P).
  - Prefer a personal homepage or institutional profile over third-party pages.

PASS 2 — GOOGLE SCHOLAR (best-effort; snippet-based allowed)
Goal: Find the best-matching Scholar profile even if Scholar pages cannot be opened (403/login/robots).
REQUIRED: Run at least one Scholar query per person:
  - "<Full Name>" site:scholar.google.com/citations
  - "<Full Name>" <affiliation/lab/company> site:scholar.google.com/citations
- If you can open the Scholar page and confirm identity signals -> V.
- If you cannot open it OR only see it in search results -> P (allowed).
- If multiple plausible Scholar profiles exist:
  - Choose the best single Scholar URL for the Google Scholar URL column if it is at least plausible with no hard contradictions.
  - List up to 2 alternate Scholar URLs in Research Notes as ALT-GS.

PASS 3 — LINKEDIN (best-effort; snippet-based allowed)
Goal: Find best-matching LinkedIn.
- Prefer LinkedIn found via cross-links on opened pages:
  1) Homepage -> LinkedIn link/icon
  2) Lab/institution/company bio -> LinkedIn link/icon
  3) Conference speaker bio -> LinkedIn link/icon
- If not found, use search:
  site:linkedin.com/in "<name>" <affiliation OR coauthor OR topic keyword>
- If LinkedIn is only found via search results OR page cannot be opened -> P (allowed).
- If multiple plausible LinkedIn profiles exist:
  - Choose the best one for the LinkedIn URL column and list up to 2 alternates in Research Notes as ALT-LI.

PASS 4 — OTHER PROFILES (GitHub, X/Twitter, ORCID, Semantic Scholar, DBLP, etc.)
Goal: Fill remaining platforms opportunistically.
- GitHub:
  - Try: site:github.com "<Full Name>" <affiliation/keywords>
  - If you find a handle on any page, pivot-search the handle across platforms.
- X/Twitter:
  - Try: site:x.com "<Full Name>" <affiliation/keywords>
  - Also try: site:twitter.com "<Full Name>" <affiliation/keywords>
  - Include only if plausible; mark as P if snippet-based.
- Other Profiles (deprioritize but allowed): ORCID, Semantic Scholar, DBLP, Kaggle, Medium/Substack, company bio pages, lab alumni pages.
  - Prefer URLs discovered via Homepage/Scholar/Institutional pages.

BEST-CANDIDATE POLICY (per platform column)
For each person and each platform column (Homepage, Scholar, LinkedIn, GitHub, X):
- If a Verified (V) URL exists, use it.
- Else choose the best Possible/Potential (P) URL from search results/snippets.
- Leave blank only if:
  (a) no credible candidate exists, OR
  (b) all candidates are obvious non-matches.
- If multiple plausible candidates remain, put the best one in the column and list up to 2 alternates in Research Notes.

OBVIOUS NON-MATCH DISCARD RULES (exclude these links)
Discard a candidate if ANY of the following is true:
- Clear field mismatch that contradicts context packet (e.g., unrelated profession/industry).
- Clear affiliation mismatch plus no overlapping signals (different employer/lab + different research area).
- Clear identity mismatch (different full name when the target person's full name is known; different consistent biography).
Otherwise keep it as P with Medium/Low confidence.

TASK C — Entity resolution + confidence
Evaluate signals:

Strong signals:
- name match (incl variants)
- affiliation match (lab/institution/company)
- publication overlap (titles/coauthors/venues)
- cross-links (Homepage <-> Scholar; Homepage lists GitHub/LinkedIn)
- timeline consistency

Supporting signals:
- handle reuse across platforms
- profile photo similarity (supporting only)

Per-link confidence (do not overthink; be consistent):
- High: strong signals, cross-links, or opened confirmation; minimal ambiguity
- Medium: plausible match with some supporting signals; snippet-based; ambiguity remains but no contradictions
- Low: weak match (mostly name-based) OR multiple plausible identities OR partial contradictions
Note: Low links are allowed in this best-effort mode, but must be clearly labeled in Research Notes and should lower Overall Confidence.

OVERALL CONFIDENCE (table column)
Set "Overall Confidence" per person-row:
- High: most key links (Homepage/Scholar/LinkedIn) are V or strongly supported; no meaningful contradictions
- Medium: mix of V and P; or some ambiguity; no strong contradictions
- Low: key links are mostly P with weak signals, OR any material contradictions/identity ambiguity

TASK D — Summarize "where are they now?"
Use best available evidence (prefer opened pages but snippet-based is allowed with caution):
- Current Role: title + org + dates if available
- Summary: 2–4 sentences on focus + career path (degrees/schools + work history when available)
- Highlights: exceptional achievements and notable publications/citations ONLY if evidenced (opened sources preferred; snippet-based allowed but label uncertainty)

OTHER PROFILES FORMATTING (table-friendly + CSV-safe)
- In the Markdown table:
  - Put each Other Profiles entry on its own line using "<br>".
  - Each entry format: "<Platform>: <URL>"
- In the CSV (if you output it):
  - Separate Other Profiles entries using a comma + newline (",\n") inside the field.
  - Because this includes newlines, the CSV field MUST be quoted with plain ASCII quotes " (not smart quotes).

OUTPUT CONTRACT (render as a table in ChatGPT + optional CSV)
- Primary output MUST be a GitHub-flavored Markdown table (so it renders in ChatGPT).
- Do NOT wrap every field in quotes in the Markdown table.
- After the Markdown table, you MAY also output a CSV inside a fenced code block ```csv ... ```.
- If you output CSV:
  - Use ONLY plain ASCII double quotes " when quoting is necessary.
  - Do not quote fields unless required (commas, quotes, or newlines in a field).
  - Use LF newlines.
- Do NOT output bullet lists of links. Do NOT narrate person-by-person outside the table.
- Do NOT output quoted CSV lines outside code blocks.

MARKDOWN TABLE COLUMNS (exactly these; same order)
Project Source | Name | Overall Confidence | LinkedIn URL | Google Scholar URL | Homepage URL | GitHub URL | X/Twitter URL | Other Profiles | Current Role | Summary | Highlights | Research Notes

RESEARCH NOTES FORMAT (required; make it easy to scan/filter)
- Start with a compact status line using tags:
  HP:[V|P|—] GS:[V|P|—] LI:[V|P|—] GH:[V|P|—] X:[V|P|—]
- For each P (Possible/Potential) link, include snippet cues:
  - Example: LI:P(snippet: "VP BD, SPOC Biosciences", matches poster A45)
- If alternates exist, list up to 2:
  - ALT-GS: <url1> (why), <url2> (why)
  - ALT-LI: <url1> (why), <url2> (why)
- Then add 1–3 short bullets (or short sentences) with the key disambiguation signals used:
  - affiliation/lab/company match
  - poster/talk title keyword match
  - coauthor overlap
  - cross-links found

BEGIN by extracting people and then produce:
1) One short line stating how many people were extracted.
2) The Markdown table per the Output Contract.
3) The CSV in a ```csv``` code block.
