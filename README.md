# Python Codes for Data Cleansing, Wrangling, and Transformation

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

# SQL Codes for Data Preparation, Aggregation, and Reporting

**1. Create Formatted Table with Cleaned Data**

```sql
-- Create a table that keeps "$" and ","" formatting as TEXT
CREATE TABLE stocks_formatted AS
SELECT *
FROM read_csv_auto('cleaned_finance_data', header = true, ALL_VARCHAR = true);
```

| Index | Count   |
|-------|---------|
| 0     | 602,962 |

Column: Count  
Data type: int64

```sql
SELECT * FROM stocks_formatted LIMIT 5;
```

| Date       | Open    | High    | Low     | Close   | Volume      | Dividends | Stock Splits | Company |
|------------|---------|---------|---------|---------|-------------|-----------|--------------|---------|
| 2018-11-29 | $43.83  | $43.86  | $42.64  | $43.08  | 167,080,000 | $0.00     | 0.0          | AAPL    |
| 2018-11-29 | $104.77 | $105.52 | $103.53 | $104.64 | 28,123,200  | $0.00     | 0.0          | MSFT    |
| 2018-11-29 | $54.18  | $55.01  | $54.10  | $54.73  | 31,004,000  | $0.00     | 0.0          | GOOGL   |
| 2018-11-29 | $83.75  | $84.50  | $82.62  | $83.68  | 132,264,000 | $0.00     | 0.0          | AMZN    |
| 2018-11-29 | $39.69  | $40.06  | $38.74  | $39.04  | 54,917,200  | $0.04     | 0.0          | NVDA    |

**2. Covert Data Types to Numeric**

```sql
-- Parse the formatted text to numeric types for analysis
CREATE VIEW stocks_clean AS
SELECT
    CAST(Date as DATE) AS Date,
    Company,
    CAST(REPLACE(REPLACE(Open,'$',''), ',','') AS DOUBLE)  AS Open,
    CAST(REPLACE(REPLACE(High,'$',''), ',','') AS DOUBLE)  AS High,
    CAST(REPLACE(REPLACE(Low,'$',''), ',','') AS DOUBLE)  AS Low,
    CAST(REPLACE(REPLACE(Close,'$',''), ',','') AS DOUBLE)  AS Close,
    CAST(REPLACE(Volume, ',', '') AS BIGINT)                 AS Volume,
    CAST(REPLACE(REPLACE(Dividends, '$',''), ',','') AS DOUBLE) AS Dividends,
FROM stocks_formatted;
```

Data type: int64

```sql
SELECT * FROM stocks_clean LIMIT 5;
```

| Date       | Company | Open   | High   | Low    | Close  | Volume   | Dividends |
|------------|---------|--------|--------|--------|--------|----------|-----------|
| 2018-11-29 | AAPL    | 43.83  | 43.86  | 42.64  | 43.08  | 167080000| 0.00      |
| 2018-11-29 | MSFT    | 104.77 | 105.52 | 103.53 | 104.64 | 28123200 | 0.00      |
| 2018-11-29 | GOOGL   | 54.18  | 55.01  | 54.10  | 54.73  | 31004000 | 0.00      |
| 2018-11-29 | AMZN    | 83.75  | 84.50  | 82.62  | 83.68  | 132264000| 0.00      |
| 2018-11-29 | NVDA    | 39.69  | 40.06  | 38.74  | 39.04  | 54917200 | 0.04      |

**3. Summarise Metrics by Company and Date**

```sql
-- Aggregate metrics per company per date
CREATE VIEW company_daily AS
SELECT
    Date,
    Company,
    SUM(Open) AS Open_Total,
    SUM(High) AS High_Total,
    SUM(Low) AS Low_Total,
    SUM(Close) AS Close_Total,
    SUM(Volume) AS Volume_Total,
    SUM(Dividends) AS Dividends_Total
FROM stocks_clean
GROUP BY Date, Company,
ORDER BY Date, Company;
```

Data type: int64

```sql
SELECT * FROM company_daily LIMIT 5;
```

| Date       | Company | Open_Total | High_Total | Low_Total | Close_Total | Volume_Total | Dividends_Total |
|------------|---------|------------|------------|-----------|-------------|--------------|-----------------|
| 2018-11-29 | A       | 68.67      | 69.59      | 68.67     | 69.00       | 2625800    | 0.0             |
| 2018-11-29 | AAPL    | 43.83      | 43.86      | 42.64     | 43.08       | 167080000  | 0.0             |
| 2018-11-29 | ABBV    | 70.47      | 71.52      | 69.96     | 71.18       | 3838000    | 0.0             |
| 2018-11-29 | ABEV    | 3.61       | 3.67       | 3.58      | 3.63        | 36131600   | 0.0             |
| 2018-11-29 | ABT     | 66.60      | 67.77      | 66.55     | 67.37       | 6447700    | 0.0             |

**4. Show Top 5 Companies by Highest High Stock Price by Date**

```sql
-- Top 5 companies per date by High_Total
SELECT
    Date,
    Company,
    Open_Total,
    High_Total,
    Low_Total,
    Close_Total,
    Volume_Total,
    Dividends_Total
FROM (
    SELECT
        Date,
        Company,
        Open_Total,
        High_Total,
        Low_Total,
        Close_Total,
        Volume_Total,
        Dividends_Total,
        ROW_NUMBER() OVER(
            PARTITION BY Date
            ORDER BY High_Total DESC, Company
        ) AS rn
    FROM company_daily
) t
WHERE rn <=5
ORDER BY Date, rn
LIMIT 15;
```

| Date       | Company | Open_Total | High_Total | Low_Total | Close_Total | Volume_Total | Dividends_Total |
|------------|---------|------------|------------|-----------|-------------|--------------|-----------------|
| 2018-11-29 | NVR     | 2547.50    | 2569.85    | 2457.10   | 2473.77     | 20500        | 0.0             |
| 2018-11-29 | BKNG    | 1866.00    | 1885.15    | 1859.71   | 1865.15     | 358100       | 0.0             |
| 2018-11-29 | AZO     | 835.53     | 836.74     | 824.02    | 825.83      | 279200       | 0.0             |
| 2018-11-29 | MTD     | 620.13     | 638.18     | 619.98    | 630.92      | 120000       | 0.0             |
| 2018-11-29 | CMG     | 489.95     | 493.98     | 482.14    | 482.56      | 426700       | 0.0             |
| 2018-11-30 | NVR     | 2480.00    | 2498.00    | 2435.33   | 2450.00     | 37800        | 0.0             |
| 2018-11-30 | BKNG    | 1868.32    | 1897.11    | 1852.80   | 1891.88     | 361000       | 0.0             |
| 2018-11-30 | AZO     | 827.09     | 827.09     | 804.57    | 809.07      | 602900       | 0.0             |
| 2018-11-30 | MTD     | 631.45     | 639.23     | 627.34    | 636.66      | 175400       | 0.0             |
| 2018-11-30 | CMG     | 480.57     | 482.55     | 463.00    | 473.21      | 956200       | 0.0             |
| 2018-12-03 | NVR     | 2472.44    | 2547.37    | 2472.44   | 2542.45     | 33000        | 0.0             |
| 2018-12-03 | BKNG    | 1904.00    | 1928.78    | 1879.29   | 1880.00     | 474200       | 0.0             |
| 2018-12-03 | AZO     | 829.00     | 835.83     | 817.00    | 820.00      | 513000       | 0.0             |
| 2018-12-03 | MTD     | 637.52     | 651.39     | 634.35    | 650.26      | 158500       | 0.0             |
| 2018-12-03 | CMG     | 486.00     | 490.57     | 483.29    | 487.00      | 507400       | 0.0             |

**5. Show Highest High Stock Price across All Dates**

```sql
-- One company per date with the highest High_Total
SELECT
  Date,
  Company,
  Open_Total,
  High_Total,
  Low_Total,
  Close_Total,
  Volume_Total,
  Dividends_Total
FROM (
  SELECT
    Date,
    Company,
    Open_Total,
    High_Total,
    Low_Total,
    Close_Total,
    Volume_Total,
    Dividends_Total,
    ROW_NUMBER() OVER (
      PARTITION BY Date
      ORDER BY High_Total DESC, Company
    ) AS rn
  FROM company_daily
) t
WHERE rn = 1
ORDER BY High_Total DESC
LIMIT 10;
```

| Date       | Company | Open_Total | High_Total | Low_Total | Close_Total | Volume_Total | Dividends_Total |
|------------|---------|------------|------------|-----------|-------------|--------------|-----------------|
| 2023-11-14 | NVR     | 6170.00    | 6356.19    | 6170.00   | 6290.72     | 22200        | 0               |
| 2023-11-22 | NVR     | 6276.77    | 6350.00    | 6194.07   | 6222.89     | 12800        | 0               |
| 2023-11-15 | NVR     | 6275.00    | 6350.00    | 6267.09   | 6292.17     | 19300        | 0               |
| 2023-11-16 | NVR     | 6293.50    | 6349.49    | 6236.84   | 6290.59     | 25000        | 0               |
| 2023-11-21 | NVR     | 6290.01    | 6344.91    | 6239.37   | 6310.92     | 18500        | 0               |
| 2023-11-20 | NVR     | 6290.01    | 6344.91    | 6226.37   | 6244.84     | 22200        | 0               |
| 2023-11-17 | NVR     | 6310.92    | 6349.49    | 6226.37   | 6290.59     | 24000        | 0               |
| 2023-11-13 | NVR     | 6349.49    | 6356.19    | 6236.84   | 6292.17     | 19900        | 0               |
| 2023-11-10 | NVR     | 6350.00    | 6356.19    | 6236.84   | 6292.17     | 22200        | 0               |
| 2023-11-09 | NVR     | 6344.91    | 6350.00    | 6236.84   | 6292.17     | 24000        | 0               |

**5. Aggregate Yearly Averages of All Stock Metrics**

```sql
-- Yearly average for all metrics
CREATE OR REPLACE VIEW yearly_average AS
SELECT
  EXTRACT(YEAR FROM Date) AS Year,
  AVG(Open) AS Avg_Open,
  AVG(High) AS Avg_High,
  AVG(Low) AS Avg_Low,
  AVG(Close) AS Avg_Close,
  AVG(Volume) AS Avg_Volume,
  AVG(Dividends) AS Avg_Dividends
FROM stocks_clean
WHERE Date IS NOT NULL
GROUP BY Year;
```

Data type: int64

```sql
-- Show all average metrics for each year (latest to oldest)
SELECT * FROM yearly_average
ORDER BY Avg_High DESC;
```

| Year | Avg_Open | Avg_High | Avg_Low | Avg_Close | Avg_Volume | Avg_Dividends |
|------|----------|----------|---------|-----------|------------|---------------|
| 2023 | 165.151  | 167.121  | 163.416 | 165.391   | 5412390    | 0.00980594    |
| 2021 | 160.368  | 162.501  | 158.020 | 160.237   | 6158960    | 0.00803593    |
| 2022 | 159.418  | 161.734  | 156.901 | 159.265   | 6443220    | 0.00883526    |
| 2020 | 126.420  | 128.031  | 124.802 | 126.439   | 6359220    | 0.00734392    |
| 2019 | 101.324  | 102.330  | 100.394 | 101.456   | 5277750    | 0.00801281    |
| 2018 | 87.694   | 88.929   | 86.075  | 87.318    | 6932500    | 0.00826656    |

**6. Year with the Highest Average High Stock Price**

```sql
-- Show the year with the highest Avg_High
SELECT * FROM yearly_average
ORDER BY Avg_High DESC
LIMIT 1;
```

| Year | Avg_Open | Avg_High | Avg_Low | Avg_Close | Avg_Volume | Avg_Dividends |
|------|----------|----------|---------|-----------|------------|---------------|
| 2023 | 165.151  | 167.121  | 163.416 | 165.391   | 5412390    | 0.00980594    |

**7. Aggregate Yearly Totals for All Stock Metrics**

```sql
-- Yearly totals for all metrics except Volume
CREATE OR REPLACE VIEW yearly_total AS
SELECT
    EXTRACT(YEAR FROM Date) AS Year,
    SUM(Open_Total) AS Total_Open,
    SUM(High_Total) AS Total_High,
    SUM(Low_Total) AS Total_Low,
    SUM(Close_Total) AS Total_Close,
    SUM(Dividends_Total) AS Total_Dividends
FROM company_daily
WHERE Date IS NOT NULL
GROUP BY Year
ORDER BY Year DESC;
SELECT * FROM yearly_total;
```

| Year | Total_Open | Total_High | Total_Low | Total_Close | Total_Dividends |
|------|------------|------------|-----------|-------------|-----------------|
| 2023 | 5174190    | 5235890    | 5119810   | 5181690     | 307.22          |
| 2022 | 6675150    | 6772110    | 6569770   | 6668720     | 369.95          |
| 2021 | 6640690    | 6729020    | 6543470   | 6635240     | 332.76          |
| 2020 | 5216100    | 5282580    | 5149350   | 5216870     | 303.01          |
| 2019 | 4051030    | 4091260    | 4013860   | 4056330     | 320.36          |
| 2018 | 844845     | 856744     | 829246    | 841221      | 79.64           |

**8. Year wtih the Highest Total High Stock Price**

```sql
-- Show the year wtih the highest Total_High
SELECT * FROM yearly_total
ORDER BY Total_High DESC
LIMIT 1;
```

| Year | Total_Open | Total_High | Total_Low | Total_Close | Total_Dividends |
|------|------------|------------|-----------|-------------|-----------------|
| 2022 | 6675150    | 6772110    | 6569770   | 6668720     | 369.95          |

# SQL Data Analysis

- The daily analysis showed consistent dominance by companies like NVR, which repeatedly ranked #1 for daily High_Total values in 2023, hitting highs above $6,350 in November.

- Looking at the 10 highest highs across all dates, every single peak was in 2023, with NVR reaching $6,356.19 at its maximum — far above any other company in previous years.

- Yearly averages highlighted clear growth trends. 2023 averaged a High of $167.12, compared to only $102.33 in 2019 and $88.93 in 2018, showing how much the market strengthened over five years.

- When rolling up to yearly totals (excluding volume), 2022 recorded the highest overall High_Total of 6,772,110, even though 2023 had stronger averages. This suggests 2022 had more trading days or broader activity, while 2023 had higher per-day values.

- Dividends also grew steadily, with 2022 paying out the most in total ($369.95), compared to just $79.64 in 2018, showing long-term growth not just in prices but also in returns to shareholders.

# Power BI Dashboards and Analysis

**1. Dashboard 1: Market & Growth**

![all_dashboards_page-0001](https://github.com/user-attachments/assets/29e689eb-645e-4d6d-9978-1b28560e2555)

Analysis:

- The year-over-year line chart shows that both average high and close stock prices climbed steadily from 2018–2023, with only a minor dip between 2021 and 2022. Growth was especially aggressive from 2018–2021, while 2022–2023 continued upward at a slower but still positive pace.

- Average highs stayed only slightly above average closes, meaning the gap between the day’s high and close values was relatively small — an indicator of market stability rather than extreme swings.

- The highest averages for both highs and closes were observed in 2023, while the lowest values occurred in 2018, reinforcing a clear long-term growth trend.

- The dividend growth area chart reveals a sharp drop in 2020, followed by a steep recovery. From 2020 to 2023, total dividends quadrupled, underscoring investor confidence and company profitability in the later years.

- Despite the 2020 dip, the overall 2018–2023 trajectory is upward across both price and dividend measures, confirming strong and consistent market expansion over the observed period.

**2. Dashboard 2: Company Comparisons**

![all_dashboards_page-0002](https://github.com/user-attachments/assets/55f98b17-f2dc-4861-904b-3b7449870816)

Analysis:

- The Total High Stock Prices by Company chart highlights an enormous gap between the top two performers. NVR, Inc. dominated with a total high stock price of $5.5M, which is almost double the second-place company, Booking Holdings (BKNG), at $2.7M.

- Most companies landed in the $530K–$900K range, such as Chipotle Mexican Grill (CMG), AutoZone (AZO), and others. A mid-tier group including First Citizens BancShares (FCNCA), MercadoLibre (MELI), Mettler-Toledo (MTD), and others reached between $900K–$2M, but still fell far short of the top two.

- This makes NVR the clear outlier in the dataset, far surpassing all competitors in total highs, while differences among mid- and lower-tier companies were more modest and less impactful.

- The Total Dividends by Company Treemap shows TransDigm Group (TDG) as the dominant leader with 116 total dividends paid from 2018–2023. Other significant dividend contributors included BlackRock (BLK) with 82, Broadcom (AVGO) with 72, Equinix (EQIX) with 59, and Public Storage (PSA) with 56.

- TDG’s dominance is even clearer when breaking down by year: in 2019, TDG contributed 32 dividends, while the next companies managed only 6.6 and 4.9; in 2023, TDG delivered 35 dividends, while the closest competitors had just 7.7 and 6.3, confirming TDG’s overwhelming leadership in dividends across the observed period.

**3. Dashboard 3: Winners and Market Volatility**

![all_dashboards_page-0003](https://github.com/user-attachments/assets/6e57b846-c7e2-492d-a563-4a1bff07cb09)

Analysis:

- In terms of total high stock prices by date, the maximum observed was $3.69M, representing the single strongest trading day across the dataset when summing all companies’ highs together.

- The champion company during the observed period was NVR, Inc., which consistently secured the highest daily high stock prices, cementing its position as the market leader in overall price performance.

- Across the period, the highest high stock prices per date exceeded the yearly average high stock price by 661%, showing just how extreme daily peaks could be compared to long-term averages.

- When analyzing the daily return % ((Close – Open) / Open) and daily volatility % ((High – Low) / Open) alongside trading volume for the top 15 companies by High_Total, MercadoLibre (MELI) stood out with the highest volatility at 0.04% and the second-highest return at 0.01%.

- AutoZone (AZO) showed the lowest volatility (0.02%) and nearly zero return, while ASML Holding (ASML) and Thermo Fisher Scientific (TMO) had average return/volatility but very high volumes of 99M and 61.5M, demonstrating strong liquidity with more stability.

# AI Machine Mearning Forecasting Visuals and Analysis

**1. Forecasted High vs Low Totals**

<img width="1779" height="980" alt="output (2)" src="https://github.com/user-attachments/assets/267856fd-a4cb-4877-82e1-78f4690c4d4d" />

Analysis:

- High totals are projected to grow +86.4% from the latest actual year to the end of the forecast horizon.

- Low totals are forecasted to rise +85.5%, tracking closely with highs and maintaining a stable spread.

- The parallel trend of highs and lows suggests steady momentum without major volatility spikes.

- Market growth trajectory indicates that 2026 highs will nearly double 2023 levels, signaling strong long-term upside.

**2. Forecasted Top 10 Leaders by High Totals**

<img width="1771" height="1180" alt="output (3)" src="https://github.com/user-attachments/assets/ccb5b3e4-7130-49b6-939a-602c194506d1" />

Analysis:

- NVR leads the forecast with a High total ≈ 630,945, making it the strongest projected performer.

- The gap between NVR and the next highest companies remains substantial, reinforcing its outlier dominance.

- Most other top-10 companies cluster significantly lower, highlighting concentration of leadership in a few firms.

- Overall, the forecast confirms NVR will prosper disproportionately relative to peers over the forecast horizon.

**3. Forecasted Return % vs Volatility %, Bubble by Company**

<img width="1779" height="1180" alt="output (4)" src="https://github.com/user-attachments/assets/8534d8a5-1ce7-4b30-9b47-2bd0d95cbc6c" />

Analysis:

- FICO is projected to have the highest forecast return % among the leaders, making it the strongest growth play.

- REGN shows the lowest forecast volatility, appealing as a more defensive yet stable option.

- NFLX carries the largest average trading volume, providing liquidity advantages even if not top on return or volatility.

- The spread of bubbles shows that companies differ more on risk-return tradeoffs than volume, offering multiple strategies.

**4. Forecasted Yearly Average Return % vs Volatility, Market Level**

<img width="1779" height="980" alt="output (5)" src="https://github.com/user-attachments/assets/45d52b12-0b39-4a92-9dbb-a9facdfa4525" />

Analysis:

- Market returns trend upward, continuing their positive trajectory from recent years.

- Volatility % also edges higher, but at a modest pace, indicating risk is not growing out of proportion.

- The divergence between returns and volatility suggests improving market efficiency — better upside without extreme added risk.

- By the forecast horizon, investors can expect higher average returns with only marginally higher volatility, a favorable market outlook.
