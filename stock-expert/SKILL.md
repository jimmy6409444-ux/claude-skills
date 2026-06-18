---
name: stock-expert
description: Analyze US and Taiwan (TW) stock markets and deliver a comprehensive financial report with macroeconomic context, sector focus, key signals, and specific stock recommendations. Use this skill whenever the user asks about stock market analysis, investment ideas, market outlook, financial reports, or wants to know which stocks to watch — especially for US and Taiwan markets. Also trigger when the user mentions TAIEX, S&P 500, TSMC, semiconductors, AI stocks, or asks "what stocks should I buy/watch". Always produce a rich visual dashboard report using the Visualizer tool alongside supporting prose.
---

# Stock Expert

A skill for delivering professional-grade US and Taiwan stock market analysis, including macroeconomic context, sector signals, stock watchlists, and risk assessment — all presented as a visual interactive report.

---

## Workflow

### 1. Research Phase (always do this first)

Before writing anything, gather current data via web search. Run these searches in parallel where possible:

- `US stock market outlook [current month year]`
- `Taiwan stock market TAIEX [current month year] semiconductor`
- `Fed interest rate decision inflation [current month year]`
- `TSMC earnings results [current quarter year]`
- `AI capex spending 2026 cloud hyperscalers`

Always prioritize sources published within the last 2–4 weeks. Key sources to look for:
- Goldman Sachs, Charles Schwab, U.S. Bank, Morgan Stanley outlooks
- CNBC, Bloomberg, Focus Taiwan, Trading Economics
- Federal Reserve official statements (federalreserve.gov)
- FactSet for earnings data

### 2. Extract Key Data Points

From search results, extract and organize:

**Macro (US)**
- Fed funds rate (current target range)
- CPI / core PCE (latest reading)
- GDP growth forecast (current year)
- Unemployment rate
- S&P 500 P/E ratio and year-end target
- S&P 500 EPS growth (blended)
- Key geopolitical or macro risks

**Macro (Taiwan)**
- TAIEX level and recent trend (record highs? correction?)
- TSMC latest quarterly results (revenue, net income, margin, YoY growth)
- TAIEX forward EPS growth forecast
- Key risks (concentration, geopolitical, rate sensitivity)

**Thematic signals**
- AI capex spending figures
- Market breadth data
- Sector leaders and laggards

### 3. Build the Visual Report

Use the `visualize:read_me` tool (modules: `chart`, `data_viz`, `mockup`) then `visualize:show_widget` to render a full HTML dashboard. The report must include all of the following sections:

#### Required sections

1. **Report header** — date, title ("US & Taiwan Stock Market Analysis"), subtitle
2. **Key macro indicators** — metric cards (8 total) covering:
   - Fed Funds Rate, US CPI, S&P 500 target, TAIEX level
   - GDP growth, unemployment, S&P 500 EPS growth, TSMC net income growth
3. **Market environment snapshot** — two side-by-side signal cards:
   - US signals: AI capex, inflation, Fed bias, breadth, P/E, geopolitical
   - Taiwan signals: TAIEX trend, AI demand, TSMC earnings, index concentration, forward EPS, rate sensitivity
   - Each signal has a colored badge: Bullish (green), Elevated/Hawkish (amber), Risk (red), Info (blue)
4. **AI capex chart** — Chart.js bar chart showing cloud capex growth and IT sector EPS growth by year
5. **US stocks to watch** — 6 stock cards in a 2-column grid, each with:
   - Company name, ticker, sector
   - 2–3 sentence investment thesis
   - Directional rating (Focus/Monitor/Caution) with icon and color
6. **Taiwan stocks to watch** — 6 stock cards (same format), covering:
   - TSMC, MediaTek, Hon Hai, ASE Technology, Delta Electronics, UMC (or equivalent)
7. **Key risks** — 3 amber risk blocks covering the top macro/market risks
8. **H2 outlook** — 3-column outlook cards (AI spending, Rate environment, Geopolitical)

#### Visual design rules

- Use CSS variables throughout for dark mode compatibility
- Metric cards: `background: var(--color-background-secondary)`, no border, `border-radius: var(--border-radius-md)`
- Status colors: `.up { color: #1D9E75 }`, `.down { color: #D85A30 }`, `.warn { color: #BA7517 }`
- Badges: use `var(--color-background-success/warning/danger/info)` and `var(--color-text-success/warning/danger/info)`
- Cards: `border: 0.5px solid var(--color-border-tertiary)`, `border-radius: var(--border-radius-lg)`
- Charts: load Chart.js from `cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js`
- Every `<canvas>` needs `role="img"` and a descriptive `aria-label`
- No hardcoded hex colors for text — always use CSS variables
- No gradients, shadows, or dark outer backgrounds

### 4. Write Supporting Prose

After the widget, write a concise text summary (3–4 paragraphs) covering:
- Macro environment recap (Fed, inflation, GDP)
- US market highlights and AI investment thesis
- Taiwan market highlights and TSMC specifics
- Investment thesis and top picks rationale

Cite sources using `` tags where applicable.

Always end with a disclaimer:
> **Disclaimer:** This is for informational purposes only and does not constitute financial advice. Always consult a licensed financial advisor before making investment decisions.

---

## Stock Selection Guidelines

### US stocks — prioritize by theme
- **AI infrastructure**: NVDA, MSFT, AVGO, AMD
- **AI software**: CRM, NOW, SNOW, PANW
- **Power/infrastructure**: NEE, XLU ETF, VST, CEG
- **Energy (geopolitical)**: XOM, CVX — context-dependent
- **Avoid**: overvalued names with no AI connection in a narrow-breadth market

### Taiwan stocks — core watchlist
| Ticker | Company | Theme |
|--------|---------|-------|
| 2330.TW / TSM | TSMC | AI foundry, advanced nodes |
| 2454.TW | MediaTek | Edge AI, custom silicon |
| 2317.TW | Hon Hai | AI server assembly |
| 3711.TW | ASE Technology | Advanced packaging (CoWoS) |
| 2308.TW | Delta Electronics | Data center power/thermal |
| 2303.TW | UMC | Mature node — caution in AI cycle |
| 2382.TW | Quanta Computer | AI server design |
| 3034.TW | Novatek | Display driver IC — watch cycle |

### Rating system
- **Focus** (green, trending-up icon): Strong conviction, clear AI/structural tailwind
- **Monitor** (amber, equal icon): Neutral — watch for catalyst or re-rating
- **Caution** (red, trending-down icon): Headwinds, lagging theme, avoid near-term

---

## Risk Framework

Always assess and report these risk categories:

1. **Monetary policy risk** — Fed hike probability, rate path, impact on tech valuations
2. **Market breadth risk** — concentration in AI mega-caps, participation rate
3. **Geopolitical risk** — Middle East (Iran/Strait of Hormuz), Taiwan Strait tension, trade policy
4. **AI cycle risk** — capex disappointment, hyperscaler spending slowdown
5. **Taiwan concentration risk** — TSMC's >40% TAIEX weight amplifies single-stock risk

---

## Output Format

- Visual widget first (full dashboard)
- 3–4 paragraph prose summary with citations second
- Disclaimer last
- Total response length: comprehensive but scannable
- Do NOT repeat widget content verbatim in prose — summarize and add context

---

## Example trigger phrases

- "analyze US and TW stock market"
- "give me a stock market report"
- "which stocks should I focus on this month?"
- "what's the outlook for TAIEX / S&P 500?"
- "AI stock recommendations"
- "Taiwan semiconductor investment thesis"
- "financial market analysis report"
