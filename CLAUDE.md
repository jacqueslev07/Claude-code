# CLAUDE.md

## Project Overview

ROI & Lead Loss Calculator — a standalone, single-page web application that helps businesses visualize the financial impact of missed leads and unanswered calls. Users input marketing metrics and receive real-time calculations of lost revenue, wasted budget, and ROI comparisons.

## Repository Structure

```
.
├── index.html   # Entire application (HTML + CSS + JS in one file)
└── README.md    # Project documentation and usage guide
```

This is a **zero-dependency** project. There is no build system, no package manager, no framework, and no external resources. Everything lives in `index.html`.

## Tech Stack

- **HTML5** — semantic markup, form inputs
- **CSS3** — inline `<style>` block; uses CSS Grid, flexbox, gradients, `backdrop-filter`, media queries for responsiveness
- **Vanilla JavaScript** — inline `<script>` block; DOM manipulation, `Intl.NumberFormat` for currency/number formatting

## Architecture

`index.html` is organized into three sections:

1. **`<style>` (lines 7–256)** — All CSS. Key design tokens:
   - Primary gradient: `#667eea` → `#764ba2`
   - Loss card gradient: `#f093fb` → `#f5576c`
   - Warning card gradient: `#fa709a` → `#fee140`
   - Font stack: `'Segoe UI', Tahoma, Geneva, Verdana, sans-serif`
   - Breakpoint: `768px` for mobile responsiveness

2. **`<body>` HTML (lines 258–375)** — Input form and results display. Results are hidden by default (`.results { display: none }`) and shown via `.show` class toggle.

3. **`<script>` (lines 377–452)** — Two utility functions (`formatCurrency`, `formatNumber`) and the main `calculate()` function. Enter key listeners are attached to all inputs.

## Key Functions

- **`formatCurrency(value)`** — Formats numbers as USD currency strings (no decimals)
- **`formatNumber(value)`** — Formats numbers with up to 1 decimal place
- **`calculate()`** — Main logic: reads inputs, validates, computes all metrics, updates DOM, reveals and scrolls to results

## Calculation Logic

Given inputs: `marketingBudget`, `totalLeads`, `missedLeads`, `customerValue`, `conversionRate`:

- `costPerLead = marketingBudget / totalLeads`
- `wastedMarketing = costPerLead * missedLeads`
- `wastedPercentage = (missedLeads / totalLeads) * 100`
- `lostCustomers = missedLeads * (conversionRate / 100)`
- `lostRevenue = lostCustomers * customerValue`
- `annualLoss = lostRevenue * 12`
- `currentROI = ((currentRevenue - marketingBudget) / marketingBudget) * 100`
- `potentialROI = ((potentialRevenue - marketingBudget) / marketingBudget) * 100`

## Development Workflow

### Running Locally

Open `index.html` directly in any browser. No server required.

```sh
# macOS
open index.html

# Linux
xdg-open index.html
```

### No Build / Test / Lint Steps

There is no build process, test suite, or linter configured. Changes are made directly to `index.html` and verified manually in a browser.

### Deployment

The single `index.html` file can be deployed to any static host (GitHub Pages, Netlify, Vercel, S3, etc.) or shared directly as a file.

## Conventions for AI Assistants

- **Single-file architecture**: All changes go into `index.html`. Do not split into separate CSS/JS files unless explicitly requested.
- **No external dependencies**: Do not introduce frameworks, libraries, CDN links, or package managers unless explicitly requested.
- **Inline styles and scripts**: CSS lives in the `<style>` block, JS lives in the `<script>` block. Keep this pattern.
- **Formatting functions**: Use `formatCurrency()` and `formatNumber()` for any new numeric displays. Do not add alternative formatting approaches.
- **DOM IDs**: Result elements use IDs (`lostRevenue`, `annualLoss`, `costPerLead`, etc.) that are updated in `calculate()`. Any new metric needs a corresponding ID and update in `calculate()`.
- **Validation**: Currently minimal (checks `marketingBudget` and `totalLeads` are non-zero). Match this pattern for any new inputs.
- **Responsive design**: All new UI must work at the `768px` breakpoint. Use the existing media query section.
- **Color scheme**: Stick to the existing gradient palette unless a design change is requested.
- **No tests exist**: If adding functionality, verify manually. If the project grows, consider adding a test framework at that point.
