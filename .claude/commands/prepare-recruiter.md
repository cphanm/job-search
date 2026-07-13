Follow these steps exactly when this command is invoked.

## Setup

1. Take `$ARGUMENTS` as the company name (e.g. `/prepare-recruiter acme` → company name is `acme`).
2. Search for the matching `.md` file across these folders in order: `done/successful/`, `done/`, `work-in-progress/`, `job-description/`. Use the company name as a case-insensitive filename match.
3. Read the file in full, focusing on: Phase 0 Company Research, Step 1 (3 Challenges), Step 2 (Relevant Experiences), Step 6 (Fit Assessment).
4. If the file has not been through `/create-resume` yet (no Resume Personalisation section exists), stop and tell the user to run `/create-resume` first.

---

## Generate Recruiter Call Brief

Append the output to the job file under a new section:

`## Recruiter Call Brief — YYYY-MM-DD`

Do not overwrite or remove any existing content in the file.

---

### Section 1 — What to lead with

Based on the JD's top priorities (Step 1 challenges) and the user's strongest evidence (Step 2), write 3–4 bullet points on what the user should open with in the recruiter call.

Rules:
- Ground every point in a specific resume bullet — company name and outcome
- Order by relevance to the JD's stated priorities, not by chronology
- Do not include gaps or weak matches here

---

### Section 2 — 3 Questions to ask the recruiter

Generate 3 questions for the user to ask the recruiter. These must:
- Be answerable by a recruiter (team structure, why the role is open, screening priorities) — not deep product or technical questions reserved for the hiring manager
- Show the user has read the JD carefully
- Help the user surface information useful for the HM stage or for assessing fit

Format:
- **Q1:** [question] — [one line on why this is worth asking]
- **Q2:** [question] — [one line on why this is worth asking]
- **Q3:** [question] — [one line on why this is worth asking]

---

### Section 3 — Gaps and how to handle them

From Step 2 (Gaps to flag) and Step 6 (Fit Assessment), list the real gaps. For each gap:
- State the gap clearly in one line
- State how the user should handle it if raised: reframe with a bridge, acknowledge and redirect, or do not volunteer

---

### Section 4 — Why this role (one line)

Write one sentence capturing the user's specific motivation for this role — grounded in the company's actual challenge and the user's trajectory. Must not be generic. Mirror the company's language where possible.
