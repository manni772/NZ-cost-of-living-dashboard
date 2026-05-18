# NZ Cost of Living Dashboard

An interactive data dashboard tracking New Zealand's inflation, wage growth, and housing costs. Built with vanilla JavaScript and Canvas API — no chart libraries used.

## Live demo
[manni772.github.io/nz-cost-of-living](https://manni772.github.io/nz-cost-of-living)

No API key or login required.

## Data sources
All data sourced from official NZ government and research publications:
- **Stats NZ** — CPI quarterly series, Labour Cost Index (LCI)
- **Reserve Bank of NZ (RBNZ)** — Tradeable/non-tradeable inflation split, OCR decisions
- **REINZ** — National and regional median house prices
- **CoreLogic** — Housing affordability and price-to-income ratios
- **NZ Public Service Commission** — Sector wage growth (LCI technical reports)

Data to Q1 2025.

## Features
- **Inflation tab** — Annual CPI line chart (all groups, tradeable, non-tradeable), CPI by category, cumulative price level since 2019
- **Wages tab** — Wage growth vs CPI, real wage growth bar chart showing 10 quarters of negative real wages, sector wage comparison
- **Housing tab** — Median prices by region, price-to-income ratios, national house price trend 2018–2025
- **Policy context tab** — Connects data trends to active NZ policy debates: fiscal drag, Working for Families adequacy, housing tax settings, distributional impacts

## Tech stack
- Vanilla HTML, CSS, JavaScript
- Canvas API for all charts (custom built — no Chart.js or similar)
- No external dependencies

## Key findings
- CPI peaked at 7.3% in Q2 2022 — highest since 1990
- Real wages fell for 10 consecutive quarters (2021–2023)
- National median house price peaked at ~$925k in 2021, correcting ~18% before stabilising
- Auckland price-to-income ratio of 11.2× — one of the least affordable cities in the OECD

## Built by
Manmeet Singh · [github.com/manni772](https://github.com/manni772)
