# Dual Momentum Portfolio Strategy

## Overview

This notebook implements a dual momentum strategy that rotates between three asset classes based on moving average crossovers. The system automatically allocates capital to stocks when momentum is positive, switches to bonds when stock momentum weakens but bond momentum remains positive, and moves to cash when both asset classes show negative momentum.

## How It Works

The strategy uses a simple but effective momentum measurement:
- **Stock allocation (VTI)**: When the 10-day moving average exceeds the 20-day moving average
- **Bond allocation (TLT)**: When stocks fail the momentum test but bonds pass
- **Cash allocation (SHY)**: When both stocks and bonds fail the momentum test

The goal is to capture uptrends while preserving capital during drawdowns by rotating to safer assets.

## Data Sources

The notebook fetches current market data automatically from Yahoo Finance using the yfinance library. No CSV files or manual data management required. The data includes:
- **VTI**: Vanguard Total Stock Market ETF
- **TLT**: iShares 20+ Year Treasury Bond ETF  
- **SHY**: iShares 1-3 Year Treasury Bond ETF

All prices use adjusted close values, which account for dividends and splits.

## Getting Started

1. Install required packages:
   ```bash
   pip install yfinance ffn pandas numpy matplotlib
   ```

2. Open the notebook and run all cells (Cell > Run All)

3. The notebook will:
   - Download current market data
   - Calculate momentum signals
   - Run the backtest
   - Display performance statistics and charts
   - Show the current recommended allocation

## Customization

You can modify the strategy parameters in the initialization cell:

**Backtest period:**
```python
start = '2000-01-01'  # Change start date
end = datetime.date.today()  # End date (defaults to today)
```

**Moving average lengths:**
```python
sma_length = 10  # Short moving average
lma_length = 20  # Long moving average
```

**Asset selection:**
```python
stock = 'SPY'   # Use S&P 500 instead of Total Market
bond = 'IEF'    # Use 7-10 Year Treasuries
money = 'BIL'   # Use 1-3 Month T-Bills
```

## Output Files

Results are automatically saved to your Desktop:
- `DMtest.csv` - Complete backtest data with signals
- `DMrebased.csv` - Rebased performance comparison
- `DMstats.csv` - Statistical summary
- `DMdrawdown.csv` - Drawdown series

## Requirements

- Python 3.8+
- pandas 2.0+
- yfinance 0.2+
- ffn
- numpy
- matplotlib

The notebook has been tested with Python 3.12 and current versions of all dependencies.

## Notes

The strategy parameters (10-day and 20-day moving averages) are for demonstration purposes. You may want to test different lookback periods based on your investment goals and risk tolerance. Past performance does not guarantee future results.

