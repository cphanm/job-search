Follow these steps exactly when this command is invoked.

## Setup

1. Read `original/resume.md` in full before doing anything else. This is mandatory.
2. Treat every bullet point as a self-contained impact. The number in one bullet point belongs only to that bullet point — never combine, transfer, or reinterpret numbers across bullet points or activities.
3. Identify which files to process:
   - If an argument is provided (e.g. `/create-resume filename.md`), process only that file in `work-in-progress/`
   - If no argument is provided ($ARGUMENTS is empty), process all `.md` files in `work-in-progress/`

---

## For each job description file, run all 6 steps below

Append the full output — including your thinking at each step — to the job description file under a new section:

`## Resume Personalisation — YYYY-MM-DD`

Do not overwrite or remove any existing content in the file.

---

### Phase 0 — Company Research

Before identifying challenges, look for the following line at the top of the job description file:

```
Homepage: https://...
```

Fetch the homepage URL. From the fetched content, extract:
- **Industry** — what sector the company operates in
- **Target customers** — who they sell to (e.g. enterprise security teams, SMB, developers, legal firms)
- **Product** — what the product does in plain terms
- **Scale/stage** — size, growth stage, or any signals about their customer base

Store this as a **Company Research** section in the output before Step 1. If a URL fails to load or returns no useful content, note it and continue — do not block on it.

Use the company research to sharpen Step 1: challenges should reflect not just the JD language but the actual customers and market the company serves.

---

### Step 1 — 3 Challenges

Using both the job description and the company research from Phase 0, identify the 3 main challenges the hiring team wants the new hire to solve. These must be specific — grounded in the JD language and the real customer context, not generic PM responsibilities.

Format:
- Challenge 1: [title] — [1–2 sentence explanation referencing customer context where relevant]
- Challenge 2: [title] — [1–2 sentence explanation referencing customer context where relevant]
- Challenge 3: [title] — [1–2 sentence explanation referencing customer context where relevant]

---

### Step 2 — Relevant Experiences

For each challenge, find the most relevant bullet point(s) from `resume.md` that directly address it.

Rules:
- Cite the exact company name as written in the resume
- Quote the exact impact statement, numbers as written — do not round, reword, or combine with another bullet
- If no bullet point directly matches a challenge, say so explicitly rather than forcing a fit
- Before citing a company, identify who it serves: SMB, mid-market, or enterprise. Do not cite an SMB-serving company as evidence for an enterprise requirement
- Governance and compliance keywords in the JD (audit trails, RBAC, policy management, access controls, compliance frameworks) are strong signals — treat them as direct matches to Chi's compliance platform work at Bandwidth and KYC/AML work at Legalzoom; do not underweight these matches
- AI feature experience at Vable (enterprise, legal/government/financial institutions, 200k+ DAU) is the primary evidence for enterprise AI product work — use it for roles requiring AI + enterprise audiences

Format:
- Challenge 1 → [Company], [exact bullet point quoted]
- Challenge 2 → [Company], [exact bullet point quoted]
- Challenge 3 → [Company], [exact bullet point quoted]

After matching challenges to resume bullets, add a **Gaps to flag** section: list any specific technical requirements from the JD (e.g. SSO, SOX, RBAC, IAM, CI/CD, specific certifications) that are not explicitly evidenced in `resume.md`. For each gap, note: "Not in resume.md — Chi may have real experience here worth adding." Do not silently omit these or assume they don't exist.

---

### Step 3 — Personalised Summary

Write a resume summary (3–4 sentences maximum) that:
- Opens with a one-line identity statement (not "I'm a product manager who...") — frame Chi's specific positioning relevant to this role, e.g. "Platform Product Manager (9+ years) building X that delivers Y"
- Names specific companies from the resume
- Uses exact numbers and impacts as written in the resume (no rounding, no combining, no interpreting)
- Directly speaks to the 3 challenges from Step 1
- Only includes claims supported by a bullet point in `resume.md` — if a gap was flagged in Step 2, do not include it here; Chi will decide whether to add it
- Ends with one forward-looking sentence stating what Chi is looking for next — specific to this role, derived from Chi's career trajectory; avoid generic phrases like "seeking a new challenge"

---

### Step 4 — Pros and Cons

Analyse the summary from Step 3 in the context of this specific job description.

- Pros: what it does well, what will resonate with the hiring team
- Cons: what is missing, weak, overstated, or could mislead

Be direct and specific. No hedging.

---

### Step 5 — 3 Alternatives

Every alternative must first pass these baseline requirements — if any are not met, rewrite until they are:
- Uses only exact claims from `resume.md` (exact numbers, exact company names, no invention)
- Addresses all 3 challenges from Step 1
- Identity framing matches both the JD and Chi's actual experience
- Forward-looking sentence is specific to this role and Chi's trajectory

Once the baseline is met, score each alternative out of 10 across 3 dimensions:

| Dimension | Points | What scores high | What scores low |
|---|---|---|---|
| Conciseness | 3 | Every word earns its place, no padding, no repetition | Verbose, restates the same point, filler phrases |
| Punchiness | 4 | Strong identity opening, impact-led sentences, direct language | Weak opening, hedging language, buries the lead |
| No-brainer clarity | 3 | Hiring team sees the fit in one read, no interpretation needed | Requires mental leaps, ambiguous framing, generic enough to fit any PM |

For each alternative:
- Write the full summary
- Score: [X]/10 — Conciseness [x]/3 · Punchiness [x]/4 · Clarity [x]/3
- Explain in 1–2 sentences what drives the score up or down

---

### Step 6 — Fit Assessment

First, distinguish between **core requirements** (stated as must-have, or repeated across multiple JD sections) and **preferred requirements** (labelled "bonus," "ideally," or listed last). Apply the rating to core requirements only.

State clearly on the first line: **Fit** / **Stretch** / **Out of Reach**

| Rating | Criteria |
|---|---|
| **Fit** | Meets all core requirements with direct resume evidence; domain overlap is direct not adjacent; experience level matches or exceeds |
| **Stretch** | Meets most core requirements (at least 2 of 3); domain is adjacent and the bridge is visible to a hiring team; gaps exist but are learnable; Chi has cleared at least recruiter stage in similar roles |
| **Out of Reach** | Missing one or more core requirements that cannot be bridged by transferable skills; domain gap is too wide; JD requires specific credentials or background with no equivalent in Chi's resume |

Then explain the reasoning in 3–5 bullet points, comparing Chi's actual background against the core requirements of this role. Base this only on what is in the resume — do not assume experience that is not evidenced.

---

### Step 7 — Application Form Questions

Check the job description file for an "Application Form Questions" section. If it is empty or absent, skip this step entirely.

For each question present, produce two things:

**A — Relevant bullets**
List the resume bullets from `resume.md` that best answer this question. Cite exact company name and exact impact statement. If no bullet directly applies, say so and flag it as a gap — Chi may have real experience not yet in the resume.

**B — Drafted answer**
Write a complete answer in first person as Chi. Rules:
- Ground every claim in a resume bullet — exact numbers, exact company names, no invention
- Be concise: answer the question directly, do not pad
- If the question is behavioural ("tell me about a time when..."), structure the answer as: situation → what Chi did → the measurable outcome
- If a gap was flagged in A, do not include it in the drafted answer — note at the end that Chi may want to add it if the experience is real
- **Match the example's market segment to the company's GTM model.** If the company is sales-led and enterprise, use examples from Bandwidth, Vable, or Pearson — not Legalzoom (PLG, SMB). Using an SMB/PLG example for a sales-led enterprise company signals the wrong GTM mental model to the hiring manager, even if the mechanics are similar.
- **Match the example's shape to the question's shape.** A question about "stopping" a low-value item needs an example where something was stopped or clearly deprioritised — not deferred or rescoped. A question about "moving the needle commercially" needs a commercial product outcome — not an internal operations improvement. If the best available example is a partial match, flag the mismatch and note it before drafting.
- **Pre-defined measurement matters.** "Proved it with evidence" means the measurement framework was in place before launch, not retrospectively applied. Make this explicit in the answer when it is true.

Format per question:
#### Q[n]: [question text]
**Relevant bullets:** ...
**Drafted answer:** ...
