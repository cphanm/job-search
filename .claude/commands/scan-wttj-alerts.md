Follow these steps exactly when this command is invoked.

## Purpose

Check the dedicated automation Gmail inbox for unread Welcome to the Jungle (WTTJ) "New match" alert emails — including ones forwarded in from another mailbox — extract every job link from each alert, dedup against jobs already tracked anywhere in this repo, append new ones to `work-in-progress/jobs-to-scan.md`, then run `/scan-jobs` to fetch full descriptions.

This command only ever touches one specific Gmail account dedicated to this automation — never any other Gmail account that might be logged into the same browser profile. The account address is not written in this file (this file is committed to a public repo); read it from `input/job-alerts-mailbox.md` (gitignored) at the start of every run. If that file doesn't exist, stop and tell the user it needs to be created first with the automation account's address. This is the same mailbox `/scan-linkedin-alerts` uses.

**Time sensitivity:** unlike LinkedIn, WTTJ's per-job links carry a signed, time-limited access token. Run this command promptly rather than letting unread alerts accumulate for a long time — an old link may have expired by the time it's processed (see Step 3).

---

## Step 0 — Confirm the correct Gmail account is active

Read `input/job-alerts-mailbox.md` to get the automation account's address and the forwarding-source address (the user's personal Gmail, which auto-forwards alert mail into the automation account). Both are used to build the search in Step 1. If the forwarding-source line isn't present, proceed with sender-based matching only (skip the `from:[forwarding source]` and personal-forward clauses below).

Use `claude-in-chrome` to open https://mail.google.com/mail/.

Before reading or searching anything, check the account avatar/email shown in the top-right corner:
- If it already matches the address from `job-alerts-mailbox.md`, continue.
- If a different account is active, click the account switcher and select the correct one. If it isn't listed as an available account, stop and tell the user this account isn't logged into the browser — do not attempt to enter credentials or sign in on the user's behalf.

Do not proceed to Step 1 until the active account is confirmed to match `job-alerts-mailbox.md`.

---

## Step 1 — Search for unread WTTJ alert emails

WTTJ sends job-match alert mail from `help@welcometothejungle.com`.

In the confirmed automation account, search: `is:unread (from:help@welcometothejungle.com OR from:[forwarding-source address] OR (subject:"Fwd:" "welcometothejungle.com"))`

Substitute `[forwarding-source address]` with the address read from `input/job-alerts-mailbox.md` in Step 0; drop that clause entirely if no forwarding-source line was present.

This catches alerts sent directly to this mailbox, alerts auto-forwarded in from the user's personal Gmail, and alerts manually forwarded in from another mailbox (identified by a "Fwd:" subject whose forwarded headers mention `welcometothejungle.com`).

If there are no results, tell the user there are no new alerts and stop — do not run Step 7.

---

## Step 2 — Extract the job link(s) from each unread email

Open each matching email one at a time. Opening an email in Gmail marks it read automatically — this is the intended dedup mechanism so the next run's `is:unread` search only picks up genuinely new alerts. No separate "mark as read" action is needed. This works the same whether the email arrived directly or as a forward.

Every WTTJ alert is a digest of several matched jobs (company, tagline, job title, location), each rendered as its own individually tracked link — there is no separate single-job format. Read the full accessibility tree of the email (not just visible text) to find every job-entry link, since each is a distinct `<a href>` even though they aren't always visually distinguishable from plain text at a glance. Collect the full href for each one (these are long SendGrid click-tracking URLs — copy them exactly, in full; a truncated copy will fail to resolve in Step 3).

---

## Step 3 — Resolve each tracked link to the real job URL

Each collected href is a SendGrid click-tracking redirect (`*.ct.sendgrid.net/ls/click?upn=...`), not a direct job URL. Resolve each one:

```bash
curl -s -o /dev/null -w "%{http_code} -> %{url_effective}\n" -L --max-redirs 10 -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36" "[tracked URL]"
```

A successful resolution lands on `https://app.welcometothejungle.com/jobs/<ID>?token=<JWT>&position=<N>&count=<M>&utm_...`. Extract `<ID>` (the alphanumeric path segment right after `/jobs/`) — this is the stable identifier for dedup, ignoring `token`/`position`/`count`/`utm_*` (per-email tracking noise, not part of the job's identity).

If a resolution lands anywhere other than `app.welcometothejungle.com/jobs/<ID>` (e.g. it lands on the plain WTTJ homepage with no `/jobs/` path), the token has very likely expired — note it as failed/expired for that job and move on rather than treating it as a fetch bug. Don't retry expired links; there's no way to re-derive a working one from an old email.

Convert every successfully resolved ID to `https://app.welcometothejungle.com/jobs/<id>`.

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
- How many links had expired tokens and were skipped, if any
- The final list of URLs added to `## To Scan`

---

## Step 7 — Run /scan-jobs

If any new URLs were added in Step 5, immediately invoke `/scan-jobs` to fetch full descriptions and create the corresponding `work-in-progress/*.md` files. If no new URLs were added (everything was a duplicate or expired), skip this step and say so.

`app.welcometothejungle.com` job pages are a JS-rendered single-page app — `/scan-jobs` must use its Chrome-headless fetch method (the same one used for Ashby/Workday) for this domain, not curl, which only returns an empty app shell.

`/scan-jobs` applies its own blacklist check (`input/blacklisted-companies.md`) after fetching each posting, since the company isn't known until the JD is fetched — no separate blacklist step is needed here.
