**Micron Shuttle – GTFS Data Structure & Temporal Analysis**
> *A learning summary of GTFS-based analysis for the Micron Shuttle (Higashi-Hiroshima)*  

1. GTFS Feed Structure Overview
   - **Feed:** `GTFS_Micron_shuttle`  
   - **Files detected:** 11 (agency, routes, trips, stops, stop_times, shapes, calendar, calendar_dates, fares, feed_info, translations)
-------------
| Indicator      | Value                             | Interpretation                                |
|----------------|-----------------------------------|-----------------------------------------------|
| Agencies       | 1 (マイクロンメモリジャパン株式会社) | Single operator (private shuttle)             |
| Routes         | 1                                 | One main service corridor                     |
| Trips          | 70                                | Individual daily departures                   | 
| Stops          | 29                                | Total stops (dense spacing)                   |
| Shapes         | 6                                 | Variants of inbound/outbound route alignments |
| Calendar       | 2 (平日, 土曜・日曜・祝日)          | Weekday vs holiday operation                  |
| Calendar dates | 22 exceptions                     | Public holidays and national breaks           |
| Period covered | 2024-05-24 → 2025-03-31           | Full service year                             |

   - Structure insight  
     - The feed is compact and well-structured. Only one route exists, but it contains multiple shape variants that correspond to different directions (inbound/outbound) and minor alignment adjustments.

2. Trip Duration 
   - Trip duration represents how long the Micron Shuttle takes to complete a full route. By contrasting weekday and holiday services, we can see whether reduced frequency or schedule relaxation affects overall travel time.
   -----------------------
| File | Trips | Mean (min) | Median (min) |
|-------|--------|-------------|---------------|
| `trip_duration_weekday.csv` | 56 | 31.5 | 30.0 |
| `trip_duration_holiday.csv` | 14 | 32.4 | 35.0 |

   - Key observations
     - Average trip ≈ 30–35 min per direction.  
     - Duration remains stable across weekday and holiday.  
     - Slightly longer median on holidays, why? 
   - Interpretation
     - Travel time consistency indicates efficient, predictable routing.
     - The system maintains similar journey times despite reduced service frequency, confirming stable operational and traffic conditions.

3. Headway 
   - Headway shows how often the Micron Shuttle departs between trips. Comparing weekday and holiday patterns reveals how the service adjusts its frequency around factory shift times.
---------------------------------
| File | Trips | Median Headway | Mean Headway | Pattern |
|-------|--------|----------------|---------------|----------|
| `headway_weekday.csv` | 55 | 10 min | 17 min | High frequency (dense peak + long midday gap) |
| `headway_holiday.csv` | 13 | 30 min | 69 min | Sparse service (few trips, long shift gap) |

   - Key Observation
     - Right-skewed distribution: many short intervals (5–15 min) and a few very long ones (300–500 min).  
     - Weekday: 2–3 shift clusters (morning & evening).  
     - Holiday: reduced operation (≈¼ weekday trips).
   - Interpretation
     - The skewed distribution proves a *shift-based timetable* — intensive service during worker transfers, long idle windows mid-day.
     - Frequency changes are temporal (schedule-driven), not congestion-driven.

4. Travel Time
   - Travel time shows how long the bus takes to move from one stop to the next. Comparing weekday and holiday results helps reveal whether route spacing or within-trip performance changes across schedules.  .
-----------------------------------------
| File | Segments | Mean (min) | Median (min) | Longest (min) |
|-------|-----------|-------------|---------------|----------------|
| `travel_times_weekday.csv` | 588 | 3.0 | 1.0 | 14.0 |
| `travel_times_holiday.csv` | 158 | 2.9 | 1.0 | 14.0 |

  - Key Observation
    - Majority of segments take **1 min** → dense stop spacing (~400–700 m).  
    - A few segments (6–14 min) correspond to terminal–factory stretches.  
    - Distribution remains identical across weekdays & holidays.
  - Interpretation
    - Spatial structure is constant — route geometry unchanged, stop spacing uniform.  
    - Variations in operation are temporal rather than spatial.

5. Combined Insights
   - By comparing trip duration, headway, and travel time together, we can see how the Micron Shuttle balances consistent routes with flexible timing. The service remains spatially stable but adapts its frequency and timing to match worker shift demand.
---------------------------------
| Dimension | Indicator | Micron Shuttle Pattern |
|------------|------------|------------------------|
| **Temporal frequency** | Headway | 10–20 min (weekday) → 30–70 min (holiday) |
| **Trip efficiency** | Duration | 30–35 min stable, slightly longer holiday median |
| **Spatial spacing** | Inter-stop time | 1–3 min, uniform across service days |
| **Overall behavior** |  | Consistent route; flexible timetable based on shift demand |

6. Visualization Outputs
Plots generated via `plot_micron_analysis.py`:

| Visualization | File Output | Description |
|----------------|-------------|--------------|
| Headway Histogram | `headway_weekday_hist.png` / `headway_holiday_hist.png` | Right-skewed distribution shows shift-based frequency |
| Travel Time Histogram | `travel_times_weekday_hist.png` / `travel_times_holiday_hist.png` | Dense stop spacing, stable geometry |
| Trip Duration Boxplot | `trip_duration_comparison.png` | Consistent travel time across services |

7. Learning Reflections
- Understood **GTFS structure** (relationship between trips, stops, shapes, calendar).  
- Learned to extract **temporal indicators** (duration, headway, travel time).  
- Practiced **Python workflow** using `gtfs-kit`, `pandas`, and `matplotlib`.  
- Discovered **shift-based pattern** through right-skewed temporal distributions.  

8. **UPCOMING LEARNING FOCUS**
   - Literature review of value capture case studies in Hong Kong and Tokyo
   - Literature review of cost-sharing schemes, especially joint development
   - Learning about GTFS data for public transport
   - Learning about  GTFS data for other private services
