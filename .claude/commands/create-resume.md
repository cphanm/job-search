Follow these steps exactly when this command is invoked.

## Setup

1. Read `input/resume.md` in full before doing anything else. This is mandatory.
2. Treat every bullet point as a self-contained impact. The number in one bullet point belongs only to that bullet point — never combine, transfer, or reinterpret numbers across bullet points or activities.
3. Identify which files to process:
   - If an argument is provided (e.g. `/create-resume filename.md`), process only that file in `work-in-progress/`
   - If no argument is provided ($ARGUMENTS is empty), process all `.md` files in `work-in-progress/`
4. **Mixed-model execution**: Steps 1, 2, and 6 run on `claude-sonnet-4-6` — delegate each of those three steps to the Agent tool with `model: claude-sonnet-4-6`, passing all context that step needs in the prompt (the subagent has no access to this conversation). Take the returned output and fold it into the file's output section as normal. Phase 0 and Steps 3, 4, 5, and 7 run inline on the current session model — do not delegate those.

---

## For each job description file, run Phase 0 and all 7 steps below

Append the full output — including your thinking at each step — to the job description file under a new section:

`## Resume Personalisation — YYYY-MM-DD`

Do not overwrite or remove any existing content in the file.

**Append incrementally, after each step, not all at once at the end.** Steps 1, 2, and 6 are delegated to subagents that read the job description file directly rather than having content pasted into their prompt (see each step's Model note) — this only works if every prior step's output is already written to the file by the time the next step runs. Write each step's output to the file as soon as that step completes.

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

**Model: delegate this step via the Agent tool with `model: claude-sonnet-4-6`.** Tell the subagent to Read the job description file (path in `work-in-progress/`) directly — by this point it already contains the JD and Phase 0's Company Research, appended. Do not paste that content into the prompt; just give the subagent the file path and this step's instructions below verbatim. Return the 3 challenges in the format specified and append them to the file's output before moving to Step 2.

Using both the job description and the company research from Phase 0, identify the 3 main challenges the hiring team wants the new hire to solve. These must be specific — grounded in the JD language and the real customer context, not generic PM responsibilities.

Read the entire JD, not just the must-have/responsibilities sections. Lines labelled "nice to have," "bonus," "ideally," "what sets you apart," or "great if you also have" often name what actually differentiates candidates at the screening stage, not just formal requirements — multiple confirmed CV-screen-only rejections have hinged on exactly this kind of line. If one of the 3 challenges is best defined by something sitting in a preferred/bonus section rather than the core requirements, include it anyway — the goal is to capture what the hiring team is actually solving for, not just what's formally labelled required.

The three challenges must be distinct underlying anxieties, not three versions of the same anxiety worded differently. For example, a JD listing "Experience with platform architecture," "Cross-functional stakeholder management," and "Technical API product experience" — the first and third are the same underlying anxiety (technical credibility) in different words; picking both wastes a slot that could cover a genuinely separate fear the hiring team has. **Before finalising the three challenges, check: are any two of them the same underlying anxiety worded differently? If yes, merge them into one and surface a third distinct challenge.**

Format:
- Challenge 1: [title] — [1–2 sentence explanation referencing customer context where relevant]
- Challenge 2: [title] — [1–2 sentence explanation referencing customer context where relevant]
- Challenge 3: [title] — [1–2 sentence explanation referencing customer context where relevant]

---

### Step 2 — Relevant Experiences

**Model: delegate this step via the Agent tool with `model: claude-sonnet-4-6`.** Tell the subagent to Read `input/resume.md`, the job description file (which now also contains Step 1's 3 challenges, appended), and `positioning/differentiators.md` if it exists — directly, not pasted into the prompt. Give it those file paths and this step's instructions/rules below verbatim. Return the evidence matches and the Gaps to flag section, and append them to the file's output before moving to Step 3.

For each challenge, find the most relevant bullet point(s) from `resume.md` that directly address it.

Rules:
- Cite the exact company name as written in the resume
- Quote the exact impact statement, numbers as written — do not round, reword, or combine with another bullet
- If no bullet point directly matches a challenge, say so explicitly rather than forcing a fit
- Before citing a company, identify who it serves: SMB, mid-market, or enterprise. Do not cite an SMB-serving company as evidence for an enterprise requirement
- Governance and compliance keywords in the JD (audit trails, RBAC, policy management, access controls, compliance frameworks) are strong signals — treat them as direct matches to the user's compliance platform and KYC/AML experience documented in resume.md; do not underweight these matches
- The user's enterprise AI feature experience (legal/government/financial institutions, 200k+ DAU — see resume.md) is the primary evidence for enterprise AI product work — use it for roles requiring AI + enterprise audiences
- **Read multi-option requirements by their actual threshold, not full coverage.** If a requirement lists several acceptable options with a threshold word ("experience in a few of the following," "any two of," "one or more of"), check resume.md against that threshold — do not flag it as a gap just because the user's evidence doesn't cover every option in the list. Count how many options are genuinely covered before deciding.
- **Check `positioning/differentiators.md` if it exists.** It lists the user's core differentiator patterns, evidenced across multiple companies and eras in resume.md — check these first before concluding a gap exists. These are rarer and more distinguishing than general adoption/retention/monetization evidence (which is real but closer to table-stakes PM competency). Do not undercount older or less obvious resume.md bullets that fit a listed pattern.
- **Prefer the most specific match, not just an adequate one.** A bullet can be accurately understood and still be the wrong pick if a more specific match exists elsewhere in resume.md. If the JD names a specific mechanism within a challenge (e.g. a particular growth motion, technical approach, or user workflow — not just the general category), search for the bullet that matches that specific mechanism, not merely one that satisfies the challenge's general shape. When two companies could both plausibly answer a challenge, check which one's specific mechanism most tightly matches the JD's own wording before picking — don't default to whichever comes to mind first or was used in a prior draft.
- **Never mint a new fact, count, or statistic by tallying across bullets.** It is fine to describe a pattern in your reasoning prose (e.g. "this sits alongside several other initiatives at the same company, showing breadth"), but do not turn that observation into a number — e.g. counting how many distinct initiatives a company section contains and reporting it as "one of four product surfaces held in parallel." That count does not appear in resume.md; it is a synthesized statistic, and Step 3 will treat anything in your Format output as quotable. If a fact isn't written in resume.md in that form, it cannot appear in the Format output — describe the pattern in prose instead, outside the quoted bullet.

Format:
- Challenge 1 → [Company], [exact bullet point quoted]
- Challenge 2 → [Company], [exact bullet point quoted]
- Challenge 3 → [Company], [exact bullet point quoted]

After matching challenges to resume bullets, add a **Gaps to flag** section: list any specific technical requirements from the JD (e.g. SSO, SOX, RBAC, IAM, CI/CD, specific certifications) that are not explicitly evidenced in `resume.md`. For each gap, note: "Not in resume.md — the user may have real experience here worth adding." Do not silently omit these or assume they don't exist. **Tag each gap as core/must-have or preferred/nice-to-have**, based on which JD section it came from — this classification carries into Step 6's rating logic, so get it right here rather than re-deriving it later.

---

### Step 3 — Personalised Summary

Write a resume summary of exactly 3 sentences:
- **Sentence 1 — Identity**: A one-line statement framing the user's specific positioning for this role. Not "I'm a product manager who..." — name the domain, customer type, or angle that matters for this JD. **Under 20 words.** **When resume.md shows both a credential (degree, certification) and hands-on professional experience for the same underlying skill, frame S1 around the hands-on experience, not the credential alone.** A credential-only phrasing (e.g. "has an engineering degree") undersells someone who also did the work professionally — it reads as a weaker, more passive claim than one grounded in what they actually built or shipped. Check resume.md for a stronger, more concrete fact before defaulting to the credential.
- **Sentence 2 — Evidence**: Exactly three claims, one per company/source, separated by semicolons — no stacking multiple outcomes from the same company into one clause. Each clause follows the format **"at [Company], [impact] by [action]"** — lead with the company name, then the quantified impact, then the method/action that drove it. The **[impact]** (the number) is the strongest proof in the whole summary — it stays in full, exact, never rounded or trimmed. The **[action]** is where conciseness cuts happen: state the mechanism in the fewest words that still make the claim credible, not the full story of how it was delivered. Example: "at [Company], cut time-on-task 60 to 10 minutes by shipping a new AI feature" — not "...by shipping a new AI feature, co-created with customers and Engineers, including RBAC, SSO and SAML" (those extra qualifiers pad the action without adding to the proof). Each of the 3 claims must map 1:1 to one of the 3 challenges identified in Step 1 — the claim must be the evidence that answers that specific challenge, not just any strong claim from that company. Exact numbers and exact company names as written in the resume. **If a bullet's action involved multiple mechanisms (e.g. a technical change plus a process change plus a stakeholder-alignment effort), name only the one most legible to a non-expert reader** — a hiring-team reader skimming a CV screen should not have to parse jargon or a multi-step chain to understand what was done; pick the single mechanism that reads clearly on its own.
- **Sentence 3 — Forward-looking**: One sentence specific to this role and this company's actual problem. Must mirror the company's language or stated challenge — not generic phrases like "seeking a new challenge." Always start with "Looking to" — never "Joining," and never "Looking to join [Company] to [verb]" (a double infinitive); go straight to the work, not the act of joining. **Under 20 words.**

Sentence 2 carries the weight of the summary — keep Sentences 1 and 3 short so the evidence is what stands out, not the framing around it.

Rules:
- Uses exact numbers and impacts as written in the resume (no rounding, no combining, no interpreting)
- Only includes claims supported by a bullet point in `resume.md` — if a gap was flagged in Step 2, do not include it here; the user will decide whether to add it
- **Check across all 3 sentences for redundancy before finalizing.** If S1 already establishes a specific echo (e.g. a same-industry/same-domain match with the target company), don't restate that same point again in a S2 clause or in S3 — make it once, in whichever sentence earns it most, and use the other sentences to add new information instead of repeating it.
- **Named customer/partner logos in an [action] clause are bloat unless the target JD names that customer or an overlapping one.** A brand name (e.g. "co-created with [Partner A], [Partner B], and [Partner C]") competes with the impact number for word count without adding proof — before including one, check whether this specific JD names that company or industry; if not, cut the name and let the number carry the clause.
- **Scale/reach metrics (e.g. "60+ countries", "20+ APIs") are not the same as brand names — keep them when the JD or company homepage emphasises global reach or multi-country/multi-market scale, cut them when the target company serves a single market.** Unlike a customer logo, a reach number is direct evidence of operating at the scale the target company itself claims — check the JD and homepage for language like "global," "worldwide," or a country/market count. If the target company is single-market (e.g. UK-only), the metric isn't evidence of relevant scale and should be cut for length like any other unsupported detail.
- **Every number in a Sentence 2 clause must appear verbatim in the resume.md bullet being quoted — never bolt on a count synthesized elsewhere.** If Step 2's reasoning tallied bullets to make a breadth argument (e.g. "one of four product surfaces held in parallel"), that tally is not itself a resume.md fact and must not be appended to the clause — even if it is directionally true. If a challenge is fundamentally about breadth across multiple initiatives, make that argument in Sentence 1's framing (which is not a quoted claim) rather than inventing a number inside a Sentence 2 evidence clause.

---

### Step 4 — Pros and Cons

Analyse the summary from Step 3 in the context of this specific job description.

- Pros: what it does well, what will resonate with the hiring team. Explicitly name any met preferred/nice-to-have requirement as a differentiator here — it's real signal, not filler, and should not stay buried until Step 6. If the JD's preferred/"what sets you apart" language overlaps with one of the core differentiator patterns checked in Step 2, flag it as the strongest possible differentiator, not just a met bonus requirement.
- Cons: what is missing, weak, overstated, or could mislead. If an unmet preferred/nice-to-have requirement is emphasized or repeated elsewhere in the JD, or is the JD's single named standout differentiator, flag it here explicitly — it carries real screening risk even though it isn't a core requirement.

Be direct and specific. No hedging.

---

### Step 5 — 3 Alternatives

Every summary (baseline and alternatives) must first pass these baseline requirements — if any are not met, rewrite until they are:
- Uses only exact claims from `resume.md` (exact numbers, exact company names, no invention)
- Addresses all 3 challenges from Step 1
- Identity framing matches both the JD and the user's actual experience
- Forward-looking sentence is specific to this role and the user's trajectory
- Exactly 3 sentences: S1 identity (under 20 words), S2 three evidence claims from three different companies/sources (one per clause, semicolon-separated, each following the "at [Company], [impact] by [action]" format — impact in full, action trimmed to the fewest credible words, naming only the single most legible mechanism when a bullet involves several — each mapped 1:1 to one of the 3 Step 1 challenges), S3 forward-looking (under 20 words)
- No point is made twice across the three sentences — e.g. if S1 already names a same-industry/same-domain match, a S2 clause echoing that same match again (rather than adding new information) is redundant and should be cut

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

**Every alternative (Option A, B, C) must score at least 8.5/10** — Option 0 is the unoptimized baseline and is exempt. If an alternative scores below 8.5, revise it once more and rescore. If it is still below 8.5 after that one revision, state explicitly why — e.g. a real gap or constraint in the underlying evidence that no amount of rewording can fix — rather than leaving a low score unexplained.

---

### Step 6 — Fit Assessment

**Model: delegate this step via the Agent tool with `model: claude-sonnet-4-6`.** Tell the subagent to Read the job description file directly — by this point it contains the JD, Phase 0's Company Research, and Step 2's Gaps to flag section (with core/preferred tags) — and `positioning/differentiators.md` if it exists, not pasted into the prompt. Give it the file paths and this step's instructions/rubric below verbatim. Return the business model verdict, the Fit/Stretch/Out of Reach rating, the fit basis, and the reasoning bullets, and append them to the file's output.

First, run a two-step business model check:

**Step A — What does the PM in this role actually build for?**
Look at the responsibilities and primary user type from Phase 0, not the company's overall label. Determine which of these applies:
- The PM owns a product built for **business customers or internal users** (e.g. enterprise SaaS, internal tooling, developer platform, B2B workflows) → no business model gap
- The PM owns a product built for **consumers, including SMB buyers transacting through a self-serve/direct funnel** (e.g. consumer-facing features, acquisition funnels, end-user growth) → check resume.md for direct evidence of owning a consumer or SMB self-serve acquisition/product motion. If resume.md shows such evidence, there is no gap here. If resume.md shows no such evidence, treat it as a business model gap.
- The PM owns a product built for **marketplace participants with two-sided buyer/seller dynamics** (matching, liquidity, or brokering both sides of a transaction) → business model gap (no marketplace-specific evidence in resume.md)
- **Government or public-sector agencies are the primary paying customer** (B2G, or B2B2G where government/public-sector procurement drives the sale, even if end-users are internal staff) → check resume.md for direct evidence of navigating a government or public-sector sales/procurement motion. Building for public-sector *end-users* does not by itself resolve this — the gap, if it exists, is in the sales/procurement motion, not the user experience. If resume.md shows no such evidence, treat it as a business model gap. If resume.md does show direct public-sector sales/procurement experience, there is no gap here.

**Step B — State the verdict on its own line before the rating:**
- `Business model: B2B — no gap` / `B2B2C — no gap` / `B2C — gap (no consumer/SMB self-serve evidence in resume.md)` or `B2C — no gap (resume.md shows direct consumer/SMB self-serve evidence)` / `Marketplace — gap` / `B2G — gap` or `B2G — no gap (resume.md shows direct public-sector sales/procurement experience)`
- If the company has a mixed model, identify which side the PM role sits on and state it explicitly: e.g. `Business model: Company is B2B2C; PM role owns the B2B merchant side — no gap` or `Business model: Company is B2B2C; PM role owns the consumer-facing side — gap (no consumer/SMB self-serve evidence in resume.md)` or `Business model: Company is B2B2G; government/public-sector procurement is the primary sale — gap (no public-sector sales/procurement evidence in resume.md)` or `Business model: Company is B2B2G; government/public-sector procurement is the primary sale — no gap (resume.md shows direct public-sector sales/procurement experience)`

**Step C — Apply the gap:**
If a gap is identified, downgrade the rating by one level: Fit → Stretch, Stretch → Out of Reach.

Then, distinguish between **core requirements** (stated as must-have, or repeated across multiple JD sections) and **preferred requirements** (labelled "bonus," "ideally," "nice to have," "what sets you apart," "great if you also have," or listed last) — use the core/preferred tags already assigned in Step 2's Gaps to flag section rather than re-deriving this from scratch. Determine the base rating from core requirements only.

**Then factor preferred requirements as a bonus, not as noise.** Multiple confirmed CV-screen-only rejections shared the same shape: core requirements were fully met, the rating came out Fit or Stretch, and the actual reason for rejection sat entirely in a preferred/"nice to have" line the rating had excluded. In practice, hiring teams use these lines as real screening filters, not soft extras — especially when a role's first pipeline stage is a CV/application review with no human contact before it. Apply this explicitly:
- If the user meets a preferred requirement, name it in the summary or pros — it's a real differentiator, not filler.
- If the user does **not** meet a preferred requirement, check how it's framed: if it's the JD's single named standout differentiator (e.g. a "What Sets You Apart" section naming one thing), or repeated/emphasized elsewhere in the JD, downgrade the rating by one level even though it's not formally "core." Treat it as functionally core for CV-screen purposes.
- If it's one item in a long list of many nice-to-haves with no special emphasis, it's lower risk — note it as a gap but don't downgrade solely on that basis.

State clearly on the first line: **Fit** / **Stretch** / **Out of Reach**

Then state the fit basis on the second line: **Direct domain match** / **Transferable skills** / **Mixed**
- **Direct domain match** — the domain and primary user type both transfer directly from the user's background; no mental leap required from the hiring team
- **Transferable skills** — the skills apply but require a mental leap on domain, primary user type, or both; the bridge is visible but not obvious
- **Mixed** — some requirements are a direct match, others rely on transferable skills; identify which is which

**Do not call it "Direct domain match" if the match relies on an analogy.** Confirmed CV-screen-only rejections have followed the same pattern: a role was rated "Fit — Direct domain match" using a bridge that required treating one resume bullet as equivalent to the JD's actual domain via a step of interpretation — e.g. an internal technical system standing in for a customer-facing product in an unrelated domain, or a platform built for one industry standing in for a specialised tool in a different one. If reaching the domain match requires that kind of translation rather than the bullet stating the same domain or mechanism outright, cap the fit basis at **Mixed** (or **Transferable skills**), not Direct — and don't let that capped basis alone still round the overall rating up to Fit.

**Weight personal/career-break projects as motivation evidence, not professional-experience evidence, when a core requirement asks for hands-on technical/builder capability.** When the JD's core ask is specifically "confidence to write code," "create PRs," "prototype products," or similar, and the only resume.md evidence is a personal project or career-break build (not a paid PM/engineering role), note explicitly that this is real but weaker-tier evidence than a professionally shipped bullet — it does not by itself close a core gap enough to justify a Fit rating. The same caution applies whenever a personal side project is the only evidence offered for a professional domain gap — it doesn't resolve it.

**Check `positioning/differentiators.md` for a combined pattern before citing an industry-label mismatch as an unbridgeable domain gap.** Some roles' real core ask is not a single domain but a specific combination of two capabilities (e.g. a technical function plus an industry context, or two domains together) — a combination most candidates have on at most one axis. `positioning/differentiators.md` may document such a combined pattern across two or more resume.md bullets from different roles/eras. Before treating "the target company's named industry has no direct resume.md precedent" as decisive grounds for a downgrade, check whether the role's actual core ask matches a documented combined pattern. If it does, cite the combination explicitly in the reasoning (not just whichever half looks more topically adjacent) — the combination itself, not either half alone, is the evidence that closes the gap. Don't let a company's product-category label override a role's actual function when a combined pattern already covers that function.

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
- **Check evidence reuse across all questions in this file before drafting any answer.** If the same company or bullet is the strongest fit for two different questions, don't draft both in isolation. Either pick a different, still-genuine example for one of them, or flag the overlap explicitly and ask which question should keep it. A reader reviewing all the answers together will notice if the same story appears twice.

Format per question:
#### Q[n]: [question text]
**Relevant bullets:** ...
**Drafted answer:** ...
