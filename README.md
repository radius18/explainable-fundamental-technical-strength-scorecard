# Explainable Fundamental + Technical Strength Scorecard

A Thinkorswim/ThinkScript portfolio case study focused on **requirements analysis, QA/UAT, data validation, scoring logic, debugging, and explainability**.

## Project Overview

This project is a rule-based 0–100 decision-support scorecard that combines five bounded components:

- Valuation / Growth — 0–20
- Profitability — 0–20
- Financial Strength — 0–20
- Technical Trend — 0–20
- Technical Momentum — 0–20

The scorecard was designed to make its logic inspectable instead of reducing the result to an unexplained single number. It supports separate Value and Growth modes and exposes the category contributions directly in the chart label.

## Working Example

The current portfolio example uses **NVDA on a daily chart in Growth Mode**. The label shows the total score together with the individual category contributions so the result can be inspected rather than treated as a black-box signal.

Selected screenshot: `Screenshot 2026-10-16 020931.png`

## Architecture and Scoring Logic

The scorecard separates five categories and keeps each category bounded to a maximum of 20 points. Value Mode uses price-to-book and price-to-sales thresholds, while Growth Mode uses gross profit margin as a quality-oriented proxy. Profitability, financial strength, trend, and momentum are scored independently before the total is capped at 100.

Selected architecture screenshot: `Screenshot 2026-10-16 021017.png`

## QA Finding: Missing Data Could Change the Meaning of the Score

A later QA review found a semantic defect in the financial-strength logic. Unavailable long-term-debt data could be converted to zero, and the low-debt rule could then award favorable points. That meant **unknown debt could be interpreted as low debt**.

The corrected implementation separates data availability from score arithmetic. Missing debt now receives zero debt points instead of being treated as zero leverage.

Corrected logic:

```thinkscript
def debtScore =
    if !hasDebt then 0
    else if debtCap < 0.6 then 8
    else 0;
```

Selected corrected-code screenshot: `Screenshot 2026-10-16 023929.png`

## Development and QA Lessons

- Compile success is only a syntax gate; it does not prove semantic correctness.
- Missing data is not just a completeness issue—it can change scoring behavior.
- Defensive fallbacks can create hidden logic defects if they change the meaning of an input.
- Composite scores benefit from explicit category bounds and visible sub-scores.
- Runtime behavior, edge cases, and data availability need separate validation.
- Uncertainty should be surfaced rather than silently converted into favorable values.

## Value and Growth Modes

The scorecard supports two valuation approaches:

- **Value Mode** uses price-to-book and price-to-sales thresholds.
- **Growth Mode** uses gross profit margin as a quality-oriented proxy.

If both modes are enabled, Growth Mode takes priority. For the NVDA portfolio example, Growth Mode is used.

## Timeframe Behavior

The technical components use the active chart aggregation. As a result, technical trend and momentum can change when the chart timeframe changes. RSI, moving averages, and the 10-bar momentum comparison are calculated from the current aggregation.

For this portfolio case, the **daily timeframe** is the preferred presentation because the score combines relatively slow-moving fundamental measures with technical trend and momentum context.

## Skills Demonstrated

- Quality Assurance
- User Acceptance Testing (UAT)
- Requirements Analysis
- Data Validation
- Troubleshooting
- Systems Thinking
- Decision-Support Design
- ThinkScript
- Technical Documentation
- Explainability and Edge-Case Reasoning

## Scope and Limitations

This is a technical portfolio and decision-support case study. It is not presented as a predictive trading model, investment recommendation, backtested strategy, or claim of market outperformance.

The public repository focuses on the design, QA process, and selected evidence rather than publishing the full private development history.
