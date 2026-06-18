# job-hunt-claude-routine-template

A template for a [Claude Code routine](https://code.claude.com/docs/en/routines) — a saved prompt that runs automatically on a schedule — that searches job boards for openings matching your resume and reports back verified matches.

## What the routine does

`routine.md` is the prompt the routine runs on every scheduled execution. It instructs Claude to:

1. Read your resume (from `resume.md`) for background.
2. Search LinkedIn, AngelList, Crunchbase, company career pages, and industry networks for roles matching up to three target job titles.
3. Drop postings that violate your hard constraints (location, work authorization, compensation floor, company stage).
4. Assess fit and interview likelihood for each remaining candidate, using per-title prioritization rules you define (e.g. "prioritize well-funded companies" for one title, "strongly prefer established companies" for another).
5. Compile 3–8 top matches (company, role, fit factors, application link), each labeled **VERIFIED** or **SEEN BUT UNVERIFIED**.
6. Apply a hard rule against fabricating postings — if nothing solid turns up, the routine reports that honestly instead of inventing roles.

Because the routine needs to browse live job postings across many external sites, it must run in a cloud environment with **full internet access** (see step 3 below).

## Files in this repo

- **`routine.md`** — the prompt template. It owns the search direction: which titles to look for, hard constraints, per-title priorities, and the anti-fabrication rules. This is the file you paste into the routine.
- **`routine-example.md`** — a filled-in version of `routine.md` for the example resume, so you can see how the placeholders map to a real candidate. For reference only; you don't paste this one.
- **`resume.md`** — a plain-Markdown example resume the routine reads each run. Replace it with your own.

## 1. Fill in the routine prompt

Open `routine.md` and replace the placeholders with your own search direction:

- `<Job Title 1>`, `<Job Title 2>`, `<Job Title 3>` (steps 2 and 4) — your target roles. Update the prioritization note under each title in step 4 to match what matters to you (company stage, funding/brand recognition, etc.).
- The hard constraints in step 3 — location/remote, work authorization, compensation floor, company stage. Postings that violate these are dropped before ranking.
- The soft preferences in step 4 — used only to break ties (industry focus, team culture, etc.).

`routine-example.md` shows all of these filled in for the example resume if you want a worked reference.

## 2. Provide your resume

### Option A — Resume stored in this repo (default)

`routine.md` step 1 reads `resume.md` from the root of this repo for the candidate's background. The repo ships with an example `resume.md` (a plain-Markdown resume — no tables, images, or PDF, so the routine can parse it reliably).

1. Replace the contents of `resume.md` with a Markdown version of your own resume.
2. Keep it focused on skills and experience with concrete, searchable keywords and quantified impact — that's what the routine matches against postings. Search direction (target titles, constraints, preferences) lives in `routine.md`, not here.
3. Commit the file. The routine clones this repo at the start of every run, so Claude reads the resume locally — no internet access or sharing settings required for this step.

> If your resume contains personal info you don't want in version history indefinitely, keep this repo private.

### Option B — Google Drive link

If you'd rather keep your resume out of the repo, point the routine at a Google Drive file instead:

1. In Google Drive, right-click your resume → **Share** → set general access to **"Anyone with the link"** (Viewer).
2. Edit `routine.md` step 1 to reference the share link instead of `resume.md`, e.g. `Review the user's resume from this Google Drive link: <link>`.
3. Claude's built-in web fetch tool can open a public/"anyone with the link" Drive file directly — no extra setup needed.

If you'd rather not make the file link-shareable, connect a Google Drive integration at [claude.ai/customize/connectors](https://claude.ai/customize/connectors) (**Settings → Connectors**) and include it under **Connectors** when you create the routine in step 4. Note that some Anthropic-hosted Google connectors must be authenticated from claude.ai directly rather than from within a session.

## 3. Create a cloud environment with full internet access

Job boards, ATSs (Greenhouse/Lever/Ashby), and career pages span far too many domains to allowlist individually, so this routine needs the **Full** network access level.

1. Go to [claude.ai/code](https://claude.ai/code) and connect your GitHub account if you haven't already.
2. Click the cloud/environment icon and select **Add environment**.
3. Configure it:
   - **Name**: e.g. `Job Hunt (Full Internet)`
   - **Network access**: **Full** (the default **Trusted** level only allows package registries, GitHub, and dev-tooling domains — it will block job boards and career pages)
   - **Environment variables** / **Setup script**: leave empty unless your variant of the routine needs them
4. Save the environment.

> Available network access levels are **None**, **Trusted** (default), **Full**, and **Custom** (your own domain allowlist). See [Network access](https://code.claude.com/docs/en/claude-code-on-the-web#network-access) for details.

## 4. Create the scheduled routine

1. Go to [claude.ai/code/routines](https://claude.ai/code/routines) and click **New routine**.
2. Configure the routine:
   - **Name**: e.g. `Job Hunt`
   - **Instructions**: paste the contents of `routine.md`
   - **Repositories**: add this repo — required for Option A so Claude can read `resume.md`; also required for Option B if you want Claude to be able to read the repo at all (otherwise it has no working directory)
   - **Environment**: select the `Job Hunt (Full Internet)` environment from step 3
   - **Connectors**: include the Google Drive connector here if you're using it for Option B with a private file; otherwise remove connectors you don't need
   - **Trigger**: choose **Schedule** and pick a cadence (e.g. daily on weekday mornings)
3. Click **Create**, then **Run now** on the routine's detail page to do a first test run.

You can also create a recurring routine from the CLI with `/schedule` (e.g. `/schedule daily job search at 8am`), but you'll still need to edit the routine on the web afterward to set its environment to **Full** network access.

## 5. Reviewing results

Each run shows up in the routine's history at [claude.ai/code/routines](https://claude.ai/code/routines). Open a run to see the full match list, verification tiers (VERIFIED / SEEN BUT UNVERIFIED), and the sources Claude checked. A green run status only means the session completed without an infrastructure error — open the transcript to see the actual results.
