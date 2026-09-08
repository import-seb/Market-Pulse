# Project Findings

This file records useful findings from our data analysis, experiments, and outside research.

Findings should include enough information that another team member can understand where the conclusion came from.

---

## EX. Finding 001 — Census sales figures are not adjusted for inflation

**Date:** 9/4/2026

**Finding:**
The Census sales figures are not adjusted for inflation, even when seasonally adjusted. 

**Source:**
`docs\research\data_search\retail and food services sales.xlsx`

**Why it matters:**
We'll have to join CPI/PCE price indexes so we can distinguish “people spent more dollars” from “people actually bought more stuff.”

**Related work:**
N/A

---

## EX. Finding 002 — Vintage data sources

**Date:** 9/7/2026

**Finding:**
A vintage compatibility mapping for our dataset to the ALFRED API.

------------------------------
#### Category 1: Fully Supported on ALFRED
These series are native to FRED, fully logged historically, and fully support the point-in-time vintage_dates parameter.

| Data Series Name | Original Agency | Primary ALFRED Ticker (series_id) | Notes / Core Aggregates |
|---|---|---|---|
| Personal Consumption Expenditures (PCE) | BEA | PCE | Broadest aggregate. |
| Personal Income and Outlays | BEA | PI / PCE | PI tracks real-time personal income data. |
| Consumer Price Index (CPI-U) | BLS | CPIAUCSL | Seasonally adjusted, all urban items. |
| Producer Price Index (PPI) | BLS | PPIACO | Broad aggregate (All Commodities). |
| BLS Import/Export Price Indexes | BLS | IR (Imports) / IQ (Exports) | Broadest trade metrics available. |
| Current Employment Statistics (CES) | BLS | PAYEMS | Non-Farm Payrolls dataset. |
| Job Openings & Labor Turnover (JOLTS) | BLS | JLTJOR | Ticker maps to total Openings (Rate). |
| Weekly Unemployment Claims | U.S. DOL | ICSA (Initial) / CCSA (Continued) | Weekly point-in-time claims. |
| Monthly Retail & Trade Inventories | Census | RETAILIMSA | Total retail inventories. |
| Monthly Wholesale Trade | Census | WHLSLRIMSA | Total merchant wholesalers inventories. |
| Quarterly E-Commerce | Census | ECOMPCTSA | E-commerce retail sales as a % of total. |
| Consumer Credit G.19 | Fed Board | TOTALSL | Total outstanding consumer credit. |
| Household Debt and Credit | NY Fed | CMDEBT | Maps the NY Fed consumer balance sheets. |
| Weekly Gasoline/Diesel Prices | EIA | GASREGW | Weekly U.S. retail regular gasoline prices. |

------------------------------
#### Category 2: Partials & Structural Nuances
These are on ALFRED, but their point-in-time behavior differs because of the structure of the data surveys.

* CPI Average Prices — BLS: (e.g., APU0000701111 for flour/bread). While available, individual consumer item raw prices rarely trigger retroactive vintage revisions. The initial release is almost always the final record.
* State Personal Consumption Expenditures — BEA: (e.g., AKPCE for Alaska). Available annually on ALFRED, but regional adjustments lag significantly behind the headline national PCE numbers.
* Annual Retail Trade Survey (ARTS) — Census: (e.g., ARTS0000000001000A). Annual tracking numbers are available on FRED, but because it is an annual benchmark survey used to adjust the monthly indicators, developers usually focus on querying the monthly indicators (RETAILIMSA) to observe revision shifts.
* 

------------------------------
#### Category 3: Not Available on ALFRED
These series are completely absent from the ALFRED. To get historical data, we have to pull raw archive files directly from the original host.

* Food Expenditure Series — USDA ERS: Not on ALFRED. Use BEA's DFXARC1M027SBEA (Food for off-premises consumption) as a proxy, or scrape the USDA ERS Site.
* Food-at-Home Monthly Area Prices — USDA ERS: Not on ALFRED. This is a localized experimental dataset hosted solely inside USDA analytical files.
* Food Price Outlook — USDA ERS: Not on ALFRED. The USDA generates these as structural monthly text/PDF forecast reports rather than time-series asset feeds.
* USDA Market News Retail Reports — USDA AMS: Not on ALFRED. These local spot-price agricultural market sheets must be fetched from the Agricultural Marketing Service (AMS).
* Business Trends and Outlook Survey — Census: Not on ALFRED. This was designed as a rapid-response experimental survey by the Census Bureau starting in the pandemic era and does not port to vintage Fed databases.
* Survey of Consumer Expectations (SCE) & SCE Household Spending — NY Fed: Not on ALFRED. While the St. Louis Fed features select baseline consumer surveys, the microdata and granular timeline metrics for the SCE must be downloaded straight from the New York Fed Data Center.
* Consumer Expenditure Survey (CE) — BLS: Not on ALFRED. The BLS publishes this annually via large standalone microdata files or nested tables due to its survey size, bypassing FRED entirely.
* IRS Filing Season Statistics — IRS: Not on ALFRED. You must scrape these administrative tax processing counts directly via the IRS.gov newsroom tables.


**Source:**
https://arxiv.org/html/2606.28670v1

**Why it matters:**
The foundational agencies offer highly revised data. ALFRED mirrors the exact data series from those agencies but layers a universal `vintage_dates` parameter on top of them.

**Related work:**
N/A

---