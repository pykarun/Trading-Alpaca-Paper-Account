# Trading Alpaca Paper Account

A Python-based algorithmic trading bot that uses a double-EMA (Exponential Moving Average) strategy with trailing stop-loss to trade TQQQ on the Alpaca paper trading platform.

## Overview

This project implements an automated trading strategy that:
- Uses QQQ price data to generate buy/sell signals based on EMA crossover
- Trades TQQQ (3x leveraged QQQ ETF) via Alpaca's paper trading API
- Implements a trailing stop-loss mechanism to protect gains
- Runs automatically via GitHub Actions on a configurable schedule

## Trading Strategy

### Signal Generation
- **Buy Signal**: When the fast EMA (9-period) crosses above the slow EMA (21-period)
- **Sell Signal**: When the fast EMA crosses below the slow EMA

### Risk Management
- **Trailing Stop-Loss**: 15% trailing stop from peak price to protect gains
- **Position Sizing**: Configurable allocation fraction of available capital

## Requirements

- Python 3.9+
- Alpaca paper trading account

### Dependencies

```
pandas
yfinance
requests
```

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd Trading-Alpaca-Paper-Account
   ```

2. Install dependencies:
   ```bash
   pip install pandas yfinance requests
   ```

3. Set up environment variables (see Configuration section)

## Configuration

### Environment Variables

Set the following environment variables with your Alpaca API credentials:

| Variable | Description | Default |
|----------|-------------|---------|
| `ALPACA_API_KEY` | Your Alpaca API key | `""` |
| `ALPACA_API_SECRET` | Your Alpaca API secret | `""` |
| `ALPACA_BASE_URL` | Alpaca API base URL | `https://paper-api.alpaca.markets` |
| `ALPACA_DATA_URL` | Alpaca data API URL | `https://data.alpaca.markets` |

### Strategy Parameters

The following parameters can be adjusted in `alpaca_trader_minimal.py`:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `EMA_FAST` | 9 | Fast EMA period |
| `EMA_SLOW` | 21 | Slow EMA period |
| `STOP_LOSS_PCT` | 15.0 | Trailing stop-loss percentage |
| `INITIAL_CAPITAL` | 10000.0 | Initial simulation capital |
| `ALLOCATION_FRACTION` | 1.0 | Fraction of cash to allocate on buy |
| `FIXED_BUY_DOLLARS` | 100.0 | Fixed dollar amount for buy orders |
| `MIN_POSITION_PCT_TO_SELL` | 50.0 | Minimum position percentage to execute sell |

## Usage

### Run Once
Execute a single trading cycle:
```bash
python alpaca_trader_minimal.py --once
```

### Run Continuously
Start the bot in continuous loop mode:
```bash
python alpaca_trader_minimal.py
```

### Dry Run (Simulation)
The script supports dry-run mode where orders are logged but not submitted to Alpaca.

## GitHub Actions Automation

The repository includes a GitHub Actions workflow (`.github/workflows/trade.yml`) that:
- Runs automatically every 30 minutes
- Can be triggered manually via workflow dispatch
- Uses repository secrets for API credentials

### Setting Up GitHub Secrets

Add the following secrets to your GitHub repository:
- `ALPACA_API_KEY`
- `ALPACA_API_SECRET`
- `ALPACA_BASE_URL` (optional, defaults to paper trading URL)
- `ALPACA_DATA_URL` (optional)

## Logging

The application logs to:
- Console output (stdout)
- File: `logs/app.log` (hourly rotation, 48 backup files)

Logs include:
- Settings used for each run
- Calculated EMA values and signals
- Account information and positions
- Order actions and results

## Project Structure

```
Trading-Alpaca-Paper-Account/
├── alpaca_trader_minimal.py   # Main trading script
├── .github/
│   └── workflows/
│       └── trade.yml          # GitHub Actions workflow
├── .gitignore                 # Git ignore rules
├── logs/                      # Log files (created at runtime)
└── README.md                  # This file
```

## Disclaimer

⚠️ **IMPORTANT**: This project is designed for **paper trading only** (simulated trading). The default configuration uses Alpaca's paper trading API.

- This software is for educational and demonstration purposes only
- Do not use this for live trading without proper due diligence
- Past performance does not guarantee future results
- Trading leveraged ETFs carries significant risk

## License

This project is provided as-is without warranty. Use at your own risk.
