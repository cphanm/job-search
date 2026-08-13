Follow these steps exactly when this command is invoked.

## Purpose

Read `work-in-progress/jobs-to-scan.md`, fetch each job posting URL, create one markdown file per job in `work-in-progress/` following the `input/job-description-template.md` structure, and auto-detect the company homepage.

---

## Step 1 — Read inputs

1. Read `work-in-progress/jobs-to-scan.md`
2. Collect all URLs listed under `## To Scan` (one per line, skip blank lines and comments)
3. Read `input/job-description-template.md` to get the file structure
4. Read `input/blacklisted-companies.md` if it exists (optional — if missing, treat the blacklist as empty and skip all blacklist checks below). Parse the `Company` and `Homepage` columns from its table for use in Step 5.
5. If no URLs are found under `## To Scan`, stop and tell the user the list is empty.

---

## Step 2 — Fetch all JDs in parallel

For each URL, detect the ATS from the URL pattern and fetch using the correct method:

**Method A — curl** (server-side rendered ATS):
- Greenhouse (standard): `boards.greenhouse.io`
- Greenhouse (embed): `job-boards.greenhouse.io`
- Lever: `jobs.lever.co`
- Workable (server-rendered): `jobs.workable.com`
- SmartRecruiters: `jobs.smartrecruiters.com`
- Jobvite: `jobs.jobvite.com`
- Recruitee: `recruitee.com`
- Teamtailor: `teamtailor.com`
- Breezy HR: `breezy.hr`
- Personio: `jobs.personio.com`
- Airbnb careers page: `careers.airbnb.com`
- Any unknown company careers page: try curl first; fall back to Method B if content is empty

```bash
curl -s -L "[URL]" | python3 -c "
import sys
from html.parser import HTMLParser

class TextExtractor(HTMLParser):
    def __init__(self):
        super().__init__()
        self.text = []
        self.skip = False
    def handle_starttag(self, tag, attrs):
        if tag in ('script', 'style'):
            self.skip = True
    def handle_endtag(self, tag):
        if tag in ('script', 'style'):
            self.skip = False
    def handle_data(self, data):
        if not self.skip:
            self.text.append(data)

p = TextExtractor()
p.feed(sys.stdin.read())
print(''.join(p.text))
" | tr -s ' \n'
```

**Method B — Chrome headless** (JS-rendered ATS):
- Ashby: `jobs.ashbyhq.com`
- Workday: `myworkdayjobs.com`
- Oracle Cloud HCM: `oraclecloud.com/hcmUI`
- Workable (client-rendered apply flow): `apply.workable.com` — plain curl returns only an empty JS shell (~7KB, no JD content); this domain doesn't match the "unknown page" fallback trigger reliably because the shell isn't literally empty, so route it to Chrome headless directly.
- Welcome to the Jungle: `app.welcometothejungle.com` — plain curl only returns an empty SPA shell with generic meta tags (title "Welcome to the Jungle - The better way to find a job in tech" regardless of the actual job); Chrome headless is required to get real content. The rendered page's `<title>` follows the pattern `[Company] [Job Title] | Welcome to the Jungle (formerly Otta)` — use it to confirm title and company if not otherwise clear from the body text.

```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --headless=new --virtual-time-budget=5000 --dump-dom "[URL]" 2>/dev/null | python3 -c "
import sys
from html.parser import HTMLParser

class TextExtractor(HTMLParser):
    def __init__(self):
        super().__init__()
        self.text = []
        self.skip = False
    def handle_starttag(self, tag, attrs):
        if tag in ('script', 'style'):
            self.skip = True
    def handle_endtag(self, tag):
        if tag in ('script', 'style'):
            self.skip = False
    def handle_data(self, data):
        if not self.skip:
            self.text.append(data)

p = TextExtractor()
p.feed(sys.stdin.read())
print(''.join(p.text))
" | tr -s ' \n'
```

**Method C — Chrome headless + real user agent** (bot-protected ATS):
- iCIMS: `icims.com`

```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --headless=new --virtual-time-budget=5000 --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36" --dump-dom "[URL]" 2>/dev/null | python3 -c "
import sys
from html.parser import HTMLParser

class TextExtractor(HTMLParser):
    def __init__(self):
        super().__init__()
        self.text = []
        self.skip = False
    def handle_starttag(self, tag, attrs):
        if tag in ('script', 'style'):
            self.skip = True
    def handle_endtag(self, tag):
        if tag in ('script', 'style'):
            self.skip = False
    def handle_data(self, data):
        if not self.skip:
            self.text.append(data)

p = TextExtractor()
p.feed(sys.stdin.read())
print(''.join(p.text))
" | tr -s ' \n'
```

**Method D — LinkedIn individual job posting** (public, no login required):
- `linkedin.com/jobs/view/<id>` (with or without a title slug before the id)
- Does **not** cover `linkedin.com/jobs/search-results/...` URLs — those require a logged-in session and are out of scope for this command.

```bash
curl -s -L -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36" "[URL]" | python3 -c "
import sys, re
from html.parser import HTMLParser

class TextExtractor(HTMLParser):
    def __init__(self):
        super().__init__()
        self.text = []
        self.skip = False
    def handle_starttag(self, tag, attrs):
        if tag in ('script', 'style'):
            self.skip = True
        elif tag in ('br', 'li'):
            self.text.append('\n')
    def handle_endtag(self, tag):
        if tag in ('script', 'style'):
            self.skip = False
    def handle_data(self, data):
        if not self.skip:
            self.text.append(data)

html = sys.stdin.read()

canonical = re.search(r'canonical\" href=\"([^\"]*)\"', html)
print('CANONICAL:', canonical.group(1) if canonical else '')

flavors = re.findall(r'topcard__flavor[^\"]*\">(.*?)</span>', html, re.S)
for f in flavors:
    print('FLAVOR:', re.sub('<[^>]+>', '', f).strip())

m = re.search(r'show-more-less-html__markup[^\"]*\">(.*?)</div>\s*<button class=\"show-more-less-html__button', html, re.S)
if not m:
    m = re.search(r'description__text description__text--rich\">(.*?)<section', html, re.S)
if m:
    p = TextExtractor()
    p.feed(m.group(1))
    text = ''.join(p.text)
    text = re.sub(r'[ \t]+', ' ', text)
    text = re.sub(r'\n\s*\n+', '\n\n', text)
    text = text.split('Show more')[0]
    print('---DESCRIPTION---')
    print(text.strip())
else:
    print('---DESCRIPTION---')
    print('NOT FOUND')
"
```

`CANONICAL` gives the job-title-and-company slug (e.g. `senior-product-manager-at-acme-co-4441818899`) — use it to confirm title and company if not otherwise clear. The `FLAVOR` lines are, in order: company name, location, and posted-date — use the first two directly as Company and Location metadata. Everything after `---DESCRIPTION---` is the job description body. The primary regex targets LinkedIn's current `show-more-less-html__markup` wrapper around the description; the second regex is a fallback for older markup that doesn't use that wrapper. `<br>` and `<li>` tags are converted to newlines so paragraph and list structure survives extraction — expect some remaining run-on text where the source itself has no internal line breaks, and reformat it into clean paragraphs/bullets in Step 5.

Issue all fetches in parallel. If a fetch fails or returns no meaningful content, note it and continue — do not block on it.

---

## Step 3 — For each fetched JD, extract metadata

From the fetched text, extract:
- **Job title** — the role title as written in the posting
- **Company name** — as written in the posting
- **Posted date** — search the raw fetched content (not the cleaned text) for a `datePosted` field. The whitespace around the colon varies by ATS, so check loosely for `"datePosted"` followed by a `YYYY-MM-DD` value — forms seen so far: `"datePosted":"YYYY-MM-DD..."`, `"datePosted" : "YYYY-MM-DD"` (Lever), and `"datePosted" content="YYYY-MM-DD...">` (meta tag). Extract just the `YYYY-MM-DD` portion. Confirmed present on: Workable (both domains), SmartRecruiters, Breezy HR, Teamtailor, Ashby, Welcome to the Jungle, Lever.
  - **Greenhouse never exposes a posted date** — no JSON-LD, no DOM field on the job page. Its board-list page has an `updated_at` value but it's identical across every job on the board (a page-cache timestamp, not per-job data) — do not use it. Leave `Posted:` blank, don't burn extra fetches looking.
  - **Workday and Oracle Cloud HCM** — not reliably extractable yet; the Chrome headless dump often stalls on a cookie-consent shell before real job content loads. Leave blank rather than guessing.
  - **Jobvite, Recruitee, Personio, iCIMS** — unverified. Check opportunistically for the same `datePosted` JSON-LD pattern; many ATS use the schema.org JobPosting standard for SEO, but treat absence as inconclusive, not confirmed-unsupported, until it's been checked against a live example.

For LinkedIn (Method D) sources, take Company and Location directly from the first two `FLAVOR` lines rather than re-parsing the description body, and take Posted date from the third `FLAVOR` line — a relative string like "5 days ago", "3 weeks ago", "1 month ago", "Xh ago", or "Just now". **Convert it to an absolute `YYYY-MM-DD` date** by subtracting the offset from today's date (the date of this scan run) — e.g. "5 days ago" scanned on 2026-08-11 becomes `2026-08-04`. Treat "Xh ago", "Xm ago", and "Just now" as today's date. Record the resulting absolute date, not the relative string — a relative string goes stale the moment the file is reopened later (e.g. when deciding whether to apply), while an absolute date stays meaningful indefinitely.

---

## Step 4 — Find company homepage (in parallel with Step 3)

Try these methods in order, stopping at the first that works:

1. **From the JD text** — look for the company's own website URL mentioned in the About or Company section
2. **From the ATS URL slug** — derive the company domain:
   - `jobs.ashbyhq.com/acmecocareers/` → strip "careers" → try `https://acmeco.io` and `https://acmeco.com`
   - `boards.greenhouse.io/acmeco/` → try `https://acmeco.com`
   - `jobs.lever.co/acmeco/` → try `https://acmeco.com`
   - `jobs.smartrecruiters.com/AcmeCo/` → try `https://www.acmeco.com`
   - Does not apply to LinkedIn or Welcome to the Jungle URLs — the slug/ID encodes the job posting, not the company domain. Go straight to WebSearch fallback if the JD text has no company URL.
3. **WebSearch fallback** — search `"[Company Name]" official website` and take the first result that matches the company

Fetch the homepage URL to confirm it loads. If no homepage can be confirmed, write `Homepage:` blank and note it.

---

## Step 5 — Check against blacklist

For each job, compare the extracted company name (Step 3) and confirmed homepage (Step 4) against every entry loaded from `input/blacklisted-companies.md`:
- Match if the company name matches an entry's `Company` (case-insensitive), or
- Match if the homepage domain matches an entry's `Homepage` domain

If a job matches, do not create a file for it in Step 6. Record it for Step 7/8 as blacklisted, noting which entry matched and that entry's `Reason`.

---

## Step 6 — Create one MD file per job

For each job, create a file in `work-in-progress/` named after the company in lowercase with hyphens (e.g. `acme-co.md`, `widget-software.md`). If a file with that name already exists, append `-2`, `-3` etc.

Populate using the template structure:

```
# [Job Title] — [Company Name]

Source: [original URL]
Homepage: [confirmed homepage URL or blank]
Posted: [date found, converted to YYYY-MM-DD if the source gave a relative string, or blank]

---

## Job Description

[Full fetched JD text, cleaned of navigation/footer boilerplate, preserving all sections verbatim]

---

## Application Form Questions

[Paste each question on its own line, numbered. Leave blank if no additional questions.]

1. 
2. 
3. 
```

Clean the JD text: remove nav menus, cookie banners, footer links, and "apply now" UI chrome. Keep every word of the actual job description — responsibilities, requirements, about the company, benefits — verbatim.

---

## Step 7 — Update jobs-to-scan.md

Move each processed URL from `## To Scan` to one of:
- `## Done`, appending the filename created next to it:
  ```
  ## Done
  - [URL] → acme-co.md
  ```
- `## Blacklisted`, if it was skipped in Step 5, noting the matched entry and reason:
  ```
  ## Blacklisted
  - [URL] → skipped, matches blacklist entry "Acme Recruiting Co" (AI recruiter — never names the actual hiring company in postings)
  ```
- `## Failed`, if it failed to fetch, with a short note on why.

---

## Step 8 — Report to the user

List each file created:
- Filename
- Job title and company name
- Homepage found or not found
- Posted date found or not found (note the ATS if not found, since some — Greenhouse, Workday, Oracle Cloud HCM — don't reliably expose one)
- Any failures or notes

List each posting skipped as blacklisted:
- Job title and company name
- Which blacklist entry matched and its reason

Then ask the user to review the files and run `/create-resume` when ready.
