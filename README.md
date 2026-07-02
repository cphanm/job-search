# Job Search — Claude Code Workflow

A personal job search system built on [Claude Code](https://claude.ai/code). Two slash commands handle the full pipeline: finding jobs, fetching job descriptions, matching against a resume, and generating personalised resume summaries and application answers — one role at a time.

---

## Setup

### Prerequisites

- [Claude Code](https://claude.ai/code) installed and authenticated
- Google Chrome installed at `/Applications/Google Chrome.app` (used for JS-rendered job boards)

### First-time setup

1. Clone this repository
2. Open the folder in Claude Code: `claude /path/to/job-search`
3. Create `original/resume.md` — paste the full, unedited text of your CV here. This file is gitignored and never leaves your machine.

That's it. No environment variables, no dependencies to install.

---

## Finding Jobs

Two files help with job discovery. Use either or both.

### `job-search-launcher.html`

Open this file in a browser. Click the button to open 12 Google ATS searches as tabs simultaneously — each filtered to the past 7 days. Covers: Workable, Greenhouse, Lever, Ashby, SmartRecruiters, Jobvite, Recruitee, Teamtailor, Breezy HR, Workday, iCIMS, Personio.

### `job-search-queries.md`

The same 12 searches as clickable Markdown links, organised by ATS tier (Startup/Tech, Mid-market, Enterprise). Use this for selective searches or to copy and modify queries — e.g. add an industry keyword: `"london" "legaltech"`.

---

## Commands

### `/scan-jobs` — Fetch job descriptions

1. Add one job URL per line under `## To Scan` in `work-in-progress/jobs-to-scan.md`
2. Run `/scan-jobs` in Claude Code
3. Claude fetches each JD, detects the ATS, and creates one `.md` file per job in `work-in-progress/`
4. Processed URLs move from `## To Scan` to `## Done` in `jobs-to-scan.md`

Each created file contains:
- Job title, company name, source URL, and confirmed company homepage
- Full job description text (cleaned of nav and footer chrome)
- Application form questions, if present on the page

Supported ATS platforms and their fetch methods:

| ATS | Method |
|-----|--------|
| Greenhouse, Lever, Workable, SmartRecruiters, Jobvite, Recruitee, Teamtailor, Breezy HR, Personio | curl (server-rendered) |
| Ashby, Workday, Oracle HCM | Chrome headless |
| iCIMS | Chrome headless with user-agent |
| Unknown / company careers pages | curl first, Chrome headless fallback |

---

### `/create-resume` — Analyse and personalise

Run after `/scan-jobs` has created the JD files.

```
/create-resume              # processes all files in work-in-progress/
/create-resume company.md   # processes one file only
```

For each file, Claude appends a `## Resume Personalisation` section containing:

1. **Company Research** — industry, customers, product, scale; fetched from the company homepage
2. **3 Challenges** — what the hiring team most needs the new hire to solve, grounded in the JD and company context
3. **Relevant Experiences** — the exact resume bullets from `original/resume.md` that address each challenge; quoted verbatim with company names
4. **Gaps** — specific JD requirements not evidenced in the resume, flagged explicitly
5. **Personalised Summary** — a 3–4 sentence resume summary using only facts and numbers from the resume, addressing the 3 challenges
6. **Pros and Cons** — direct feedback on the summary: what will resonate, what is weak or missing
7. **3 Alternative Summaries** — each scored out of 10 across conciseness, punchiness, and clarity
8. **Fit Assessment** — Fit / Stretch / Out of Reach, with reasoning against the core JD requirements
9. **Application Form Answers** — drafted answers for any questions present in the file, grounded in resume evidence

---

## File Structure

```
job-search/
├── README.md                          # this file
├── CLAUDE.md                          # Claude Code project instructions
├── job-search-launcher.html           # one-click browser launcher for 12 ATS searches
├── job-search-queries.md              # the same 12 searches as Markdown links
│
├── original/                          # read-only source files
│   ├── resume.md                      # your CV — GITIGNORED, never committed
│   └── job-description-template.md   # template structure for JD files
│
├── work-in-progress/                  # active pipeline
│   ├── jobs-to-scan.md                # queue: add URLs here before running /scan-jobs
│   └── [company].md                   # one file per role; created by /scan-jobs, filled by /create-resume
│
├── done/                              # GITIGNORED — move a file here when the application is sent
│
├── not-apply-since-lower-chance/      # GITIGNORED — move a file here when the domain/experience gap is too large to bridge
│
└── .claude/
    ├── settings.json                  # Claude Code permission allowlist for this project
    └── commands/
        ├── scan-jobs.md               # /scan-jobs command definition
        └── create-resume.md           # /create-resume command definition
```

---

## Workflow

```
1. Find roles       job-search-launcher.html or job-search-queries.md
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
        |
     ┌──┴──┐
     |     |
   Apply  Skip
     |     |
     v     v
  done/   not-apply-since-lower-chance/
```

Move the file to `done/` once the application is submitted. Move it to `not-apply-since-lower-chance/` if the gap analysis shows the role is too far from the current profile for this market. Both folders are gitignored.

---

## What Is and Is Not Committed

| File / Folder | Committed | Reason |
|---|---|---|
| `original/resume.md` | No | Personal data |
| `done/` | No | Contains resume analysis linked to applications sent |
| `not-apply-since-lower-chance/` | No | Same |
| `session-context.md` | No | Personal session notes |
| `.obsidian/` | No | Local app config |
| `.claude/settings.json` | No | Contains local permission allowlist — varies per machine |
| `work-in-progress/jobs-to-scan.md` | Yes | Queue file — no personal data |
| `work-in-progress/[company].md` | No | JD + personalisation analysis tied to your resume |
| `original/job-description-template.md` | Yes | Reusable template |
| `job-search-launcher.html` | Yes | Generic search tool |
| `job-search-queries.md` | Yes | Generic search links |
| `.claude/commands/` | Yes | Reusable slash commands |
| `CLAUDE.md` | Yes | Project instructions |

---

## Adapting for a Different Role or Market

To adapt this for a different job title, location, or ATS platform:

- **Search queries:** edit `job-search-queries.md` and `job-search-launcher.html` — replace `"product+manager"` and `"london"` with the target role and location
- **Resume:** replace the contents of `original/resume.md` with the new CV
- **ATS support:** to add a new ATS, update the fetch logic in `.claude/commands/scan-jobs.md` under Step 2
