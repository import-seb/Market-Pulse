# Data Leakage in Market Prediction

## Why we care

A big problem in market prediction is accidentally using information that **wasn't actually available yet** when the prediction would have been made.

For example, a government agency might release one number in February, then revise it in March. If we test a model using the revised March number to simulate a February prediction, the model is basically cheating.

That is a form of **data leakage / look-ahead bias**.

The goal is to make sure our model only sees the information that would have existed at that point in time.

---

# ALFRED and Historical Data

ALFRED is basically FRED with a time machine.

FRED normally gives us the latest version of an economic series.

ALFRED lets us ask:

> What did this dataset look like on a specific date in the past?

That is useful because many economic indicators get revised after they are first released.

---

## Category 1: Good ALFRED Support

These series are available through ALFRED and have historical versions we can query.

| Data Series                       | Agency          | Example Ticker  | Notes                      |
| --------------------------------- | --------------- | --------------- | -------------------------- |
| Personal Consumption Expenditures | BEA             | `PCE`           | Broad consumer spending    |
| Personal Income                   | BEA             | `PI`            | Personal income            |
| Consumer Price Index              | BLS             | `CPIAUCSL`      | Inflation                  |
| Producer Price Index              | BLS             | `PPIACO`        | Producer prices            |
| Import / Export Prices            | BLS             | `IR` / `IQ`     | Trade prices               |
| Nonfarm Payrolls                  | BLS             | `PAYEMS`        | Employment                 |
| JOLTS                             | BLS             | `JLTJOR`        | Job openings               |
| Initial / Continued Claims        | DOL             | `ICSA` / `CCSA` | Weekly unemployment claims |
| Retail Inventories                | Census          | `RETAILIMSA`    | Retail inventory           |
| Wholesale Inventories             | Census          | `WHLSLRIMSA`    | Wholesale inventory        |
| E-Commerce                        | Census          | `ECOMPCTSA`     | Online sales share         |
| Consumer Credit                   | Federal Reserve | `TOTALSL`       | Consumer borrowing         |
| Household Debt                    | NY Fed          | `CMDEBT`        | Household balance sheets   |
| Gasoline Prices                   | EIA             | `GASREGW`       | Weekly gas prices          |

These are probably the easiest sources to use if we want to build a historical dataset without accidentally using revised values.

---

## Category 2: Available, but a little weird

Some datasets exist in ALFRED, but revisions are less important or the release structure is different.

### CPI Average Prices

Example: `APU0000701111`

These are prices for specific items.

They usually don't get revised much after release, so the first published value is often the final value.

### State Personal Consumption Expenditures

Example: `AKPCE`

State-level PCE is available, but it is annual and usually comes out much later than national PCE.

### Annual Retail Trade Survey

Example: `ARTS0000000001000A`

This is available, but it is mainly used as an annual benchmark.

For our purposes, monthly retail indicators may be more useful because we can actually observe revisions happening over time.

---

## Category 3: Not on ALFRED

For these, we would need to get historical files directly from the original agency.

### USDA Food Expenditure Series

Not on ALFRED.

Possible alternative: BEA's `DFXARC1M027SBEA` for food purchased for off-premises consumption.

### USDA Food-at-Home Monthly Area Prices

Not on ALFRED.

We would need USDA's own files.

### USDA Food Price Outlook

Not on ALFRED.

These are mainly released through USDA reports rather than a normal historical time-series API.

### USDA Market News Retail Reports

Not on ALFRED.

These would need to come from USDA AMS.

### Census Business Trends and Outlook Survey

Not on ALFRED.

Historical data would need to come directly from Census.

### NY Fed Survey of Consumer Expectations

Not on ALFRED.

Detailed historical SCE data needs to come from the New York Fed.

### BLS Consumer Expenditure Survey

Not on ALFRED.

BLS publishes this through larger tables and microdata files.

### IRS Filing Season Statistics

Not on ALFRED.

These would have to come directly from IRS historical tables.


---

# The Dates We Need to Keep Straight

This is probably the most important part.

One number can have several different dates attached to it.

## Observation / Reference Date

This is:

> What period does the number describe?

Example:

BLS reports unemployment for **January 2026**.

January 2026 is the observation/reference period.

---

## Publication / Release Date

This is:

> When did people actually get access to the number?

Example:

January unemployment data might be released on **February 6, 2026**.

So:

* Observation date = January 2026
* Release date = February 6, 2026

The model cannot use that January number before February 6 because nobody knew it yet.

---

## Revision / Vintage Date

This is:

> When did an old number get updated?

Example:

January employment data is released in February.

Then in March, BLS gets more information and changes the January number.

The March release creates a **new vintage** of the January data.

This matters because if we're pretending to make a forecast in February, we must use the February version, not the revised March version.

---

## Prediction / Forecast Date

This is:

> When are we pretending the model is making its prediction?

This date controls what the model is allowed to know.

If our prediction date is:

**March 1, 2024**

then every feature needs to answer:

> Was this information publicly available by March 1, 2024?

If not, the model should not get it.

---

# The Basic Rule

For every historical prediction:

**Only use data that had already been released by the prediction date.**

Not:

> What do we know about January 2024 today?

Instead:

> What did someone actually know on March 1, 2024?

That difference is the whole point of using historical vintages.

---

# Example

Suppose we're predicting retail demand on March 1.

At that moment we might have:

* last week's unemployment claims
* last week's gas prices
* January retail sales
* December inventory numbers
* quarterly data from several months ago

That looks messy, but that is realistic.

Economic data does **not** all update at the same time.

If we clean everything up by filling in values that were released later, we may accidentally give the model future information.

---

# Missing Data Is Sometimes Correct

If we forecast every week, some monthly or quarterly features simply won't have a new value yet.

That's okay.

A missing value can literally mean:

> This information had not been released yet.

We should be careful about filling those values because some filling methods could introduce future information.

Models like **XGBoost** can handle missing values directly.

So instead of forcing every feature to have a value, we may be able to leave some values missing and let the model learn how to work with the information available at that time.

---

# Useful Research Papers

## Croushore & Stark — "A Real-Time Data Set for Macroeconomists"

This is one of the foundational papers on this problem.

The main idea is that testing forecasting models using today's revised economic data can give misleading results.

They built historical snapshots of what economic data actually looked like at different points in time.

This work helped lead to the Philadelphia Fed's real-time database and the type of historical data available through ALFRED.

Paper:
https://www.sciencedirect.com/science/article/abs/pii/S0304407601000720

Philadelphia Fed database:
https://www.philadelphiafed.org/surveys-and-data/real-time-data-research/real-time-data-set-for-macroeconomists

---

## Koenig — "The Use and Abuse of 'Real-Time' Data in Economic Forecasting"

This paper also looks at what happens when forecasts use revised data instead of the information that was actually available at the time.

It gives examples using indicators such as retail sales and industrial production.

Paper:
https://www.philadelphiafed.org/-/media/frbp/assets/events/2001/real-time-data-analysis/koenig.pdf

---

# Newer ML Examples

## MacroCast

This paper applies the same idea to newer forecasting models.

The important part for us is that it uses historical vintage data so future revisions don't leak into older predictions.

Paper:
https://arxiv.org/html/2606.28670v1

---

## Macro-Aware Time Series Forecasting

This paper is interesting because it combines data that updates at different speeds.

For example:

* daily or weekly market data
* monthly economic data
* quarterly macroeconomic data

The important idea is that each feature is aligned based on what was actually available when the forecast was made.

Paper:
https://arxiv.org/html/2606.00624v1

---

# How Often Some Important Indicators Get Revised

| Indicator             | Agency          | Normal Revisions                           |
| --------------------- | --------------- | ------------------------------------------ |
| GDP                   | BEA             | Next 2 months + larger benchmark revisions |
| Nonfarm Payrolls      | BLS             | Next 2 months + annual revision            |
| Retail Sales          | Census          | Next 2 months + annual revision            |
| Personal Income / PCE | BEA             | Next 2 months + annual revision            |
| Industrial Production | Federal Reserve | Next few months + annual revision          |
| Housing Starts        | Census / HUD    | Next 2 months + annual revision            |

CPI and PPI usually behave differently.

The original price observations are rarely revised, although seasonally adjusted versions can change later.

So the amount of leakage risk depends on the dataset.