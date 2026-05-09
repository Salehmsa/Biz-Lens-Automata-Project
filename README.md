# E-Commerce Performance Dashboard

An interactive executive dashboard analyzing ~214K e-commerce order lines across India — covering growth, leakage, logistics performance, and geographic concentration.

**Prepared by: SALEH MAHBUB**

Live preview: built with TanStack Start + React 19 + Tailwind v4 + Recharts.

---

## What it does

Cleans and joins three raw datasets into a star schema, then visualizes the key business KPIs a CXO needs to make decisions:

- **Sales_Data.xlsx** — order-line transactions (units, status, breach, logistics type, partner, pincode, category)
- **Pincode_mapping.csv** — pincode → State / Region / Circle / Division / District
- **BU_mapping.csv** — Category → Business Unit (Electronics, Mobile, Appliances, Home, LifeStyle, Book, Others)

The processed data is pre-aggregated into compact JSON (`src/data/*.json`) so the dashboard is fully client-side and instant.

---

## Features

### KPIs (6 cards)
Total Units · Delivered · Returned (Leakage) · Cancelled · SLA Breach Rate · On-Time Rate

### Visuals (12 charts)
1. Monthly trend — units by status (stacked area)
2. MoM growth — delivered units %
3. Business Unit mix (pie)
4. Geographic distribution by State
5. Order status funnel
6. Leakage rate by Business Unit
7. SLA breach by Logistic Partner
8. SLA breach by Logistics Type
9. Partner volume mix (donut)
10. Top 10 districts by volume + return rate
11. Region × BU heatmap
12. Leakage waterfall (Total → Cancelled → Returned → Net Delivered)

### Filters (7)
Month · Business Unit · Category · State · Logistic Partner · Logistics Type · Status

### Other
- Dark / Light mode toggle (persisted to localStorage, respects system preference)
- Executive Insights panel auto-narrating Growth, Leakage, Logistics Health, and Recommendations

---

## Tech stack

- **Framework:** TanStack Start v1 (React 19, SSR-ready)
- **Build:** Vite 7
- **Styling:** Tailwind CSS v4 with semantic design tokens (`src/styles.css`, oklch color space)
- **Charts:** Recharts
- **UI primitives:** shadcn/ui (Radix)
- **Data prep:** Python + DuckDB (offline) → static JSON

---

## Project structure

```
src/
├── routes/
│   ├── __root.tsx        # html/head/body shell
│   └── index.tsx         # the dashboard
├── data/
│   ├── agg.json          # main aggregate fact table
│   ├── states.json       # state-level rollups
│   └── top_districts.json
├── components/ui/        # shadcn components
└── styles.css            # design tokens (light + dark)
```

---

## Run locally

```bash
bun install
bun run dev
```

Open the preview URL printed in the terminal.

---

## Data model (star schema)

```
                  ┌─────────────┐
                  │  DIM_Date   │
                  └──────┬──────┘
                         │
┌──────────────┐   ┌─────▼─────┐   ┌──────────────┐
│  DIM_Geo     │◄──┤ FACT_Sales├──►│ DIM_Product  │
│  (Pincode)   │   └──┬──────┬─┘   │ (BU/Category)│
└──────────────┘      │      │     └──────────────┘
                ┌─────▼──┐ ┌─▼────────┐
                │ DIM_   │ │ DIM_     │
                │ Status │ │ Logistics│
                └────────┘ └──────────┘
```

`FACT_Sales` grain = one row per order line, joined on Pincode and Category.

---

## Credits

Dashboard, data model, and analysis by **Saleh Mahbub**.
