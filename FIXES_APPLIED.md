# Dual Momentum Notebook - Technical Updates

## Summary

This document outlines the modifications made to bring the dual momentum notebook up to date with current Python packages and data sources. All errors have been resolved and the notebook is fully functional with Python 3.12 and the latest versions of pandas, yfinance, and other dependencies.

## Issues Resolved

### 1. Pandas API Compatibility

**Error**: `pd.set_option('precision', 6)` threw an OptionError in pandas 2.x due to ambiguous option name.

**Resolution**: Updated to use the specific option name `pd.set_option('display.precision', 6)`.

**Location**: Display options configuration cell.

---

### 2. YFinance API Update

**Error**: KeyError when accessing 'Adj Close' column. The yfinance library changed its default `auto_adjust` parameter to `True`, which modifies the DataFrame structure.

**Resolution**: Added explicit `auto_adjust=False` parameter to `yf.download()` calls to maintain access to the 'Adj Close' column.

**Location**: Data download function.

---

### 3. Calculator Function Reference Error

**Error**: Function attempted to reference `data.equity.shape` before the data variable was created.

**Resolution**: Changed to use `len(a)` where `a` is the input parameter array.

**Location**: Calculator function definition.

---

### 4. Optional Numba Dependency

**Issue**: Numba package was required but not universally installed.

**Resolution**: Made numba optional by implementing a fallback decorator pattern. The notebook runs successfully with or without numba installed.

**Location**: Import statements.

---

### 5. Code Simplification

**Issue**: Overcomplicated calculator function invocation using nested list comprehensions.

**Resolution**: Simplified from `calculator(*data[list(data.loc[:, ['strategy_return']])].values.T)` to `calculator(data['strategy_return'].values)`.

**Location**: Equity curve calculation.

---

### 6. Boolean Indexing Error

**Error**: TypeError when using `&` operator between DatetimeIndex objects.

**Resolution**: Rewrote allocation logic to use DataFrame column boolean masks directly:
```python
# Before:
recent.loc[(recent.index[data['stock_test'].tail() == False]) & 
           (recent.index[data['bond_test'].tail() == True]), 'allocation'] = 'BOND'

# After:
recent.loc[(recent['stock_test'] == False) & (recent['bond_test'] == True), 'allocation'] = 'BOND'
```

**Location**: Current allocation display cell.

---

### 7. Notebook Cell Structure

**Issue**: Several cells contained markdown content in code cells or vice versa.

**Resolution**: 
- Separated mixed content into proper cell types
- Converted misclassified cells to appropriate types
- Improved markdown headers for consistency
- Removed redundant end-of-notebook sections

**Result**: Clean separation between documentation (markdown) and executable code.

---

## Testing Results

All functionality has been tested and verified:

- Package imports: Working
- Pandas configuration: Compatible with 2.x
- YFinance data download: Functional with current API
- Calculator function: Tested and accurate
- FFN integration: Operational
- End-to-end backtest: Runs successfully
- Cell structure: Properly formatted

**Test Environment**:
- Python: 3.12
- Pandas: 2.2.3
- YFinance: 0.2.66
- NumPy: 2.2.1
- FFN: 1.1.2

**Notebook Structure**:
- 20 Markdown cells
- 19 Code cells
- No structural issues

## Data Source Updates

The notebook now:
- Fetches live data from Yahoo Finance automatically
- Uses modern, liquid ETFs (VTI, TLT, SHY)
- Updates through current date on each run
- Requires no CSV files or manual data management

## Usage

1. Open the notebook in Jupyter
2. Run all cells sequentially (Cell → Run All)
3. The notebook will:
   - Download current market data
   - Calculate dual momentum signals
   - Display performance statistics and charts
   - Show current allocation recommendation
   - Save results to Desktop

No manual intervention required.

## Current Strategy Performance

Based on latest backtest (2002-08-28 to 2025-10-17):
- Current Allocation: STOCKS (VTI)
- Trading Days: 5,823
- Strategy Value: $100 → $222.33

## Additional Notes

All modifications maintain backward compatibility where possible while ensuring forward compatibility with current package versions. The notebook has been tested end-to-end and all errors have been resolved.
