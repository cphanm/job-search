Follow these steps exactly when this command is invoked.

## Setup

1. Take `$ARGUMENTS` as the company name (e.g. `/thank-hm acme` → company name is `acme`).
2. Search for the matching `.md` file across these folders in order: `done/successful/`, `done/`, `work-in-progress/`, `job-description/`. Use the company name as a case-insensitive filename match.
3. Read the file in full.

---

## Prompt for Interview Context

Before doing anything else, ask the user:

> "What did you discuss in the hiring manager interview? Share: topics and questions covered, what the HM emphasised or seemed most interested in, examples you gave and their reactions, anything unexpected, and any follow-up materials you offered to send."

Wait for the user's response before proceeding.

---

## Generate Interview Notes and Thank You Email

Append the output to the job file under a new section:

`## HM Interview — YYYY-MM-DD`

Do not overwrite or remove any existing content in the file.

---

### Section 1 — Interview notes

Summarise what the user shared as structured bullet points:
- Questions asked and key topics covered
- Examples the user gave and how they landed
- What the HM emphasised or returned to
- Anything unexpected or worth noting
- Any follow-up materials the user offered to send
- HM name and title if provided

---

### Section 2 — How the user's answers map to the JD

Compare what the user described against the JD's essential criteria and the challenges from Step 1. For each topic the user raised, map it to the specific criterion it addresses. Flag any essential criterion not covered — note it as a gap to address in a follow-up or next interview stage.

---

### Section 3 — Thank you email

Draft a thank you email to the hiring manager. Rules:
- More substantive than the recruiter email — 3–4 short paragraphs
- Reference specific topics discussed in the interview, not generic PM talking points
- Demonstrate that the user understood what the HM cares about most — reflect their language and priorities back
- If the user offered to send follow-up materials, reference them in the email
- End with a specific forward-looking line tied to the role's challenge or the HM's stated priority
- Use the HM's name if the user provided it; otherwise leave as [Name]
- Subject line format: `Thank you — [Role title], [Company]`

Format the email as a blockquote:
> Subject: ...
>
> [email body]
