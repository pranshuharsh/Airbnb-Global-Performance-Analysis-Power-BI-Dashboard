# Airbnb Global Performance Analysis | Power BI Dashboard

A 3-page Power BI dashboard analyzing Airbnb's global performance across 10 cities from 2008 to 2020 — built on 2,79,712 listings and 5.3M reviews. The project examines supply growth lifecycle, pricing by room type, guest satisfaction across 5 dimensions, host trust signals, and seasonal review patterns.

---

## Business Questions Answered

- Which cities dominate Airbnb's global market share?
- How did listing inventory evolve across Airbnb's growth lifecycle?
- Which room types generate the highest average prices?
- What dimensions of guest experience most affect satisfaction scores?
- What seasonal patterns drive review activity across cities?
- How verified and trustworthy is the host base?

---

## Dashboard Pages

### Page 1 — Supply Growth

Tracks how Airbnb's listing inventory grew and contracted over time, broken down by room type (Entire Place, Private Room, Hotel Room, Shared Room). The timeline is structured across six business phases:

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

- Pareto chart ranks 10 cities by listing count — Paris, NYC, and Sydney together account for nearly half of all listings and 59% of total reviews
- Average price breakdown by room type: Hotel room ($800) > Entire place ($673) > Shared room ($580) > Private room ($462)
- Ratings heatmap across 5 dimensions — Accuracy, Cleanliness, Communication, Location, Value — for all 10 cities
- Cleanliness scores lowest platform-wide; Value scores second lowest, particularly in high-cost cities like NYC and Paris


---

### Page 3 — Trust & Seasonality

Covers host verification patterns and monthly review frequency.

- 2x2 trust matrix splits hosts by identity verification and profile picture presence
  - 66.9% are fully verified with a profile photo (highest trust)
  - Only 0.3% have neither signal (lowest trust)
- Seasonality chart shows Paris and Rome dominate Apr–Aug reviews; New York peaks Nov–Dec
- Review frequency: 86.5% of reviewers wrote only once; 98.8% wrote 3 times or fewer


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
| Power BI Desktop | Dashboard building, visual design |
| DAX | 32 custom measures — aggregations, Pareto logic, trust segmentation, seasonality |
| Power Query | Data transformation, column typing, table relationships |
| Data Modeling | Two-table fact schema (Listings + Reviews) joined on `listing_id` |

---

## Workflow

```
Raw CSV Data → Power Query (Transform & Clean) → Data Modeling (Relationships) → DAX Measures → Dashboard Visualization
```

---

## Data Model

Two fact tables connected via `listing_id`:

- **Listings** — property details, host info, room type, pricing, review scores
- **Reviews** — individual review records with date and reviewer ID

---

## DAX Highlights

```
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

- Paris, NYC, and Sydney account for ~23%, ~13%, and ~10% of total listings respectively — high market concentration in Western cities
- Hotel rooms averaged $800 — the highest of any room type — despite being the newest category on the platform (introduced post-2017)
- Cleanliness is the lowest-rated dimension in nearly every city, pointing to a consistent quality gap at scale
- Mexico City and Rio de Janeiro rank highest across all satisfaction dimensions; Istanbul and Hong Kong rank lowest
- 98.8% of reviewers wrote 3 or fewer reviews — the platform's review base is driven almost entirely by one-time guests, not repeat reviewers
- One reviewer account recorded 283 total reviews, with the last two submitted on the same day for two Bangkok listings — flagged as a potential data anomaly

---

## Files in This Repo

```
├── Airbnb.pbix                  # Power BI report file
├── Data/
│   ├── listings.csv             # Listings data (~280K rows)
│   └── reviews.csv              # Reviews data (~5.3M rows)
├── screenshots/
│   ├── page1.png
│   ├── page2.png
│   └── page3.png
└── README.md
```

---

*Data sourced from publicly available Airbnb listings datasets. This project is for portfolio and learning purposes.*
