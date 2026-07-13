Follow these steps exactly when this command is invoked.

## Purpose

Read `work-in-progress/jobs-to-scan.md`, fetch each job posting URL, create one markdown file per job in `work-in-progress/` following the `original/job-description-template.md` structure, and auto-detect the company homepage.

---

## Step 1 — Read inputs

1. Read `work-in-progress/jobs-to-scan.md`
2. Collect all URLs listed under `## To Scan` (one per line, skip blank lines and comments)
3. Read `original/job-description-template.md` to get the file structure
4. If no URLs are found under `## To Scan`, stop and tell the user the list is empty.

---

## Step 2 — Fetch all JDs in parallel

For each URL, detect the ATS from the URL pattern and fetch using the correct method:

**Method A — curl** (server-side rendered ATS):
- Greenhouse (standard): `boards.greenhouse.io`
- Greenhouse (embed): `job-boards.greenhouse.io`
- Lever: `jobs.lever.co`
- Workable: `jobs.workable.com`
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

Issue all fetches in parallel. If a fetch fails or returns no meaningful content, note it and continue — do not block on it.

---

## Step 3 — For each fetched JD, extract metadata

From the fetched text, extract:
- **Job title** — the role title as written in the posting
- **Company name** — as written in the posting

---

## Step 4 — Find company homepage (in parallel with Step 3)

Try these methods in order, stopping at the first that works:

1. **From the JD text** — look for the company's own website URL mentioned in the About or Company section
2. **From the ATS URL slug** — derive the company domain:
   - `jobs.ashbyhq.com/tesslcareers/` → strip "careers" → try `https://tessl.io` and `https://tessl.com`
   - `boards.greenhouse.io/anthropic/` → try `https://anthropic.com`
   - `jobs.lever.co/stripe/` → try `https://stripe.com`
   - `jobs.smartrecruiters.com/Experian/` → try `https://www.experian.com`
3. **WebSearch fallback** — search `"[Company Name]" official website` and take the first result that matches the company

Fetch the homepage URL to confirm it loads. If no homepage can be confirmed, write `Homepage:` blank and note it.

---

## Step 5 — Create one MD file per job

For each job, create a file in `work-in-progress/` named after the company in lowercase with hyphens (e.g. `tessl.md`, `mri-software.md`). If a file with that name already exists, append `-2`, `-3` etc.

Populate using the template structure:

```
# [Job Title] — [Company Name]

Source: [original URL]
Homepage: [confirmed homepage URL or blank]

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

## Step 6 — Update jobs-to-scan.md

Move each processed URL from `## To Scan` to `## Done`, appending the filename created next to it:

```
## Done
- [URL] → tessl.md
```

If a URL failed to fetch, move it to a `## Failed` section with a short note on why.

---

## Step 7 — Report to the user

List each file created:
- Filename
- Job title and company name
- Homepage found or not found
- Any failures or notes

Then ask the user to review the files and run `/create-resume` when ready.
