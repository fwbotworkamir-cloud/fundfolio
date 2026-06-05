# Fund Folio

> Free, in-browser scoring and tax-aware exit planner for Indian mutual fund portfolios.

**Live: [fundfolio.sky-sigma.com](https://fundfolio.sky-sigma.com)**

## What it does

Paste, upload, or type your mutual funds — get back:

- **0–100 composite score per fund** weighted across:
  - Alpha vs SEBI category benchmark (60%)
  - Position-size discipline (30%)
  - Holding duration (10%)
- **Verdict per fund**:

  | Verdict | Trigger |
  |---|---|
  | `HOLD-STRONG` | Alpha > 3% over target, healthy position size |
  | `HOLD` | In line with or above category target |
  | `OBSERVE` | Mild underperformance or thematic held < 1 year |
  | `TRIM` | Single fund > 20% of book |
  | `EXIT-LOSS` | Negative absolute return — tax-loss harvest candidate |
  | `EXIT-MICRO` | Non-thematic position < ₹2 L — admin overhead exceeds impact |
  | `EXIT-UNDERPERF` | Held > 1 year, annualised return < 70% of category target |

- **Tax-phased exit plan** distributed across FY26-27 / FY27-28 / FY28-29 to use the ₹1.25 L LTCG exemption per PAN per FY (or ₹2.5 L combined when spousal exemption is enabled). Sequences lowest-gain first to defer tax on the largest gains.
- **Pro-rata LTCG tax** estimate per exit (12.5% rate, exemption applied).
- **Export** as JSON, Markdown, or PDF.

## Designed for

Indian mutual fund investors — resident or NRI, retail or HNI — who want institutional-grade scoring without a wealth manager subscription. Built around the constraint set most long-term investors actually face:

- Cannot day-trade — long-hold + observe.
- 12.5% LTCG with ₹1.25 L exemption per PAN per FY.
- Institutional-style posture — no thematic chasing, no panic selling.
- Tax cost must be visible per action.

## Input modes

Choose whichever fits your data:

| Mode | What you give it | When to use |
|---|---|---|
| **Paste CSV** | Header row + fund rows pasted from any spreadsheet | Fastest for any data you already have in Excel or Google Sheets |
| **Excel upload** | `.xlsx` or `.xls` file | AMC export, broker statement, or your own tracker sheet — column names are auto-detected |
| **CAMS / KFin PDF** | Quarterly Consolidated Account Statement | Best path if you have a CAMS CAS — the parser extracts every scheme, units, cost, and current value automatically |
| **Manual entry** | Fill the form | One-off additions or small portfolios |

Column auto-detection looks for any header containing:
- `Fund Name`, `Scheme Name`, `Scheme`, `Name` → fund name
- `Category`, `SEBI Category` → SEBI category (auto-inferred from name if missing)
- `Invested`, `Cost`, `Purchase Value` → invested amount
- `Current`, `Market Value`, `Value` → current value
- `First Invest`, `Purchase Date`, `Date` → first investment date

Numbers can include `₹`, commas, or suffix shortcuts (`5L`, `1.2Cr`, `300K`).

## Privacy

**Your portfolio data never leaves your browser.**

- No server. The whole app is a single `index.html` file.
- No analytics. No cookies. No login. No tracking.
- File uploads (Excel, PDF) are read directly by JavaScript in your browser — the bytes never go to a server.
- Optional libraries (SheetJS for Excel, PDF.js for PDF) are loaded from public CDNs only when you actually upload a file.
- "Save in this browser" uses `localStorage` — stays on your device, scoped to this domain.

Want maximum paranoia? Save the page locally and run it offline. Source is public — audit it.

## Run locally

```bash
git clone https://github.com/fwbotworkamir-cloud/fundfolio.git
cd fundfolio
open index.html      # macOS
xdg-open index.html  # Linux
start index.html     # Windows
```

No build step. No dependencies. No package manager.

## Self-host

GitHub Pages, Netlify, Cloudflare Pages, S3 — anywhere that can serve a static file. Drop the `index.html` and you're done. Optionally configure a `CNAME` file for a custom domain.

## Known limits

- **No live NAVs.** You enter current values manually. A future version could pull from AMFI's daily NAV file.
- **Category targets are static.** A fund's return is compared to a long-term annualised target for its SEBI category (e.g. "Mid Cap = 14.5%"), not to a live rolling benchmark.
- **No regime detection.** In a market-wide correction, every fund will look like an underperformer. The model doesn't currently distinguish "wrong-regime" from "wrong-fund."
- **Holding-period scaling is binary.** A fund held 14 months and one held 5 years are both compared to the full annualised target.

All three are fixable in a v2.

## Disclaimer

Model output only. Not financial advice. Run any redemption decision past a Chartered Accountant for tax treatment, and a SEBI-registered investment advisor for asset allocation. NRI investors should also confirm NRO repatriation rules with their bank.

## License

MIT — use it, fork it, deploy it under your own subdomain. No attribution required (though appreciated).

---

Built by [Sky Sigma](https://sky-sigma.com).
