# Project Decisions

This file keeps track of important decisions made during the project.

The goal is to remember what we decided, why we decided it, and when it changed.

---

## Decision 001 — Repository Structure

**Date:** September 1, 2026

**Decision:**
Use separate folders for ingestion, data quality, analysis, confidence, interface, tests, notebooks, and documentation.

**Why:**
It keeps different parts of the project organized without committing us to a complicated architecture too early.

**Status:** Active

---

## Decision 002 — APIs

**Date:** 9/4/2026

**Decision:**

We will use the following API, as of 9/4/2026:
- Monthly Retail Trade and Food Services: `api.census.gov/data/timeseries/eits/mrts.html`
    - key required
- Advance Monthly Sales for Retail and Food Services: `api.census.gov/data/timeseries/eits/marts.html`
    - key required

- BLS - Bureau of Labor Statistics
`https://api.bls.gov/publicAPI/v2/timeseries/data/`
    - key required
- BEA - Bureau of Economic Analysis
`https://apps.bea.gov/api/data/`
    - key required
- BTOS - Business Trends and Outlook Survey
`https://www.census.gov/hfp/btos/api/` 
    - key not required
- U.S. Department of Labor
`https://apiprod.dol.gov/v4/get/ETA/ui_national_weekly_claims/csv?limit=10000&X-API-KEY=OUR_KEY`
    - key required

**Why:**
Provided foundation data sources

**Status:** Active
