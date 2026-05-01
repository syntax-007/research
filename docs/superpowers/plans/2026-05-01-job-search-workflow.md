# Job Search Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a Google Sheet job pipeline tracker and a plain-English workflow file that lets the agent source engineering leadership roles and manage the application pipeline on demand.

**Architecture:** Two deliverables — a Google Sheet named "Job Pipeline" (created via Google Sheets MCP) and a workflow instruction file at `workflows/job-search.md`. The workflow file is the agent's recipe; the sheet is the persistent tracker. No code is written.

**Tech Stack:** Google Sheets MCP (`mcp__google-sheets__*`), plain-English workflow format (matching existing `workflows/research-report.md` pattern)

---

### Task 1: Create the Google Sheet tracker

**Files:**
- Create: `resources/job-pipeline-sheet.md` — stores the spreadsheet URL for future reference

- [ ] **Step 1: Create the spreadsheet**

  Use the Google Sheets MCP tool `sheets_create_spreadsheet` with title `Job Pipeline`.

  Expected result: a spreadsheet ID and URL are returned.

- [ ] **Step 2: Add the header row**

  Use `sheets_update_values` to write the header row to `Sheet1!A1:I1`:

  ```
  Date Found | Company | Role Title | Level | Source | Job URL | Comp (Listed) | Remote | Status
  ```

  Exact values (as a row array):
  ```
  ["Date Found", "Company", "Role Title", "Level", "Source", "Job URL", "Comp (Listed)", "Remote", "Status"]
  ```

- [ ] **Step 3: Bold and freeze the header row**

  Use `sheets_format_cells` to bold row 1 (range `Sheet1!A1:I1`).

  Use `sheets_update_sheet_properties` to freeze the first row so it stays visible when scrolling.

- [ ] **Step 4: Verify the sheet looks correct**

  Use `sheets_get_values` on range `Sheet1!A1:I1` and confirm all 9 column headers are present in order:
  `Date Found, Company, Role Title, Level, Source, Job URL, Comp (Listed), Remote, Status`

- [ ] **Step 5: Save the sheet URL to resources**

  Create `resources/job-pipeline-sheet.md` with the following content (replace `<SPREADSHEET_URL>` and `<SPREADSHEET_ID>` with the actual values returned in Step 1):

  ```markdown
  # Job Pipeline Sheet

  **URL:** <SPREADSHEET_URL>
  **Spreadsheet ID:** <SPREADSHEET_ID>
  **Sheet name:** Sheet1

  Used by the job-search workflow to track sourced roles.
  ```

- [ ] **Step 6: Commit**

  ```bash
  git add resources/job-pipeline-sheet.md
  git commit -m "feat(resources): add job pipeline Google Sheet reference"
  ```

---

### Task 2: Write the job search workflow file

**Files:**
- Create: `workflows/job-search.md`

- [ ] **Step 1: Create `workflows/job-search.md`**

  Write the file with the following exact content (replace `<SPREADSHEET_ID>` with the actual ID saved in Task 1):

  ````markdown
  # Job Search Workflow

  A plain-English recipe for sourcing engineering leadership roles and managing the job application pipeline.

  **Google Sheet tracker:** See `resources/job-pipeline-sheet.md` for the sheet URL.
  **Spreadsheet ID:** <SPREADSHEET_ID>
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
  ````

- [ ] **Step 2: Verify the file was written correctly**

  Read `workflows/job-search.md` and confirm:
  - Candidate profile section is present with all strengths listed
  - Both Phase 1 and Phase 2 sections are present with all steps
  - Spreadsheet ID placeholder has been replaced with the actual ID from Task 1
  - Deduplication rule is clearly stated
  - Tracker column reference table matches the spec (9 columns)

- [ ] **Step 3: Commit**

  ```bash
  git add workflows/job-search.md
  git commit -m "feat(workflows): add job search workflow"
  ```

---

## Spec Coverage Check

| Spec requirement | Covered by |
|---|---|
| Google Sheet named "Job Pipeline" with 9 columns | Task 1, Steps 1–3 |
| Header row bolded and frozen | Task 1, Step 3 |
| Sheet URL saved for reference | Task 1, Step 5 |
| Deduplication by Company + Role Title | Task 2, Phase 1 Step 3 |
| Status defaults to "New" | Task 2, Phase 1 Step 4 |
| Phase 1 triggered by "run job search" | Task 2, Phase 1 |
| Phase 2 triggered by "review my pipeline" | Task 2, Phase 2 |
| Both phases triggered together | Task 2, combined trigger section |
| Stale role flagging (New + >7 days) | Task 2, Phase 2 Step 3 |
| All 5 sources searched | Task 2, Phase 1 Step 1 |
| Comp floor $180k applied | Task 2, Phase 1 Step 2 |
| Remote-only filter | Task 2, Phase 1 Step 2 |
| Candidate profile in workflow for fit evaluation | Task 2, Candidate Profile section |
