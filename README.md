# 🚗 BMW Sales Performance Dashboard — Power BI

An end-to-end interactive business intelligence dashboard built in Power BI, analysing BMW vehicle sales data across models, countries, and sales channels — featuring advanced DAX measures, SVG sparklines, and a custom UX-driven filter experience.

---

## 📸 Dashboard Preview

<img width="1604" height="924" alt="Screenshot 2026-04-28 145703" src="https://github.com/user-attachments/assets/127b8ac6-04da-429b-a60b-3fa90d783b37" />

<img width="1553" height="880" alt="Screenshot 2026-04-28 145725" src="https://github.com/user-attachments/assets/62affa88-99b0-4c6a-a128-daf98c646dc4" />

---

## 📁 Project Structure

```
bmw-sales-dashboard/
│
├── BMW_Sales_Dashboard.pbix       # Main Power BI file
├── data/
│   └── bmw_sales_data.csv         # Raw sales data (source CSV)
├── images/
│   └── car_images/                # BMW model images (hosted via GitHub raw links)
│       ├── bmw_3series.png
│       ├── bmw_5series.png
│       └── ...
├── flags/
│   └── country_flags/             # Country flag images
└── README.md
```

---

## ⚙️ How Car Images Were Handled

Power BI requires publicly accessible URLs to render images in visuals. Since local file paths don't work inside `.pbix` files, the car images were uploaded directly to this GitHub repository and referenced using **GitHub raw links**.

**How to replicate this:**
1. Upload your image files to a folder in this repo (e.g., `/images/car_images/`)
2. Open the image on GitHub and click **"Raw"**
3. Copy the raw URL — it will look like:
   ```
   https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/car_images/bmw_3series.png
   ```
4. Paste this URL into your data source file or dim model table under the image column
5. In Power BI's Data View, set the column's **Data Category → Image URL**

> ✅ This ensures images render correctly inside slicers, tables, and card visuals without needing external hosting.

---

## 🔄 Data Pipeline Overview

### 1. Power Query (ETL)
- Loaded raw BMW sales CSV and renamed it as the **fact table**
- Removed null/empty rows for data integrity
- Created **3 normalised dimension tables** from the fact table:
  - `dim_model` — Model name + Model ID (index) + car image URL
  - `dim_country` — Region, Country + Country ID (index) + flag image URL
  - `dim_channel` — Sales channel + Channel ID (index)
- Merged dimension IDs back into the fact table and removed original text columns
- Imported and merged **car images** and **country flags** via external files

### 2. Data Modelling
- Built a **Star Schema** with one-to-many relationships from all dimension tables and a Calendar table to the fact table
- Created a **DAX Calendar Table** with Year, Month, Month Number, Week Day, and Week Number columns
- Set image and flag columns' Data Category to **Image URL** for correct rendering

---

## 📐 DAX Measures

All measures are stored in a dedicated **Measures** table for clean model organisation.

| Category | Measure | Description |
|---|---|---|
| Revenue | `Revenue` | Total revenue (SUM of unit price × quantity) |
| Revenue | `Revenue PY` | Previous year revenue using DATEADD |
| Revenue | `Revenue Variance` | Current vs previous year difference |
| Revenue | `Revenue Growth %` | DIVIDE of variance over previous year |
| Revenue | `Revenue Growth Arrow` | Formatted % with ▲/▼ Unicode arrow (UNICHAR) |
| Quantity | `Quantity Sold` | Total units sold |
| Quantity | `Qty Sold PY` | Previous year quantity |
| Quantity | `Qty Variance & Growth` | Same logic as revenue measures |
| Pricing | `Average Price` | Revenue ÷ Quantity Sold |
| Pricing | `Average Price Formatted` | Dollar-formatted display using FORMAT() |
| Context | `Selected Period` | Dynamic text showing active date filter range |

---

## 📊 Dashboard Pages

### Page 1 — Sales Dashboard
- **Trend Chart**: Line + clustered column chart showing Revenue vs Previous Year Revenue
- **Field Parameter Slicer**: Dynamically switches X-axis between Year, Month, and Day
- **Top Selling Models Slicer**: Image-based slicer sorted by Quantity Sold
- **Most Expensive Models Slicer**: Image-based slicer filtered to Top 4 by Average Price
- **Sales by Country Table**: Country flag, name, quantity, revenue, and **SVG Sparklines** for inline trend visualisation

### Page 2 — Model Detail View
- **Text Search Slicer**: Search models by name
- **Model Image Slicer**: Horizontal single-select image slicer
- **Dynamic Labels**: Shape-based text linked to DAX measures for better alignment
- **Sales Over Time Table**: Year-wise breakdown of Quantity Sold and Revenue per selected model

---

## 📈 Key Findings & KPIs

> Derived from the completed dashboard covering **Jan 2019 – Dec 2023** across global markets.

| KPI | Value |
|---|---|
| 💰 Total Global Revenue | **$1.13bn** |
| 📊 Year-over-Year Revenue Growth | **+24.7%** (vs $0.90bn previous year) |
| 🚗 Total Units Sold | **15,000+** |
| 🏆 Top Selling Model | **BMW Z4** — 666 units | $52M revenue |
| 💎 Most Expensive Model | **BMW M8** — avg. price $79.1K |

### 🌍 Top Markets by Units Sold
| Country | Units Sold | Revenue | YoY Growth |
|---|---|---|---|
| 🇺🇸 United States | 1,019 | $75M | +26.6% ▲ |
| 🇲🇽 Mexico | 1,017 | $77M | +26% ▲ |
| 🇨🇦 Canada | 959 | $73M | +24.2% ▲ |
| 🇳🇬 Nigeria | 755 | $57M | +24.8% ▲ |
| 🇰🇪 Kenya | 660 | $50M | +22% ▲ |
| 🇪🇬 Egypt | 639 | $48M | +31.5% ▲ |

### 🏎️ Top Selling Models (by Units)
| Rank | Model | Units Sold |
|---|---|---|
| 1 | BMW Z4 | 666 |
| 2 | BMW 8 Series | 641 |
| 3 | BMW M4 | 620 |
| 4 | BMW i8 | 615 |
| 5 | BMW i3 | 611 |

### 💎 Most Expensive Models (by Avg. Price)
| Model | Average Price |
|---|---|
| BMW M8 | $79.1K |
| BMW iX3 | $78.2K |
| BMW X4 | $77.7K |
| BMW M4 | $77.6K |

### 📦 Sales by Channel
| Channel | Units Sold | Share |
|---|---|---|
| Wholesale | ~7K | 45% |
| Dealership | ~5K | 33% |
| Online | ~3K | 22% |

### 💡 Key Business Insights
- **North America dominates** global sales, with the US and Mexico together accounting for over 2,000 units and $152M in combined revenue
- **Africa is an emerging growth market** — Nigeria (+24.8%) and Egypt (+31.5%) recorded among the highest YoY growth rates globally
- **Wholesale remains the primary sales channel** at 45%, while online sales at 22% indicate a growing digital sales opportunity
- **BMW Z4 leads volume** while the **BMW M8 leads pricing** — highlighting a clear split between high-volume and premium segments
- Revenue grew consistently from 2019 to 2023, with a notable dip in 2020–2021 likely reflecting global market disruptions, followed by a strong recovery

---

## 🎛️ Interactivity & UX Features

- **Page Navigation Buttons**: Seamlessly switch between Dashboard and All Models pages
- **Custom Collapsible Filter Pane**:
  - Built using grouped shapes and slicers (Year, Region, Channel, Model)
  - Controlled via **two bookmarks**: `show_filter_pane` and `hide_filter_pane`
  - Bookmark "Data" option unchecked to preserve active filters when toggling the pane
  - Triggered by Filter (open) and Close (hide) buttons

---

## 🛠️ Tools & Skills Used

- **Power BI Desktop** — Report development
- **Power Query (M Language)** — Data transformation and normalisation
- **DAX** — Measure creation, time intelligence, dynamic formatting
- **SVG** — Custom sparkline visuals embedded in table rows
- **GitHub** — Image hosting via raw URLs for Power BI image rendering
- **Data Modelling** — Star schema design with dimension and fact tables

---

## 🚀 How to Run This Project

1. Clone or download this repository
2. Open `BMW_Sales_Dashboard.pbix` in **Power BI Desktop**
3. If prompted, update the data source path to point to `data/bmw_sales_data.csv`
4. Refresh the data — all relationships, measures, and visuals will load automatically
5. Image visuals rely on the GitHub raw URLs in this repo — they will work as long as the `/images/` folder remains in place

---

## 👤 Author

**Shubhan Krishnan Sridhar**
- 📧 sridharshubhan@gmail.com
- 💼 https://www.linkedin.com/in/shubhan-krishnan-sridhar-05885632a/
- 🐙 [[GitHub Profile URL](https://github.com/shubhansridhar)]
