# Job Search Workflow Design

**Date:** 2026-05-01
**Status:** Approved

---

## Overview

A two-phase, on-demand job search workflow that sources relevant engineering leadership roles from multiple job boards and tracks them in a Google Sheet pipeline. The agent is triggered by plain-English commands and updates the tracker with deduplicated results.

---

## Target Roles

- Engineering Manager
- Senior Engineering Manager
- Director of Engineering
- Senior Director of Engineering

---

## Search Criteria

- **Remote:** Remote only (US-based companies)
- **Compensation:** ≥ $180k/year where listed; unlisted roles are included but flagged
- **Sources:** LinkedIn Jobs, Indeed, Glassdoor, Levels.fyi, company career pages

**Candidate profile context (used to evaluate fit):**
- 15+ years experience; currently Senior Engineering Manager at Oak Street Health (CVS)
- Background: healthtech, fintech, SaaS
- Strengths: Agile/Scrum, team scaling, CI/CD, HIPAA/SOC2 compliance, AWS/Azure, AI/generative AI, C#, Python, JavaScript, React, Grafana
- Certifications: PSM I, PSF, PAL-EBM, PAL I

---

## Google Sheet Tracker

Sheet name: **Job Pipeline**

| Column | Description |
|---|---|
| Date Found | Date the role was added (YYYY-MM-DD) |
| Company | Company name |
| Role Title | Exact job title from posting |
| Level | EM / Sr EM / Dir / Sr Dir |
| Source | LinkedIn / Indeed / Glassdoor / Levels.fyi / Company Site |
| Job URL | Direct link to the job posting |
| Comp (Listed) | Salary range if posted; blank if not listed |
| Remote | Yes / Hybrid / Unknown |
| Status | New / Applied / Interviewing / Offer / Rejected / Passed |

**Deduplication rule:** Before adding a row, check for an existing entry with the same Company + Role Title. Skip if duplicate found.

**Default status on creation:** New

---

## Workflow Phases

### Phase 1 — Source & Surface
Triggered by: *"run job search"*

1. Search each source for the four target titles with remote filter applied
2. Evaluate each result against search criteria (title match, remote, comp floor where listed)
3. Deduplicate against existing tracker rows
4. Append qualifying new roles to the Google Sheet
5. Report: number of new roles added and total pipeline size

### Phase 2 — Pipeline Review
Triggered by: *"review my pipeline"*

1. Read current tracker
2. Summarize counts by Status column
3. Flag any roles with Status = "New" that were added more than 7 days ago
4. Report summary to user

Both phases can be run together: *"run job search and review my pipeline"*

---

## Workflow File

Location: `workflows/job-search.md`

Follows the same plain-English recipe format as existing workflows in the project.

---

## Out of Scope

- Cover letter or outreach draft generation (can be added as Phase 3 later)
- Automated scheduled runs (on-demand only for now)
- Interview prep or offer comparison
