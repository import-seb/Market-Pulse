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

ALFRED API will supplement the foundational APIS (Census.gov, BLS, BEA, and DOL)

**Why:**
The foundational agencies offer highly revised data. ALFRED mirrors the exact data series from those agencies but layers a universal `vintage_dates` parameter on top of them.

**Status:** Active

