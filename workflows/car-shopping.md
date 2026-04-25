# Car Shopping Workflow

A plain-English recipe for finding dealer car listings that match your criteria, with pricing and red flag annotations.

**Use when:** You already know what type of car you want and need to find real listings.

---

## Step 1 — Intake (Ask Before Searching)

Before searching, collect the following from the user. Wait for all answers before proceeding.

- **Make / Model** — Can be flexible (e.g., "Honda Accord or Toyota Camry")
- **Year range** — e.g., 2018–2022
- **Max mileage** — e.g., 60,000 miles
- **Max price** — e.g., $25,000
- **Location + max distance** — e.g., Chicago, IL within 50 miles
- **Must-haves / dealbreakers** — e.g., automatic transmission, no black exterior

Do not proceed until all fields are answered.

---

## Step 2 — Search

Search for listings across these dealer listing sites:
- CarGurus
- Cars.com
- Autotrader

Construct search queries from the intake criteria (make, model, year range, price, location). For each site, collect all matching listings with:
- Title
- Price
- Mileage
- Year
- Location (city/state)
- Listing URL
- Listing date (date posted)

**Target:** Collect at least 10 candidates. If fewer than 5 are found, note this in the output and proceed with what's available.

---

## Step 3 — Filter

Apply the user's criteria to eliminate listings that don't qualify:

- Drop listings outside the price range
- Drop listings exceeding max mileage
- Drop listings outside the year range
- Drop listings beyond the max distance
- Drop listings missing key details (no price, no mileage, or no location)
- Apply any dealbreakers from intake (e.g., wrong transmission)
- **Drop all private seller listings — only dealer listings proceed**

What remains is the working candidate list.

---

## Step 4 — Price Check

For each candidate, estimate the fair market value for that specific make/model/year/mileage. Use a web search such as:

> "2019 Honda Accord 45000 miles market value"

Compare the listing price to the market estimate. Apply labels for significant outliers:

- **OVERPRICED** — listing price is more than 15% above market estimate
- **UNDERPRICED** — listing price is more than 15% below market estimate

Listings within 15% of market receive no pricing label.

---

## Step 5 — Red Flag Check

For each dealer listing, check for the following warning signs and annotate accordingly:

| Flag | Condition |
|------|-----------|
| `UNDERPRICED` | Already flagged in Step 4 — repeat here as a caution |
| `HIGH MILEAGE` | Mileage ÷ age of car > 15,000 miles/year (e.g., 5-year-old car with >75k miles) |
| `SLOW TO SELL` | Listing was posted more than 60 days ago |
| `SPARSE LISTING` | Description is vague, minimal, or missing |
| `FEW PHOTOS` | No photos or only 1–2 photos |

A listing can have multiple flags. Flags are informational — they do not automatically disqualify a listing.

---

## Step 6 — Save Output

Save the shortlist to: `output/car-search-[YYYY-MM-DD].md`

Use this format:

```
# Car Search Results
**Date:** [today's date] | **Criteria:** [one-line summary of intake criteria]

---

## Shortlist

| # | Listing | Price | Miles | Year | Distance | Flags |
|---|---------|-------|-------|------|----------|-------|
| 1 | [Title](url) | $X,XXX | XXk | XXXX | XX mi | FLAG1 / FLAG2 |
| 2 | [Title](url) | $X,XXX | XXk | XXXX | XX mi | |
...

---

## Notes
- [Any observations, e.g., "Only 4 results found — consider expanding year range or distance"]
```

**Sort order:** Fewest flags first, then price ascending within each flag tier.

Confirm the output file path to the user when done.
