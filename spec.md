VRP Offer Modeler – Product Design Spec

1. Product overview

Product name: VRP Offer ModelerType: Single-page web app (SPA)Purpose: Let employees model voluntary retirement program (VRP) scenarios, compare them to “stay” and “layoff” baselines, and compute personal “take/don’t take” breakpoints.

2. Primary goals

Goal 1: Help users understand the true value of a VRP vs. staying employed.

Goal 2: Provide breakpoint analysis: “At what offer level should I say yes?”

Goal 3: Support both pre-offer (what should I hope for?) and post-offer (should I accept?) modes.

Goal 4: Keep all data local in browser (no backend required initially).

3. Target users & use cases

Users:

Mid/late-career employees (50+) in tech/large enterprises.

Financially literate but not necessarily experts.

Internal advocates (HR, managers) wanting to sanity-check VRP design.

Core use cases:

Pre-offer modeling:“If a VRP comes, what would a good offer look like for me?”

Post-offer decision:“Given this actual offer, should I take it or stay?”

Scenario comparison:“What if I work 2 more years vs. take the package now?”

4. Page structure & UX

Single-page layout with four main panels:

User profile & assumptions

Offer definition (actual or hypothetical)

Scenario outputs & charts

Breakpoint analysis & recommendation

4.1 Top-level layout

Header:

App name, short tagline (“Model your VRP decision with clarity.”)

Mode toggle: Pre-offer / Post-offer

Main content:Two-column layout on desktop, stacked on mobile.

5. Inputs & data model

5.1 User profile inputs

Category

Field

Type

Demographics

Age

number

Demographics

Planned retirement age (if staying)

number

Employment

Years of service

number

Employment

Current annual salary

currency

Employment

Bonus % (optional)

%

Employment

RSU/Equity annual value

currency

Employment

RSU unvested value

currency

Family

Spouse/partner age

number

Family

Dependents in college (Y/N, count)

number/bool

Family

Annual college cost (per dependent)

currency

Health

Current coverage type (self/spouse/family)

enum

Health

Employer coverage cost share (optional)

currency/%

5.2 Financial assumptions

Investment return (real) – default 3–4%

Inflation – default 2–3%

Tax rate (effective) – simple slider or % input

Social Security start age – default 67, user-adjustable

Planned annual retirement spending – currency

5.3 Offer definition inputs

Pre-offer mode:

Cash multiple (e.g., 1.0x, 1.5x, 2.0x salary)

Fixed cash amount (override)

Weeks per year of service (if known)

Minimum weeks base

Healthcare coverage duration (months)

COBRA paid? (Y/N)

Additional retirement bonus (currency)

Stock vesting continuation (months)

Post-offer mode:

Enter actual offer terms:

Cash amount

Healthcare duration & type

Any pension/retirement credits

Vesting continuation details

6. Core calculations

6.1 Baseline scenarios

Scenario A – Stay (planned years):

Salary + bonus + RSU over remaining work years.

Employer healthcare value.

Additional retirement savings contributions.

Social Security timing as configured.

Scenario B – Involuntary layoff (severance baseline):

Severance formula (e.g., 12 weeks + 2 weeks/year).

Healthcare continuation (e.g., 6 months).

Time to new job (user input: 0–12 months).

Impact on retirement savings (drawdown vs. contributions).

Scenario C – VRP (offer):

Cash payout (net of taxes using effective rate).

Healthcare coverage value.

Any vesting continuation.

Retirement savings drawdown until SS/Medicare.

6.2 Outputs

Net present value (NPV) of each scenario at retirement age.

Years of retirement fully funded under each scenario.

Probability buffer (simple: years of cushion vs. planned lifespan).

College funding sufficiency (can you cover X years at Y cost?).

7. Breakpoint analysis

Core feature: “What offer would make me indifferent between staying and taking the VRP?”

Mechanics:

Solve for cash payout such that:

NPV(Stay) ≈ NPV(VRP)

Show:

Breakpoint cash amount (e.g., “At ~$340k, VRP ≈ staying 2 more years.”)

Current offer vs. breakpoint (e.g., “Your offer is 82% of your breakpoint.”)

Traffic light indicator:

Green: Offer ≥ 110% of breakpoint

Yellow: 90–110%

Red: < 90%

8. UI elements & interactions

Sliders for:

Years you planned to keep working

Investment return

Tax rate

Time to new job (layoff scenario)

Toggle buttons for:

Pre-offer / Post-offer

Include/exclude spouse healthcare

Charts:

Bar chart: NPV of each scenario (Stay / Layoff / VRP)

Line chart: Projected portfolio balance over time per scenario

Summary cards:

“Stay” card: key metrics

“VRP” card: key metrics

“Layoff baseline” card

“Breakpoint” card with clear “Take / Lean Take / Lean Stay / Stay” label

9. Tech design (high level)

Frontend:

Framework: React / Svelte / Vue (any SPA-friendly stack)

State management: local (hooks/store)

All calculations client-side

Persistence:

LocalStorage for saving scenarios (no server required initially)

“Export/import JSON” for sharing scenarios

Privacy:

No data sent to server by default

Clear statement: “All calculations run in your browser.”

10. Future enhancements

Multi-offer comparison (e.g., “What if they sweeten it?”).

“Advisor mode” – anonymized profiles for financial planners.

Integration with Social Security calculators (via API).

PDF export of scenario summary for discussion with spouse/advisor.

