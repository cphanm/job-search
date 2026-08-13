# Job Search — Claude Code Workflow

A personal job search system built on [Claude Code](https://claude.ai/code). Nine slash commands handle the full pipeline: guided setup, optionally pulling job links from LinkedIn and Welcome to the Jungle alert emails, fetching job descriptions, matching against a resume, generating personalised resume summaries and application answers, ranking the active pipeline by fit, and managing the interview pipeline from recruiter call through to hiring manager interview.

---

## Setup

### Prerequisites

- [Claude Code](https://claude.ai/code) installed and authenticated
- Google Chrome installed at `/Applications/Google Chrome.app` (used for JS-rendered job boards)

### First-time setup

1. Clone this repository
2. Open the folder in Claude Code: `claude /path/to/job-search`
3. Run `/setup` and follow the prompts — it walks through capturing your resume, optionally your behavioral story library, and building your `positioning/` files (see below) through a short back-and-forth review, not a single-shot output. Safe to re-run later, e.g. after updating your resume.

That's it. No environment variables, no dependencies to install.

**What `/setup` creates, manually if you'd rather skip the guided version:**
- `input/resume.md` — paste the full, unedited text of your CV here. Gitignored, never leaves your machine.
- `input/story-library.md` — paste your behavioral story library here. Used only by `/create-resume` Step 7 when drafting application form answers, since resume bullets are often too compressed to carry a full behavioral story. Also gitignored.
- `input/job-alerts-mailbox.md` — (optional) the address of a dedicated Gmail account used by `/scan-linkedin-alerts` and `/scan-wttj-alerts` to read job-alert emails. Not created by `/setup`; create it manually if you want either command. Gitignored.
- `input/blacklisted-companies.md` — (optional) a table of companies/posters to exclude from consideration (e.g. AI recruiters that never name the real hiring company). `/scan-jobs` checks it after fetching each posting and skips any match instead of creating a file. Not created by `/setup`; create it manually if you want this filter. Gitignored.
- `positioning/differentiators.md` — the 1–3 patterns in your resume that make you stand out, not just qualified (e.g. a skill combination that shows up across multiple roles/eras). Derived from `input/resume.md` alone, never the story library — this is what a recruiter or hiring panel actually sees. `/create-resume` checks this before flagging a gap and when deciding what to surface as a differentiator. Gitignored — this isn't raw input, it's a conclusion about your input, which is why it lives in its own folder rather than `input/`.
- `positioning/career-narrative.md` — the fuller evidence behind `differentiators.md`: recurring problems solved, decisions you personally owned, the trade-offs resume.md actually states, and what would make a hiring panel trust you with a roadmap. Also gitignored.

---

## Finding Jobs

Two files help with job discovery. Use either or both.

### `job-search-launcher.html`

Open this file in a browser. Click the button to open 12 Google ATS searches as tabs simultaneously — each filtered to the past 7 days. Covers: Workable, Greenhouse, Lever, Ashby, SmartRecruiters, Jobvite, Recruitee, Teamtailor, Breezy HR, Workday, iCIMS, Personio.

### `job-search-queries.md`

The same 12 searches as clickable Markdown links, organised by ATS tier (Startup/Tech, Mid-market, Enterprise). Use this for selective searches or to copy and modify queries — e.g. add an industry keyword: `"london" "legaltech"`.

---

## Commands

### `/setup` — Guided first-time setup

Run once when starting fresh, and safe to re-run later (e.g. after updating your resume — it skips anything already in place unless you ask it to redo a step).

1. Checks what already exists across `input/` and `positioning/`
2. Prompts you to create `input/resume.md` (you paste your CV — Claude never writes this file)
3. Prompts you to create `input/story-library.md`, with a clear explanation of what it's actually for (Step 7 application answers only — see `/create-resume` below) so you can decide whether to invest time in it now
4. Reads `input/resume.md` alone (never the story library — positioning reflects what a recruiter sees, not the fuller narrative behind it), proposes 3–5 candidate recurring patterns with evidence, and asks you to sort them into genuine differentiators vs. table-stakes competency — expect a few rounds of back-and-forth, not a single-shot answer
5. Writes `positioning/differentiators.md` and `positioning/career-narrative.md` once you've confirmed the split, and shows you both for review

### `/scan-linkedin-alerts` — Pull job links from LinkedIn alert emails

Optional, requires `input/job-alerts-mailbox.md` (see Setup above).

1. Checks a dedicated Gmail inbox for unread LinkedIn job-alert emails — sent directly, or forwarded in from another mailbox (detected by a "Fwd:" subject referencing LinkedIn's alert sender)
2. Extracts every job link from each alert (multi-job digests and single-job alerts both supported)
3. Dedups against every job already tracked anywhere in the repo
4. Appends new links to `work-in-progress/jobs-to-scan.md` under `## To Scan`
5. Runs `/scan-jobs` automatically if any new links were added

This command only ever touches the one Gmail account named in `input/job-alerts-mailbox.md` — never another account logged into the same browser.

---

### `/scan-wttj-alerts` — Pull job links from Welcome to the Jungle alert emails

Optional, requires `input/job-alerts-mailbox.md` (see Setup above).

1. Checks the same dedicated Gmail inbox for unread Welcome to the Jungle "New match" alert emails — sent directly, or forwarded in from another mailbox
2. Each alert is a digest of several matched jobs, each with its own tracked link; resolves each link to the real job URL and extracts a stable job ID
3. Dedups against every job already tracked anywhere in the repo
4. Appends new links to `work-in-progress/jobs-to-scan.md` under `## To Scan`
5. Runs `/scan-jobs` automatically if any new links were added

This command only ever touches the one Gmail account named in `input/job-alerts-mailbox.md` — never another account logged into the same browser. Note: the resolved job links carry a time-limited access token, so scan promptly after an alert arrives rather than letting unread alerts pile up.

---

### `/scan-jobs` — Fetch job descriptions

1. Add one job URL per line under `## To Scan` in `work-in-progress/jobs-to-scan.md`
2. Run `/scan-jobs` in Claude Code
3. Claude fetches each JD, detects the ATS, and creates one `.md` file per job in `work-in-progress/`
4. Processed URLs move from `## To Scan` to `## Done` in `jobs-to-scan.md`

Each created file contains:
- Job title, company name, source URL, and confirmed company homepage
- Posted date, where the ATS exposes one (see reliability notes below)
- Full job description text (cleaned of nav and footer chrome)
- Application form questions, if present on the page

Supported ATS platforms and their fetch methods:

| ATS | Method |
|-----|--------|
| Greenhouse, Lever, Workable (server-rendered, `jobs.workable.com`), SmartRecruiters, Jobvite, Recruitee, Teamtailor, Breezy HR, Personio | curl (server-rendered) |
| Ashby, Workday, Oracle HCM, Workable (client-rendered apply flow, `apply.workable.com`), Welcome to the Jungle (app.welcometothejungle.com) | Chrome headless |
| iCIMS | Chrome headless with user-agent |
| LinkedIn (`linkedin.com/jobs/view/<id>`, public postings only — not logged-in search results) | curl with a browser user-agent |
| Unknown / company careers pages | curl first, Chrome headless fallback |

**Posted date reliability** — most ATS expose a `datePosted` field (schema.org JobPosting, used for SEO), extracted automatically: confirmed working on Workable, SmartRecruiters, Breezy HR, Teamtailor, Ashby, Welcome to the Jungle, Lever, and LinkedIn (as a relative string like "5 days ago"). **Greenhouse never exposes one** — confirmed absent, not worth re-checking. Workday and Oracle HCM are inconclusive (the fetch itself often stalls on a cookie-consent wall before reaching the date). Jobvite, Recruitee, Personio, and iCIMS are unverified — checked opportunistically, not guaranteed.

---

### `/create-resume` — Analyse and personalise

Run after `/scan-jobs` has created the JD files.

```
/create-resume              # processes all files in work-in-progress/
/create-resume company.md   # processes one file only
```

For each file, Claude appends a `## Resume Personalisation` section containing, in this order:

1. **Company Research** — industry, customers, product, scale, and primary user type (who the PM in this role builds for day-to-day); fetched from the company homepage
2. **3 Challenges** — what the hiring team most needs the new hire to solve, grounded in the whole JD (including nice-to-have/bonus lines, not just the must-have section) and company context; each title states the hiring team's underlying fear in under 12 words, not a reworded Responsibilities/Skills line; auto-generated tag/keyword lists at the end of a JD (e.g. numbered topic phrases repeating themes already in the prose) are read as metadata, not a menu of acceptable candidate backgrounds; every claim in a challenge's explanation must trace to a specific JD quote or be explicitly flagged as inference — no stating a consequence, stat, or named customer as fact unless the JD actually says it; if a challenge is sharpened beyond the JD's own generic wording (e.g. "anticipate future trends" naming no specific trend), the added specificity must trace to a named, checked source — a JD quote or a specific Phase 0 finding actually re-confirmed to contain it — not a vague appeal to "general market context"
3. **Relevant Experiences** — the exact resume bullets from `input/resume.md` that address each challenge; quoted verbatim with company names; each citation is tagged **Direct match** or **Bridge** (with the interpretive step named — e.g. an internal tool standing in for a customer-facing one, or a different industry standing in for this one) so a hiring-team read never overstates how literal a match is; a bullet must match both the challenge's general skill *and* any specific circumstance it names (scale, timeframe, structural condition) to count as Direct — matching the skill alone under a different circumstance is a Bridge; checked against `positioning/differentiators.md` first so a real match isn't undercounted just because it's an older or less obvious bullet; when multiple bullets could plausibly answer a challenge, the one matching the JD's most specific language is preferred over a merely-adequate general match
4. **Gaps** — specific JD requirements not evidenced in the resume, flagged explicitly and tagged as core/must-have or preferred/nice-to-have; multi-option requirements ("a few of the following") are checked against their actual threshold, not full list coverage
5. **Fit Assessment** — opens with a business model check: identifies what the PM in the role actually builds for (business customers = no gap; consumers or marketplace = gap; for mixed-model companies, states explicitly which side the role sits on and whether that creates a gap); then issues Fit / Stretch / Out of Reach against core requirements, with preferred/nice-to-have requirements factored in as a bonus — an unmet requirement that's the JD's named standout differentiator (or repeated/emphasized elsewhere) can still trigger a one-level downgrade even though it isn't formally core; followed by fit basis: whether the domain and primary user type transfer directly or require a mental leap; an "adjacent domain" cited to bridge a gap must match both industry and actual function — sharing an industry label while failing on function (e.g. a different stage of the same broader process) means the gap stays open, not bridged; every JD quote used to justify a claim is checked against its actual section (a company-description line can't be borrowed to inflate a responsibilities-section claim), and items inside an "or" list (e.g. "SaaS, platform, fintech, or similarly complex products") are read as one-sufficient, not all-required; reasoning is grounded in the Relevant Experiences step's own Direct/Bridge tags rather than re-flattening a bridge into a direct match
6. **Personalised Summary** — a 3-sentence resume summary using only facts and numbers from the resume; each of the 3 evidence claims maps 1:1 to one of the 3 challenges, one per company; the forward-looking sentence checks a challenge's sourcing before restating an inferred mechanism as fact, falling back to the JD's own generic wording if it can't be traced to a named source (the Scored Summaries step applies the same check to its alternatives)
7. **Pros and Cons** — direct feedback on the summary: what will resonate, what is weak or missing; met and unmet preferred/nice-to-have requirements are named explicitly here as differentiators or risks, not left implicit
8. **Scored Summaries** — the Personalised Summary is reproduced as Option 0 (Baseline) and scored alongside 3 alternatives; all four scored out of 10 across conciseness, punchiness, and clarity in one block for direct comparison; each alternative must score at least 8.5 — if not, it's revised once and rescored, with an explicit note if it's still below 8.5 after that
9. **Application Form Answers** — drafted answers for any questions present in the file; uses `input/story-library.md` for narrative shape and context, cross-checked against `input/resume.md` for exact numbers; each answer capped at 300 words; checked for evidence reuse across the file's own questions before drafting, so the same company/story isn't used twice without a reason

**Execution note:** the Challenges, Relevant Experiences, and Fit Assessment steps run together as a single delegated call to a subagent pinned to `claude-sonnet-4-6`, which reads `input/resume.md`, the job description file, and `positioning/differentiators.md` once and writes each step's output to the file itself as it completes — instead of three separate cold-start calls re-reading the same files. The subagent is primed with a senior-product-leader persona to sharpen judgment on ambiguous framing calls. It has Read/Write/Edit/Grep/Glob/Bash only — no WebFetch or browser access, so it can't verify anything against a live web page itself; any check that needs that (e.g. Phase 0's company research) has to run on the orchestrating session instead. The remaining steps run inline on the session's default model.

---

### `/rank-pipeline` — Table of every role by fit

```
/rank-pipeline                    # scans work-in-progress/ (default)
/rank-pipeline done/successful    # scans any other folder
```

Reads every `.md` file in the target folder that has a completed Fit Assessment and produces one markdown table — Company, Industry, Business Model, Posted, Fit — ordered Fit → Stretch → Out of Reach. Posted date is pulled verbatim from each file's `Posted:` line and left blank where the ATS never exposed one. Reruns are appended, not overwritten, so the table always reflects the most recent verdict in each file, not the original one. Files with no Fit Assessment yet are listed separately rather than guessed at.

---

### `/prepare-recruiter acme` — Prepare for a recruiter call

Run before a recruiter call. Reads the existing analysis from `/create-resume` and appends a `## Recruiter Call Brief` section containing:

1. **What to lead with** — 3–4 bullet points ordered by JD priority, grounded in specific resume evidence
2. **3 Questions to ask** — recruiter-appropriate questions (team structure, why the role is open, screening priorities), each with a one-line rationale
3. **Gaps and how to handle them** — each real gap from the fit assessment with a suggested handling strategy: reframe, acknowledge and bridge, or do not volunteer
4. **Why this role** — one sentence capturing specific motivation, mirroring the company's language

Requires `/create-resume` to have been run first on the same file.

---

### `/thank-recruiter acme` — Send a thank you after a recruiter call

Run after a recruiter call. Prompts for context about what was discussed, then appends a `## Recruiter Call` section containing:

1. **Call notes** — structured summary of topics covered, recruiter's stated priorities, and anything unexpected
2. **JD mapping** — how what was discussed maps to the JD's essential criteria; flags any criteria not yet covered as worth raising in the HM stage
3. **Thank you email** — short draft (3 paragraphs max) that references the actual conversation and echoes the recruiter's stated priorities back with specific evidence

---

### `/thank-hm acme` — Send a thank you after a hiring manager interview

Run after an HM interview. Prompts for context, then appends a `## HM Interview` section containing:

1. **Interview notes** — questions asked, examples given, what the HM emphasised, anything unexpected, and any follow-up materials offered
2. **JD mapping** — how what was discussed maps to the essential criteria; flags any gaps not yet addressed
3. **Thank you email** — more substantive than the recruiter email (3–4 paragraphs); reflects the HM's specific priorities and language; references any follow-up materials if offered

---

## File Structure

```
job-search/
├── README.md                          # this file
├── CLAUDE.md                          # Claude Code project instructions
├── job-search-launcher.html           # one-click browser launcher for 12 ATS searches
├── job-search-queries.md              # the same 12 searches as Markdown links
│
├── input/                             # read-only source files
│   ├── resume.md                      # your CV — GITIGNORED, never committed
│   ├── story-library.md               # behavioral story library — GITIGNORED, never committed
│   ├── job-alerts-mailbox.md          # (optional) automation Gmail address for /scan-linkedin-alerts and /scan-wttj-alerts — GITIGNORED
│   ├── blacklisted-companies.md       # (optional) companies /scan-jobs should skip — GITIGNORED
│   └── job-description-template.md   # template structure for JD files
│
├── positioning/                       # GITIGNORED — derived from input/, not raw input itself
│   ├── differentiators.md             # (optional) the 1-3 patterns that make you stand out, not just qualified
│   └── career-narrative.md            # (optional) fuller evidence behind differentiators.md — problems, decisions, trade-offs, trust signals
│
├── work-in-progress/                  # active pipeline
│   ├── jobs-to-scan.md                # queue: add URLs here before running /scan-jobs
│   └── [company].md                   # one file per role; created by /scan-jobs, filled by /create-resume
│
├── done/                              # GITIGNORED — move a file here when the application is sent
│   └── failed/                        # GITIGNORED — move a file here when the application is confirmed rejected
│
├── skipped/      # GITIGNORED — move a file here when the domain/experience gap is too large to bridge
│
└── .claude/
    ├── settings.json                  # Claude Code permission allowlist for this project
    └── commands/
        ├── setup.md                   # /setup command definition
        ├── scan-linkedin-alerts.md    # /scan-linkedin-alerts command definition
        ├── scan-wttj-alerts.md        # /scan-wttj-alerts command definition
        ├── scan-jobs.md               # /scan-jobs command definition
        ├── create-resume.md           # /create-resume command definition
        ├── rank-pipeline.md           # /rank-pipeline command definition
        ├── prepare-recruiter.md       # /prepare-recruiter command definition
        ├── thank-recruiter.md         # /thank-recruiter command definition
        └── thank-hm.md                # /thank-hm command definition
```

---

## Workflow

```
1. Find roles       job-search-launcher.html or job-search-queries.md
   (or)             /scan-linkedin-alerts or /scan-wttj-alerts (optional, needs input/job-alerts-mailbox.md)
        |
        v
2. Queue URLs       work-in-progress/jobs-to-scan.md  →  ## To Scan
        |
        v
3. Fetch JDs        /scan-jobs
                    → creates work-in-progress/[company].md per role
        |
        v
4. Analyse          /create-resume
                    → appends personalisation analysis to each file
        |
        v
5. Review           read the fit assessment, pros/cons, and alternative summaries
                    (or) /rank-pipeline for a table of every role by fit, across the whole folder
        |
     ┌──┴──┐
     |     |
   Apply  Skip
     |     |
     v     v
  done/   skipped/
     |
6. Recruiter call
     |
     ├── Before     /prepare-recruiter [company]
     |              → what to lead with, questions to ask, gap handling, why this role
     |
     └── After      /thank-recruiter [company]
                    → prompts for call context, generates call notes, JD mapping, thank you email
        |
        v
   Recruiter outcome
  ┌──┴──┐
  |     |
Pass  Rejected
  |     |
  |     v
  |  done/failed/
  |
  v
7. HM interview
     |
     └── After      /thank-hm [company]
                    → prompts for interview context, generates interview notes, JD mapping, thank you email
        |
        v
8. Outcome
  ┌──┴──┐
  |     |
Pass  Rejected
        |
        v
  done/failed/
```

Move the file to `done/` once the application is submitted. Move it to `skipped/` if the gap analysis shows the role is too far from the current profile for this market. When a rejection is confirmed, move the file from `done/` into `done/failed/`. All folders are gitignored.

---

## What Is and Is Not Committed

| File / Folder                       | Committed | Reason                                                   |
| ----------------------------------- | --------- | -------------------------------------------------------- |
| `input/resume.md`                   | No        | Personal data                                            |
| `input/story-library.md`            | No        | Personal data                                            |
| `input/job-alerts-mailbox.md`       | No        | Automation Gmail account address                         |
| `input/blacklisted-companies.md`    | No        | May reflect personal application decisions                |
| `positioning/differentiators.md`    | No        | Derived from personal resume data                        |
| `positioning/career-narrative.md`   | No        | Derived from personal resume data                        |
| `done/`                             | No        | Contains resume analysis linked to applications sent     |
| `done/failed/`                      | No        | Confirmed rejections — same personal data concerns       |
| `skipped/`                          | No        | Same                                                     |
| `session-context.md`                | No        | Personal session notes                                   |
| `.obsidian/`                        | No        | Local app config                                         |
| `.claude/settings.json`             | No        | Contains local permission allowlist — varies per machine |
| `work-in-progress/jobs-to-scan.md`  | Yes       | Queue file — no personal data                            |
| `work-in-progress/[company].md`     | No        | JD + personalisation analysis tied to your resume        |
| `input/job-description-template.md` | Yes       | Reusable template                                        |
| `job-search-launcher.html`          | Yes       | Generic search tool                                      |
| `job-search-queries.md`             | Yes       | Generic search links                                     |
| `.claude/commands/`                 | Yes       | Reusable slash commands                                  |
| `CLAUDE.md`                         | Yes       | Project instructions                                     |

---

## Adapting for a Different Role or Market

To adapt this for a different job title, location, or ATS platform:

- **Search queries:** edit `job-search-queries.md` and `job-search-launcher.html` — replace `"product+manager"` and `"london"` with the target role and location
- **Resume:** replace the contents of `input/resume.md` with the new CV
- **Positioning files:** if `positioning/differentiators.md` or `positioning/career-narrative.md` exist, re-run `/setup` (or its Step 3 manually) after changing the resume — both are derived from the specific CV they were written against and won't necessarily hold for a different one
- **ATS support:** to add a new ATS, update the fetch logic in `.claude/commands/scan-jobs.md` under Step 2
