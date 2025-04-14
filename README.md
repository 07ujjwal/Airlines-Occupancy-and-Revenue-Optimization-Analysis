# Airline Occupancy Rate Analysis

## Objective

The goal of this data analysis project is to identify opportunities to increase the occupancy rate on low-performing flights, which can ultimately lead to increased profitability for the airline.

---

## Table of Contents

1. [Project Description](#project-description)
2. [Data Overview](#data-overview)
3. [Data Cleaning and Preprocessing](#data-cleaning-and-preprocessing)
4. [Analysis & Insights](#analysis--insights)
5. [Future Work](#future-work)
6. [Installation](#installation)
7. [License](#license)

---

## Project Description

This project uses SQL and Python to analyze the performance of flights, particularly focusing on identifying factors affecting the occupancy rates. By examining key metrics such as revenue, occupancy rate, and ticket bookings, the project provides insights into how the airline can optimize flight scheduling and increase profitability.

The analysis includes:

- Identifying planes with more than 100 seats.
- Analyzing ticket booking trends over time.
- Calculating average charges for different fare conditions.
- Analyzing the average occupancy rate for each aircraft.
- Estimating potential revenue increase by improving occupancy rates.

---

## Data Overview

The dataset is stored in a SQLite database (`travel.sqlite`) with the following tables:

1. **aircrafts_data**: Data about the aircrafts.
2. **airports_data**: Information about airports.
3. **boarding_passes**: Data about boarding passes issued.
4. **bookings**: Data on the bookings made by customers.
5. **flights**: Details of the flights operated.
6. **seats**: Seat details for different aircraft.
7. **ticket_flights**: Information about the tickets issued for each flight.
8. **tickets**: Information about the tickets issued.

---

## Data Cleaning and Preprocessing

The following data cleaning steps were performed:

1. **Checking for Missing Values**: The dataset was examined for missing values, and appropriate handling methods were applied.
2. **Conversion of Date Columns**: The `book_date` column in the `tickets` table was converted to a datetime format to enable time-based analysis.
3. **Join Operations**: Various joins were made between tables to extract relevant information for analysis, such as joining `ticket_flights` with `flights` to get aircraft details.

---

## Analysis & Insights

### 1. **Identifying Planes with More Than 100 Seats**

We calculated the number of seats for each aircraft and identified those with more than 100 seats.

````sql
SELECT aircraft_code, COUNT(*) as num_seats
FROM seats
GROUP BY aircraft_code
HAVING num_seats > 100
ORDER BY num_seats DESC

### 2. Ticket Bookings Over Time
The trend of the number of tickets booked over time was visualized.

```python
plt.plot(x.index, x['date'], marker='^')```

### 3. **Revenue Analysis**
The revenue earned over time was also analyzed, showing the total amount generated from bookings.

```python
plt.plot(y.index, y['total_amount'], marker='o')```


### 4. **Average Charges by Fare Conditions**
The analysis of average ticket prices per aircraft, based on fare conditions, was carried out.

```python
sns.barplot(data = df, x = 'aircraft_code', y = 'avg_amount', hue = 'fare_conditions')```


### 5. **Occupancy Rate Analysis**
We calculated the average occupancy rate for each aircraft.

```sql
SELECT a.aircraft_code, AVG(a.seats_count) as booked_seats, b.num_seats,
AVG(a.seats_count)/b.num_seats as occupancy_rate
FROM (SELECT aircraft_code, flights.flight_id, COUNT(*) as seats_count
      FROM boarding_passes
      INNER JOIN flights
      ON boarding_passes.flight_id=flights.flight_id
      GROUP BY aircraft_code, flights.flight_id) as a
INNER JOIN (SELECT aircraft_code, COUNT(*) as num_seats FROM seats
            GROUP BY aircraft_code) as b
ON a.aircraft_code = b.aircraft_code
GROUP BY a.aircraft_code```

### 6. **Potential Revenue Increase**
We estimated the potential increase in annual turnover by improving occupancy rates by 10%.

```python
occupancy_rate['Inc Total Annual Turnover'] = (total_revenue['total_revenue'] / occupancy_rate['occupancy_rate']) * occupancy_rate['Inc occupancy rate```

````

---

This `README.md` provides a clear description of the project, its objectives, installation steps, and an outline of the data analysis steps performed, along with code snippets for better understanding. You can place this file in your GitHub repository to give others a comprehensive overview of your project. ```
