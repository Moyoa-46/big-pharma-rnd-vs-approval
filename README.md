# The Billion-Dollar Bet: Does More R&D Spending Buy More FDA Approvals?

An exploratory data analysis of 13 big pharma companies (2015–2024) asking whether spending more on R&D, or carrying a bigger pipeline, leads to more FDA drug approvals.

**[Interactive dashboard (Tableau Public)]([https://public.tableau.com/views/TheBillionDollarBetBigPharmaRDvsFDAApprovals/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link])** · **[Write-up]([https://medium.com/@makinneye/more-r-d-money-same-number-of-new-drugs-what-13-big-pharma-companies-show-a58ed9dacfc4?sharedUserId=makinneye])**



## Why this question

Drug R&D spending keeps rising while new approvals per dollar have fallen for decades, a trend known as Eroom's Law (Moore's Law, backwards). Cost-per-drug estimates also range from about $1.3B to $2.6B depending on method. This project tests the idea at company level: does the bet pay off?

## Key findings

- Approvals vary widely across companies, and no single metric predicts them well. The strongest link is Phase 3 trial count (correlation 0.26).
- R&D spend from five years earlier (0.23) tracks approvals about twice as closely as same-year spend (0.11), consistent with a long development cycle.
- Acquisition-driven spikes (e.g. Merck 2023–24, AbbVie 2018) are flagged as outliers, not organic R&D growth.
- The biggest R&D spender: Merck is near the lowest on efficiency, ranking 12th of 13 on lag-adjusted efficiency. Sanofi is last at 0.142.

## Data

| Variable | Source |
|---|---|
| FDA approvals (NDA/BLA only) | [FDA drug approvals bulk download](https://download.open.fda.gov/drug/drugsfda/drug-drugsfda-0001-of-0001.json.zip) |
| R&D spend | SEC EDGAR API, gap-filled with Macrotrends exports |
| Pipeline size | [ClinicalTrials.gov API v2](https://clinicaltrials.gov/api/v2/studies) |
| Market cap | Yahoo Finance via `yfinance` |

## Method

1. Extracted four sources and matched FDA sponsor names to tickers with a manual bridge.
2. Built a master table of 120 company-years.
3. Added a 5-year lagged R&D column, because drugs take years to reach approval.
4. Analysed trends, company comparisons, efficiency (approvals per $1B) and correlations.

## Challenges

- The openFDA API was suspended, so I used the bulk download.
- Macrotrends blocked scraping, so I combined SEC EDGAR data with earlier exports.
- Company names differed across sources.
- A review caught an approvals-counting bug (sponsor rows were counted instead of approvals) and generics in the data, both fixed.

## Limitations

- Small sample: 13 companies, uneven coverage for Gilead, Biogen and Lilly.
- A fixed 5-year lag simplifies an 8–10 year development cycle.
- Correlation is not causation.
- Acquisition-inflated spend and market-cap outliers (Merck, AbbVie, AstraZeneca) can distort comparisons, and the AstraZeneca market-cap figures are still being verified.
- Market cap values come from Yahoo Finance share counts and may be overstated for ADR-listed companies (AstraZeneca appears roughly 2× too high). Market cap is used only as a size proxy, so conclusions about R&D spend and approvals are unaffected.

## Repository structure

```
├── data/
│   ├── raw/          # source extracts
│   └── processed/    # master_df.csv, efficiency_df.csv
├── notebooks/
│   └── 01_eda2.ipynb
├── scripts/          # data extraction
├── requirements.txt
└── README.md
```

## How to run

```bash
git clone https://github.com/Moyoa-46/big-pharma-rnd-vs-approval.git
cd big-pharma-rnd-vs-approval
pip install -r requirements.txt
```

Run the scripts in `scripts/` to refresh the raw data (the SEC EDGAR script needs your own contact email in its `User-Agent`), then open `notebooks/01_eda2.ipynb` and run all cells.

## Tools

Python (pandas, matplotlib, seaborn, yfinance), Jupyter, Tableau Public.

## Author

Moyosioluwa Akinneye · [LinkedIn]([https://www.linkedin.com/in/moyo-akinneye-628393251/])
