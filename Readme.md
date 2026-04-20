# SectionA_G1_Rapido_Ride_Analysis
Newton School of Technology | Data Visualization & Analytics | Capstone 2

## Project Overview
| Field | Details |
| :--- | :--- |
| **Project Title** | Rapido Ride Analysis: Understanding Demand, Cancellations & Revenue in Bangalore |
| **Sector** | Transportation / Ride-Hailing |
| **Team ID** | G-1 |
| **Section** | A |
| **Faculty Mentor** | To be assigned |
| **Institute** | Newton School of Technology |
| **Submission Date** | April 28, 2026 |

## Team Members
| Role | Name | GitHub Username |
| :--- | :--- | :--- |
| **Project Lead** | Nipun Patlori | nipun1803 |
| **Data Lead** | Aniket Pathak | Aniket-bit7 |
| **ETL Lead** | Vedant Madne | vedant-valid |
| **Analysis Lead** | Akshay Y | Akkii71 |
| **Visualization Lead** | Saswataduity Bhuin | — |
| **Strategy Lead** | Sayan Bhattacharya | — |
| **PPT and Quality Lead** | Narendra Singh | codernsingh |

## Business Problem
Ride-hailing platforms like Rapido face a persistent operational challenge: balancing rider demand with driver supply across different service types and time windows. High cancellation rates erode customer trust, reduce revenue, and signal inefficiencies in resource allocation. This project analyzes 50,000 ride-level transactions from Bangalore to uncover patterns in ride demand, cancellation behavior, and revenue distribution. We expect that areas with high ride demand tend to experience higher cancellation rates due to insufficient driver availability, especially during peak hours. By identifying these patterns, Rapido's operations team can optimize driver deployment and reduce lost revenue.

### Core Business Question
How do service type, time of day, and location influence ride cancellation rates and revenue — and where should Rapido focus driver allocation to minimize cancellations during peak demand?

### Decision Supported
This analysis enables Rapido's operations team to identify high-cancellation zones and peak-hour bottlenecks, allowing them to implement targeted driver incentives, surge allocation strategies, and service-specific supply planning to improve completion rates and maximize revenue.

## Data Cleaning & ETL Methodology
The cleaning process was designed to transform raw ride-level transactional data into a high-quality, analysis-ready dataset focused on **demand, cancellation, and revenue insights**.

Key steps included:
1. **Missing Value Handling (Business-Logic Driven)**: Cancelled rides naturally have no fare or payment data. Instead of dropping these rows, we set `ride_charge`, `misc_charge`, and `total_fare` to `0` and `payment_method` to `"Not Applicable"`. For completed rides, `total_fare` was recalculated as `ride_charge + misc_charge` to fix inconsistencies.
2. **Datetime Conversion**: The `date` and `time` columns were converted from raw strings to proper `datetime64` types, enabling time-series analysis, hour-level grouping, and day-of-week segmentation.
3. **Duplicate Removal**: Duplicate records were removed by deduplicating on `ride_id`, keeping only the first occurrence.
4. **Categorical Standardization**: All text columns (`services`, `ride_status`, `payment_method`, `source`, `destination`) were lowercased and stripped. Service names were normalized (e.g., `bike lite` → `bike_lite`, `cab economy` → `cab_economy`).
5. **Anomaly Filtering**: Computed `avg_speed_kmh` and dropped completed rides with physically implausible speeds (> 120 km/h), zero distance, or zero duration — removing 2,003 anomalous records.
6. **Feature Engineering**: Created 13 new analytical columns including `revenue`, `year`, `month`, `day`, `hour`, `day_of_week`, `peak_hour`, `completed`, `is_weekend`, `avg_speed_kmh`, `fare_per_km`, and `ride_efficiency`.

For the full implementation, see `notebooks/01_extraction.ipynb` and `notebooks/02_cleaning.ipynb`.

## Dataset
| Attribute | Details |
| :--- | :--- |
| **Source Name** | Bangalore Rapido Ride Data (Simulated) |
| **Row Count (Raw)** | 50,000 |
| **Row Count (Cleaned)** | 47,997 |
| **Column Count (Raw)** | 13 |
| **Column Count (Cleaned)** | 26 |
| **Time Period Covered** | June 2024 to August 2024 |
| **Format** | CSV |

### Key Columns Used
| Column Name | Description | Role in Analysis |
| :--- | :--- | :--- |
| `ride_status` | Whether the ride was completed or cancelled | **Target Variable** (KPI 1 — Cancellation Rate) |
| `total_fare` / `revenue` | Total fare charged for a completed ride | **Target Variable** (KPI 2 — Revenue) |
| `services` | Type of Rapido service (auto, bike, bike_lite, cab_economy, parcel) | Segmentation / Filter |
| `hour` | Hour of the day extracted from timestamp | Peak demand identification |
| `source` / `destination` | Pickup and drop-off locations in Bangalore | Geographic demand analysis |
| `duration` | Ride duration in minutes | Operational efficiency |
| `distance` | Ride distance in kilometres | Fare and efficiency analysis |
| `payment_method` | Payment channel (GPay, Paytm, Amazon Pay, QR scan) | Customer behavior analysis |
| `peak_hour` | Binary flag: 1 if ride occurred during peak hours (8–10 AM, 5–7 PM) | Demand pattern analysis |

## KPI Framework
| KPI | Definition | Formula / Computation |
| :--- | :--- | :--- |
| **Ride Cancellation Rate (%)** | Percentage of rides cancelled out of total rides booked | `(Count of rides with ride_status = 'cancelled') / (Total Rides) * 100` |
| **Average Revenue per Ride (₹)** | Average fare earned per completed ride | `Sum(revenue where ride_status = 'completed') / Count(completed rides)` |

## Tableau Dashboard
*Detailed Tableau link and screenshots will be added in Phase 2.*
- **Dashboard URL**: [To be added]
- **Executive View**: Summary of cancellation rates across service types, peak vs. off-peak hours, and top revenue-generating zones.
- **Operational View**: Detailed breakdown of ride volume, cancellation hotspots, and fare distribution by service type and time window.

## Repository Structure
```
SectionA_G1_Rapido_Ride_Analysis/
|-- README.md
|-- data/
|   |-- raw/                         # Original dataset (rides_data.csv)
|   `-- processed/                   # Cleaned output (cleaned_rides.csv)
|-- notebooks/
|   |-- 01_extraction.ipynb          # Data loading and initial inspection
|   |-- 02_cleaning.ipynb            # Cleaning, transformation & feature engineering
|   |-- 03_eda.ipynb
|   |-- 04_statistical_analysis.ipynb
|   `-- 05_final_load_prep.ipynb
|-- scripts/
|   `-- etl_pipeline.py
|-- tableau/
|   |-- screenshots/
|   `-- dashboard_links.md
|-- reports/
|   |-- project_report_template.md
|   `-- presentation_outline.md
|-- docs/
|   `-- data_dictionary.md
|-- DVA-oriented-Resume/
`-- DVA-focused-Portfolio/
```

## Tech Stack
| Tool | Purpose |
| :--- | :--- |
| **Python** | ETL, Cleaning, and Statistical Analysis |
| **Pandas** | Data manipulation and transformation |
| **Tableau Public** | Dashboard design and visualization |
| **GitHub** | Version control and collaboration |

## Evaluation Rubric
| Area | Marks | Focus |
| :--- | :--- | :--- |
| **Problem Framing** | 10 | Is the business question clear and well-scoped? |
| **Data Quality and ETL** | 15 | Is the cleaning pipeline thorough and documented? |
| **Analysis Depth** | 25 | Are statistical methods applied correctly with insight? |
| **Dashboard and Visualization** | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| **Business Recommendations** | 20 | Are insights actionable and well-reasoned? |
| **Storytelling and Clarity** | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

## Submission Checklist
### GitHub Repository
- [x] Public repository created with the correct naming convention (`SectionName_TeamID_ProjectName`)
- [x] All notebooks committed in `.ipynb` format
- [x] `data/raw/` contains the original, unedited dataset
- [X] `data/processed/` contains the cleaned pipeline output
- [ ] `tableau/screenshots/` contains dashboard screenshots
- [ ] `tableau/dashboard_links.md` contains the Tableau Public URL
- [ ] `docs/data_dictionary.md` is complete
- [x] `README.md` explains the project, dataset, and team
- [x] All members have visible commits and pull requests

### Tableau Dashboard
- [ ] Published on Tableau Public and accessible via public URL
- [ ] At least one interactive filter included
- [ ] Dashboard directly addresses the business problem

### Project Report
- [ ] Final report exported as PDF into `reports/`
- [ ] Cover page, executive summary, sector context, problem statement
- [ ] Data description, cleaning methodology, KPI framework
- [ ] EDA with written insights, statistical analysis results
- [ ] Dashboard screenshots and explanation
- [ ] 8-12 key insights in decision language
- [ ] 3-5 actionable recommendations with impact estimates

### Presentation Deck
- [ ] Final presentation exported as PDF into `reports/`
- [ ] Title slide through recommendations, impact, limitations, and next steps

### Individual Assets
- [ ] DVA-oriented resume updated to include this capstone
- [ ] Portfolio link or project case study added

## Contribution Matrix
| Team Member | Dataset & Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT & Viva |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Nipun Patlori** | Support | Owner | Owner | Support | Support | Support | Support |
| **Aniket Pathak** | Owner | Support | Support | Owner | Owner | Support | Support |
| **Vedant Madne** | Support | Support | Support | Support | Support | Support | Owner |
| **Akshay Y** | Support | Support | Support | Owner | Owner | Support | Support |
| **Saswataduity Bhuin** | Support | Support | Support | Support | Owner | Support | Support |
| **Sayan Bhattacharya** | Support | Support | Support | Support | Support | Support | Support |
| **Narendra Singh** | Support | Support | Support | Support | Support | Support | Owner |

**Declaration:** We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts.

**Team Lead Name:** Nipun Patlori  
**Date:** April 20, 2026

## Academic Integrity
All analysis, code, and recommendations in this repository are the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.
