# Data

## Dataset — Airbnb San Francisco Bay Area Properties

This project uses three data files covering Airbnb properties in the San Francisco Bay Area.

---

### abb.csv — Training Data

**5,247 properties** with known attrition outcomes. Used to train and cross-validate both models.

| Column | Description |
|---|---|
| `propertyid` | Unique property identifier |
| `latitude` | Geographic latitude (San Francisco Bay Area) |
| `longitude` | Geographic longitude |
| `bedrooms` | Number of bedrooms |
| `bathrooms` | Number of bathrooms (can be fractional) |
| `averagedailyrateusd` | Average nightly rate in USD |
| `rating_overall` | Overall guest satisfaction score (0–100) |
| `rating_communication` | Guest rating for host communication |
| `rating_accuracy` | Guest rating for listing accuracy |
| `rating_cleanliness` | Guest rating for cleanliness |
| `rating_checkin` | Guest rating for check-in experience |
| `rating_location` | Guest rating for property location |
| `rating_value` | Guest rating for value for money |
| `reservationdays1–12` | Monthly reservation days for each of 12 calendar months |
| `nmon` | Months the property has been listed on Airbnb |
| `attrition` | **Target variable** — did the host leave? (1 = left, 0 = stayed) |

---

### abb_new.csv — Prediction Data

**1,312 new-market properties** with no attrition label. The model generates a gift recommendation for each one. Same variable structure as abb.csv, without the `attrition` column.

---

### abb_new_updated.csv — Output File

The final deliverable. Contains all 1,312 prediction properties with an additional `gift` column (0 or 1) indicating the model's recommendation.

| Column | Description |
|---|---|
| All columns from abb_new.csv | Original property variables |
| `gift` | **1 = send $1,000 retention gift · 0 = no intervention needed** |

**How to use it:**
- Filter `gift = 1` to get the 355 properties that should receive the gift
- Sort by `averagedailyrateusd` descending to see highest-value properties first
- Cross-reference `propertyid` with your CRM or property management system to action the decisions

---

## Data Source

The dataset was provided as part of the MGMT 52610 course at Purdue University. It covers Airbnb properties in the San Francisco Bay Area. The data files are included in this repository for reproducibility.

## Feature Engineering

Three additional features are computed in the notebook before modelling:

| Derived Feature | Calculation |
|---|---|
| `total_reservation_days` | Sum of reservationdays1 through reservationdays12 |
| `annual_revenue` | averagedailyrateusd × total_reservation_days |
| `annual_profit` | annual_revenue × 0.15 (Airbnb's 15% margin) |
| `average_rating` | Mean of the 6 component rating scores |
| `p_star_cutoff` | $1,000 / annual_profit — property-specific intervention threshold |
