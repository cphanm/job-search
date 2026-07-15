Follow these steps exactly when this command is invoked.

## Setup

1. Read `input/resume.md` in full before doing anything else. This is mandatory.
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
- **Business model** — B2B, B2B2C, B2C, or marketplace; identify who pays and who uses the product; if mixed, say which is primary
- **Target customers** — who they sell to (e.g. enterprise security teams, SMB, developers, legal firms)
- **Product** — what the product does in plain terms
- **Scale/stage** — size, growth stage, or any signals about their customer base
- **Primary user type** — who the PM in this role will be building for primarily (e.g. developers, enterprise ops teams, consumer end users, internal teams, compliance officers); this is not the same as target customers — it is the specific user audience the PM owns day-to-day

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
- Governance and compliance keywords in the JD (audit trails, RBAC, policy management, access controls, compliance frameworks) are strong signals — treat them as direct matches to the user's compliance platform and KYC/AML experience documented in resume.md; do not underweight these matches
- The user's enterprise AI feature experience (legal/government/financial institutions, 200k+ DAU — see resume.md) is the primary evidence for enterprise AI product work — use it for roles requiring AI + enterprise audiences

Format:
- Challenge 1 → [Company], [exact bullet point quoted]
- Challenge 2 → [Company], [exact bullet point quoted]
- Challenge 3 → [Company], [exact bullet point quoted]

After matching challenges to resume bullets, add a **Gaps to flag** section: list any specific technical requirements from the JD (e.g. SSO, SOX, RBAC, IAM, CI/CD, specific certifications) that are not explicitly evidenced in `resume.md`. For each gap, note: "Not in resume.md — the user may have real experience here worth adding." Do not silently omit these or assume they don't exist.

---

### Step 3 — Personalised Summary

Write a resume summary of exactly 3 sentences:
- **Sentence 1 — Identity**: A one-line statement framing the user's specific positioning for this role. Not "I'm a product manager who..." — name the domain, customer type, or angle that matters for this JD.
- **Sentence 2 — Evidence**: Exactly three claims, each from a different company or source, separated by semicolons. Exact numbers and exact company names as written in the resume. One claim per clause — do not stack multiple outcomes from the same company into one clause.
- **Sentence 3 — Forward-looking**: One sentence specific to this role and this company's actual problem. Must mirror the company's language or stated challenge — not generic phrases like "seeking a new challenge." Always start with "Looking to" — never "Joining"; the user is applying, not starting.

Rules:
- Uses exact numbers and impacts as written in the resume (no rounding, no combining, no interpreting)
- Only includes claims supported by a bullet point in `resume.md` — if a gap was flagged in Step 2, do not include it here; the user will decide whether to add it

---

### Step 4 — Pros and Cons

Analyse the summary from Step 3 in the context of this specific job description.

- Pros: what it does well, what will resonate with the hiring team
- Cons: what is missing, weak, overstated, or could mislead

Be direct and specific. No hedging.

---

### Step 5 — 3 Alternatives

Every summary (baseline and alternatives) must first pass these baseline requirements — if any are not met, rewrite until they are:
- Uses only exact claims from `resume.md` (exact numbers, exact company names, no invention)
- Addresses all 3 challenges from Step 1
- Identity framing matches both the JD and the user's actual experience
- Forward-looking sentence is specific to this role and the user's trajectory
- Exactly 3 sentences: S1 identity, S2 three evidence claims from three different companies/sources (one per clause, semicolon-separated), S3 forward-looking

Open this step by reproducing the Step 3 summary verbatim as **Option 0 — Baseline** and scoring it using the same rubric below. Then produce three alternatives. All four are scored in this step so the user can compare them directly.

Once the baseline is met, score each summary out of 10 across 3 dimensions:

| Dimension | Points | What scores high | What scores low |
|---|---|---|---|
| Conciseness | 3 | Every word earns its place, no padding, no repetition | Verbose, restates the same point, filler phrases |
| Punchiness | 4 | Strong identity opening, impact-led sentences, direct language | Weak opening, hedging language, buries the lead |
| No-brainer clarity | 3 | Hiring team sees the fit in one read, no interpretation needed | Requires mental leaps, ambiguous framing, generic enough to fit any PM |

For each summary (Option 0 through Option C):
- Write the full summary
- Score: [X]/10 — Conciseness [x]/3 · Punchiness [x]/4 · Clarity [x]/3
- Explain in 1–2 sentences what drives the score up or down

---

### Step 6 — Fit Assessment

First, run a two-step business model check:

**Step A — What does the PM in this role actually build for?**
Look at the responsibilities and primary user type from Phase 0, not the company's overall label. Determine which of these applies:
- The PM owns a product built for **business customers or internal users** (e.g. enterprise SaaS, internal tooling, developer platform, B2B workflows) → no business model gap
- The PM owns a product built for **consumers or marketplace participants** (e.g. consumer-facing features, acquisition funnels, end-user growth, buyer/seller dynamics) → business model gap

**Step B — State the verdict on its own line before the rating:**
- `Business model: B2B — no gap` / `B2B2C — no gap` / `B2C — gap` / `Marketplace — gap`
- If the company has a mixed model, identify which side the PM role sits on and state it explicitly: e.g. `Business model: Company is B2B2C; PM role owns the B2B merchant side — no gap` or `Business model: Company is B2B2C; PM role owns the consumer-facing side — gap`

**Step C — Apply the gap:**
If a gap is identified, downgrade the rating by one level: Fit → Stretch, Stretch → Out of Reach.

Then, distinguish between **core requirements** (stated as must-have, or repeated across multiple JD sections) and **preferred requirements** (labelled "bonus," "ideally," "nice to have," "what sets you apart," "great if you also have," or listed last). Determine the base rating from core requirements only.

**Then factor preferred requirements as a bonus, not as noise.** Multiple confirmed CV-screen-only rejections shared the same shape: core requirements were fully met, the rating came out Fit or Stretch, and the actual reason for rejection sat entirely in a preferred/"nice to have" line the rating had excluded. In practice, hiring teams use these lines as real screening filters, not soft extras — especially when a role's first pipeline stage is a CV/application review with no human contact before it. Apply this explicitly:
- If the user meets a preferred requirement, name it in the summary or pros — it's a real differentiator, not filler.
- If the user does **not** meet a preferred requirement, check how it's framed: if it's the JD's single named standout differentiator (e.g. a "What Sets You Apart" section naming one thing), or repeated/emphasized elsewhere in the JD, downgrade the rating by one level even though it's not formally "core." Treat it as functionally core for CV-screen purposes.
- If it's one item in a long list of many nice-to-haves with no special emphasis, it's lower risk — note it as a gap but don't downgrade solely on that basis.

State clearly on the first line: **Fit** / **Stretch** / **Out of Reach**

Then state the fit basis on the second line: **Direct domain match** / **Transferable skills** / **Mixed**
- **Direct domain match** — the domain and primary user type both transfer directly from the user's background; no mental leap required from the hiring team
- **Transferable skills** — the skills apply but require a mental leap on domain, primary user type, or both; the bridge is visible but not obvious
- **Mixed** — some requirements are a direct match, others rely on transferable skills; identify which is which

| Rating | Criteria |
|---|---|
| **Fit** | Meets all core requirements with direct resume evidence; domain overlap is direct not adjacent; experience level matches or exceeds; business model is B2B or B2B2C |
| **Stretch** | Meets most core requirements (at least 2 of 3); domain is adjacent and the bridge is visible to a hiring team; gaps exist but are learnable; the user has cleared at least recruiter stage in similar roles; no B2C or marketplace business model gap |
| **Out of Reach** | Missing one or more core requirements that cannot be bridged by transferable skills; domain gap is too wide; JD requires specific credentials or background with no equivalent in the user's resume; OR business model is primarily B2C or marketplace |

Then explain the reasoning in 3–5 bullet points, comparing the user's actual background against the core requirements of this role. Base this only on what is in the resume — do not assume experience that is not evidenced.

---

### Step 7 — Application Form Questions

Check the job description file for an "Application Form Questions" section. If it is empty or absent, skip this step entirely.

**Before drafting any answer:** Read `input/story-library.md` in full. It contains the user's full behavioral story library (S1–S14 and supplementary stories) with strategic framing, narrative shape, tactics, and context that resume.md does not have. Use story-library.md to find the best story fit for each question, then cross-check exact numbers against resume.md before writing. Do not draft answers from resume.md alone.

For each question present, produce two things:

**A — Relevant bullets**
List the resume bullets from `resume.md` that best answer this question. Cite exact company name and exact impact statement. If no bullet directly applies, say so and flag it as a gap — the user may have real experience not yet in the resume.

**B — Drafted answer**
Write a complete answer in first person as the user. Rules:
- **Under 300 words.** Count and confirm before writing. Cut until every sentence earns its place.
- Ground every claim in a resume bullet — exact numbers, exact company names, no invention
- Be concise: answer the question directly, do not pad
- If the question is behavioural ("tell me about a time when..."), structure the answer as: situation → what the user did → the measurable outcome. Use the story-library.md version of the story as the narrative spine — it has the strategic framing and action detail that resume.md bullet points compress away
- If a gap was flagged in A, do not include it in the drafted answer — note at the end that the user may want to add it if the experience is real
- **Match the example's market segment to the company's GTM model.** Check resume.md for which of the user's roles were sales-led/enterprise vs. PLG/SMB, and pick the example accordingly. Using an SMB/PLG example for a sales-led enterprise company signals the wrong GTM mental model to the hiring manager, even if the mechanics are similar.
- **Match the example's shape to the question's shape.** A question about "stopping" a low-value item needs an example where something was stopped or clearly deprioritised — not deferred or rescoped. A question about "moving the needle commercially" needs a commercial product outcome — not an internal operations improvement. If the best available example is a partial match, flag the mismatch and note it before drafting.
- **Pre-defined measurement matters.** "Proved it with evidence" means the measurement framework was in place before launch, not retrospectively applied. Make this explicit in the answer when it is true.

Format per question:
#### Q[n]: [question text]
**Relevant bullets:** ...
**Drafted answer:** ...
