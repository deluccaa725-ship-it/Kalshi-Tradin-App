# Kalshi Heatfinder

Kalshi Heatfinder is a Kalshi-native prediction market bot scaffold built for paper-trading-first strategy development. It includes a signed Kalshi API client, a lightweight Flask dashboard, configurable market filters, and starter strategies for BTC and weather-style contracts.

## Highlights

- Paper trading workflow with CSV trade logging
- Kalshi request signing for authenticated endpoints
- Flask dashboard with status and recent trade views
- Strategy scanning with configurable series prefixes and title keywords
- Risk controls including daily stop loss, max position size, exposure caps, and trade cooldowns
- Starter strategy modules for BTC 15-minute contracts and weather-style markets

## Current project layout

```text
Kalshi-Heatfinder/
|-- app.py
|-- requirements.txt
|-- .env.example
|-- README.md
|-- data/
|-- scripts/
|   |-- discover_markets.py
|   `-- review_btc_side_bias.py
|-- templates/
`-- kalshi_heatfinder/
    |-- config.py
    |-- engine.py
    |-- executor.py
    |-- market_selector.py
    |-- models.py
    |-- clients/
    |   |-- kalshi_http.py
    |   |-- open_meteo.py
    |   `-- spot_prices.py
    `-- strategies/
        |-- base.py
        |-- btc_15m.py
        |-- crypto.py
        `-- weather.py
```

## Requirements

- Python 3.10+
- A Kalshi demo or live API key only if you want authenticated requests
- A Kalshi private key file only if you want signed demo or live order placement

## Installation

```powershell
cd C:\Users\deluc\Documents\Kalshi-Heatfinder
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

## Configuration

The included `.env.example` is set up for demo mode and paper trading.

Example:

```env
KALSHI_ENV=demo
PAPER_TRADING=true
LOG_LEVEL=INFO
DASHBOARD_PORT=5050

KALSHI_API_KEY_ID=
KALSHI_PRIVATE_KEY_PATH=
KALSHI_BASE_URL=
KALSHI_PUBLIC_BASE_URL=https://api.elections.kalshi.com

DAILY_STOP_LOSS_USD=50
MAX_POSITION_USD=15
TRADES_CSV_PATH=data/trades.csv
TRADE_COOLDOWN_SEC=21600

WEATHER_SERIES_PREFIXES=
WEATHER_TITLE_KEYWORDS=climate,temperature,weather,rain
BTC_15M_SERIES_PREFIXES=KXBTC15M
BTC_15M_TITLE_KEYWORDS=btc,15 min,price up

WEATHER_ENABLED=true
BTC_15M_ENABLED=true
BTC_15M_SCAN_INTERVAL_SEC=30
BTC_15M_EDGE_THRESHOLD=0.05
BTC_15M_MIN_MINUTES_TO_EXPIRY=7
BTC_15M_YES_MIN_CONFIDENCE=0.60
BTC_15M_NO_MIN_CONFIDENCE=0.55
```

Notes:

- Leave the API key fields blank if you are only testing public market discovery.
- `TRADES_CSV_PATH` defaults to `data/trades.csv`.
- The BTC strategy is configured around the `KXBTC15M` series by default.

## Running the bot

```powershell
.\.venv\Scripts\python.exe app.py
```

By default the dashboard is available at:

`http://127.0.0.1:5050`

Useful endpoints:

- `GET /` for the dashboard
- `GET /api/status` for bot state
- `GET /api/trades` for recent paper trades
- `POST /api/start` to start the engine
- `POST /api/stop` to stop the engine
- `GET /health` for a simple health check

## Scripts

Discover live BTC 15-minute markets:

```powershell
.\.venv\Scripts\python.exe scripts\discover_markets.py --category crypto --prefix KXBTC15M
```

## Safety and usage notes

- This project is currently structured for paper-trading-first experimentation.
- Review all risk settings before enabling any authenticated trading workflow.
- Do not commit your `.env` file or private key files.

## Status

The current codebase is set up to run in paper mode with a dashboard, trade logging, strategy toggles, and configurable market filters. It is a solid starting point for iterating on Kalshi-native strategies before wiring in production execution.
