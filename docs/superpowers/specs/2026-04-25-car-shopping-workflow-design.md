# Car Shopping Workflow — Design Spec
**Date:** 2026-04-25
**Status:** Approved

---

## Overview

A plain-English workflow for actively searching for a car when the user already knows what they want. Takes a structured criteria list as input and produces a ranked shortlist of dealer listings with red flag annotations and pricing outlier flags.

---

## Scope

- **In scope:** Active listing search, filtering, price-check outliers, red flag annotation, output
- **Out of scope:** Early-stage car exploration, negotiation guidance, financing advice
- **Seller type:** Dealer listings only — private sellers are excluded

---

## Steps

### Step 1 — Intake

Ask the user for their search criteria before doing anything. Required fields:
- Vehicle type / make / model (can be flexible, e.g., "Honda Accord or Toyota Camry")
- Year range
- Max mileage
- Max price
- Location + max distance (miles)
- Any must-haves or dealbreakers (e.g., color, transmission, features)

Wait for all criteria before proceeding.

---

### Step 2 — Search

Using the criteria from Step 1, run web searches across major dealer listing sites (CarGurus, Cars.com, Autotrader). Construct search queries from the criteria — make, model, year range, price, location. Collect all matching listings, noting:
- Title
- Price
- Mileage
- Year
- Location
- Listing URL
- Listing date

Aim for at least 10 candidates before moving on. If fewer than 5 are found, note this and proceed with what's available.

---

### Step 3 — Filter

Apply the user's criteria to cut the candidate list:
- Drop anything outside price range, mileage cap, year range, or distance
- Drop anything missing key details (no price, no mileage, no location)
- Apply any dealbreakers from intake
- **Drop all private seller listings — only dealer listings proceed**

---

### Step 4 — Price Check

For each remaining candidate, estimate fair market value for that specific make/model/year/mileage via web search. Flag pricing outliers:

- `OVERPRICED` — listing price >15% above market estimate
- `UNDERPRICED` — listing price >15% below market estimate (treat as a potential red flag)

Listings within the normal range receive no pricing label.

---

### Step 5 — Red Flag Check

For each dealer listing, scan and annotate with any of the following flags:

- `UNDERPRICED` — already flagged in Step 4, but note it here as a warning
- `HIGH MILEAGE` — average mileage/year exceeds 15k (e.g., a 5-year-old car with >75k miles)
- `SLOW TO SELL` — listing posted >60 days ago
- `SPARSE LISTING` — vague or missing description
- `FEW PHOTOS` — no photos or very few photos

A listing can have multiple flags. Flags are informational, not disqualifying.

---

### Step 6 — Output

Save shortlist to `output/car-search-[YYYY-MM-DD].md` in this format:

```
# Car Search Results
**Date:** [today] | **Criteria:** [summary of criteria]

---

## Shortlist

| # | Listing | Price | Miles | Year | Distance | Flags |
|---|---------|-------|-------|------|----------|-------|
| 1 | [title](url) | $X | Xk | XXXX | X mi | FLAG1 / FLAG2 |
...

---

## Notes
- [Any notable observations, e.g., "Only 4 results found — consider expanding criteria"]
```

Sort order: fewest flags first, then price ascending.

Confirm the output file path to the user when done.

---

## Design Decisions

- **Dealer-only:** Private sellers excluded — reduces noise and risk for the user
- **Pricing threshold at 15%:** Balances sensitivity without over-flagging normal market variance
- **Mileage threshold at 15k/year:** Industry-standard "average" annual mileage benchmark
- **Listing age at 60 days:** Signals a car that the market has passed on
- **Linear pipeline:** Matches existing workflow style; simple, predictable, easy to maintain
