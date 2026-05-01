# Job Search Workflow

A plain-English recipe for sourcing engineering leadership roles and managing the job application pipeline.

**Google Sheet tracker:** See `resources/job-pipeline-sheet.md` for the sheet URL.
**Spreadsheet ID:** 1-2pSoXpWk1gul1x3STs8n06SetfPG_XJ9kqagkGPVJ4
**Sheet name:** Sheet1

---

## Candidate Profile

Use this profile to evaluate role fit throughout the workflow.

- **Name:** Richard Peters
- **Current role:** Senior Engineering Manager, Oak Street Health (CVS)
- **Experience:** 15+ years in engineering leadership
- **Target roles:** Engineering Manager, Senior Engineering Manager, Director of Engineering, Senior Director of Engineering
- **Location requirement:** Remote only (US-based companies)
- **Compensation floor:** $180k/year (filter out roles explicitly listed below this; include unlisted roles but flag them)
- **Background:** Healthtech, fintech, SaaS
- **Strengths:** Agile/Scrum, team scaling, CI/CD, HIPAA/SOC2 compliance, AWS/Azure, AI/generative AI, C#, Python, JavaScript, React, Grafana, Node.js, Golang, Kubernetes, Kafka
- **Certifications:** PSM I, PSF, PAL-EBM, PAL I

---

## Tracker Column Reference

| Column | Values |
|---|---|
| Date Found | YYYY-MM-DD (today's date) |
| Company | Company name |
| Role Title | Exact job title from posting |
| Level | EM / Sr EM / Dir / Sr Dir |
| Source | LinkedIn / Indeed / Glassdoor / Levels.fyi / Company Site |
| Job URL | Direct link to the posting |
| Comp (Listed) | Salary range if posted; blank if not listed |
| Remote | Yes / Hybrid / Unknown |
| Status | New (default on creation) |

**Deduplication rule:** Before adding any row, read the existing sheet and check for a row where both Company and Role Title match the new entry. If a match exists, skip it.

---

## Phase 1 — Source & Surface

**Triggered by:** "run job search"

### Step 1 — Search each source

Search all five sources below using the four target titles. Apply remote filter on each platform where available.

**Search titles:**
- "Engineering Manager"
- "Senior Engineering Manager"
- "Director of Engineering"
- "Senior Director of Engineering"

**Sources to search:**
1. **LinkedIn Jobs** — search each title with "Remote" filter and location set to "United States"
2. **Indeed** — search each title with "Remote" filter
3. **Glassdoor** — search each title with "Remote" filter
4. **Levels.fyi** — search job listings for each title
5. **Company career pages** — search career pages of well-known tech companies (e.g., Stripe, Airbnb, Shopify, Figma, Notion, Linear, Vercel, GitHub, HashiCorp, PagerDuty, Datadog, New Relic, Twilio, Okta, Snowflake, Databricks, Confluent, MongoDB)

### Step 2 — Evaluate each result

For each result found, evaluate fit against the candidate profile:
- Title must be one of the four target titles (or a close equivalent — use judgment)
- Must be remote (skip hybrid-only unless labeled "Remote / Hybrid")
- If comp is listed and is explicitly below $180k, skip the role
- If comp is not listed, include the role but set Comp (Listed) to blank

### Step 3 — Deduplicate

Read all existing rows from the tracker sheet. For each qualifying role, check whether a row already exists with the same Company AND Role Title. Skip any duplicates.

### Step 4 — Append new rows

For each qualifying, non-duplicate role, append a new row to the sheet using the column order:
`Date Found, Company, Role Title, Level, Source, Job URL, Comp (Listed), Remote, Status`

Set Status to "New" for all new rows.

### Step 5 — Report to user

After all sources are searched, report:
- How many new roles were added
- Total number of roles in the pipeline
- Any sources that returned no results or could not be searched

---

## Phase 2 — Pipeline Review

**Triggered by:** "review my pipeline"

### Step 1 — Read the tracker

Read all rows from the sheet.

### Step 2 — Summarize by status

Count rows grouped by the Status column and report:

```
Pipeline summary:
- New: X
- Applied: X
- Interviewing: X
- Offer: X
- Rejected: X
- Passed: X
- Total: X
```

### Step 3 — Flag stale roles

Identify any rows where Status = "New" and Date Found is more than 7 days ago. List them by Company and Role Title so the user can decide whether to act or update the status.

### Step 4 — Report to user

Deliver the summary and stale role list clearly and concisely.

---

## Running Both Phases

**Triggered by:** "run job search and review my pipeline"

Execute Phase 1 first, then Phase 2. Report both outputs together.
