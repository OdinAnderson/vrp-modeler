# VRP Offer Modeler

A single-page web app for modeling **Voluntary Retirement Program (VRP)** offers — compare them to "stay" and "layoff" baselines, and find the personal **breakpoint** where taking the package is at least as good as staying.

> _Model your VRP decision with clarity._

**Live:** https://odinanderson.github.io/vrp-modeler/

---

## What it does

- **"What if?" modeling** — "If a VRP comes, what would a good offer look like for me?" Either dial in a hypothetical (cash multiple of salary, weeks/year of service, healthcare months, vesting continuation) or use the cash override to test an exact dollar amount.
- **Breakpoint analysis** — Solves for the cash payout where `NPV(VRP) ≈ NPV(Stay)` and shows your offer as a percent of that breakpoint, with a traffic-light verdict (`TAKE` / `LEAN TAKE` / `LEAN STAY` / `STAY`).
- **Projections** — NPV bar chart and per-year portfolio balance chart for all three scenarios.
- **Privacy** — All math runs in your browser. Nothing leaves the tab. State is auto-saved to `localStorage`, with JSON export/import for sharing scenarios.

## Inputs

- **Profile** — age, planned retirement age, years of service, salary, bonus %, RSU annual + unvested, spouse age, college dependents, healthcare coverage tier.
- **Assumptions** — real investment return, inflation, effective tax rate, Social Security start age, planned annual retirement spending.
- **Layoff baseline** — severance formula (weeks base + weeks/year of service), expected time-to-new-job, healthcare continuation.
- **Offer** — cash multiple of salary (or fixed cash override), weeks/year of service, healthcare months, vesting continuation, retirement bonus, COBRA paid Y/N, acceptance window.

Every input has a hover tooltip explaining what it is and how it affects the model.

## Calculation model

A simple-but-honest projection in real (today's) dollars:

- **Stay** — contributes ~15% of salary + ~50% of RSU + ~40% of bonus per year while working; withdraws planned retirement spending from `retireAge` onward; Social Security reduces draw at `ssStartAge`.
- **Layoff** — severance up front (after tax), no contributions during the gap, then re-employment at 90% of prior salary; same retirement drawdown.
- **VRP** — offer cash (after tax) + healthcare bridge value + pro-rata vesting value as starting portfolio; immediate retirement; same drawdown rules.
- **Breakpoint** — binary search on offer cash until VRP's projected balance at `retireAge` matches the Stay scenario's.

Numbers are deliberately first-order. Easy to refine later (Monte Carlo, sequence-of-returns risk, marginal tax brackets, joint-life longevity).

## Run locally

It's a single static file. No build step, no dependencies.

```pwsh
# Just open it
Start-Process index.html

# Or serve over HTTP (any static server works)
python -m http.server 8000
# then visit http://localhost:8000/
```

## Deploy

Hosted on **GitHub Pages** from the repo root:

1. Push to `main`.
2. **Settings → Pages → Source: Deploy from branch → `main` / `/` (root)**.
3. Live at `https://<user>.github.io/vrp-modeler/` within ~30s.

## Tech

- One `index.html` — no framework, no bundler, no `node_modules`.
- Vanilla JS, hand-rolled SVG charts.
- Cyberpunk visual style (Orbitron + Share Tech Mono + Instrument Serif, neon cyan/magenta/amber on `#050505`), inspired by [delx.ai](https://delx.ai).

## Files

- [index.html](index.html) — the entire app.
- [spec.md](spec.md) — original product design spec.

## Status

`v0.1` — functional, opinionated, intentionally simple. PRs welcome.

## License

MIT.
