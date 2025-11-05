# TravelTide Metadata

This database contains customer, session, flight, and hotel booking information from **TravelTide**, an online travel startup. It enables exploration of how travelers search, book, and combine flights and hotels, revealing patterns in discount sensitivity, cancellation behavior, baggage needs, and trip length.

## Table of Contents

1. [Overview](#overview)
2. [Database Schema](#database-schema)
3. [Table Descriptions](#table-descriptions)  
   - [Table: `flights`](#table-flights)  
   - [Table: `hotels`](#table-hotels)  
   - [Table: `sessions`](#table-sessions)  
   - [Table: `users`](#table-users)  

---

## Overview

| Key | Value |
| --- | --- |
| **Total Tables** | 4 (`users`, `sessions`, `flights`, `hotels`) |
| **Data Source** | Provided by Masterschool |
| **Primary Use Cases** | Data Cleaning, Feature Engineering, Customer Segmentation, Business Strategy, Visualization & Reporting |
| **Database Access** | `postgres://Test:bQNxVzJL4g6u@ep-noisy-flower-846766.us-east-2.aws.neon.tech/TravelTide?sslmode=require` |

---

## Database Schema

![TravelTide Database Schema](/data/traveltide-db-schema.png)

*Entity‑relationship diagram showing the four tables (`users`, `sessions`, `flights`, `hotels`) and their relationships.*

---

## Table Descriptions

### Table: `flights`

| Key | Value |
| --- | --- |
| **Total Rows** | 1,901,038 |
| **Total Columns** | 13 |
| **Grain** | Each row represents a single booked flight within a trip |
| **Primary Key** | `trip_id` |
| **Foreign Keys** | Referenced by `sessions.trip_id` |

The following columns capture details of each booked flight, including origin, destination, airline, and fare information.

| Column Name | Data Type | Description |
| --- | --- | --- |
| **trip_id** | `string` _(PK)_ | Unique trip identifier |
| **origin_airport** | `string` | Departure airport |
| **destination** | `string` | Destination city |
| **destination_airport** | `string` | Destination airport |
| **seats** | `int` | Number of seats booked |
| **return_flight_booked** | `boolean` | Whether a return flight was booked |
| **departure_time** | `timestamp` | Departure time |
| **return_time** | `timestamp` | Return time |
| **checked_bags** | `int` | Number of checked bags |
| **trip_airline** | `string` | Airline used |
| **destination_airport_lat** | `float` | Latitude of destination airport |
| **destination_airport_lon** | `float` | Longitude of destination airport |
| **base_fare_usd** | `float` | Pre‑discount airfare |

---

### Table: `hotels`

| Key | Value |
| --- | --- |
| **Total Rows** | 1,918,617 |
| **Total Columns** | 7 |
| **Grain** | Each row represents a single booked hotel stay within a trip |
| **Primary Key** | `trip_id` |
| **Foreign Keys** | Referenced by `sessions.trip_id` |

The following columns capture details of each booked hotel stay, including brand, duration, and pricing.

| Column Name | Data Type | Description |
| --- | --- | --- |
| **trip_id** | `string` _(PK)_ | Unique trip identifier |
| **hotel_name** | `string` | Hotel brand name |
| **nights** | `int` | Number of nights stayed |
| **rooms** | `int` | Number of rooms booked |
| **check_in_time** | `timestamp` | Hotel check‑in time |
| **check_out_time** | `timestamp` | Hotel check‑out time |
| **hotel_per_room_usd** | `float` | Pre‑discount nightly rate per room |

---

### Table: `sessions`

| Key | Value |
| --- | --- |
| **Total Rows** | 5,408,063 |
| **Total Columns** | 13 |
| **Grain** | Each row represents a single browsing session by a user |
| **Primary Key** | `session_id` |
| **Foreign Keys** | `user_id` references `users.user_id` |
| | `trip_id` references `flights.trip_id` / `hotels.trip_id` |

The following columns capture details of each browsing session, including timing, discounts, bookings, and cancellations.

| Column Name | Data Type | Description |
| --- | --- | --- |
| **session_id** | `string` _(PK)_ | Unique browsing session identifier |
| **user_id** | `int` _(FK)_ | Links to `users.user_id` |
| **trip_id** | `string` _(FK)_ | Links to `flights.trip_id` / `hotels.trip_id` |
| **session_start** | `timestamp` | Session start time |
| **session_end** | `timestamp` | Session end time |
| **flight_discount** | `boolean` | Whether a flight discount was offered |
| **hotel_discount** | `boolean` | Whether a hotel discount was offered |
| **flight_discount_amount** | `float` | % off base fare |
| **hotel_discount_amount** | `float` | % off base nightly rate |
| **flight_booked** | `boolean` | Whether a flight was booked |
| **hotel_booked** | `boolean` | Whether a hotel was booked |
| **page_clicks** | `int` | Number of page clicks in session |
| **cancellation** | `boolean` | Whether the session was for cancellation |

---

### Table: `users`

| Key | Value |
| --- | --- |
| **Total Rows** | 1,020,926 |
| **Total Columns** | 11 |
| **Grain** | Each row represents a unique registered user |
| **Primary Key** | `user_id` |
| **Foreign Keys** | Referenced by `sessions.user_id` |

The following columns capture demographic and account details for each TravelTide user.

| Column Name | Data Type | Description |
| --- | --- | --- |
| **user_id** | `int` _(PK)_ | Unique user identifier |
| **birthdate** | `datetime` | User’s date of birth |
| **gender** | `string` | User gender |
| **married** | `boolean` | Marital status |
| **has_children** | `boolean` | Whether the user has children |
| **home_country** | `string` | Country of residence |
| **home_city** | `string` | City of residence |
| **home_airport** | `string` | Preferred hometown airport |
| **home_airport_lat** | `float` | Latitude of home airport |
| **home_airport_lon** | `float` | Longitude of home airport |
| **sign_up_date** | `datetime` | Date of TravelTide account creation |
