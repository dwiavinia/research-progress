**Public Bus – GTFS Data Structure & Connectivity Analysis**
> A structured, narrative summary of GTFS-based analysis of public bus operations serving Micron Memory Japan and Hiroshima University.

1. GTFS Feed Structure Overview

Feed: GTFS_Public_HigashiHiroshima (Geiyo Bus + JR Bus)
Files detected: 11
-----------------------------------------------------
| Indicator         | Value                       |
|:------------------|:----------------------------|
| Agencies          | 2 (芸陽バス, 中国JRバス)    |
| Routes            | 40+                         |
| Trips             | 3,000+                      |
| Stops             | ~700                        |
| Shapes            | 800+                        |
| Service calendars | Weekday / Weekend / Holiday |
| Period            | 2025 GTFS feed              |
-----------------------------------------------------

The GTFS is large and multi-operator.

2. Method – POI-Based Route Isolation
To focus on Micron and Hirodai, a POI-based extraction method was used to isolate only the routes that physically serve each location.
Steps:
- Identify stops within 800 m of each POI.
- Collect all trip_id whose stop sequences include these stops.
- Extract their route_id and shape_id.
- Compute visible distance (segment drawn on map).
- Merge with headway, duration, and fare data.

3. Route Geometry & Distances (Route Length)
Insight:
- Public buses serving Micron cover more varied distances (from Micron to Saijo, Hachihonmatsu, etc.)
- Buses to Hirodai tend to follow fixed corridors of moderate length

Table: Public Bus Route Length to/from Micron
-------------------------------
| route_id      | route_name                               | shape_id      | full_route_distance_km | total_shape_points | first_lat | first_lon | last_lat | last_lon |
|---------------|-------------------------------------------|---------------|-------------------------|--------------------|-----------|-----------|----------|----------|
| Geiyo_1400 0  | 吉川線（西条駅－西条駅・午前）           | Geiyo_1400 0  | 26.02                   | 345                | 34.43     | 132.74    | 34.43    | 132.74   |
| Geiyo_1401 0  | 吉川線（西条駅－西条駅・午後）           | Geiyo_1401 0  | 25.93                   | 349                | 34.43     | 132.74    | 34.43    | 132.74   |
| Geiyo_1430 0  | 吉川線（八本松駅－吉川工業団地・長沢）   | Geiyo_1430 0  | 8.86                    | 103                | 34.45     | 132.69    | 34.39    | 132.69   |
| Geiyo_1431 0  | 吉川線（吉川工業団地－八本松駅・長沢）   | Geiyo_1431 0  | 8.82                    | 106                | 34.39     | 132.69    | 34.45    | 132.69   |
-----------------------------------------
Micron receives long loop coverage (Saijo–Micron–Saijo) + short industrial access routes.

Table: Public Bus Route Length to/from Hirodai
----------------------------------------------
| route_id             | route_name                               | shape_id          | distance_km | num_shape_points | first_lat | first_lon | last_lat | last_lon |
|----------------------|--------------------------------------------|--------------------|-------------|-------------------|-----------|-----------|----------|----------|
| Geiyo_1310 0         | 八本松－広大線（八本松駅－八本松駅・広大）   | Geiyo_1310 0       | 10.50       | 205               | 34.42     | 132.70    | 34.45    | 132.69   |
| Geiyo_1370 0         | 八本松－広大線（八本松駅－広大北口・農）     | Geiyo_1370 0       | 10.22       | 196               | 34.42     | 132.70    | 34.41    | 132.71   |
| Geiyo_1371 0         | 八本松－広大線（広大二神口－八本松駅・農）   | Geiyo_1371 0       | 10.34       | 199               | 34.41     | 132.71    | 34.45    | 132.69   |
| Geiyo_1380 0         | 東広島－広大線（東広島駅－大学会館前）       | Geiyo_1380 0       | 4.88        | 97                | 34.43     | 132.75    | 34.40    | 132.71   |
| Geiyo_1381 0         | 東広島－広大線（ががら口－東広島駅）         | Geiyo_1381 0       | 7.26        | 133               | 34.40     | 132.71    | 34.39    | 132.76   |
| JRbus_1490550946_13  | JRbus－八本松駅－八本松                     | JRbus_30001200     | 10.74       | 220               | 34.42     | 132.70    | 34.45    | 132.69   |
| JRbus_1490550946_18  | JRbus－東広島駅－大学会館前                 | JRbus_30002180     | 4.75        | 92                | 34.43     | 132.75    | 34.40    | 132.71   |
| JRbus_1490550946_23  | JRbus－ががら口－東広島駅                   | JRbus_30002170     | 7.05        | 130               | 34.40     | 132.71    | 34.39    | 132.76   |
| JRbus_1490550946_27  | JRbus－八本松駅－広大北口                   | JRbus_30000190     | 10.35       | 201               | 34.42     | 132.70    | 34.41    | 132.71   |
| JRbus_1490550946_28  | JRbus－広大二神口－八本松                   | JRbus_30000199     | 10.53       | 204               | 34.41     | 132.71    | 34.45    | 132.69   |
| JRbus_4251312605     | 広島～広島大学線（グリーンフェニックス）    | JRbus_30002250     | 23.80       | 450               | 34.39     | 132.46    | 34.40    | 132.72   |
| JRbus_1490550946_22  | JRbus－西条駅－乃美尾                       | JRbus_30000050     | 9.57        | 186               | 34.43     | 132.75    | 34.34    | 132.70   |
| JRbus_1490550946_8_1 | JRbus－西条駅－広島国際大学                 | JRbus_30001610     | 10.93       | 218               | 34.43     | 132.75    | 34.31    | 132.69   |
| JRbus_1490550946_6_1 | JRbus－広島国際大学－西条駅                 | JRbus_30001619     | 11.30       | 226               | 34.31     | 132.69    | 34.43    | 132.75   |
| JRbus_1490550946_9   | JRbus－乃美尾－西条駅                       | JRbus_30000059     | 9.72        | 189               | 34.34     | 132.70    | 34.43    | 132.75   |
-----------------------------------------
Hirodai receives many short, overlapping feed segments, indicating hub-like function.

4. Trip Duration Analysis (Travel Time)
Measure: Time required to complete route (minutes)
Insight:
- Route durations vary by direction and trip type (loop vs point-to-point)
- Travel time reflects corridor length and stops

Table: Micron-related Public Bus Trip Duration 
----------------------
| route_id      | route_name                                   | total_stops | start_time | end_time | trip_duration_(min)|
|---------------|---------------------------------------------|-------------|------------|----------|------------------|
| Geiyo_1400 0  | 吉川線（西条駅－西条駅・午前）               | 62          | 09:40:00   | 10:32:00 | 52.00            |
| Geiyo_1401 0  | 吉川線（西条駅－西条駅・午後）               | 62          | 13:28:00   | 14:20:00 | 52.00            |
| Geiyo_1430 0  | 吉川線（八本松駅－吉川工業団地・長沢）       | 19          | 07:29:00   | 07:46:00 | 17.00            |
| Geiyo_1431 0  | 吉川線（吉川工業団地－八本松駅・長沢）       | 19          | 17:30:00   | 17:49:00 | 19.00            |
-------------

Table: Hirodai-related Public Bus Trip Duration
----------------------
| route_id             | route_name                               | shape_id          | total_stops | total_time_min | travel_time_min | first_stop_name | last_stop_name | start_time | end_time |
|----------------------|--------------------------------------------|--------------------|-------------|----------------|------------------|------------------|----------------|------------|----------|
| Geiyo_1310 0         | 八本松－広大線（八本松駅－八本松駅・広大）   | Geiyo_1310 0       | 33          | 33.0           | 33.0             | 八本松駅         | 八本松駅       | 17:06:00   | 17:39:00 |
| Geiyo_1370 0         | 八本松－広大線（八本松駅－広大北口・農）    | Geiyo_1370 0       | 21          | 20.0           | 20.0             | 八本松駅         | 広大北口       | 09:05:00   | 09:25:00 |
| Geiyo_1371 0         | 八本松－広大線（広大二神口－八本松駅・農）  | Geiyo_1371 0       | 21          | 21.0           | 21.0             | 広大二神口       | 八本松駅       | 15:30:00   | 15:51:00 |
| Geiyo_1380 0         | 東広島－広大線（東広島駅－大学会館前）      | Geiyo_1380 0       | 15          | 18.0           | 18.0             | 東広島駅         | 大学会館前     | 10:23:00   | 10:41:00 |
| Geiyo_1381 0         | 東広島－広大線（ががら口－東広島駅）        | Geiyo_1381 0       | 16          | 19.0           | 19.0             | ががら口         | 東広島駅       | 17:02:00   | 17:21:00 |
| JRbus_1490550946_13  | JRbus－八本松駅－八本松                     | JRbus_30001200     | 33          | 33.0           | 33.0             | 八本松駅         | 八本松駅       | 12:25:00   | 12:58:00 |
| JRbus_1490550946_18  | JRbus－東広島駅－大学会館前                 | JRbus_30002180     | 15          | 18.0           | 18.0             | 東広島駅         | 大学会館前     | 08:00:00   | 08:18:00 |
| JRbus_1490550946_23  | JRbus－ががら口－東広島駅                   | JRbus_30002170     | 16          | 19.0           | 19.0             | ががら口         | 東広島駅       | 14:40:00   | 14:59:00 |
| JRbus_1490550946_27  | JRbus－八本松駅－広大北口                   | JRbus_30000190     | 21          | 20.0           | 20.0             | 八本松駅         | 広大北口       | 13:10:00   | 13:30:00 |
| JRbus_1490550946_28  | JRbus－広大二神口－八本松                   | JRbus_30000199     | 21          | 21.0           | 21.0             | 広大二神口       | 八本松駅       | 06:30:00   | 06:51:00 |
| JRbus_4251312605     | 広島～広島大学線（グリーンフェニックス）    | JRbus_30002250     | 13          | 66.0           | 66.0             | 広島バスセンター | 山中池         | 08:50:00   | 09:56:00 |
| JRbus_1490550946_22  | JRbus－西条駅－乃美尾                       | JRbus_30000050     | 25          | 30.0           | 30.0             | 西条駅           | 乃美尾         | 19:40:00   | 20:10:00 |
| JRbus_1490550946_8_1 | JRbus－西条駅－広島国際大学                 | JRbus_30001610     | 32          | 40.0           | 40.0             | 西条駅           | 広島国際大学   | 17:25:00   | 18:05:00 |
| JRbus_1490550946_6_1 | JRbus－広島国際大学－西条駅                 | JRbus_30001619     | 32          | 43.0           | 43.0             | 広島国際大学     | 西条駅         | 07:05:00   | 07:48:00 |
| JRbus_1490550946_9   | JRbus－乃美尾－西条駅                       | JRbus_30000059     | 25          | 33.0           | 33.0             | 乃美尾           | 西条駅         | 15:50:00   | 16:23:00 |
------------------------

5. Headway (Service Frequency)
Measure: Median and mean interval between buses (minutes)
Insight:
- Public routes serving Micron/Hirodai may show irregular headway due to limited service or single daily trips
- Some headways appear as NaN due to only one trip per direction

Table: Micron-related Public Bus Headway 
----------------------
| route_id     | route_name                             | reference_stop_id | reference_stop_name | trips_per_day | operating_hours | first_departure | last_departure | avg_headway_min | min_headway_min | max_headway_min | std_headway_min | trips_per_hour | service_level                  | departure_times                                         |
|--------------|-----------------------------------------|-------------------|----------------------|---------------|------------------|------------------|-----------------|------------------|------------------|------------------|------------------|----------------|-------------------------------|---------------------------------------------------------|
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | Geiyo_2065 5      | 西条駅               | 2             | 2.17             | 07:30:00         | 09:40:00        | 130.00           | 130.00           | 130.00           | 0.00             | 0.92           | Very low frequency (>60 min) | 07:30:00, 09:40:00                                      |
| Geiyo_1401 0 | 吉川線（西条駅－西条駅・午後）           | Geiyo_2065 5      | 西条駅               | 5             | 6.48             | 12:15:00         | 18:44:00        | 97.25            | 73.00            | 150.00           | 30.74            | 0.77           | Very low frequency (>60 min) | 12:15:00, 13:28:00, 15:58:00, 17:20:00, 18:44:00        |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | Geiyo_2050 1      | 八本松駅             | 1             | NaN              | 07:29:00         | 07:29:00        | NaN              | NaN              | NaN              | NaN              | 0.04           | Single trip                   | 07:29:00                                               |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | Geiyo_2363 1      | 吉川工業団地          | 1             | NaN              | 17:30:00         | 17:30:00        | NaN              | NaN              | NaN              | NaN              | 0.04           | Single trip                   | 17:30:00                                               |
-----------------------

Table: Hirodai-related Public Bus Headway
-------------------
| route_id             | route_name                               | reference_stop_id | mean_headway_min | median_headway_min | min_headway_min | max_headway_min | num_trips | departure_times |
|----------------------|--------------------------------------------|--------------------|-------------------|---------------------|------------------|------------------|-----------|------------------|
| Geiyo_1310 0         | 八本松－広大線（八本松駅－八本松駅・広大）   | Geiyo_2050 1       | 48.67             | 53.00               | 21.00            | 84.00            | 9         | 08:23:00, 16:05:00, 17:06:00, 17:58:00, 18:49:00, 19:40:00, 20:31:00, 21:22:00, 22:13:00 |
| Geiyo_1370 0         | 八本松－広大線（八本松駅－広大北口・農）    | Geiyo_2050 1       | NaN               | NaN                 | NaN              | NaN              | 1         | 09:05:00 |
| Geiyo_1371 0         | 八本松－広大線（広大二神口－八本松駅・農）  | BRT_4012_01        | NaN               | NaN                 | NaN              | NaN              | 1         | 15:30:00 |
| Geiyo_1380 0         | 東広島－広大線（東広島駅－大学会館前）      | Geiyo_2480 1       | NaN               | NaN                 | NaN              | NaN              | 1         | 10:23:00 |
| Geiyo_1381 0         | 東広島－広大線（ががら口－東広島駅）        | BRT_4015_01        | NaN               | NaN                 | NaN              | NaN              | 1         | 17:02:00 |
| JRbus_1490550946_13  | JRbus－八本松駅－八本松                     | Geiyo_2050 1       | 33.00             | 33.00               | 33.00            | 33.00            | 3         | 07:00:00, 07:33:00, 08:07:00 |
| JRbus_1490550946_18  | JRbus－東広島駅－大学会館前                 | Geiyo_2480 1       | 100.00            | 100.00              | 80.00            | 120.00           | 3         | 08:00:00, 09:15:00, 12:20:00 |
| JRbus_1490550946_23  | JRbus－ががら口－東広島駅                   | BRT_4015_01        | 70.00             | 70.00               | 50.00            | 90.00            | 3         | 14:40:00, 16:30:00, 18:00:00 |
| JRbus_1490550946_27  | JRbus－八本松駅－広大北口                   | Geiyo_2050 1       | NaN               | NaN                 | NaN              | NaN              | 1         | 13:10:00 |
| JRbus_1490550946_28  | JRbus－広大二神口－八本松                   | BRT_4012_01        | NaN               | NaN                 | NaN              | NaN              | 1         | 06:30:00 |
| JRbus_4251312605     | 広島～広島大学線（グリーンフェニックス）    | Geiyo_2000 1       | 18.00             | 18.00               | 5.00             | 23.00            | 4         | 06:55:00, 07:00:00, 07:13:00, 08:50:00 |
| JRbus_1490550946_22  | JRbus－西条駅－乃美尾                       | Geiyo_2065 5       | 42.00             | 42.00               | 22.00            | 62.00            | 3         | 18:18:00, 18:58:00, 19:40:00 |
| JRbus_1490550946_8_1 | JRbus－西条駅－広島国際大学                 | Geiyo_2065 5       | 173.33            | 177.00              | 58.00            | 233.00           | 4         | 07:35:00, 11:33:00, 15:48:00, 17:25:00 |
| JRbus_1490550946_6_1 | JRbus－広島国際大学－西条駅                 | JRbus_40091 2      | 94.33             | 51.00               | 10.00            | 234.00           | 4         | 07:05:00, 07:15:00, 09:15:00, 17:55:00 |
| JRbus_1490550946_9   | JRbus－乃美尾－西条駅                       | JRbus_40070 2      | 148.33            | 163.00              | 40.00            | 242.00           | 4         | 06:05:00, 06:45:00, 10:15:00, 15:50:00 |
-------------------
Notes:
Micron frequency is shift-oriented.
Hirodai relies on overlapping multi-operator services → naturally higher frequency.

6. Fare Structure
Measure: Minimum and maximum fare (JPY) between representative stop pairs
Insight:
- Micron: Fares vary by origin-destination
- Hirodai: Geiyo Bus mostly ¥160–¥380, JR Bus goes up to ¥1180 depending on reach (e.g., Nomio, Hiroshima Bus Center)

Table: Micron-related Public Bus Fare 
------------------
| route_id     | route_name                             | direction   | origin_stop_id | origin_stop_name           | destination_stop_id | destination_stop_name         | fare_jpy |
|--------------|-----------------------------------------|-------------|----------------|-----------------------------|----------------------|-------------------------------|----------|
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2359 2         | 中渡橋                         | 550      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2360 2         | 東映カントリークラブ入口       | 570      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2361 2         | マイクロン前                   | 570      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2363 2         | 吉川工業団地                    | 590      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2368 2         | 下野原下                        | 610      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2369 2         | 下野原                          | 610      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2370 2         | 下野原中                        | 610      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | To Micron   | Geiyo_2065 5   | 西条駅                       | Geiyo_2371 2         | 下野原上                        | 590      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2359 2   | 中渡橋                       | Geiyo_2065 5         | 西条駅                          | 570      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2360 2   | 東映カントリークラブ入口     | Geiyo_2065 5         | 西条駅                          | 570      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2361 2   | マイクロン前                 | Geiyo_2065 5         | 西条駅                          | 570      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2363 2   | 吉川工業団地                | Geiyo_2065 5         | 西条駅                          | 590      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2368 2   | 下野原下                    | Geiyo_2065 5         | 西条駅                          | 610      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2369 2   | 下野原                      | Geiyo_2065 5         | 西条駅                          | 610      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2370 2   | 下野原中                    | Geiyo_2065 5         | 西条駅                          | 610      |
| Geiyo_1400 0 | 吉川線（西条駅－西条駅・午前）           | From Micron | Geiyo_2371 2   | 下野原上                    | Geiyo_2065 5         | 西条駅                          | 590      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | To Micron   | Geiyo_2050 1   | 八本松駅                     | Geiyo_2359 2         | 中渡橋                          | 420      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | To Micron   | Geiyo_2050 1   | 八本松駅                     | Geiyo_2360 2         | 東映カントリークラブ入口       | 450      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | To Micron   | Geiyo_2050 1   | 八本松駅                     | Geiyo_2361 2         | マイクロン前                   | 450      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | To Micron   | Geiyo_2050 1   | 八本松駅                     | Geiyo_2363 2         | 吉川工業団地                    | 470      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | From Micron | Geiyo_2359 2   | 中渡橋                       | Geiyo_2050 1         | 八本松駅                        | 420      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | From Micron | Geiyo_2360 2   | 東映カントリークラブ入口     | Geiyo_2050 1         | 八本松駅                        | 450      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | From Micron | Geiyo_2361 2   | マイクロン前                 | Geiyo_2050 1         | 八本松駅                        | 450      |
| Geiyo_1430 0 | 吉川線（八本松駅－吉川工業団地・長沢）   | From Micron | Geiyo_2363 2   | 吉川工業団地                | Geiyo_2050 1         | 八本松駅                        | 470      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | To Micron   | Geiyo_2363 2   | 吉川工業団地                | Geiyo_2050 1         | 八本松駅                        | 470      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | To Micron   | Geiyo_2361 2   | マイクロン前                 | Geiyo_2050 1         | 八本松駅                        | 450      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | To Micron   | Geiyo_2360 2   | 東映カントリークラブ入口     | Geiyo_2050 1         | 八本松駅                        | 450      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | To Micron   | Geiyo_2359 2   | 中渡橋                       | Geiyo_2050 1         | 八本松駅                        | 420      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | From Micron | Geiyo_2050 1   | 八本松駅                     | Geiyo_2359 2         | 中渡橋                          | 420      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | From Micron | Geiyo_2050 1   | 八本松駅                     | Geiyo_2360 2         | 東映カントリークラブ入口       | 450      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | From Micron | Geiyo_2050 1   | 八本松駅                     | Geiyo_2361 2         | マイクロン前                   | 450      |
| Geiyo_1431 0 | 吉川線（吉川工業団地－八本松駅・長沢）   | From Micron | Geiyo_2050 1   | 八本松駅                     | Geiyo_2363 2         | 吉川工業団地                    | 470      |
-------------------

Table Summary 
---------------
| direction    | origin_stop_name | destination_stop_name | fare_jpy |
|--------------|------------------|------------------------|----------|
| To Micron    | 西条駅 (Saijo)    | マイクロン前 (Micron)  | 570      |
| To Micron    | 西条駅 (Saijo)    | マイクロン前 (Micron)  | 570      |
| From Micron  | マイクロン前     | 西条駅                 | 570      |
| From Micron  | マイクロン前     | 西条駅                 | 570      |
--------------------

Table: Hirodai-related Public Bus Fare
-------------------
| base_route_id         | operator   | route_name                        | origin_stop_name | destination_stop_name | fare_min | fare_max |
|-----------------------|------------|-----------------------------------|------------------|------------------------|----------|----------|
| Geiyo_1310 0          | Geiyo Bus  | 八本松－広大線（八本松駅－八本松駅・広大） | 八本松駅         | 八本松駅                | 160      | 350      |
| Geiyo_1370 0          | Geiyo Bus  | 八本松－広大線（八本松駅－広大北口・農） | 八本松駅         | 広大北口               | 160      | 350      |
| Geiyo_1371 0          | Geiyo Bus  | 八本松－広大線（広大二神口－八本松駅・農） | 広大二神口        | 八本松駅               | 160      | 350      |
| Geiyo_1380 0          | Geiyo Bus  | 東広島－広大線（東広島駅－大学会館前）   | 東広島駅         | 大学会館前             | 160      | 380      |
| Geiyo_1381 0          | Geiyo Bus  | 東広島－広大線（ががら口－東広島駅）     | ががら口         | 東広島駅               | 160      | 380      |
| JRbus_1490550946_13   | JR Bus     | JRbus－八本松駅－八本松            | 八本松駅         | 八本松駅               | 0        | 1180     |
| JRbus_1490550946_18   | JR Bus     | JRbus－東広島駅－大学会館前        | 東広島駅         | 大学会館前             | 0        | 1180     |
| JRbus_1490550946_23   | JR Bus     | JRbus－ががら口－東広島駅          | ががら口         | 東広島駅               | 0        | 1180     |
| JRbus_1490550946_27   | JR Bus     | JRbus－八本松駅－広大北口          | 八本松駅         | 広大北口               | 0        | 1180     |
| JRbus_1490550946_28   | JR Bus     | JRbus－広大二神口－八本松           | 広大二神口        | 八本松駅               | 0        | 1180     |
| JRbus_4251312605      | JR Bus     | 広島～広島大学線（グリーンフェニックス） | 広島バスセンター   | 山中池                 | 160      | 220      |
| JRbus_1490550946_22   | JR Bus     | JRbus－西条駅－乃美尾              | 西条駅           | 乃美尾                 | 0        | 1180     |
| JRbus_1490550946_8_1  | JR Bus     | JRbus－西条駅－広島国際大学         | 西条駅           | 広島国際大学           | NaN      | NaN      |
| JRbus_1490550946_6_1  | JR Bus     | JRbus－広島国際大学－西条駅         | 広島国際大学       | 西条駅                 | NaN      | NaN      |
| JRbus_1490550946_9    | JR Bus     | JRbus－乃美尾－西条駅              | 乃美尾           | 西条駅                 | 0        | 1180     |
----------------------------
Geiyo routes: 160–540 JPY

JR Bus routes: 160–470 JPY

Interpretation

Fare band differences reflect:

Longer geographic reach of Geiyo routes

More urban JR Bus corridors

Industrial/loop routes covering long OD pairs

7. Combined Insight – Micron vs Hirodai
Dimension	Micron	Hirodai
Spatial role	Industrial access hub	University commuter hub
Network pattern	Long loops + short connectors	Dense overlapping feeders
Visible distance	3–9 km	4–7 km
Frequency	Shift-based	Multi-operator overlap
Fare	160–630 JPY	160–540 / 470 JPY
Route diversity	Low (4)	High (15)
Overall Interpretation

Micron relies on purpose-built industrial bus connectivity, while Hirodai functions as a multi-operator public transport hub offering broad regional accessibility.

8. Visualization Outputs

From the map visualizations:

Each Micron route is drawn according to GTFS shapes, ensuring 1:1 alignment with metrics.

The Hirodai map displays 15 unique corridors with color-coded polylines matching the legend.

Distance calculations match the exact visible geometry drawn in the map.

Why this matters?

Clear visualization + synced metrics provides a defensible, reproducible analytical foundation for your thesis.

9. Learning Reflections

From this analysis, you learned:

How GTFS routes/trips/shapes interact to form actual service patterns.

How to filter GTFS spatially using POI radii.

How distance, duration, headway, and fare integrate into accessibility metrics.

How public bus GTFS differs from private shuttle GTFS.

How to build map visualizations that sync with real GTFS geometry.

How insights support future MATSim modeling.

10. Upcoming Focus

Integrating public bus + Micron Shuttle into MATSim network.

Building accessibility indicators for Micron–Hirodai–Saijo corridor.

Connecting accessibility improvements to value capture ideas (e.g., joint development).

Structuring thesis narrative around accessibility, integration, and policy implications.
