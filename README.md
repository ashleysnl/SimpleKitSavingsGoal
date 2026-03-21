# SimpleKit Savings Goal Calculator

This repository contains the SimpleKit Savings Goal Calculator, a static browser-based planning tool for:

- estimating how long it takes to reach a savings goal
- calculating the contribution required to hit a goal by a target date
- comparing multiple savings goal scenarios
- exporting and importing scenarios as JSON
- printing a clean report through the browser print flow

The tool keeps the shared SimpleKit core shell integration and existing Google Analytics setup intact.

## What The Tool Does

The calculator supports two main modes:

1. `Time to reach goal`
   - Inputs: goal amount, current savings, contribution amount, contribution frequency, annual interest rate, compounding frequency, and start date
   - Outputs: projected time to goal, goal date, total contributions, total interest earned, final projected balance, and projection table

2. `Required contribution`
   - Inputs: goal amount, current savings, target date, contribution frequency, annual interest rate, compounding frequency, and start date
   - Outputs: required contribution per contribution period, total contributions by target date, total interest earned, target-date balance, and a pace label (`Steady`, `Stretch`, or `Aggressive`)

## Scenario Management

Users can:

- create multiple scenarios
- duplicate a scenario
- rename a scenario
- delete a scenario
- compare selected scenarios side by side
- auto-save the current planner state in `localStorage`
- reset all scenarios with confirmation

## Save / Load JSON

The calculator exports all scenarios and app state into a JSON file with:

- `schemaVersion`
- `exportedAt`
- `settings`
- `mode`
- `selectedScenarioId`
- `compareScenarioIds`
- `scenarios`

Suggested file name format:

- `simplekit-savings-goals-YYYY-MM-DD.json`

Imports validate that a scenario array exists and then normalize older or incomplete shapes onto the current schema defaults where possible.

## Print / Save PDF

The tool does not generate server PDFs.

Instead it:

- prepares a print-friendly report section in the page
- hides interactive controls in print mode
- simplifies surfaces for black-and-white output
- keeps key summary cards, assumptions, milestones, and comparison notes together

Users can save a PDF with the browser’s print dialog after clicking `Print / Save PDF`.

## Savings Math Assumptions

The projection engine intentionally separates contribution timing from growth assumptions:

- The entered annual rate is treated as a nominal annual rate with the selected compounding frequency.
- That nominal rate is converted into an effective annual rate.
- The effective annual rate is then converted into a daily planning rate for simulation.
- Contributions are assumed to occur at the end of each contribution period.
- Timeline projections simulate forward from one contribution date to the next.
- Target-date mode solves the required contribution with binary search, which keeps the solver consistent even when contribution timing is irregular, such as semi-monthly schedules.

This approach makes the calculator robust when:

- contribution frequency and compounding frequency differ
- interest is zero
- starting balance is zero
- contributions are tiny or very large
- target dates are close, far away, or invalid

## File Structure

```text
/
  index.html
  assets/
    css/
      styles.css
    js/
      app.js
  README.md
```

## Shared Core Integration

This repo preserves the standard SimpleKit integration pattern:

- `window.SimpleKitPage` is defined in `index.html`
- `https://core.simplekit.app/core.css` is loaded
- `https://core.simplekit.app/core.js` is loaded
- shared mount points remain in place for the header, support area, and footer

Do not remove the analytics snippet or shared core load pattern unless the platform conventions change centrally.
