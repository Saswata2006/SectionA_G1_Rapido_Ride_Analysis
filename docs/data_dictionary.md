# Data Dictionary: Bangalore Rapido Ride Analysis

This document provides a comprehensive description of the attributes present in the cleaned Bangalore Rapido Ride dataset (`cleaned_rides.csv`). The dataset contains information about ride requests, fares, locations, and seasonal features.

---

## 1. Core Attributes (Raw Data)

These attributes were extracted directly from the raw dataset and standardized during the cleaning process.

| Attribute | Data Type | Description |
| :--- | :--- | :--- |
| `ride_id` | String | Unique identifier for each ride request (e.g., RD...). |
| `services` | Categorical | The type of Rapido service used (mapped to `bike`, `bike_lite`, `auto`, `cab_economy`, `parcel`). |
| `date` | DateTime | The date on which the ride was requested (YYYY-MM-DD). |
| `time` | String | The raw time of the ride request. |
| `ride_status` | Categorical | The final status of the ride (`completed` or `cancelled`). |
| `source` | String | The pickup location/area for the ride (standardized). |
| `destination` | String | The drop-off location/area for the ride (standardized). |
| `duration` | Integer | The total duration of the ride in minutes. |
| `distance` | Float | The total distance traveled in kilometers (km). |
| `ride_charge` | Float | The base fare for the ride (0 for cancelled rides). |
| `misc_charge` | Float | Additional charges like tolls or peak pricing (0 for cancelled rides). |
| `total_fare` | Float | The total amount paid for the ride (`ride_charge` + `misc_charge`). |
| `payment_method`| Categorical | The method of payment used (`paytm`, `gpay`, `amazon_pay`, `qr_scan` or `not applicable`). |

---

## 2. Derived Attributes (Feature Engineering)

These attributes were created during the data transformation phase to facilitate temporal and business analysis.

| Attribute | Data Type | Description |
| :--- | :--- | :--- |
| `time_stamp` | DateTime | Combined date and time object for precise time-series analysis. |
| `year` | Integer | The year extracted from the `date`. |
| `month` | Integer | The month (1-12) extracted from the `date`. |
| `day_of_week` | String | Name of the day (e.g., `Monday`, `Tuesday`). |
| `hour` | Integer | The hour of the day (0-23) when the ride was requested. |
| `peak_hour` | Integer (0/1)| Flag indicating if the ride was during peak hours (8-10 AM or 5-7 PM). |
| `is_weekend` | Integer (0/1)| Flag indicating if the ride was requested on a Saturday or Sunday. |
| `completed` | Integer (0/1)| Binary version of `ride_status` (1 = Completed, 0 = Cancelled). |

---

## 3. Performance & Efficiency Metrics

Calculated metrics used to measure operational efficiency and pricing trends.

| Attribute | Data Type | Description |
| :--- | :--- | :--- |
| `avg_speed_kmh` | Float | Average speed of the ride calculated as `distance / (duration/60)`. |
| `fare_per_km` | Float | The cost effectiveness calculated as `total_fare / distance`. |
| `ride_efficiency`| Float | A ratio of distance to time, representing travel fluidity. |

---

## 4. Key Mappings & Logic

### Ride Status Logic
- **Completed**: Rides that were successfully finished. All fare components are present and valid.
- **Cancelled**: Rides that were aborted. For these records, `ride_charge`, `misc_charge`, and `total_fare` have been standardized to `0` to avoid null-influence on financial aggregations.

### Anomaly Filtering
- Rides marked as `completed` but with a `distance` of 0 or greater than 100km are removed.
- Rides with `duration` of 0 or less are removed.

### Peak Hour Definition
The `peak_hour` flag is set to `1` for the following windows:
- **Morning Peak**: 08:00 AM - 10:59 AM
- **Evening Peak**: 05:00 PM - 07:59 PM
