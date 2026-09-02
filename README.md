# Market Pulse

Market Pulse is a capstone project focused on taking different market signals and turning them into something a business can actually understand and use.

The basic idea is simple:

* Collect useful market data
* Check whether the data looks reliable
* Look for real trends instead of random movement
* Show how confident we are in the result
* Present the result in a clear way

The project should also be able to say **“we do not have enough information yet”** instead of forcing an answer.

## Team

* Maria (Project Lead)
* Maksim
* Sebastian

Product Owner: Ravi Venugopal, Giggso

## Project Structure

```text
docs/        Project notes and technical decisions
src/         Main project code
tests/       Tests
data/        Raw and cleaned data
notebooks/   Exploration and experiments
```

Inside `src/`:

```text
ingestion/   Getting data into the project
quality/     Checking for missing, old, or unusual data
analysis/    Finding trends in the data
confidence/  Measuring how much we should trust a result
interface/   Showing the results to the user
```

## Current Status

The exact market questions, data sources, and final design are still being decided with the Product Owner.

For now, the repo is set up so the team can start organizing the work.

## Setup

Create a Python environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Working Together

We will use GitHub Issues to track work and Pull Requests to review changes before merging them.

Larger future work stays in the backlog. Once a task is clear enough for someone to start, it can move into Ready and then through the rest of the project board.

More detailed project decisions will be kept in the `docs/` folder.
