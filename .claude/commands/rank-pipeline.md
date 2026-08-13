Follow these steps exactly when this command is invoked.

## Setup

1. Take `$ARGUMENTS` as the folder to scan. If empty, default to `work-in-progress/`.
2. List all `.md` files in that folder.
3. For each file, check whether it contains a `### Step 6 — Fit Assessment` section. Files without this section have not been through `/create-resume`'s Fit Assessment — set them aside and list them separately at the end under "Not yet assessed" rather than guessing a rating.

## Extract per file

For each file that has a Step 6 section, extract:

- **Company**: from the first line's title, format `# Role Title — Company Name`. Strip parenthetical notes like "(recruitment agency; hiring company not named in posting)" or "(sourced via ...)" from the name shown in the table; if the actual hiring company is genuinely unknown (agency posting), keep the agency name and add a footnote.
- **Industry**: from the Phase 0 "Company Research" section's Industry line. Compress to a short phrase (roughly 5–8 words) for the table — do not paste the full sentence.
- **Business model**: from the Company Research Business model line and/or the Step 6 business-model verdict line (`Business model: X — no gap` / `gap`). Use the short label (B2B, B2C, B2B2C, Marketplace, B2B-SMB, Developer platform, etc.). If the company is mixed and the PM owns one side, note that in parentheses, e.g. `B2B2C (owns B2B side)`.
- **Posted**: from the file's `Posted:` line near the top. Use the `YYYY-MM-DD` value as-is. `/scan-jobs` converts LinkedIn's relative dates (e.g. "5 days ago") to an absolute `YYYY-MM-DD` at scan time, so files should normally already carry an absolute date. If an older file still has a relative string (from a scan run before this convention), keep it verbatim rather than guessing what it converts to — don't recompute it against today's date, since the offset was relative to the original scan date, not the current one. If the line is blank, leave the cell blank rather than guessing — some ATS (Greenhouse, Workday, Oracle Cloud HCM) never expose a posted date.
- **Fit**: the most recent Fit / Stretch / Out of Reach verdict in the file.

**Reruns are appended, not overwritten — this is the part most likely to be read wrong.** A file may contain one or more `#### Step 6 Rerun` or `#### Step 2 + Step 6 Rerun` sections after the original `### Step 6 — Fit Assessment` section, and may also contain multiple full `## Resume Personalisation — YYYY-MM-DD` sections from repeated whole-file runs. The rating lines format inconsistently across sessions/dates — look for all of: `**Fit**`, `**Stretch**`, `**Out of Reach**`, `**Fit: X**`, `**Fit:** X`, `**Fit / Stretch / Out of Reach: X**`, and `New verdict: X`. Take the **last one in file order** as authoritative, not the first or the one that looks most prominent. If a rerun section is present, its verdict wins even if the original Step 6 section's verdict looks more complete or better-argued.

## Handle known caveats

- If a file's skills-fit rating is overridden by a standing personal exclusion noted inline in the file (e.g. an industry excluded from the current search scope, independent of resume fit), use the **practical** verdict — the one that actually determines whether to apply — not the raw skills-fit rating, and mark that row with a footnote explaining the override.
- If parsing is ambiguous for any file (conflicting verdicts, no clear final line, multiple candidate ratings with no clear ordering), read that file directly and resolve it from the actual text rather than guessing.

## Output

Produce one markdown table with columns: `Company | Industry | Business Model | Posted | Fit`. Order rows Fit → Stretch → Out of Reach, alphabetically by company within each tier. After the table:
- List any files skipped as "Not yet assessed" (no Step 6 section found).
- List any footnotes for overridden/caveated verdicts.
