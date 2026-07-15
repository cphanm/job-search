Follow these steps exactly when this command is invoked.

## Purpose

Guide the user through first-time setup of this job search workflow: capturing their resume, optionally capturing their behavioral story library, and building the `positioning/` files that `/create-resume` uses to avoid undercounting the user's real differentiators. Safe to re-run later — each step checks whether its output already exists and skips it, so re-running after a resume update only touches what needs to change.

---

## Step 0 — Check current state

Check for the existence of:
- `input/resume.md`
- `input/story-library.md`
- `positioning/differentiators.md`
- `positioning/career-narrative.md`

Report which already exist. Skip any step below whose output already exists, unless the user explicitly asks to redo it (e.g. "redo positioning" after updating their resume).

---

## Step 1 — Resume

If `input/resume.md` does not exist:

Ask the user to create `input/resume.md` and paste the full, unedited text of their CV into it. Do not create or write this file directly — this is the user's source-of-truth personal data, and it stays entirely under their control. Wait for confirmation before moving on.

If it already exists, confirm and move to Step 2.

---

## Step 2 — Story library (optional)

If `input/story-library.md` does not exist:

Explain its actual purpose clearly, so the user can decide whether to invest time in it now or skip it: it is used only by `/create-resume` Step 7 to draft application form answers. A resume's bullet points are compressed for space and often don't carry enough narrative detail for a behavioral question ("tell me about a time...") — the story library exists to hold that fuller shape (situation, actions, trade-offs, results) separately. It plays no role in the `positioning/` files built in Step 3 below — those come from the resume alone, because the resume is what recruiters and hiring panels actually see.

Ask if the user wants to create it now (paste their stories, or paste a rough version to refine later) or skip it and add it later. Do not create or write this file directly, for the same reason as Step 1.

---

## Step 3 — Positioning (interactive — do not skip the back-and-forth)

Only run once `input/resume.md` exists. Uses `input/resume.md` alone — never `input/story-library.md` — because positioning describes what a recruiter or hiring panel sees on the page, not the fuller narrative behind it.

1. Read `input/resume.md` in full.
2. Identify 3–5 candidate recurring patterns: problems the user has solved more than once, across multiple companies or time periods, evidenced by specific bullets (quote them). Look across the whole resume, not just the most recent roles — older roles often carry evidence that gets undercounted.
3. Present the candidates to the user with their supporting evidence. Do not decide for the user which are genuine differentiators versus table-stakes competency — ask explicitly. A pattern is a **differentiator** if it's a rarer combination most qualified candidates in this field wouldn't also have; it's **table-stakes competency** if most qualified candidates would also claim it (e.g. general adoption/retention/monetization work is usually table-stakes; a specific unusual combination repeated across eras is usually a differentiator).
4. Iterate on the split based on the user's feedback. Expect to revise more than once — this is a judgment call the user needs to make, not something to get right on the first pass.
5. Once confirmed, write two files (these are derived analysis, not raw personal data, so — unlike Steps 1 and 2 — write them directly):

**`positioning/differentiators.md`** — the confirmed differentiator patterns only, each with a one-line description and the two forms/companies it shows up in if relevant.

**`positioning/career-narrative.md`** — the fuller picture, covering:
- Recurring customer/business problems solved (all confirmed patterns, differentiators and table-stakes both, labelled as such)
- Product decisions the user personally owned (signalled by role titles like "Sole Product Manager" and ownership verbs — "owned," "led," "defined")
- What trade-offs shaped those decisions, using only what's explicitly stated in resume.md bullets (not inferred) — note plainly that resume.md gives outcomes more than reasoning, so this section will be thinner than a story-library-sourced version would be
- What signals in the resume would make a hiring panel trust the user with a roadmap (e.g. quantified claims, ownership range, consistent before/after framing)

6. Show both files to the user for review after writing them.

---

## Step 4 — Wrap-up

Summarize what now exists (resume, story library if created, both positioning files). Point to the next step in the workflow: `job-search-launcher.html` or `job-search-queries.md` to find roles, then `/scan-jobs` to start the pipeline.
