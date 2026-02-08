**General Overview**
- In MATSim, the population does not directly appear as a population.xml.gz file.
- Instead, it is created through several transformation steps, starting from survey data and ending with executable population plans.
- Chronologically, the process can be summarized as follows:
  - Raw survey and diary data → weighted trips → building/facility assignment → population plans → MATSim simulation

**Stage 1 — Base Data: Trip Diary and Household Data**
- The population originates from older data sources (approximately 10 years old).
- These data consist of:
  - trip diary records,
  - household and person attributes,
  - time-related information (e.g., departure time, activity duration).
- Key point
  - At this stage, the data are not yet a MATSim population.
  - They represent observed human travel behavior, typically stored in tabular formats.
- Typical file formats
  - trip_diary.csv
  - household.csv
  - person_attributes.csv
  - (The exact file names may differ, but the data types follow this structure.)

**Stage 2 — Weighting and Expansion (Population Scaling)**
- The survey data are weighted to represent a larger population.
- The process includes:
  - household weights,
  - origin-based weighting,
  - building attractiveness.
- Meaning
  - A small survey sample is expanded into hundreds of thousands of agents.
  - Population sizes such as 240,000 agents emerge at this stage.
- Typical outputs
  - weighted_trips.csv
  - expanded_trips.csv
- Important note
  - At this stage, the population is still abstract.
  - Agents do not yet have specific building locations.

**Stage 3 — Building / Facility Assignment (Critical Stage)**
- This stage is directly related to the population loss issue.
- A building or facility assignment process is applied using:
  - building attractiveness,
  - origin–destination weighting.
- Each trip is assigned:
  - an origin building ID (O_Bid),
  - a destination building ID (D_Bid).
- What happens:
  - Not all trips can be successfully assigned to buildings.
  - Trips that fail assignment do not have valid O_Bid or D_Bid.
  - From the script logic:
  - df1 = df[~(df.O_Bid.isnull() | df.D_Bid.isnull())]
  - Trips without valid building assignments are removed.
- Related files
  - trips_with_building.csv
  - buildings.shp or buildings.csv
  - building attractiveness or probability files
- Key implication
  - There is an explicit stage where the population must fit the building layer.
  - This is the stage where agents can be lost from the population.

**Stage 4 — Population Generation (Java Application)**
- This step is described as the most important stage in demand generation.
- It is executed through a Java-based population generation application.
Inputs

Trip data that passed the building assignment,

household weights,

timing information.

Output

population.xml.gz

This file contains:

agents,

activities (e.g., home, work),

travel plans.

Main output file

population.xml.gz — this is the file used directly in MATSim.

Important implication

Agents removed in Stage 3 will not appear in this file.

Stage 5 — Population Used in MATSim
What is explained

The population.xml.gz file is referenced in the config.xml.

MATSim only reads:

the agents contained in this file,

not the original survey population.

Result

The effective population entering the simulation may be 144,000 agents instead of 240,000.

Chronological Summary (Very Brief)

Trip diary (raw survey data)

Weighting and expansion → ~240,000 agents

Building assignment

trips without valid origin or destination buildings are removed

Population generation (Java)

population.xml.gz (effective population)

MATSim simulation
