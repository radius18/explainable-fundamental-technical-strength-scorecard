# Explainable Fundamental + Technical Strength Scorecard

A Thinkorswim/ThinkScript portfolio case study focused on **requirements analysis, QA/UAT, data validation, scoring logic, debugging, and explainability**.

## Project Overview

This project is a rule-based 0–100 decision-support scorecard that combines five bounded components:

- Valuation / Growth — 0–20
- Profitability — 0–20
- Financial Strength — 0–20
- Technical Trend — 0–20
- Technical Momentum — 0–20

The scorecard was designed to make the logic inspectable rather than reduce the result to an unexplained single number. It supports separate Value and Growth modes and exposes the category contributions directly in the chart label.

## Working Example

![NVDA daily scorecard](Screenshot%202026-10-16%20020931.png)

*Representative daily-chart view on NVDA showing the total score and visible category sub-scores in Growth Mode.*

## Architecture and Scoring Logic

![Scorecard architecture](Screenshot%202026-10-16%20021017.png)

*ThinkScript view documenting the five-category scorecard structure and the Value/Growth design.*

## QA Finding: Missing Data Could Change the Meaning of the Score

A later QA review found a semantic defect in the financial-strength logic. Unavailable long-term-debt data could be converted to zero, and the low-debt rule could then award favorable points. That meant **unknown debt could be interpreted as low debt**.

The corrected implementation separates data availability from score arithmetic. Missing debt now receives zero debt points instead of being treated as zero leverage.

![Corrected missing-data logic](Screenshot%202026-10-16%20023929.png)

*Corrected financial-strength logic: missing debt data is detected explicitly and cannot receive favorable low-debt points.*

## Development and QA Lessons

- Compile success is only a syntax gate; it does not prove semantic correctness.
- Missing data is not just a completeness issue—it can change scoring behavior.
- Defensive fallbacks can create hidden model-logic defects if they change the meaning of an input.
- Composite scores benefit from explicit category bounds and visible sub-scores.
- Runtime behavior, edge cases, and data availability need separate validation.
- Uncertainty should be surfaced rather than silently converted into favorable values.

## Value and Growth Modes

The scorecard supports two valuation approaches:

- **Value Mode** uses price-to-book and price-to-sales thresholds.
- **Growth Mode** uses gross profit margin as a quality-oriented proxy.

If both modes are enabled, Growth Mode takes priority. For the NVDA example shown here, Growth Mode is the more appropriate configuration for the intended portfolio demonstration.

## Timeframe Behavior

The technical components use the active chart aggregation. As a result, technical trend and momentum can change when the chart timeframe changes. For example, RSI, moving averages, and the 10-bar momentum comparison are calculated from the current aggregation.

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
