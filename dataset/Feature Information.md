# **Features For Data Visualization**

## Table: `trips`

### Columns:

**Original Features**

| Column Name            | Type                         | Description                                                                 |
|------------------------|------------------------------|-----------------------------------------------------------------------------|
| `vehicle_id`           | Foreign Key (`motorcycle_profile.vehicle_id`) | References the vehicle associated with the trip.           |
| `trip_id`              | Unique Key                   | Unique identifier for the trip.                                             |
| `avg_speed`            | FLOAT                        | Average speed of the motorcycle during the trip.                            |
| `duration`             | FLOAT (minutes)            | Total duration of the trip in minutes.                                        |
| `total_mileage`        | FLOAT (km)                   | Total distance covered during the trip in kilometers.                       |
| `end_point`            | STRING(lat , lon ) | Latitude and longitude of the ending point of the trip.                               | 
| `start_trip_timestamp` | TIMESTAMP                    | Timestamp marking the start of the trip.                                    |

**New Features**

| Column Name            | Type                         | Description                                                                                      | 	How It Was Derived                                                 |
|------------------------|------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| `start_trip_day`       | OBJECT                       | The day of the week when the trip started (e.g., Sunday, Monday, etc)                            | Extracted using .dt.day_name() from the start_trip_timestamp column.  |
| `start_trip_day_cat`   | OBJECT                       | Categorizes the day into Weekdays (Monday–Friday) or Weekend (Saturday–Sunday).                  | Applied .apply() with a lambda function to classify day names.        |
| `start_trip_time_cat`  | OBJECT                       | Time of day when the trip started, categorized into Midnight, Morning, Noon, Evening, and Night. | Created using pd.cut() on the hour value extracted from start_trip_timestamp with predefined time bins. |
| `distance_bins`        | OBJECT                       | Categorizes the total distance (in kilometers) into groups: <3 km, 3–5 km, 6–10 km, 11–20 km, >20 km. | Created using pd.cut() on total_mileage with defined distance bins. |
| `duration_bins`        | OBJECT                       | (object)	Categorizes the trip duration (in minutes) into: <15 minutes, 16–30 minutes, 31–60 minutes, 61–90 minutes, >90 minutes. | Created using pd.cut() on duration with defined duration bins. |


## Table: `user_profile`

### Columns:

**Original Features**

| Column Name           | Type     | Description                                                                 |
|-----------------------|----------|-----------------------------------------------------------------------------|
| `user_id`             | Primary Key | Unique identifier for each user. Also used to join with `motorcycle_profile.borrower_id`. |
| `status`              | INTEGER  | Current status of the user (e.g., 16 = Normal Active, 18 = Late Active, 20 = Inactive, 21 = Default Active, 22 = Paid Off).                        |
| `date_of_birth`       | DATE     | Date of birth of the user.                                                  |
| `gender`              | INTEGER  | Gender of the user (e.g., 0 = Male, 1 = Female).               |
| `marital_status`      | INTEGER  | Marital status of the user (e.g., 0 = Single, 1 = 1 Married, 2 = 2 Divorce).    |
| `city`                | STRING   | City of residence.                                                          |
| `avg_income`          | FLOAT    | Average monthly income of the user.                                         |
| `work_experience`     | STRING   | Description of the user's work experience.       |
| `total_service_month` | INTEGER  | Total length of service in months.                                          |

**New Features**

| Column Name            | Type                         | Description                                                                                      | 	How It Was Derived                                                 |
|------------------------|------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| `loyalty_level`        | OBJECT                       | Indicates the user's loyalty level based on their total months of service. The categories are:   |  Created using pd.cut() on total_service_month with custom bins and labels. |
|                        |                              | - New User: 0–24 months                                                                          |                                                                       |
|                        |                              | - Moderately Loyal: 25–48 months                                                                 |                                                                       |
|                        |                              | - Loyal User: 49–60 months                                                                       |                                                                       |
|                        |                              |- Highly Loyal: >60 months                                                                        |                                                                       |
| `age`                  | INT                          | The calculated age of the user in full years                                          | Computed by subtracting the user's date of birth from today's date using a custom function calculate_age().        |
| `age_range`            | OBJECT                       | Groups users into age categories: <25 y.o, 25–35 y.o, 36–45 y.o, >45 y.o                         | Created using pd.cut() on the age column with predefined age bins.    |

# **Features For Modeling**

## Table: Join table `trips` and  `user_profile` and `motorcycle_profile`

### Columns:

**Original Features**

| Column Name            | Type                         | Description                                                                 |
|------------------------|------------------------------|-----------------------------------------------------------------------------|
| `vehicle_id`           | OBJECT                       | References the vehicle associated with the trip.                            |
| `trip_id`              | Unique Key                   | Unique identifier for the trip.                                             |
| `start_trip_timestamp` | TIMESTAMP                    | Timestamp marking the start of the trip.                                    |
| `duration`             | FLOAT (minutes)              | Total duration of the trip in minutes.                                      |

**New Features**

| Column Name            | Type                         | Description                                                                                      | 	How It Was Derived                                                 |
|------------------------|------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| `last_ride`            | DATETIME                     | The timestamp of the most recent trip taken by the vehicle.                                      | Calculated using .agg(last_ride=('start_trip_timestamp', 'max'))      |
| `recency`              | INT                          | The number of days since the last trip. Lower values indicate more recent activity.              | Computed as the difference in days between the day after the latest timestamp (max_day) and the most recent trip (x.max()). |
| `frequency`            | INT                          | The total number of unique trips taken by the vehicle. Indicates how often the vehicle is used.  | Calculated using nunique() on the trip_id column for each vehicle.    |
| `monetary`             | FLOAT                        | The total duration (in minutes) of all trips taken by the vehicle. Represents the cumulative value of usage. | Summed using the duration column for each vehicle.        |