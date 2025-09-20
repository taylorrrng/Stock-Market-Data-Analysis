# Python for Data Cleansing, Wrangling, and Transformation

**1. Import and Check Raw Data**

```python
# Import raw data & show the first 5 rows
import pandas as pd
df = pd.read_csv('raw_finance_data')
print(df.head(5));
```

| Date                | Open      | High      | Low       | Close     | Volume     | Dividends | Stock Splits | Company |
|---------------------|-----------|-----------|-----------|-----------|------------|-----------|--------------|---------|
| 2018-11-29 00:00:00 | 43.829761 | 43.863354 | 42.639594 | 43.083508 | 167080000  | 0.00      | 0.0          | AAPL    |
| 2018-11-29 00:00:00 | 104.769074| 105.519257| 103.534595| 104.636131| 28123200   | 0.00      | 0.0          | MSFT    |
| 2018-11-29 00:00:00 | 54.176498 | 55.007500 | 54.099998 | 54.729000 | 31004000   | 0.00      | 0.0          | GOOGL   |
| 2018-11-29 00:00:00 | 83.749496 | 84.499496 | 82.616501 | 83.678497 | 132264000  | 0.00      | 0.0          | AMZN    |

**2. Duplicate Check**

```python
# Check for duplicates
duplicates = df.duplicated()
print("Number of duplicate rows:", duplicates.sum())
df[duplicates];
```

Number of duplicate rows: 0

**3. Null Value Check**

```python
# Check for null values
print(df.isnull().sum())
total_nulls = df.isnull().sum().sum()
print("Total null values:", total_nulls);
```

| Column       | Null Values |
|--------------|-------------|
| Date         | 0           |
| Open         | 0           |
| High         | 0           |
| Low          | 0           |
| Close        | 0           |
| Volume       | 0           |
| Dividends    | 0           |
| Stock Splits | 0           |
| Company      | 0           |

Total null values: 0

**4. Date Format Conversion**

```python
# Convert current date format to YYYY-MM-DD
df['Date'] = pd.to_datetime(df['Date'], errors='coerce')
df['Date'] = df['Date'].dt.date
print("After cleaning:", df['Date'].head())
print("Data type:", df['Date'].dtype);
```

| Index | Date       |
|-------|------------|
| 0     | 2018-11-29 |
| 1     | 2018-11-29 |
| 2     | 2018-11-29 |
| 3     | 2018-11-29 |
| 4     | 2018-11-29 |

Column: Date  
Data type: object

**5. Round Up Price Columns**

```python
# Round price columns to 2 decimal places
price_cols = ['Open', 'High', 'Low', 'Close']
df[price_cols] = df[price_cols].astype(float).round(2)
print(df[price_cols].dtypes)
print(df[price_cols].head(5));
```

| Column | Data Type |
|--------|-----------|
| Open   | float64   |
| High   | float64   |
| Low    | float64   |
| Close  | float64   |

| Index | Open  | High  | Low   | Close |
|-------|-------|-------|-------|-------|
| 0     | 43.83 | 43.86 | 42.64 | 43.08 |
| 1     | 104.77| 105.52| 103.53| 104.64|
| 2     | 54.18 | 55.01 | 54.10 | 54.73 |
| 3     | 83.75 | 84.50 | 82.62 | 83.68 |
| 4     | 39.69 | 40.06 | 38.74 | 39.04 |

dtype: object

**6. Convert Columns to USD $**

```python
# Add "$" to price and dividend columns
def money(x):
    try:
        return f"${float(x):,.2f}"
    except:
        return x
for c in price_cols + ['Dividends']:
    df[c] = df[c].apply(money)
print(df.head(5));
```

| Date       | Open    | High    | Low     | Close   | Volume     | Dividends | Stock Splits | Company |
|------------|---------|---------|---------|---------|------------|-----------|--------------|---------|
| 2018-11-29 | $43.83  | $43.86  | $42.64  | $43.08  | 167080000  | $0.00     | 0.0          | AAPL    |
| 2018-11-29 | $104.77 | $105.52 | $103.53 | $104.64 | 28123200   | $0.00     | 0.0          | MSFT    |
| 2018-11-29 | $54.18  | $55.01  | $54.10  | $54.73  | 31004000   | $0.00     | 0.0          | GOOGL   |
| 2018-11-29 | $83.75  | $84.50  | $82.62  | $83.68  | 132264000  | $0.00     | 0.0          | AMZN    |
| 2018-11-29 | $39.69  | $40.06  | $38.74  | $39.04  | 54917200   | $0.04     | 0.0          | NVDA    |

**7. Improve Volume Column's Readability**

```python
# Show "," in the Volume column for better readability
print(df['Volume'].head())
print(df['Volume'].dtype)
df['Volume'] = df['Volume'].apply(lambda v: f"{int(v):,}")
print(df[['Volume']].head(5))
```

| Index | Volume      |
|-------|-------------|
| 0     | 167,080,000 |
| 1     | 28,123,200  |
| 2     | 31,004,000  |
| 3     | 132,264,000 |
| 4     | 54,917,200  |

Column: Volume  
Data type: int64

**8. Data Check Post Cleaning**

```python
# Check dataset after cleaning
print("Dataset info after cleaning:", df.info())
print("First 5 rows:", df.head());
```

Dataset Info
- Entries: 602,962  
- Columns: 9  
- Memory Usage: 41.4+ MB  

| Column       | Non-Null Count | Dtype   |
|--------------|----------------|---------|
| Date         | 205,486        | object  |
| Open         | 602,962        | object  |
| High         | 602,962        | object  |
| Low          | 602,962        | object  |
| Close        | 602,962        | object  |
| Volume       | 602,962        | object  |
| Dividends    | 602,962        | object  |
| Stock Splits | 602,962        | float64 |
| Company      | 602,962        | object  |

Dtypes: float64(1), object(8)  

First 5 Rows:
| Date       | Open    | High    | Low     | Close   | Volume      | Dividends | Stock Splits | Company |
|------------|---------|---------|---------|---------|-------------|-----------|--------------|---------|
| 2018-11-29 | $43.83  | $43.86  | $42.64  | $43.08  | 167,080,000 | $0.00     | 0.0          | AAPL    |
| 2018-11-29 | $104.77 | $105.52 | $103.53 | $104.64 | 28,123,200  | $0.00     | 0.0          | MSFT    |
| 2018-11-29 | $54.18  | $55.01  | $54.10  | $54.73  | 31,004,000  | $0.00     | 0.0          | GOOGL   |
| 2018-11-29 | $83.75  | $84.50  | $82.62  | $83.68  | 132,264,000 | $0.00     | 0.0          | AMZN    |
| 2018-11-29 | $39.69  | $40.06  | $38.74  | $39.04  | 54,917,200  | $0.04     | 0.0          | NVDA    |

**9. Export Cleaned Data**

```python
# Export cleaned dataset
df.to_csv("cleaned_finance_data", index = False);
```
