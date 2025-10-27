---
type: ProjectLayout
title: Renaissance Trading Engine
date: '2025-03-19'
client: ''
description: >-
  A project where I aimed to closely emulate "Renaissance Technologies'" trading
  strategies under a machine learning model with using Alpaca API for chart data
  and brokerage account.
featuredImage:
  type: ImageBlock
  url: /images/Ekran Resmi 2025-10-27 23.29.36.png
  altText: Project Six Image
  caption: ''
  elementId: ''
media:
  type: ImageBlock
  url: /images/Ekran Resmi 2025-10-27 23.29.36.png
  altText: Project Six image
  caption: Renaissance Trading Engine
  elementId: ''
bottomSections: []
addTitleSuffix: true
metaTags: []
colors: colors-a
backgroundImage:
  type: BackgroundImage
  url: /images/bg2.jpg
  backgroundSize: cover
  backgroundPosition: center
  backgroundRepeat: no-repeat
  opacity: 100
---
# Renaissance Trading Engine

*A systematic research-to-execution platform for equities & futures — built to take strategies from a notebook to live, with discipline.*

> **TL;DR**: An end-to-end Python engine that ingests market data, engineers features, trains models, runs event-driven backtests with realistic costs/slippage, and pushes signals to live brokers with strict risk controls. Designed for fast iteration (research) and reliable execution (production).



## Why I built this

As a professional futures trader and ML tinkerer, I wanted one cohesive codebase that:

*   lets me **prototype ideas quickly** (Jupyter first, clean Python modules later),

*   **backtest without lies** (no look-ahead, realistic fills, costs & slippage),

*   and **execute live** with robust risk management (vol-targeting, daily loss limits, kill-switches).

This project is the backbone of that workflow.



## What’s under the hood

**Core stack**

*   Python (pandas, NumPy, scikit-learn, PyTorch/XGBoost)

*   Event-driven backtester (fills, slippage models, portfolio accounting)

*   Optuna for hyper-parameter search & walk-forward

*   Data connectors: offline CSV/Parquet, `yfinance` (research), broker/vendor feeds (prod)

*   Orchestration: CLI + config-first (YAML), optional Docker for reproducible runs

*   Live trading: broker adapters (Alpaca today; IB via `ib_insync` on the roadmap)

**Markets & universes**

*   S\&P 500 universe (signals & portfolio construction)

*   Index & futures focus (e.g., ES, NQ) with volatility targeting



## Capabilities at a glance

*   **Research**

    *   Feature engineering (momentum, volatility, seasonality, market microstructure-aware bars)

    *   Cross-validation & **walk-forward** training/evaluation

    *   Model zoo: linear (ridge/lasso), tree-based (XGBoost/LightGBM), and sequence models (LSTM/Temporal CNN) for experiments

    *   Ensemble meta-models & stacking

*   **Backtesting**

    *   **Event-driven** simulation (orders, partial fills, cancellations)

    *   Slippage models: fixed bps, % of spread, and volume-participation (VWAP-style)

    *   Transaction costs: commissions, fees, market impact

    *   Portfolio accounting: cash, equity, M2M PnL, drawdown, exposure, turnover

    *   **No look-ahead**, **no overlap leakage**, timezone-safe resampling

*   **Risk & Portfolio**

    *   Volatility targeting (e.g., 10% annualized)

    *   Kelly-fraction scaling **with caps**

    *   Max daily loss & **hard kill-switch**

    *   Position concentration & sector caps for equities

    *   Rebalancing policies: periodic or signal-threshold

*   **Live execution**

    *   Signal generation on schedule (cron/CLI)

    *   Broker adapters (Alpaca ready; IB planned)

    *   Pre-trade checks, order throttling, retry logic

    *   Trade logs + daily risk report



## Architecture

```
data/
  ├─ raw/                 # vendor or csv/parquet inputs
  ├─ processed/           # cleaned & aligned datasets
  └─ features/            # cached feature matrices

engine/
  ├─ data/                # loaders, resamplers, vendor adapters
  ├─ features/            # indicators, transforms, alpha factors
  ├─ models/              # wrappers for sklearn/xgb/torch
  ├─ backtest/            # event loop, fills, portfolio, metrics
  ├─ risk/                # vol-target, kelly, constraints
  ├─ execution/           # broker clients, order router
  └─ reporting/           # metrics, tearsheets, html/csv export

research/
  ├─ notebooks/           # idea scratchpads, EDA, prototypes
  └─ experiments/         # config-driven runs & WFO results

configs/
  ├─ data.yaml            # tickers, universes, sampling
  ├─ features.yaml        # factor definitions
  ├─ model.yaml           # model family + hyperparams
  ├─ backtest.yaml        # dates, costs, slippage, capital
  └─ live.yaml            # broker keys, live run toggles

cli.py                    # `python cli.py backtest --config ...`

```

> *Note: Structure is representative; the project evolves as the research stack grows.*



## Research workflow

1.  **Notebook ideation** → sanity check an alpha, visualize signals vs returns.

2.  **Config the experiment** (no hard-coding): date ranges, universe, features, model.

3.  **Run walk-forward**: rolling retrain & test, Optuna tuning per window.

4.  **Evaluate**: Sharpe, Sortino, max DD, Calmar, exposure, turnover, hit rate.

5.  **Promote to live**: lock config, enable risk limits, push to broker.



## Backtesting methodology

*   **Walk-forward** split (train → validate → test), not random K-folds on time series.

*   **Transaction costs** included by default.

*   **Slippage** configurable per venue/liquidity.

*   **Clock safety**: aligns timestamps, prevents look-ahead on features & targets.

*   **Position sizing** uses volatility estimates from non-overlapping windows.



## Risk controls that actually bite

*   Daily loss limit (e.g., -2% equity) → **auto flat, disable trading for session**

*   Max drawdown circuit breaker (e.g., -10%) → requires manual re-arm

*   Per-symbol and sector exposure caps

*   Hard notional/leveraged gross limits



## Example config (snippet)

```
# backtest.yaml
start: "2015-01-01"
end:   "2024-12-31"
capital: 100000
universe: "SP500"
rebalance: "WEEKLY"

costs:
  commission_bps: 0.5
  borrow_bps: 0.0
slippage:
  model: "spread_pct"
  spread_pct: 0.02

risk:
  vol_target_annual: 0.10
  kelly_fraction_cap: 0.25
  max_daily_loss_pct: 0.02

```

**Run it**

```
# Backtest
python cli.py backtest --config configs/backtest.yaml --model configs/model.yaml

# Walk-forward optimization
python cli.py wfo --experiment configs/experiments/wfo_sp500.yaml

# Live (paper first!)
python cli.py live --config configs/live.yaml --paper

```



## Results (illustrative)

| Metric           | Value\* |
| ---------------- | ------- |
| CAGR             | —       |
| Sharpe           | —       |
| Sortino          | —       |
| Max Drawdown     | —       |
| Win Rate         | —       |
| Average Turnover | —       |

\* *Numbers vary by strategy/config; the engine’s purpose is to make these **auditable & reproducible**.*



## What I learned building this

*   Most backtests lie unless you **model the market** (costs, slippage, delays).

*   **Walk-forward** beats random folds for time series every day of the week.

*   Clean, **config-driven pipelines** make it easy to go from idea → live safely.

*   The best alpha is worthless without **risk discipline** and an execution layer.



## Roadmap

*    Interactive Brokers adapter (`ib_insync`)

*    Intraday bar aggregation & microstructure features (tick/quote -> bars)

*    Online learning with regime detection

*    Factor library expansion (cross-sectional & intraday)

*    Dashboard (live PnL, exposure, risk states)



## Notes & disclaimers

This codebase is for **educational and research** purposes. It is **not financial advice**. Trading involves substantial risk; past performance does not guarantee future results.



## Links

*   **Code**: *Private repo* (available on request)

*   **Tech**: Python, pandas, NumPy, scikit-learn, XGBoost/PyTorch, Optuna, YAML/CLI

*   **Contact**: Reach out if you want to discuss systematic strategy research or collaboration.



### Want a peek under the hood?

I’m happy to demo the engine on a paper-trading session (signals → orders → fills → daily risk report), or walk through an experiment from raw data to a full walk-forward teardown.

