Follow these steps exactly when this command is invoked.

## Setup

1. Take `$ARGUMENTS` as the company name (e.g. `/thank-recruiter acme` → company name is `acme`).
2. Search for the matching `.md` file across these folders in order: `done/successful/`, `done/`, `work-in-progress/`, `job-description/`. Use the company name as a case-insensitive filename match.
3. Read the file in full.

---

## Prompt for Call Context

Before doing anything else, ask the user:

> "What did you discuss in the recruiter call? Share: topics that came up, what the recruiter said about the role or team priorities, any questions you asked and their answers, and anything unexpected."

Wait for the user's response before proceeding.

---

## Generate Call Notes and Thank You Email

Append the output to the job file under a new section:

`## Recruiter Call — YYYY-MM-DD`

Do not overwrite or remove any existing content in the file.

---

### Section 1 — Call notes

Summarise what the user shared as structured bullet points:
- What the user discussed (key themes and examples given)
- Recruiter's stated priorities or answers
- Any gaps raised or not raised
- Recruiter name if provided

---

### Section 2 — How the user's answers map to the JD

Compare what the user described in the call against the JD's essential criteria. For each topic the user raised, map it to the specific criterion it addresses. Flag any essential criterion not covered in the conversation — note it as worth surfacing in the HM stage.

---

### Section 3 — Thank you email

Draft a short thank you email to the recruiter. Rules:
- 3 short paragraphs max
- Reference what was actually discussed in the call — not generic PM talking points
- Echo the recruiter's stated priorities back using the user's specific evidence from the conversation
- End with a clear forward-looking line (next steps or motivation)
- Use the recruiter's name if the user provided it; otherwise leave as [Name]
- Subject line format: `Thank you — [Role title], [Company]`

Format the email as a blockquote:
> Subject: ...
>
> [email body]
