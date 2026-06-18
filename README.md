# Advertising Performance & User Engagement Analytics Dashboard

##  Project Overview
This repository contains a comprehensive **Power BI Analytics Dashboard** that monitors, visualizes, and analyzes advertising campaign performance and user engagement metrics across multiple digital platforms (Facebook and Instagram). By blending campaign budgets, targeted ad configurations, user demographics, and multi-tiered behavioral event tracking (impressions, clicks, likes, shares, comments, purchases), this dashboard provides actionable insights into marketing ROI, audience engagement patterns, and optimal budget allocation strategies.

---

##  Data Architecture & Schema
The dashboard is powered by a relational star/snowflake schema consisting of 4 primary datasets with a total of **400,000 tracked user events**:

### 1. `campaigns.csv` (Dimension)
Contains high-level marketing campaign parameters, scheduling, and budgets.
* `campaign_id`: Unique identifier for each campaign.
* `name`: Campaign name/descriptor (e.g., Summer, Winter, Launch, Q3).
* `start_date` & `end_date`: Operational timeline.
* `duration_days`: Lifespan of the campaign.
* `total_budget`: Financial allocation in USD.

### 2. `ads.csv` (Dimension)
Defines the specific creative attributes and platform placement of individual advertisements.
* `ad_id`: Unique identifier for each advertisement creative.
* `campaign_id`: Foreign key referencing the parent campaign.
* `ad_platform`: Digital platform where the ad ran (`Facebook`, `Instagram`).
* `ad_type`: Visual format of the advertisement (`Image`, `Video`, `Stories`, `Carousel`).
* `target_gender`, `target_age_group`, `target_interests`: Pre-defined demographic parameters used for audience targeting.

### 3. `users.csv` (Dimension)
Contains anonymized profile metadata for users who interacted with the advertisements.
* `user_id`: Unique identifier for each user.
* `user_gender` & `user_age`: Actual demographic markers of the consumer.
* `age_group`: Categorical age brackets (e.g., 18-24, 25-34).
* `country` & `location`: Geographic details.
* `interests`: Extracted personal preferences and tags (e.g., fitness, technology, gaming).

### 4. `ad_events.csv` (Fact Table)
The core transactional table tracking granular, real-time user touchpoints.
* `event_id`: Unique transaction hash for the event.
* `ad_id` & `user_id`: Dimensional foreign keys linking back to the creative and consumer profile.
* `timestamp`: Exact date and time of interaction.
* `day_of_week` & `time_of_day`: Derived temporal attributes for pattern analysis.
* `event_type`: The explicit user action (`Impression`, `Click`, `Like`, `Share`, `Comment`, `Purchase`).

---

##  Key Metrics & DAX KPI Framework
The dashboard implements advanced Data Analysis Expressions (DAX) to evaluate full-funnel efficiency and campaign health:

* **Total Impressions**: `COUNTROWS(FILTER(ad_events, ad_events[event_type] == "Impression"))`
* **Total Clicks**: `COUNTROWS(FILTER(ad_events, ad_events[event_type] == "Click"))`
* **Total Conversions (Purchases)**: `COUNTROWS(FILTER(ad_events, ad_events[event_type] == "Purchase"))`
* **Click-Through Rate (CTR)**: `[Total Clicks] / [Total Impressions]`
* **Conversion Rate (CR)**: `[Total Conversions] / [Total Clicks]`
* **Cost Per Click (CPC)**: `[Total Allocated Budget] / [Total Clicks]`
* **Cost Per Conversion / Acquisition (CPA)**: `[Total Allocated Budget] / [Total Conversions]`
* **Social Engagement Index**: Aggregated sum of non-conversion actions (`Likes` + `Shares` + `Comments`).

---

##  Dashboard Features & Views
The Power BI report is structured into intuitive reporting tabs designed to cater to both executive and operational workflows:

1. **Executive Overview Dashboard**: High-level KPIs representing total spend, overall CTR, total purchase conversions, and platform-level performance breakdown.
2. **Campaign & Ad Format Analysis**: Comparative matrix mapping campaign duration and budget against performance metrics. Visual cross-filtering allows identification of high-performing ad formats (e.g., Carousel vs. Video) across Facebook and Instagram.
3. **Audience Demographics & Targeting Matrix**: Compares actual user demographics (`user_gender`, `user_age`) against intended campaign targeting vectors (`target_gender`, `target_age_group`) to detect audience drift and targeting optimization opportunities.
4. **Temporal & Behavioral Patterns**: Line charts and heatmaps tracking user engagement by `day_of_week` and `time_of_day` to optimize real-time ad bidding schedules.

---

## 🛠️ Setup and Installation Instructions
To open and interact with this project locally, follow these steps:

1. **Clone the Repository**:
   ```bash
   git clone (https://github.com/BhavnaDhanrajThombre/Meta_Ad_Dashboard.git)
   cd ad-performance-analytics

Also view dashboard directly 

    (https://app.powerbi.com/groups/me/reports/de7b1fdf-6c19-4a93-aa49-83490bbc83ff/bd684bb9b704d96a6020?experience=power-bi)


Verify Data Assets: Ensure campaigns.csv, ads.csv, users.csv, and ad_events.csv are stored within the root or /data directory.

Open in Power BI:

Launch Power BI Desktop.

Open the .pbix project file.

Fix Data Source Paths (If broken):

Go to Home > Transform Data > Data source settings.

Change the Source paths to point to your local path where the CSV files are stored.

Click Refresh to reload the 400,000 rows into the local data model.

 Key Business Insights Extracted
Platform Efficiency: Instagram generally yields a higher Social Engagement Index (Likes/Shares), whereas Facebook drives a higher absolute Purchase Conversion Rate for older demographics.

Format Dominance: Carousel and Video formats show a significantly stronger CTR compared to standard static Image ads across both platforms.

Targeting Alignment: Campaigns with explicit interest matching (e.g., targeting 'fitness' to users with 'fitness' tags) display up to a 3x higher Conversion Rate than broad-audience configurations.


