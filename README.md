# Kite PnL Analysis using Python

## Project Overview

This project analyzes trading order data from Kite using Python and Pandas.

The main objective of the project is to calculate trading turnover and different charges for individual trades, perform stock-wise and trade-type analysis, and finally calculate Gross PnL, Net PnL, and the percentage of charges on Gross PnL.

The project demonstrates practical Data Analyst skills such as data cleaning, data transformation, aggregation, financial calculations, and Excel report generation.

---

## Technologies & Tools

* Python
* Pandas – Data cleaning, analysis, grouping, and calculations
* NumPy – Numerical operations
* Matplotlib – Data visualization
* Seaborn – Data visualization
* Jupyter Notebook
* Excel – Final analysis report

The notebook imports Pandas, NumPy, Matplotlib, and Seaborn.

---

## Dataset

The project uses order data containing information such as:

| Column       | Description           |
| ------------ | --------------------- |
| `Time`       | Time of the trade     |
| `Type`       | BUY or SELL           |
| `Instrument` | Stock traded          |
| `Product`    | Trading product type  |
| `Qty.`       | Quantity traded       |
| `Avg. price` | Average trading price |
| `Status`     | Order status          |

The dataset is filtered to include only completed MIS trades before further analysis.

---

## Data Cleaning & Preprocessing

The raw order data required some preprocessing before performing the calculations.

### 1. Filtering Trades

Only trades with `MIS` as the product and `COMPLETE` as the status were selected.

```python
data = data[(data["Product"]=="MIS") & (data["Status"]=="COMPLETE")]
```

### 2. Quantity Conversion

The quantity column originally contained values such as `1000/1000`. The actual quantity was extracted and converted into an integer.

```python
data["Qty."] = data["Qty."].str.split("/").str[0].astype(int)
```

This resulted in a numeric quantity column that could be used for further calculations.

### 3. Turnover Calculation

Turnover was calculated using the average price and quantity:

```python
data["Turnover"] = data["Avg. price"] * data["Qty."]
```

This created a turnover value for each individual trade.

---

## Trading Charges Calculation

The project calculates several charges associated with each trade.

The calculated charges include:

* Brokerage
* STT/CTT
* ETC
* SEBI charges
* Stamp charges
* GST
* Total charges

The total charges are calculated by adding the individual charges together.

## Brokerage is calculated from turnover and capped at ₹20 per trade in the notebook.

## Analysis Performed

### 1. Individual Trade Charges

The project calculates the different trading charges for every completed MIS trade.

The final individual-trade analysis contains:

* Turnover
* Brokerage
* STT/CTT
* ETC
* SEBI
* Stamp charges
* GST
* Total charges

This helps understand how much each trade costs after applying the different charges.

---

### 2. Stock-wise and Type-wise Analysis

The trades were grouped by:

* Instrument
* Type (BUY/SELL)

The analysis calculates:

* Total quantity
* Average price
* Turnover
* Brokerage
* STT/CTT
* ETC
* SEBI charges
* Stamp charges
* GST
* Total charges

The notebook uses Pandas `groupby()` and aggregation to perform this analysis.
For the analyzed data, the stocks include:

* ASHOKLEY
* TATAMOTORS

---

### 3. Gross PnL Calculation

Gross PnL was calculated by comparing the total SELL turnover with the total BUY turnover for each stock.

```python
result["Gross Pnl"] = result["SELL"] - result["BUY"]
```

The calculated Gross PnL was:

| Instrument | Gross PnL |
| ---------- | --------: |
| ASHOKLEY   | ₹1,410.00 |
| TATAMOTORS |   ₹387.50 |

---

### 4. Net PnL Calculation

After calculating Gross PnL, the total trading charges were deducted to calculate Net PnL.

```python
net_PnL = result["Gross Pnl"] - Total_charges
```

The final Net PnL was:

| Instrument | Gross PnL | Total Charges |   Net PnL |
| ---------- | --------: | ------------: | --------: |
| ASHOKLEY   | ₹1,410.00 |       ₹388.59 | ₹1,021.41 |
| TATAMOTORS |   ₹387.50 |        ₹90.82 |   ₹296.68 |

---

### 5. Charges as a Percentage of Gross PnL

The project also calculates how much of the Gross PnL was consumed by trading charges.

```python
Charges_on_Gross_PnL = (Total_charges / result["Gross Pnl"]) * 100
```

The results were:

| Instrument | Charges as % of Gross PnL |
| ---------- | ------------------------: |
| ASHOKLEY   |                    27.56% |
| TATAMOTORS |                    23.44% |

---

## Excel Report

The final analysis is exported to an Excel workbook named `kite Pnl.xlsx`.

The notebook creates three sheets:

1. **Charges for Individual Trade**
2. **Stock Analysis**
3. **Overall Summary**

The Excel workbook is generated using Pandas `ExcelWriter`.

---

## Project Structure

```text
Kite-PnL-Analysis/
│
├── kitepnl(1).ipynb
├── orders (1).csv
├── kite Pnl.xlsx
└── README.md
```

---

## How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/Kite-PnL-Analysis.git
```

### 2. Navigate to the project

```bash
cd Kite-PnL-Analysis
```

### 3. Install the required libraries

```bash
pip install pandas numpy matplotlib seaborn openpyxl jupyter
```

### 4. Open Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
kitepnl(1).ipynb
```

Make sure the order CSV file is available in the same project folder before running the notebook.

