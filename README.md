# Airbnb Global Performance Analysis — Power BI Dashboard

A 3-page Power BI dashboard analyzing Airbnb's global footprint across **10 cities from 2008 to 2020**, built on nearly 280K listings and 5.3 million reviews. The project maps Airbnb's full supply lifecycle, digs into pricing and guest satisfaction, and surfaces patterns in host trust and seasonal demand.

> Dashboard exported as PDF — see `Airbnb Power BI Project.pdf`

---

## What This Project Covers

Six questions drove the analysis:

- Which cities dominate Airbnb's global market share?
- How did listing supply evolve through Airbnb's growth phases?
- Which room types command the highest prices?
- What dimensions of guest experience most influence satisfaction?
- What seasonal patterns show up in review activity?
- How verified and trustworthy is the host base?

---

## Dashboard Pages

### Page 1 — Supply Growth

Tracks how Airbnb's listing inventory grew and contracted over 12 years, broken down by room type (Entire Place, Private Room, Hotel Room, Shared Room). The timeline maps to six business phases:

| Phase | Period | What happened |
|---|---|---|
| Introduction | 2008–2010 | Early listings, mostly private rooms |
| Growth | 2010–2013 | Rapid supply expansion |
| Maturity | 2013–2015 | Peak new listings reached in 2015 |
| Decline | 2015–2017 | Local regulations slowed growth; Airbnb turned profitable in 2016 |
| Reinvention | 2017–2019 | Hotel rooms introduced; new growth phase began |
| COVID-19 | 2020 | Sharp drop across all listing types |

---

### Page 2 — Market Share & Ratings

Shows which cities dominate the platform and how guests rate their experience.

- **Pareto chart** ranks 10 cities by listing count — Paris, NYC, and Sydney together hold nearly half of all listings and 59% of total reviews
- **Average price by room type**: Hotel room ($800) > Entire place ($673) > Shared room ($580) > Private room ($462)
- **Ratings heatmap** covers 5 dimensions (Accuracy, Cleanliness, Communication, Location, Value) across all 10 cities
- Cleanliness scores lowest platform-wide; Value scores second lowest, especially in high-cost cities like NYC and Paris

---

### Page 3 — Trust & Seasonality

Covers host verification patterns and how review volume shifts by month.

- **2x2 trust matrix** splits hosts by identity verification and profile picture presence
  - 66.9% are fully verified with a profile photo (highest trust tier)
  - Only 0.3% have neither signal (lowest trust tier)
- **Seasonality chart** shows Paris and Rome dominate Apr–Aug reviews, reflecting peak European summer travel; New York peaks Nov–Dec
- **Review frequency**: 86.5% of reviewers wrote only once; 98.8% wrote 3 times or fewer
- One reviewer account logged 283 total reviews, with the last two submitted on the same day for two Bangkok listings — flagged as a likely data anomaly

---

## Key Metrics

| Metric | Value |
|---|---|
| Total Listings | 2,79,712 |
| Cities Covered | 10 |
| Unique Hosts | 1,82,024 |
| Property Types | 144 |
| Total Reviews | 5,373K |
| DAX Measures Built | 32 |

---

## Cities Covered

Paris · New York · Sydney · Rome · Rio de Janeiro · Istanbul · Mexico City · Bangkok · Cape Town · Hong Kong

---

## Tech Stack

| Tool | Usage |
|---|---|
| Power BI Desktop | Dashboard building and visual design |
| DAX | 32 custom measures — aggregations, Pareto logic, trust segmentation, seasonality |
| Power Query | Data transformation, column typing, table relationships |
| Data Modeling | Two-table fact schema (Listings + Reviews) joined on `listing_id` |

---

## Workflow

```
Raw CSV Data → Power Query (Transform & Clean) → Data Modeling → DAX Measures → Dashboard
```

---

## Data Model

Two fact tables connected via `listing_id`:

- **Listings** — property details, host info, room type, pricing, review scores
- **Reviews** — individual review records with date and reviewer ID

---

## DAX Highlights

```dax
-- Pareto cumulative % for city market share
Cumulative % =
VAR CurrentRank = [City Rank]
RETURN
  DIVIDE(
    CALCULATE([Total Listing], FILTER(ALL(Listings[city]), [City Rank] <= CurrentRank)),
    CALCULATE([Total Listing], ALL(Listings[city]))
  )

-- Host trust segmentation (verified identity + profile picture)
Verified_Profile =
  CALCULATE([Hosts Total],
    Listings[host_identity_verified] = "t",
    Listings[host_has_profile_pic] = "t")

-- Monthly review share for seasonality analysis
% of Monthly Reviews =
  DIVIDE(
    CALCULATE([Total Reviews], ALLEXCEPT(Reviews, Reviews[date])),
    CALCULATE([Total Reviews], ALL(Reviews[date]))
  )
```

---

## Notable Findings

- Paris, NYC, and Sydney account for roughly 23%, 13%, and 10% of total listings — supply is heavily concentrated in Western cities
- Hotel rooms averaged $800, the highest of any room type, despite being the newest category on the platform (introduced post-2017)
- Cleanliness is the lowest-rated dimension in nearly every city — a consistent quality gap at scale
- Mexico City and Rio de Janeiro rank highest across all five satisfaction dimensions; Istanbul and Hong Kong rank lowest
- 98.8% of reviewers wrote 3 or fewer reviews — review volume is driven almost entirely by one-time guests, not repeat contributors
- One reviewer account recorded 283 reviews, with the last two submitted on the same day for two Bangkok listings — likely a data anomaly worth noting

---

## Files in This Repo

```
├── Airbnb Power BI Project.pdf  # Dashboard export (all 3 pages)
├── Data/
│   ├── listings.csv             # Listings data (~280K rows)
│   └── reviews.csv              # Reviews data (~5.3M rows)
└── README.md
```

---

## How to Use

1. Clone or download the repo
2. Open the `Data/` folder and load `listings.csv` and `reviews.csv` into Power BI Desktop
3. Rebuild or connect your own `.pbix` file using the data model described above
4. Refer to `Airbnb Power BI Project.pdf` to see the full dashboard output

---

*Data sourced from publicly available Airbnb listings datasets. This project is for portfolio and learning purposes.*
