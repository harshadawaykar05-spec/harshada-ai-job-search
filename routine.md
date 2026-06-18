
Search for job opportunities matching the user's profile and preferences.

1. Review the user's resume in `resume.md` at the root of this repository to understand their background, skills, and experience.
2. Search multiple job boards and sources (LinkedIn, AngelList, Crunchbase, company career pages, industry networks) for open roles in these target titles: <Job Title 1>, <Job Title 2>, and <Job Title 3>.
3. Apply the following hard constraints and drop any posting that violates one of them:
   - Location: <location and/or remote requirements>
   - Work authorization: <e.g., US citizen — no visa sponsorship needed>
   - Compensation floor: <minimum acceptable total compensation>
   - Company stage: <e.g., Series B or later, or publicly traded>
4. For each remaining role, assess fit against the resume and likelihood of interview, applying these per-title priorities:
   - <Job Title 1>: prioritize any company stage and quality of fit.
   - <Job Title 2>: prioritize well-known or well-funded companies.
   - <Job Title 3>: strongly prioritize recognizable, established companies.
   Use these soft preferences only to break ties, never to exclude: <soft preferences, e.g., industry focus or team culture>.
5. Compile a list of 3–8 top matches with: company name, role title, key fit factors, and application link. Only include a role if you have observed an actual posting for it (see HARD RULE below). Label each match by verification tier:
   - VERIFIED — you opened/confirmed the live posting yourself.
   - SEEN BUT UNVERIFIED — a real posting reference exists but you couldn't open it (e.g., 403/login); cite where you saw it and report only the details you actually observed.
6. Record and de-duplicate presented roles using a running CSV at `history/presented-roles.csv` with the columns: Title, Company, URL, Posting Date, Original Find Date.
   - Before finalizing the matches from step 5, load this CSV (create it with a header row if it does not exist) and drop any role that already appears in it — match on Company + Title + URL — so a role is presented at most once across runs.
   - After presenting, append one row per newly presented role. Set Original Find Date to today's date (the date the role is first presented); use the posting's stated date for Posting Date, leaving it blank if none is given.
   - Commit the updated CSV to the `history/` folder on the `main` branch. Do NOT create a separate branch.
7. Flag any roles that seem like unusually strong matches (e.g., recent skill alignment, explicit mention of the user's background).

If no promising matches are found, report what you searched and note any gaps. Reporting "no confirmed matches found" is a valid and correct outcome.

HARD RULE — Never fabricate or infer job postings:
- A job may ONLY appear in the matches list if you have observed an actual posting for it: a real listing on a job board, careers page, ATS (Greenhouse/Lever/Ashby), aggregator entry, or hiring post (e.g., HN "Who is Hiring"), with a real role title for that specific company.
- NEVER turn hiring intent into a posting. "The company is hiring across engineering," "the raise will fund engineering hires," or "they're growing the team" are NOT job postings. Do not derive a role from funding news, press releases, or general "they're probably hiring" reasoning.
- NEVER state role details (title, level, location, responsibilities, comp) that you did not actually see in a posting. Do not guess or extrapolate them.
- It IS acceptable to surface a real posting you genuinely found but could not fully open (403/login). Include it under SEEN BUT UNVERIFIED, cite the exact source, and report only what you actually saw.
- Companies with only general hiring signals and no confirmed matching posting may be mentioned separately as "companies to watch" — never as an applyable match, and state explicitly that there is no confirmed open role.
- Honesty over completeness: fewer real matches (or none) is always better than one made-up or assumed role.
