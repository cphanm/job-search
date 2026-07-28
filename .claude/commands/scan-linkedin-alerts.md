Follow these steps exactly when this command is invoked.

## Purpose

Check the dedicated automation Gmail inbox for unread LinkedIn job-alert digest emails — including ones forwarded in from another mailbox — extract every job link from each alert, dedup against jobs already tracked anywhere in this repo, append new ones to `work-in-progress/jobs-to-scan.md`, then run `/scan-jobs` to fetch full descriptions.

This command only ever touches one specific Gmail account dedicated to this automation — never any other Gmail account that might be logged into the same browser profile. The account address is not written in this file (this file is committed to a public repo); read it from `input/linkedin-mailbox.md` (gitignored) at the start of every run. If that file doesn't exist, stop and tell the user it needs to be created first with the automation account's address.

---

## Step 0 — Confirm the correct Gmail account is active

Read `input/linkedin-mailbox.md` to get the automation account's address.

Use `claude-in-chrome` to open https://mail.google.com/mail/.

Before reading or searching anything, check the account avatar/email shown in the top-right corner:
- If it already matches the address from `linkedin-mailbox.md`, continue.
- If a different account is active, click the account switcher and select the correct one. If it isn't listed as an available account, stop and tell the user this account isn't logged into the browser — do not attempt to enter credentials or sign in on the user's behalf.

Do not proceed to Step 1 until the active account is confirmed to match `linkedin-mailbox.md`.

---

## Step 1 — Search for unread LinkedIn alert emails

In the confirmed automation account, search: `is:unread (from:jobalerts-noreply@linkedin.com OR (subject:"Fwd:" "jobalerts-noreply@linkedin.com"))`

This catches both alerts sent directly to this mailbox and alerts forwarded in from another mailbox (identified by a "Fwd:" subject whose forwarded headers mention `jobalerts-noreply@linkedin.com`).

If there are no results, tell the user there are no new alerts and stop — do not run Step 7.

---

## Step 2 — Extract the job link(s) from each unread email

Open each matching email one at a time. Opening an email in Gmail marks it read automatically — this is the intended dedup mechanism so the next run's `is:unread` search only picks up genuinely new alerts. No separate "mark as read" action is needed. This works the same whether the email arrived directly or as a forward — Gmail renders the forwarded HTML inline, so the original alert content and links are present the same way.

Each email is one of two formats:
- **Digest** (multiple jobs) — find the "See all jobs" button and copy its full underlying URL (a `linkedin.com/jobs/search-results/...` link).
- **Single-job alert** (one job, no "See all jobs" button) — find the job title link/button instead and copy its full underlying URL (a `linkedin.com/jobs/view/<id>...` or `linkedin.com/comm/jobs/view/<id>...` link).

---

## Step 3 — Parse job IDs out of each link

For each digest ("See all jobs") URL collected in Step 2, parse the query string:
- If `originToLandingJobPostings` is present, split its value on commas — this is the full list of job IDs in that digest.
- Otherwise, fall back to the `currentJobId` parameter alone.

For each single-job alert URL collected in Step 2, extract the ID directly from the URL path (e.g. `/jobs/view/4441817767` or `/comm/jobs/view/4441817767`), ignoring any tracking query parameters.

Convert every job ID to `https://www.linkedin.com/jobs/view/<id>`.

Collect the full set of job URLs across all emails processed this run, de-duplicating any ID that appears in more than one email.

---

## Step 4 — Dedup against jobs already tracked in the repo

For each job ID, grep the repo for that ID appearing in any existing file's `Source:` line or body — check `work-in-progress/`, `done/`, `done/successful/`, `done/failed/`, `skipped/`, and `not-apply-since-lower-chance/`. Drop any ID already referenced anywhere in the repo. Keep only genuinely new job URLs.

---

## Step 5 — Append new URLs to jobs-to-scan.md

Read `work-in-progress/jobs-to-scan.md`. Append each new job URL (from Step 4) as its own line under `## To Scan`. Skip any URL that's somehow already listed there.

---

## Step 6 — Report to the user before proceeding

State clearly:
- How many alert emails were processed
- Total job IDs found across those emails
- How many were new vs. already-tracked duplicates (and which companies/titles were skipped as duplicates, if known)
- The final list of URLs added to `## To Scan`

---

## Step 7 — Run /scan-jobs

If any new URLs were added in Step 5, immediately invoke `/scan-jobs` to fetch full descriptions and create the corresponding `work-in-progress/*.md` files. If no new URLs were added (everything was a duplicate), skip this step and say so.
