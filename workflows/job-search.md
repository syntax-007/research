# Job Search Workflow

A plain-English recipe for sourcing engineering leadership roles and tracking them in a date-tabbed Google Sheet.

**Google Sheet tracker:** See `resources/job-pipeline-sheet.md` for the sheet URL.
**Spreadsheet ID:** 1-2pSoXpWk1gul1x3STs8n06SetfPG_XJ9kqagkGPVJ4

---

## Candidate Profile

Use this profile to evaluate role fit throughout the workflow.

- **Name:** Richard Peters
- **Current role:** Senior Engineering Manager, Oak Street Health (CVS)
- **Experience:** 15+ years in engineering leadership
- **Target roles:** Engineering Manager, Senior Engineering Manager, Director of Engineering, Senior Director of Engineering
- **Location requirement:** Remote only (US-based companies)
- **Compensation floor:** $180k/year (filter out roles explicitly listed below this; include unlisted roles)
- **Background:** Healthtech, fintech, SaaS
- **Strengths:** Agile/Scrum, team scaling, CI/CD, HIPAA/SOC2 compliance, AWS/Azure, AI/generative AI, C#, Python, JavaScript, React, Grafana, Node.js, Golang, Kubernetes, Kafka
- **Certifications:** PSM I, PSF, PAL-EBM, PAL I

---

## Sheet Structure

Each job search run writes to a date-named tab (e.g., `2026-05-01`).

- If a tab for today's date already exists, append rows to it.
- If no tab for today's date exists, create a new tab named with today's date (YYYY-MM-DD), add a bold frozen header row, then append rows.

**Columns (5):**

| Column | Values |
|---|---|
| Company | Company name |
| Role Title | Exact job title from posting |
| Level | EM / Sr EM / Dir / Sr Dir |
| Source | LinkedIn / Indeed / Glassdoor / Levels.fyi / Company Site |
| Job URL | Direct link to the posting |

**Deduplication rule:** Before adding any row, check all existing rows on today's tab for a matching Company + Role Title. Skip duplicates.

---

## Phase 1 — Source & Surface

**Triggered by:** "run job search"

### Step 1 — Determine today's tab

Check the spreadsheet for a tab named with today's date (YYYY-MM-DD).
- If it exists: use it, append below existing rows.
- If it does not exist: create a new sheet tab named with today's date, write the bold frozen header row (`Company, Role Title, Level, Source, Job URL`), then append rows starting at row 2.

### Step 2 — Search each source

Search using the four target titles. Apply remote and "past month" filters where available. **Only include postings from the last 30 days — skip anything older.**

**Search titles:**
- "Engineering Manager"
- "Senior Engineering Manager"
- "Director of Engineering"
- "Senior Director of Engineering"

**Sources to search (in priority order):**
1. **LinkedIn Jobs** *(primary)* — fetch the following URL pattern directly using WebFetch for each title (URL-encode spaces as `+`):
   ```
   https://www.linkedin.com/jobs/search/?keywords=TITLE&location=United+States&f_WT=2&f_TPR=r2592000
   ```
   Parameters: `f_WT=2` = Remote filter, `f_TPR=r2592000` = Past 30 days. This returns ~60 listings per page with job IDs. Fetch individual job pages at `https://www.linkedin.com/jobs/view/JOB_ID/` to verify each role is open and get full details. Use `&start=25` or `&start=50` to paginate to additional result pages.
2. **Greenhouse / Lever / Ashby** *(secondary, for verification)* — use `site:job-boards.greenhouse.io OR site:jobs.lever.co OR site:jobs.ashbyhq.com` to find postings. Fetch each URL to confirm it is still open before adding. Skip any that redirect or show a closed/error page.
3. **Company career pages** — search career pages of well-known tech companies (e.g., Stripe, Airbnb, Shopify, Figma, Notion, Linear, Vercel, GitHub, HashiCorp, PagerDuty, Datadog, New Relic, Twilio, Okta, Snowflake, Databricks, Confluent, MongoDB)

### Step 3 — Evaluate each result for fit

For each result found, evaluate against the candidate profile before adding:
- Title must be one of the four target titles (or a close equivalent — use judgment)
- Must be remote (skip hybrid-only or in-office roles)
- Posted within the last 30 days — skip older postings
- If comp is listed and is explicitly below $180k, skip the role
- Assess fit: does the job description align with the candidate's background (Agile leadership, SaaS/healthtech/fintech, modern cloud stack, team scaling)? Skip roles requiring deep specializations not in the candidate's profile (e.g., hardware, data center ops, Salesforce-only stacks)
- Cap at 25 roles per run — prioritize best fit over quantity

### Step 4 — Deduplicate

Read all existing rows on today's tab. For each qualifying role, check whether a row already exists with the same Company AND Role Title. Skip any duplicates.

### Step 5 — Append new rows

For each qualifying, non-duplicate role, append a new row in column order:
`Company, Role Title, Level, Source, Job URL`

### Step 6 — Report to user

After all sources are searched, report:
- How many new roles were added
- Total rows on today's tab
- Any sources that returned no results or could not be searched

---

## Phase 2 — Pipeline Review

**Triggered by:** "review my pipeline"

### Step 1 — Read all tabs

Get the list of all date-named tabs in the spreadsheet.

### Step 2 — Summarize

Report:
- Total roles found per date tab (most recent first)
- Grand total across all tabs

### Step 3 — Report to user

Deliver the summary concisely.

---

## Running Both Phases

**Triggered by:** "run job search and review my pipeline"

Execute Phase 1 first, then Phase 2. Report both outputs together.
