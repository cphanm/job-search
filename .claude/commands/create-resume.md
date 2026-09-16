Follow these steps exactly when this command is invoked.

## Setup

1. Read `input/resume.md` in full before doing anything else. This is mandatory.
2. Treat every bullet point as a self-contained impact. The number in one bullet point belongs only to that bullet point — never combine, transfer, or reinterpret numbers across bullet points or activities.
3. Identify which files to process:
   - If an argument is provided (e.g. `/create-resume filename.md`), process only that file in `work-in-progress/`
   - If no argument is provided ($ARGUMENTS is empty), process all `.md` files in `work-in-progress/`
4. **All 7 steps run inline on the current session model — no Agent delegation.** Before starting Step 1, adopt this framing for Steps 1-3 (Challenges, Relevant Experiences, Fit Assessment): *"You are a senior product leader with 40 years of experience hiring and being hired for product management roles. Bring that judgment to these three steps."* Reason through each of Steps 1-3 out loud before writing its formatted output — think like a senior product leader weighing the evidence, not like a checklist. Write each step's output directly to the job description file (append, never overwrite existing content) as soon as that step completes, before moving to the next.

---

## For each job description file, run Phase 0 and all 7 steps below

Append the full output — including your thinking at each step — to the job description file under a new section:

`## Resume Personalisation — YYYY-MM-DD`

Do not overwrite or remove any existing content in the file.

**Append incrementally, after each step, not all at once at the end.** All 7 steps run inline on the current session model (see Setup point 4). Write each step's output to the job description file as soon as that step completes, before moving to the next — Step 2 needs Step 1's challenges already in the file, and Step 3 needs Step 2's Gaps-to-flag section already in the file.

**Steps run in this order deliberately:** the Challenges, Relevant Experiences, and Fit Assessment steps come first because Fit Assessment only depends on those two plus Phase 0 — not on the summary, pros/cons, or alternatives. Finding out a role is Out of Reach happens before any time is spent writing and scoring a polished summary for it, not after.

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

Store this as a **Company Research** section in the output before the Challenges step. If a URL fails to load or returns no useful content, note it and continue — do not block on it.

Use the company research to sharpen the Challenges step: challenges should reflect not just the JD language but the actual customers and market the company serves.

---

### Step 1 — 3 Challenges

**Runs inline on the current session model, with the senior-product-leader framing from Setup point 4.** By this point the job description file already contains the JD and Phase 0's Company Research, appended — write this step's output to the file as soon as it's ready, before moving on to Step 2.

Using both the job description and the company research from Phase 0, identify the 3 main challenges the hiring team wants the new hire to solve. These must be specific — grounded in the JD language and the real customer context, not generic PM responsibilities.

Read the entire JD, not just the must-have/responsibilities sections. Lines labelled "nice to have," "bonus," "ideally," "what sets you apart," or "great if you also have" often name what actually differentiates candidates at the screening stage, not just formal requirements — multiple confirmed CV-screen-only rejections have hinged on exactly this kind of line. If one of the 3 challenges is best defined by something sitting in a preferred/bonus section rather than the core requirements, include it anyway — the goal is to capture what the hiring team is actually solving for, not just what's formally labelled required.

**Distinguish genuine requirement lines from auto-generated tag or keyword lists.** Some JDs end with a numbered or bulleted list of short topic phrases that just repeat themes already covered in the prose above (e.g. "1. [Skill area]. 2. [Skill area]. 3. [Skill area]..."). This is typically ATS/aggregator-generated metadata summarizing what the posting is about, not a menu of acceptable candidate backgrounds written by the hiring team. Read these as topic labels, not requirements with "or" logic offering the candidate a substitute path — e.g. a tag like "[Target industry] Knowledge or Niche Market Expertise" most likely describes the company's own niche (the target industry is itself a niche market), not an offer that any niche experience satisfies it. If a tag repeats a theme already stated in prose elsewhere in the JD, treat the prose version as the source of truth and the tag as redundant, not as new evidence of a distinct requirement.

The three challenges must be distinct underlying anxieties, not three versions of the same anxiety worded differently. For example, a JD listing "Experience with platform architecture," "Cross-functional stakeholder management," and "Technical API product experience" — the first and third are the same underlying anxiety (technical credibility) in different words; picking both wastes a slot that could cover a genuinely separate fear the hiring team has. **Before finalising the three challenges, check: are any two of them the same underlying anxiety worded differently? If yes, merge them into one and surface a third distinct challenge.**

**State each title as the hiring team's fear, not the PM's task, in under 12 words.** A title that just renames a Responsibilities or Skills line (e.g. turning "own end-to-end discovery for our new market" into "Rigorous discovery in a new market") is a paraphrase, not yet a challenge. Ask: does this title name what breaks, stalls, or gets missed if they hire the wrong person — or does it just hand the JD's own words back to them? If it's the latter, rewrite it as the risk. A title long enough to carry its own justification (e.g. "Discovery velocity collapses back to 'months' if the PM cannot build working prototypes with AI tools without engineering support") is doing the explanation sentence's job twice — name the fear in a short phrase, save the reasoning for the explanation that follows. Count the words before finalizing.

**Every factual claim in a challenge's explanation must trace to a specific JD clause — cite it with a short phrase and section name, or explicitly mark the sentence as inference.** Do not state a consequence, mechanism, or numeric claim as if it's from the JD unless it is. Before finalising each challenge, go sentence by sentence: is this a direct paraphrase of something the JD says, or is it something being inferred? If inferring, either cut it or flag it clearly as inference (e.g. "not stated directly, but implied by: '[quote]'") rather than stating it with confident language ("likely," "almost certainly," "confirms") that makes it read as fact. Do not attach a named customer/logo to a challenge unless the JD explicitly connects that customer to the challenge — a logo listed elsewhere as a general trust signal is not evidence for a specific claim. Do not reuse a stat from one part of the JD (e.g. a platform-wide number) to support an unrelated specific claim (e.g. implying it applies per-merchant) unless the JD itself states that connection. Avoid inventing "X, not Y" contrasts that aren't present in the JD.

**A challenge's title cannot be more specific than a named, checked source supports.** If the JD itself is vague about a challenge's mechanism (e.g. "anticipate future trends" without naming which trends), it's fine to sharpen it with a specific inference — but only if that inference traces to a named source that actually contains it: a JD clause (quoted), or a specific Phase 0 Company Research finding (e.g. "homepage's product roadmap section names AI-assisted analysis as a stated direction"). Before citing Phase 0 as the source, re-read Phase 0's own text and confirm the signal is actually there — don't cite Phase 0 by name for an inference Phase 0 didn't itself surface, and don't cite "general market context" as a source, since it isn't a checked one. Every inferred specificity needs a one-line origin note (e.g. "Sourced from homepage: [what it said]" or "Sourced from JD: [quote]") so a reader can see where the leap came from, not just that a leap was made. If no named, verified source supports the added specificity, keep the challenge title and explanation at the JD's own level of generality rather than inventing a mechanism.

**Each Grounded-in claim goes on its own bulleted line — never joined with semicolons into a single line.** This is a hard formatting rule, not a stylistic suggestion: even when every claim is short, one merged line reads as a wall of text; separate lines read at a glance.

Format (count words/sentences before finalizing each challenge — this step is read at a glance, not studied):
- Challenge 1: [title, under 12 words] — [1 sentence explanation, ~20-30 words]
  Grounded in:
  - [3-6 word phrase + JD section name, one claim per line, e.g. "Lovable/Bolt prototyping tools — Skills" — not a quoted clause; mark any inference explicitly]
  - [next claim, same format, own line]
- Challenge 2: [title, under 12 words] — [1 sentence explanation, ~20-30 words]
  Grounded in:
  - [3-6 word phrase + JD section name, one claim per line, e.g. "Lovable/Bolt prototyping tools — Skills" — not a quoted clause; mark any inference explicitly]
  - [next claim, same format, own line]
- Challenge 3: [title, under 12 words] — [1 sentence explanation, ~20-30 words]
  Grounded in:
  - [3-6 word phrase + JD section name, one claim per line, e.g. "Lovable/Bolt prototyping tools — Skills" — not a quoted clause; mark any inference explicitly]
  - [next claim, same format, own line]

---

### Step 2 — Relevant Experiences

**Runs inline on the current session model, with the senior-product-leader framing from Setup point 4.** `input/resume.md`, the job description file (which now also contains Step 1's 3 challenges, just written), and `positioning/differentiators.md` (if it exists) are already in context — write this step's output to the file as soon as it's ready, before moving on to Step 3.

For each challenge, find the most relevant bullet point(s) from `resume.md` that directly address it.

Rules:
- Cite the exact company name as written in the resume
- Quote the bullet's exact wording and numbers as written — do not round, reword, paraphrase, or combine with another bullet
- **Truncate the quote — do not reproduce the full bullet.** Chi knows her own resume; a full re-quote isn't needed to identify it. Quote only the first ~10-15 words (usually where the impact/number sits) and the last ~10-15 words (usually the mechanism), joined by an ellipsis. Never cut through the middle of a number or company name — if the impact number falls outside the first ~15 words, extend the opening fragment to include it rather than dropping it.
- If no bullet point directly matches a challenge, say so explicitly rather than forcing a fit
- Before citing a company, identify who it serves: SMB, mid-market, or enterprise. Do not cite an SMB-serving company as evidence for an enterprise requirement
- Governance and compliance keywords in the JD (audit trails, RBAC, policy management, access controls, compliance frameworks) are strong signals — treat them as direct matches to the user's compliance platform and KYC/AML experience documented in resume.md; do not underweight these matches
- The user's enterprise AI feature experience (legal/government/financial institutions, 200k+ DAU — see resume.md) is the primary evidence for enterprise AI product work — use it for roles requiring AI + enterprise audiences
- **GTM model precision.** resume.md's own "Tools & environment" lines label each company's motion — don't collapse them into one "PLG" bucket. Some companies are labelled bottom-up, self-serve PLG explicitly; others are labelled "GTM for Enterprise" or "GTM for Enterprise and Mid-market" — the latter's self-serve activation mechanics (docs, API specs/collections, tutorial videos) still sit inside an enterprise/mid-market motion, not bottom-up PLG. Label evidence accordingly — "product-led sales" or "self-serve within an enterprise motion" for the latter, never "PLG." When a JD's PLG ask is itself enterprise-scale, an enterprise-labelled company's self-serve motion is usually the closer GTM match than a company formally labelled PLG but serving SMB — don't default to the PLG label just because it says PLG.
- **Check what "self-serve" actually describes before citing it — get the mechanism right, not just the keyword.** When a bullet describes an adoption model as "self-serve" via API docs/specs/collections/tutorial videos, that describes API/developer self-serve — engineers integrating via documentation, no sales call — not a business-user-facing UI. Do not cite that kind of evidence for a UI/portal self-service surface (e.g. an admin dashboard or customer portal); it only bridges the API-adoption half of that kind of ask. Also check whether the specific capability described is the company's actual sold product, or an enabling/onboarding capability sitting underneath the real product line with its own separate commercial mechanics (e.g. a fee plus a % commission on the downstream product's revenue) — read resume.md's own context lines carefully rather than assuming a headline revenue number represents the company's core product line.
- **Check for multiple product lines within the same company before citing a bullet — don't conflate them.** resume.md may list more than one distinct product section for the same company, each with its own mechanics. A compliance/KYC bullet from one product line is not interchangeable with a monetization/pricing bullet from a different product line at the same company, even if both are loosely "self-serve" — read the full company section to confirm which specific product a bullet belongs to before using it as evidence for a specific mechanism (e.g. "compliance sitting inside a self-serve journey" needs the bullet that's actually part of a self-serve journey, not just any bullet from a self-serve-labelled company).
- **Read multi-option requirements by their actual threshold, not full coverage.** If a requirement lists several acceptable options with a threshold word ("experience in a few of the following," "any two of," "one or more of"), check resume.md against that threshold — do not flag it as a gap just because the user's evidence doesn't cover every option in the list. Count how many options are genuinely covered before deciding.
- **Check `positioning/differentiators.md` if it exists.** It lists the user's core differentiator patterns, evidenced across multiple companies and eras in resume.md — check these first before concluding a gap exists. These are rarer and more distinguishing than general adoption/retention/monetization evidence (which is real but closer to table-stakes PM competency). Do not undercount older or less obvious resume.md bullets that fit a listed pattern.
- **Scan every company in resume.md before finalizing a citation — do not default to whichever companies are already prominent or already cited elsewhere in this run.** Small, early-career, or less-prominent entries (e.g. older roles near the bottom of the Work Experience list) are easy to overlook once a few companies get established as the "usual" answers. Before finalizing each challenge's citation, explicitly check every company section — including ones not yet cited for any challenge — for a closer mechanism/audience match, not just the companies that already came up earlier in the run. A bullet that literally echoes the JD's own wording (matching phrase or near-identical language) is not automatically the strongest match: verify mechanism (what was actually built or improved) and audience (who used it — internal vs. external, which industry, which user type) independently of any lexical overlap before picking it. A precise mechanism/audience match without a phrase echo beats a phrase echo without a precise mechanism/audience match.
- **Default to 3 distinct companies across the 3 challenges — a repeat is the exception, not a coin-flip tie-breaker.** Only cite the same company for more than one challenge when every one of that company's citations is a **Direct match** (never a Bridge) and no other company in resume.md offers a comparable match for at least one of those challenges — check every other company's relevant section before accepting a repeat, not just whichever companies are already prominent in this run. A Bridge already means the match required an interpretive step; that alone disqualifies it from justifying a repeat, and is a signal to look harder for a different company's evidence for that challenge instead.
- **If a genuine repeat survives that check, flag it to Chi when Step 2 completes — do not let the Personalised Summary step resolve it unilaterally.** State plainly that two challenges point to the same company, and ask which she prefers: (a) substitute a weaker-but-distinct-company bullet for one of the two challenges, or (b) keep the repeat and let the summary drop to 2 distinct companies. Present both as a concrete preview (exact clause wording), not an abstract description. Default recommendation is (b) — diluting a genuine Direct match to force company diversity has previously produced a weaker application than just stating both real facts — but state the recommendation and wait for her answer rather than deciding silently. This is a judgment call for Chi, not a formatting choice for this step to make on its own.
- **When two companies could both plausibly satisfy a challenge, name the specific reason the chosen one won — not just that it was chosen.** Passing the mechanism check is not enough to make a citation self-explanatory if a real alternative existed. If the deciding factor isn't visible in the bullet's content alone — e.g. the company's own stage/size mirrors the target company's stage, even though a different company's bullet also technically matches the challenge's literal ask — say so explicitly in the "because" clause. A citation that doesn't name why it beat a real alternative leaves that comparison invisible to Chi reviewing the file later.
- **Check every clause within a candidate bullet, not just its headline outcome, before selecting a citation.** Dense, multi-sentence resume.md entries (Bandwidth's in particular) often bundle several distinct claims into one bullet — e.g. an investment case, a self-serve GTM motion, and a delivery-tracking tool inside the same paragraph. Read the full bullet and identify the specific clause whose claim most precisely matches the challenge's actual language, not just whichever clause is most prominent or sits at the start. If the truest-matching clause sits in the middle of a longer bullet, quote from that clause's own surrounding words instead of defaulting to the bullet's first and last ~10-15 words (see the truncation rule below) — a clause in the middle can be the correct citation even when it isn't where the bullet's most eye-catching number sits.
- **Prefer the most specific match, not just an adequate one.** A bullet can be accurately understood and still be the wrong pick if a more specific match exists elsewhere in resume.md. If the JD names a specific mechanism within a challenge (e.g. a particular growth motion, technical approach, or user workflow — not just the general category), search for the bullet that matches that specific mechanism, not merely one that satisfies the challenge's general shape. When two companies could both plausibly answer a challenge, check which one's specific mechanism most tightly matches the JD's own wording before picking — don't default to whichever comes to mind first or was used in a prior draft.
- **Never mint a new fact, count, or statistic by tallying across bullets.** It is fine to describe a pattern in your reasoning prose (e.g. "this sits alongside several other initiatives at the same company, showing breadth"), but do not turn that observation into a number — e.g. counting how many distinct initiatives a company section contains and reporting it as "one of four product surfaces held in parallel." That count does not appear in resume.md; it is a synthesized statistic, and the Personalised Summary step will treat anything in your Format output as quotable. If a fact isn't written in resume.md in that form, it cannot appear in the Format output — describe the pattern in prose instead, outside the quoted bullet.
- **State whether each match is direct or requires a bridge, and name the bridge explicitly.** A bullet "directly matches" a challenge only if the bullet's own language describes the same mechanism, audience, or domain the challenge names — not a similar-sounding one. If citing the bullet requires treating one thing as equivalent to another (an internal tool standing in for a customer-facing one, an API-adoption motion standing in for a UI/portal motion, a different industry standing in for this one), state that translation explicitly as the bridge — do not present it as if the bullet proves the challenge outright. This applies in addition to the specific precision rules above (GTM model, self-serve mechanism, multi-product-line), which cover recurring bridge failures; this rule is the general case for any other bridge.
- **Check the challenge's specific circumstance, not just its general skill category, before calling a match Direct.** A challenge title often names a specific condition alongside a general skill — a stated scale, timeframe, or structural condition (e.g. two newly combined user groups, a named regulatory event, a specific count). A bullet can nail the general skill (e.g. root-cause discovery, stakeholder alignment) while being earned under a different circumstance entirely (e.g. within one long-standing account, not across two newly combined populations). That's a Bridge on the circumstance, even when the skill itself is a genuine Direct match — name the specific circumstance the bullet doesn't cover rather than letting the skill match carry the whole label.

**Keep the "because" reason to a single clause under 15 words — do not argue the case in full here, whichever label applies.** e.g. "because it's a personal project, not professionally shipped B2B work" or "because it's the same root-cause discovery mechanism, enterprise B2B" — not a multi-sentence justification stacking every caveat, and not a parenthetical tacked onto "Direct match" that re-explains the whole reasoning. Fuller reasoning about whether the match holds belongs in the Fit Assessment step, which already re-derives it — restating it at length here duplicates that work. Count words before finalizing each label.

Format (count words before finalizing each label; truncate each quote per the rule above; title line ends at the label, "because" starts its own bullet line below):
**Challenge 1 → [Company] → Direct match** / **Bridge**
- because [under 15 words]
- "[first ~10-15 words of bullet]... [last ~10-15 words of bullet]"

**Challenge 2 → [Company] → Direct match** / **Bridge**
- because [under 15 words]
- "[first ~10-15 words of bullet]... [last ~10-15 words of bullet]"

**Challenge 3 → [Company] → Direct match** / **Bridge**
- because [under 15 words]
- "[first ~10-15 words of bullet]... [last ~10-15 words of bullet]"

After matching challenges to resume bullets, add a **Gaps to flag** section: list any specific technical requirements from the JD (e.g. SSO, SOX, RBAC, IAM, CI/CD, specific certifications) that are not explicitly evidenced in `resume.md`. For each gap, note: "Not in resume.md — the user may have real experience here worth adding." Do not silently omit these or assume they don't exist. **Tag each gap as core/must-have or preferred/nice-to-have**, based on which JD section it came from — this classification carries into the Fit Assessment step's rating logic, so get it right here rather than re-deriving it later.

---

### Step 3 — Fit Assessment

**Final of Steps 1-3, runs inline on the current session model (see Setup point 4).** By this point the job description file contains the JD, Phase 0's Company Research, Step 1's challenges, and Step 2's Gaps to flag section (with core/preferred tags); `positioning/differentiators.md` is already in context. Write this step's output to the file, then proceed directly to the Personalised Summary step.

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

Then, distinguish between **core requirements** (stated as must-have, or repeated across multiple JD sections) and **preferred requirements** (labelled "bonus," "ideally," "nice to have," "what sets you apart," "great if you also have," or listed last) — use the core/preferred tags already assigned in the Relevant Experiences step's Gaps to flag section rather than re-deriving this from scratch. Determine the base rating from core requirements only.

**Then factor preferred requirements as a bonus, not as noise.** Multiple confirmed CV-screen-only rejections shared the same shape: core requirements were fully met, the rating came out Fit or Stretch, and the actual reason for rejection sat entirely in a preferred/"nice to have" line the rating had excluded. In practice, hiring teams use these lines as real screening filters, not soft extras — especially when a role's first pipeline stage is a CV/application review with no human contact before it. Apply this explicitly:
- If the user meets a preferred requirement, name it in the reasoning below — it's a real differentiator, not filler.
- If the user does **not** meet a preferred requirement, check how it's framed: if it's the JD's single named standout differentiator (e.g. a "What Sets You Apart" section naming one thing), or repeated/emphasized elsewhere in the JD, downgrade the rating by one level even though it's not formally "core." Treat it as functionally core for CV-screen purposes.
- If it's one item in a long list of many nice-to-haves with no special emphasis, it's lower risk — note it as a gap but don't downgrade solely on that basis.

State clearly on the first line: **Fit** / **Stretch** / **Out of Reach**

Then state the fit basis on the second line: **Direct domain match** / **Transferable skills** / **Mixed**
- **Direct domain match** — the domain and primary user type both transfer directly from the user's background; no mental leap required from the hiring team
- **Transferable skills** — the skills apply but require a mental leap on domain, primary user type, or both; the bridge is visible but not obvious
- **Mixed** — some requirements are a direct match, others rely on transferable skills; identify which is which

**Do not call it "Direct domain match" if the match relies on an analogy.** Confirmed CV-screen-only rejections have followed the same pattern: a role was rated "Fit — Direct domain match" using a bridge that required treating one resume bullet as equivalent to the JD's actual domain via a step of interpretation — e.g. an internal technical system standing in for a customer-facing product in an unrelated domain, or a platform built for one industry standing in for a specialised tool in a different one. If reaching the domain match requires that kind of translation rather than the bullet stating the same domain or mechanism outright, cap the fit basis at **Mixed** (or **Transferable skills**), not Direct — and don't let that capped basis alone still round the overall rating up to Fit.

**Weight personal/career-break projects as motivation evidence, not professional-experience evidence, when a core requirement asks for hands-on technical/builder capability.** When the JD's core ask is specifically "confidence to write code," "create PRs," "prototype products," or similar, and the only resume.md evidence is a personal project or career-break build (not a paid PM/engineering role), note explicitly that this is real but weaker-tier evidence than a professionally shipped bullet — it does not by itself close a core gap enough to justify a Fit rating. The same caution applies whenever a personal side project is the only evidence offered for a professional domain gap — it doesn't resolve it.

**Before applying that weighting, check whether the JD names a specific tool, platform, or technology by name (e.g. "Claude Code," "Figma," "Linear") and whether resume.md's independent/career-break section uses that exact named tool in public, shipped work — not a private exercise.** An exact tool-name match in public, substantial independent work is stronger evidence than generic "motivation" framing, and stronger than defaulting straight to "not in resume.md professionally" or filing the requirement as an unaddressed gap. Cite it as a Bridge — the bridge being employment status (independent vs. paid role), not mechanism or tool-name — and say so explicitly, the same way any other Bridge names what the interpretive leap is. Still note it isn't inside a paid role; just don't discard real, specific, public, name-matched evidence by reflexively treating all personal projects as equally weak. Check this before writing "no direct match" for any challenge tied to a named tool.

**Check `positioning/differentiators.md` for a combined pattern before citing an industry-label mismatch as an unbridgeable domain gap.** Some roles' real core ask is not a single domain but a specific combination of two capabilities (e.g. a technical function plus an industry context, or two domains together) — a combination most candidates have on at most one axis. `positioning/differentiators.md` may document such a combined pattern across two or more resume.md bullets from different roles/eras. Before treating "the target company's named industry has no direct resume.md precedent" as decisive grounds for a downgrade, check whether the role's actual core ask matches a documented combined pattern. If it does, cite the combination explicitly in the reasoning (not just whichever half looks more topically adjacent) — the combination itself, not either half alone, is the evidence that closes the gap. Don't let a company's product-category label override a role's actual function when a combined pattern already covers that function.

**A cited "adjacent domain" bridge must hold on both industry and function before it counts as a bridge at all.** Before naming a bullet as the closest available evidence for a domain gap, check whether it matches the target role's actual function — what the product does, what stage of the process it touches — not just a shared industry or category label. If the bullet fails on function even though it shares an industry label (e.g. a different stage of the same broader process, a different sub-product entirely), say so plainly and conclude the gap remains open — do not still present it as the closest adjacent domain, since noting the mismatch and then treating it as a bridge anyway contradicts itself.

| Rating | Criteria |
|---|---|
| **Fit** | Meets all core requirements with direct resume evidence; domain overlap is direct not adjacent; experience level matches or exceeds; business model is B2B or B2B2C |
| **Stretch** | Meets most core requirements (at least 2 of 3); domain is adjacent and the bridge is visible to a hiring team; gaps exist but are learnable; the user has cleared at least recruiter stage in similar roles; no B2C or marketplace business model gap |
| **Out of Reach** | Missing one or more core requirements that cannot be bridged by transferable skills; domain gap is too wide; JD requires specific credentials or background with no equivalent in the user's resume; OR business model is primarily B2C or marketplace |

Then explain the reasoning in 3–5 bullet points, comparing the user's actual background against the core requirements of this role. Base this only on what is in the resume — do not assume experience that is not evidenced. **Cap each bullet at 2 sentences, ~40-60 words** — this step states the verdict-relevant comparison, not the full case; count words before finalizing each bullet.

**Ground every reasoning bullet in what Step 2 already tagged.** If Step 2 tagged a citation as a Bridge, the reasoning bullet must describe it as a bridge (e.g. "X, though this requires treating Y as equivalent to Z"), not restate it as if it directly meets the requirement. Do not introduce a new interpretive leap in the Fit Assessment reasoning that wasn't already surfaced in Step 2 — if closing a gap requires a leap that Step 2 didn't make, that's a sign the requirement isn't actually met and should stay a gap, not get quietly closed here. Avoid confident language ("clearly," "directly," "obviously") for any claim that required interpretation to reach.

**Attribute every JD quote to its actual section, and do not treat items in an "or" list as strict requirements.** When citing JD language to justify a claim — especially a "repeated/emphasized elsewhere in the JD" claim used to trigger the preferred-requirement downgrade — quote only from the section actually being claimed; do not pull vocabulary from a different section (e.g. a general company/platform-description section) and present it as if it appears in the responsibilities section. Also check whether a requirement bullet lists several acceptable domains joined by "or" (e.g. "SaaS, platform, fintech, or similarly complex digital products") — an "or" list means any one of those domains is sufficient, not that the JD requires all of them or is emphasizing any one specifically. Do not treat a domain named inside an "or" list as if the JD demands specifically that domain.

---

### Step 4 — Personalised Summary

Write a resume summary of exactly 3 sentences:
- **Sentence 1 — Identity**: A one-line statement framing the user's specific positioning for this role. Not "I'm a product manager who..." — name the domain, customer type, or angle that matters for this JD. **Under 20 words.** **When resume.md shows both a credential (degree, certification) and hands-on professional experience for the same underlying skill, frame S1 around the hands-on experience, not the credential alone.** A credential-only phrasing (e.g. "has an engineering degree") undersells someone who also did the work professionally — it reads as a weaker, more passive claim than one grounded in what they actually built or shipped. Check resume.md for a stronger, more concrete fact before defaulting to the credential.
- **Sentence 2 — Evidence**: Exactly three claims, one per company/source, separated by semicolons — no stacking multiple outcomes from the same company into one clause. Each clause follows the format **"at [Company], [impact] by [action]"** — lead with the company name, then the quantified impact, then the method/action that drove it. The **[impact]** (the number) is the strongest proof in the whole summary — it stays in full, exact, never rounded or trimmed. The **[action]** is where conciseness cuts happen: state the mechanism in the fewest words that still make the claim credible, not the full story of how it was delivered. Example: "at [Company], cut time-on-task 60 to 10 minutes by shipping a new AI feature" — not "...by shipping a new AI feature, co-created with customers and Engineers, including RBAC, SSO and SAML" (those extra qualifiers pad the action without adding to the proof). Each of the 3 claims must map 1:1 to one of the 3 challenges identified in the Challenges step — the claim must be the evidence that answers that specific challenge, not just any strong claim from that company. Exact numbers and exact company names as written in the resume. **If a bullet's action involved multiple mechanisms (e.g. a technical change plus a process change plus a stakeholder-alignment effort), name only the one most legible to a non-expert reader** — a hiring-team reader skimming a CV screen should not have to parse jargon or a multi-step chain to understand what was done; pick the single mechanism that reads clearly on its own.
- **Sentence 3 — Forward-looking**: One sentence specific to this role and this company's actual problem. Must mirror the company's language or stated challenge — not generic phrases like "seeking a new challenge." Always start with "Looking to" — never "Joining," and never "Looking to join [Company] to [verb]" (a double infinitive); go straight to the work, not the act of joining. **Under 20 words.** **If S3 mirrors a challenge whose specific mechanism was an inference rather than a JD quote, check the Challenges step's origin note for that inference before restating it here.** If the inference had a named, verified source (JD clause or a specific Phase 0 finding), it's fine to state it plainly in S3 — the sourcing was already established at the Challenge level. If the challenge's Grounded-in line couldn't trace it to a real source, S3 must fall back to the JD's own more generic wording for that challenge rather than repeating the unsupported specific claim.

Sentence 2 carries the weight of the summary — keep Sentences 1 and 3 short so the evidence is what stands out, not the framing around it.

**If the clause that most precisely answers a challenge (per Step 2, including the multi-clause-bullet check above) has no number of its own, do not swap in a different clause from the same bullet just because it has one — check first whether that different clause still answers the specific challenge.** A clause with a real number that doesn't address the challenge is not a valid substitute for one that does address it but lacks a number; picking the number-bearing clause anyway silently breaks the "answers that specific challenge" rule above. Instead, pair the true mechanism with a real, honestly-caused number from a different sentence in the same bullet, using non-causal language ("and," "then") rather than "by" — "and" states two true facts side by side, "by" claims the second caused the first. Never write "by" between two clauses drawn from different sentences unless the bullet itself states that causal relationship; inventing causation between two true-but-separate facts is a factual error, not a stylistic simplification.

**When Step 2 cites more than one Direct-match bullet for a single challenge (e.g. a merged challenge covering two facets of one fear), Sentence 2 can only use one — pick the bullet whose language most literally mirrors the JD's own wording for that challenge, and prefer the one that reinforces a thematic thread another clause is already carrying (e.g. two AI-evidenced clauses read as a more coherent identity than one) over a bullet that merely sounds more senior or strategic.** State which bullet was set aside and why, the same way a Direct/Bridge tag makes reasoning visible elsewhere in this step.

Rules:
- Uses exact numbers and impacts as written in the resume (no rounding, no combining, no interpreting)
- Only includes claims supported by a bullet point in `resume.md` — if a gap was flagged in the Relevant Experiences step, do not include it here; the user will decide whether to add it
- **Check across all 3 sentences for redundancy before finalizing.** If S1 already establishes a specific echo (e.g. a same-industry/same-domain match with the target company), don't restate that same point again in a S2 clause or in S3 — make it once, in whichever sentence earns it most, and use the other sentences to add new information instead of repeating it.
- **Named customer/partner logos in an [action] clause are bloat unless the target JD names that customer or an overlapping one.** A brand name (e.g. "co-created with [Partner A], [Partner B], and [Partner C]") competes with the impact number for word count without adding proof — before including one, check whether this specific JD names that company or industry; if not, cut the name and let the number carry the clause.
- **Scale/reach metrics (e.g. "60+ countries", "20+ APIs") are not the same as brand names — keep them when the JD or company homepage emphasises global reach or multi-country/multi-market scale, cut them when the target company serves a single market.** Unlike a customer logo, a reach number is direct evidence of operating at the scale the target company itself claims — check the JD and homepage for language like "global," "worldwide," or a country/market count. If the target company is single-market (e.g. UK-only), the metric isn't evidence of relevant scale and should be cut for length like any other unsupported detail.
- **Every number in a Sentence 2 clause must appear verbatim in the resume.md bullet being quoted — never bolt on a count synthesized elsewhere.** If the Relevant Experiences step's reasoning tallied bullets to make a breadth argument (e.g. "one of four product surfaces held in parallel"), that tally is not itself a resume.md fact and must not be appended to the clause — even if it is directionally true. If a challenge is fundamentally about breadth across multiple initiatives, make that argument in Sentence 1's framing (which is not a quoted claim) rather than inventing a number inside a Sentence 2 evidence clause.
- **Default each Sentence 2 clause to the exact bullet the Relevant Experiences step's Format section cited for that challenge — do not substitute a different bullet or company here, even with a footnote.** Pulling in a bullet the Relevant Experiences step never cited for that challenge (even if it's a real, accurate resume.md fact) is evidence drift: it bypasses the Direct/Bridge check and the SMB/enterprise, GTM, and multi-product-line precision rules already applied upstream. If the Relevant Experiences step's citations repeat a company across 2 challenges, that should already have been flagged and resolved with Chi at Step 2 (see Step 2's repeat-handling rule) — this step is not the place to unilaterally pick a substitute company, noted or not. If a genuinely stronger, more specific match exists elsewhere in resume.md that the Relevant Experiences step missed, go back and correct Step 2's citation first (and re-check whether that changes the Direct/Bridge tag) rather than patching it forward in Sentence 2 alone.
- **Company sourcing is decided at Step 2, not here.** If Step 2's citations repeat a company across two challenges, that repeat is resolved at Step 2 (flag it to Chi, get her (a)/(b) answer, update Step 2's citation if needed) before this step ever runs — Sentence 2 should then simply reflect whatever Step 2 ended up with. This step diversifying or substituting sourcing on its own re-introduces the exact silent-swap failure the rule above exists to prevent.

---

### Step 5 — Pros and Cons

Analyse the summary from the Personalised Summary step in the context of this specific job description. **Draw directly from the Fit Assessment step's reasoning rather than re-deriving gaps and differentiators from scratch** — the core/preferred requirement analysis and business-model check are already done by this point; this step's job is to translate those findings into what resonates and what doesn't, not to re-litigate them independently.

- Pros: what it does well, what will resonate with the hiring team. Name any preferred/nice-to-have requirement the Fit Assessment step already identified as met — it's real signal, not filler. If the JD's preferred/"what sets you apart" language overlaps with one of the core differentiator patterns checked in the Relevant Experiences step, flag it as the strongest possible differentiator, not just a met bonus requirement.
- Cons: what is missing, weak, overstated, or could mislead. Carry forward any preferred/nice-to-have gap the Fit Assessment step flagged as functionally core (emphasized or repeated elsewhere in the JD, or the JD's single named standout differentiator) — it carries real screening risk even though it isn't a core requirement.

Be direct and specific. No hedging.

---

### Step 6 — 3 Alternatives

Every summary (baseline and alternatives) must first pass these baseline requirements — if any are not met, rewrite until they are:
- Uses only exact claims from `resume.md` (exact numbers, exact company names, no invention)
- Addresses all 3 challenges from the Challenges step
- Identity framing matches both the JD and the user's actual experience
- Forward-looking sentence is specific to this role and the user's trajectory, and follows the Personalised Summary step's S3 sourcing rule — no unsourced inferred mechanism from a Challenge stated as fact
- Exactly 3 sentences: S1 identity (under 20 words), S2 three evidence claims from three different companies/sources (one per clause, semicolon-separated, each following the "at [Company], [impact] by [action]" format — impact in full, action trimmed to the fewest credible words, naming only the single most legible mechanism when a bullet involves several — each mapped 1:1 to one of the 3 challenges from the Challenges step), S3 forward-looking (under 20 words)
- No point is made twice across the three sentences — e.g. if S1 already names a same-industry/same-domain match, a S2 clause echoing that same match again (rather than adding new information) is redundant and should be cut

**For the three alternatives (Option A, B, C), S1 and S3 carry the differentiation — S2 stays fixed to Step 2's citations for all three, per the Personalised Summary step's sourcing rules (same companies, same exact numbers, same 1:1 challenge mapping in every option).**

Both S1 and S3 must additionally clear this bar in all three options — it is not something that varies between them:
- **S1** bridges identity to this specific JD — names the domain, customer type, or angle that matters for this role, not a generic PM line.
- **S3** is inspirational and growth-focused, blended — ties the user's next chapter to the company's own specific trajectory. It must still pass the Personalised Summary step's S3 sourcing rule (mirrors a real JD/Phase 0-grounded challenge, no unsupported inferred mechanism) — the inspirational tone sits on top of a sourced fact, it does not replace it with generic "seeking growth" language.

Each option then carries one distinct framing angle on top of that bar, expressed through S1's phrasing and S3's tone (and, for tone only, how tightly S2's [action] half is trimmed) — never through S2's sourcing:
- **Option A — Tighter, linear-threaded**: S1's domain/theme word reappears explicitly in S3; tightest word count of the three.
- **Option B — More personal & voice-forward**: S1 framed in a more human, first-person voice.
- **Option C — Senior & strategic**: S1 framed with more seniority/strategic weight.

Open this step by reproducing the Personalised Summary verbatim as **Option 0 — Baseline** and scoring it using the same rubric below. Then produce three alternatives using the angles above. All four are scored in this step so the user can compare them directly.

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
- **Match the example's market segment to the company's GTM model.** Check resume.md for which of the user's roles match the target company's actual motion — some are bottom-up PLG/SMB self-serve, others are product-led sales within an enterprise/mid-market motion (self-serve activation that still sits inside a sales-assisted enterprise relationship), not literal PLG — and pick the example whose GTM shape matches, not just whichever company is topically adjacent. Using an SMB/PLG example for a sales-led or product-led-sales enterprise company signals the wrong GTM mental model to the hiring manager, even if the mechanics are similar.
- **Match the example's shape to the question's shape.** A question about "stopping" a low-value item needs an example where something was stopped or clearly deprioritised — not deferred or rescoped. A question about "moving the needle commercially" needs a commercial product outcome — not an internal operations improvement. If the best available example is a partial match, flag the mismatch and note it before drafting.
- **Pre-defined measurement matters.** "Proved it with evidence" means the measurement framework was in place before launch, not retrospectively applied. Make this explicit in the answer when it is true.
- **Check evidence reuse across all questions in this file before drafting any answer.** If the same company or bullet is the strongest fit for two different questions, don't draft both in isolation. Either pick a different, still-genuine example for one of them, or flag the overlap explicitly and ask which question should keep it. A reader reviewing all the answers together will notice if the same story appears twice.

Format per question:
#### Q[n]: [question text]
**Relevant bullets:** ...
**Drafted answer:** ...
