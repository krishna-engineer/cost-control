# Vessel Cost Control Analysis

An independent review of operating cost control across a 14-vessel fleet, covering
January 2024 to July 2026, with attention to what a handover review would need to know.

The brief asks two questions: 
1. what should management have known earlier
2. and what should be done differently on future vessels. 

It also states that a simple budget-versus-actual summary is not sufficient — so the work here is about direction and detection rather than totals.

---

## Findings

The findings are in [`brain_storming/01_findings.md`](brain_storming/01_findings.md).

For readability, the same content is also presented as a formatted page:
**[docs/vessel_cost_findings.html](docs/vessel_cost_findings.html)** 

In short, my findings are:

- **Fleet overspend has roughly doubled since 2024** — 1.92% => 2.79% => 3.17%, comparing
  the same months  (Jan-July) each year. It built gradually rather than arriving as an event.

- **The deterioration is concentrated.** 5 vessels moved by more than 3 percentage
  points; four improved. At fleet level, only Safety & Compliance and Repairs &
  Maintenance rise in every year.

- **Cost variance and deferred exposure point in opposite directions.** The 5 vessels
  carrying the most deferred work all sit within roughly 1.5 points of budget, while the
  2 worst overspenders sit near the bottom of the exposure list. The handover risk
  labels also contradict the exposure figures they sit beside.

- **Two columns in the workbook could not be relied on.** `Off_Budget_USD` and
  `Budget_Status` do not describe the spending they claim to, and both were excluded
  from the analysis rather than worked around.

---

## The analysis

All work is in a single notebook: [`notebooks/01_data_understanding.ipynb`](notebooks/01_data_understanding.ipynb).

Every cell has its output, so the notebook can be read straight through without running anything. 

It follows the order the analysis actually took.

Anyone wanting to know how a finding was reached should start there rather than with the
findings file.

---

## Scope

This submission is partial as I got short time to work on this, but I am stopping at a point where the findings are supported. 

[`brain_storming/02_next_steps.md`](brain_storming/02_next_steps.md) sets out where the analysis
will go next.

---

## Running it locally

Dependencies are managed with [uv](https://docs.astral.sh/uv/).

```bash
git clone https://github.com/krishna-engineer/cost-control.git
cd cost-control
uv sync --all-groups
uv run jupyter notebook
```

The sample dataset is already present at expected at `sample_dataset` folder.