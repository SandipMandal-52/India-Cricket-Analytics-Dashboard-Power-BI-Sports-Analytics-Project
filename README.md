
# 🏏 India Cricket Analytics Dashboard — Power BI Sports Analytics Project

> **A 4-page interactive Power BI dashboard analyzing India cricket player performance across Batting, Bowling, and Fielding disciplines — with individual player drill-down and head-to-head comparative analysis of top 5 batsmen, built on ESPN cricket data.**

---

## 📌 Project Overview

Cricket generates enormous amounts of structured performance data — but most fans and analysts access it through static scorecard pages. This project transforms ESPN cricket data into an **interactive, player-filterable Power BI dashboard** covering all three disciplines of the game: Batting, Bowling, and Fielding.

The dashboard allows users to select any player from a dropdown, instantly see their full career stat profile across their active years, and compare top batsmen head-to-head on key performance metrics.

---

## 📊 Dashboard Structure — 4 Pages

### Page 1 — Batting Data Analytics (Individual Player View)

**Player dropdown slicer** → shows active career year range dynamically (e.g., A Nehra: 2002–2011)

| KPI Card | Description |
|----------|-------------|
| Strike Rate | Runs scored per 100 balls |
| Total Runs | Career run tally |
| Total Innings | Innings batted |
| Matches | Total matches played |
| Balls Faced | Total deliveries faced |
| Average | Runs per dismissal |
| 4's | Boundary count |
| 6's | Six count |
| 0's | Duck count |
| Not Outs | Times not dismissed |
| Half Centuries | 50s scored |
| Centuries | 100s scored |
| Highest Score | Best individual innings |

---

### Page 2 — Bowling Data Analytics (Individual Player View)

**Player dropdown slicer** → active career years shown (e.g., A Jadeja: 1992–2000)

| KPI Card | Description |
|----------|-------------|
| Overs | Total overs bowled |
| Wickets | Career wickets taken |
| Total Matches | Matches in which bowled |
| Total Balls Played | Balls delivered |
| Total Runs | Runs conceded |
| Strike Rate | Balls per wicket |
| Maiden Overs | Overs conceding zero runs |
| Average | Runs per wicket |
| 4's | Four-wicket hauls |
| Total Innings | Innings bowled |
| Five Wickets | Five-wicket hauls |
| Economy | Runs per over |
| Best Bowling Innings | Best figures in an innings |

---

### Page 3 — Fielding Data Analytics (Individual Player View)

**Player dropdown slicer** → active career years shown (e.g., AM Rahane: 2013–2018)

| KPI Card | Description |
|----------|-------------|
| Catches | Total catches taken |
| Catches as WK | Wicketkeeper catches |
| Catches as Fielder | Outfield catches |
| Total Innings | Innings in the field |
| Dismissals/Inning | Average dismissals per innings |
| Number of Matches | Total matches fielded |
| Match Days | Best match-day fielding stat |
| Number of Stumpings | Stumpings by WK |

---

### Page 4 — Batting Data Analytics (Comparative — Top 5 Batsmen)

**Head-to-head comparison across India's top 5 ODI batsmen:**
SR Tendulkar | V Kohli | SC Ganguly | R Dravid | M Azharuddin

| Visual | Metric Compared |
|--------|----------------|
| Runs by Player (bar) | SR Tendulkar 2.0K → V Kohli 1.8K → SC Ganguly 1.3K → R Dravid 1.3K → M Azharuddin 1.1K |
| Balls Faced by Player (bar) | SR Tendulkar 2.6K → V Kohli 2.0K → R Dravid 2.0K → SC Ganguly 1.7K → M Azharuddin 1.6K |
| Strike Rate by Player (bar) | V Kohli 90 → SC Ganguly 77 → SR Tendulkar 76 → M Azharuddin 70 → R Dravid 67 |
| 6s by Player (bar) | SC Ganguly 31 → V Kohli 29 → SR Tendulkar 15 → M Azharuddin 5 → R Dravid 4 |

**Key visible insight:** Kohli has the highest strike rate (90) despite not having the most runs — showing his efficiency relative to balls faced. Ganguly leads in sixes (31), reflecting his aggressive batting style. Dravid has the lowest strike rate (67) consistent with his anchor role.

---

## 🎯 Learning Goals (Course Objectives)

This project was built to develop four core Power BI competencies using real sports data:

| Goal | What Was Practiced |
|------|-------------------|
| **Goal 1 — Real-World Data Extraction** | Connected to ESPN cricket data sources, cleaned raw data using Power Query |
| **Goal 2 — Effective Data Modeling** | Structured relationships between player, match, and performance tables for accurate slicing |
| **Goal 3 — DAX Measure Engineering** | Calculated sports-specific KPIs — batting average, economy rate, strike rate, dismissals per inning |
| **Goal 4 — Interactive Dashboard Design** | Built player-selector slicers, KPI card layouts, comparative bar charts with a consistent sports-themed UI |

---

## 🛠️ Technical Implementation

### Data Source
- **ESPN Cricinfo** — ODI cricket player performance data
- Connected via Power Query (static export or web connector)

### Power Query Cleaning Steps
- Promoted headers and corrected column data types
- Handled null values in bowling/fielding columns for specialist batsmen
- Standardized player name formats across batting, bowling, and fielding tables
- Removed duplicate entries and irrelevant columns
- Normalized career year range into a display field for the slicer header

### Data Model
```
dim_Players ──────────────────────────────────────┐
  (player_id, player_name, career_years)           │
                                                   ▼
                              ┌──────────────────────────────┐
                              │      fact_Batting            │
                              │  (runs, innings, average...) │
                              └──────────────────────────────┘
                              ┌──────────────────────────────┐
                              │      fact_Bowling            │
                              │  (wickets, economy, overs...)│
                              └──────────────────────────────┘
                              ┌──────────────────────────────┐
                              │      fact_Fielding           │
                              │  (catches, stumpings...)     │
                              └──────────────────────────────┘
```

### DAX Measures Built

```dax
-- Batting Average
Batting Average =
DIVIDE(SUM(fact_Batting[Runs]), SUM(fact_Batting[Dismissals]), 0)

-- Strike Rate (Batting)
Batting Strike Rate =
DIVIDE(SUM(fact_Batting[Runs]), SUM(fact_Batting[Balls_Faced]), 0) * 100

-- Bowling Economy
Economy Rate =
DIVIDE(SUM(fact_Bowling[Runs_Conceded]), SUM(fact_Bowling[Overs]), 0)

-- Bowling Strike Rate
Bowling Strike Rate =
DIVIDE(SUM(fact_Bowling[Balls_Bowled]), SUM(fact_Bowling[Wickets]), 0)

-- Dismissals Per Inning (Fielding)
Dismissals Per Inning =
DIVIDE(SUM(fact_Fielding[Total_Dismissals]), SUM(fact_Fielding[Total_Innings]), 0)

-- Career Year Range (for slicer display label)
Career Range =
MIN(fact_Batting[Year]) & " - " & MAX(fact_Batting[Year])
```

---

## 📁 Project Structure

```
india-cricket-analytics-dashboard/
│
├── Cricket_Analytics_Dashboard.pbix     # Main Power BI file
│
├── datasets/
│   ├── batting_data.csv                 # ODI batting records
│   ├── bowling_data.csv                 # ODI bowling records
│   └── fielding_data.csv                # ODI fielding records
│
├── assets/
│   ├── page1_batting_individual.png
│   ├── page2_bowling_individual.png
│   ├── page3_fielding_individual.png
│   └── page4_batting_comparative.png
│
└── README.md
```

---

## ▶️ How to Use

**1. Clone the repository**
```bash
git clone https://github.com/SandipMandal-52/india-cricket-analytics-dashboard.git
```

**2. Open in Power BI Desktop**
```
Open Cricket_Analytics_Dashboard.pbix
Refresh data source if prompted — point to your local /datasets/ folder
```

**3. Interact with the dashboard**
```
Page 1 — Batting:  Select any player from the dropdown to see full batting career profile
Page 2 — Bowling:  Select any player to see bowling figures and career span
Page 3 — Fielding: Select any player to see fielding stats and dismissal breakdown
Page 4 — Top 5:    Comparative view of India's top 5 ODI batsmen across 4 metrics
                   Use the Top 5 Player slicer to filter to specific players
```

---

## 💡 Key Insights from the Dashboard

**Batting (Comparative)**
- SR Tendulkar leads in total runs (2.0K) and balls faced (2.6K) — highest volume player in the dataset
- V Kohli leads in strike rate (90) — most efficient scorer among the top 5
- SC Ganguly leads in sixes (31) — most aggressive in boundary hitting
- R Dravid has the lowest strike rate (67) and fewest sixes (4) — consistent with his defensive anchor role

**Bowling**
- The individual player slicer reveals career-span filtering automatically, preventing cross-era comparison errors
- Economy and Average together give a complete picture — a low average bowler with high economy may still be costly

**Fielding**
- Dismissals/Inning is the most meaningful fielding efficiency metric — total catches alone favors players with more matches
- WK catches vs Fielder catches distinction enables accurate wicketkeeper vs outfield performance comparison

---

## 🔮 Next Steps

- **Add Test and T20I format tabs** — separate pages for all three international formats with format slicer
- **All-rounder composite score** — DAX measure combining batting + bowling + fielding into a single player performance index
- **Team-level analysis** — Win/loss records correlated with individual player performance
- **Live ESPN data refresh** — Replace static CSV with live web connector for real-time data updates

---

## 🛠️ Tools & Environment

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power_Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)

| Tool | Role |
|------|------|
| Power BI Desktop | Dashboard design, KPI cards, slicers, comparative visuals |
| Power Query | ESPN data extraction, cleaning, and transformation |
| DAX | Sports KPI calculations — average, strike rate, economy, dismissals/inning |
| ESPN Cricinfo | Source data for ODI batting, bowling, and fielding records |

---

## 📝 About This Project

This project was built as part of a **Sports Analytics with Power BI** course using ESPN cricket data. The dataset and course structure were provided externally. The data modeling, DAX measure design, dashboard layout, and insight analysis were built independently through the course.

---

## 👤 Author

**Sandip Mandal** — EDP Analyst | Aspiring Data Analyst
📍 Nagpur, Maharashtra, India
🔗 [LinkedIn](https://linkedin.com/in/sandipmandal52) | [GitHub](https://github.com/SandipMandal-52) | 📧 sandipmandalcv@gmail.com

---

*From ESPN data to player intelligence — cricket analytics built with Power BI.*

---
