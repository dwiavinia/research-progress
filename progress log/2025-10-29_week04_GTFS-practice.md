RESEARCH CONCEPTUALIZATION
Research Goal
To evaluate how integrating private shuttle services with the public transport system affects both network efficiency and traveler behavior, and to assess its policy feasibility as a sustainable strategy for improving urban mobility and service accessibility.

Sub-Goals
- Data & Network Preparation
  To combine GTFS feeds of public bus and private shuttle into a unified, MATSim-compatible network representing the current   and integrated systems.
- Behavioral Analysis
  To analyze how travelers (agents) adapt their route and mode choices when shuttle access, fares, and capacity are modified under policy-driven integration scenarios.
- Efficiency Evaluation
  To measure changes in total system travel time, congestion, and vehicle utilization after the integration of shuttle and bus services.
- Policy Assessment
  To interpret simulation results as policy evidence—evaluating whether shuttle–bus integration can serve as an effective, balanced solution for first–last mile connectivity and spatial accessibility.

Research Question
- How do travelers adapt their route and mode choices when shuttle access and fares change?
- To what extent does integration reduce travel time and congestion?
- Can shuttle–bus integration be considered a feasible and balanced transport policy?

Data
- Public Transport Network
  GTFS feed of public bus routes, stops, trips, and schedules.
  Source: Local transport agency (GTFS)
- Private Shuttle Network
  Shuttle routes, stops, and schedule converted to GTFS format.
  Source: Micron company data / field survey
- Road Network
  Street network, intersections, and coordinates.
  Source: OpenStreetMap (OSM shapefile)
- Population & Demand Data
  Synthetic population, OD data, or demographic distribution.
  Source: Census data / generated model
- Policy & Operation Parameters
  Fare, capacity, headway, transfer stops, and access rules. (To be determined)
  Source: Derived from official or assumed parameters
- Spatial Reference Data 
  Administrative boundaries.
  Source: GIS 

Methodology
1. Data Pre-processing
   Clean and merge GTFS feeds from public transport and private shuttle.
   Align schedules and define transfer stops.
   Convert the merged dataset into MATSim-compatible XML files (network.xml, transitSchedule.xml, vehicles.xml).
   Tools: Python (gtfs-kit, partridge), pt2matsim
2. Scenario Development
   Define four policy scenarios:
   - Baseline (no integration)
   - Open Integration (public access)
   - Regulated Integration (controlled access)
   - Coordinated Network (long-term policy)
   Tools: MATSim configuration folders
3. Simulation in MATSim
   Run agent-based simulations for each scenario.
   Allow agents to iteratively replan routes and modes until behavioral equilibrium is reached.
   Modules: MATSim core modules (network, transit, population)
4. Indicator Extraction and Analysis
   Extract outputs to evaluate three dimensions:
   Efficiency: travel time, congestion, load factor.
   Behavior: mode share, number of replanning agents.
   Policy Impact: service coverage.
   Tools: MATSim Events and Accessibility modules, Python or R for post-processing
5. Policy Interpretation
   Compare results across scenarios to assess how integration policies influence efficiency and user behavior.
   Derive policy implications for local transport planning.
   
Scenario Design
Scenario 0 – Baseline
Current system where the Micron Shuttle serves only Micron employees, with no integration with the bus network.
Policy Setting: No policy integration.
Purpose: To represent current inefficiencies and access limitations.

Scenario 1A – Open Integration
Shuttle becomes part of the public transport system; public access allowed with a unified fare structure.
Policy Setting: Full policy access.
Purpose: To test maximum efficiency gain and behavioral change potential.

Scenario 1B – Regulated Integration
Shuttle integrated but with fare or capacity limits; coordinated schedule with existing buses.
Policy Setting: Controlled policy.
Purpose: To balance efficiency and fairness, and prevent bus ridership loss.

Scenario 2 – Coordinated Network (Vision)
Long-term policy vision where shuttle and buses operate under a unified institutional framework with integrated GTFS data and schedules.
Policy Setting: Strategic governance.
Purpose: To evaluate long-term sustainability and governance model for urban mobility improvement.

CURRENT LEARNING FOCUS
- GTFS Practice
Workflow Structure
GTFS Practice/
│
├── sample-feed-1.zip                  # Dummy GTFS feed (downloaded for practice)
│
├── gtfs_extracted/                    # Unzipped raw GTFS files
│   ├── stops.txt
│   ├── routes.txt
│   ├── trips.txt
│   ├── stop_times.txt
│   └── calendar.txt
│
├── practice_1.py                      # Python cleaning script (remove missing / unused entries)
│
├── gtfs_cleaned/                      # First cleaned version after running the script
│
├── gtfs_cleaned_v2/                   # Final validated feed (no errors)
│   ├── stops.txt
│   ├── routes.txt
│   ├── trips.txt
│   ├── stop_times.txt
│   ├── calendar.txt
│   └── agency.txt
│
├── gtfs-validator-7.1.0-cli.jar       # GTFS Validator tool (by MobilityData)
└── validation_report_v2/              # Validation output (HTML + JSON report)

- GTFS Conversion to MATSim
Workflow Structure
│
├── pt2matsim-master/                  # Converter tool (Java + Maven project)
│   ├── src/main/java/org/matsim/pt2matsim/run/
│   │   └── Gtfs2TransitSchedule.java  # Main converter class (entry point)
│   └── pom.xml                        # Maven build file (auto-download dependencies)
│
└── MATSimInput/                       # Output folder for converted results
  ├── transitSchedule.xml              # MATSim transit schedule (generated successfully)
  ├── transitVehicles.xml              # MATSim vehicle file (generated successfully)
 
- MATSim Simulation Practice
Input → Configuration → Execution → Output
MATSim Dummy Run/
│
├── scenarios/dummyNia/                                  # Scenario folder (custom dummy setup)
│   ├── `network.xml`                                    # Minimal 3-node, 4-link test network (✅ created manually)
│   ├── `population.xml`                                 # Single-agent plan (home–work–home, mode=car)
│   └── `config.xml`                                     # Central configuration file (paths, controler, strategy modules)
│
├── src/main/java/sandbox/
│   └── `RunDummySimulationNia.java`                     # Main Java entry point for running the simulation
│
├── pom.xml                                              # Maven build file (manages MATSim dependencies)
│
├── Run Configurations (IntelliJ IDEA)
│   ├── Working directory → `matsim-example-project/`    # Verified correct (defines where MATSim writes outputs)
│   └── Main class → `sandbox.RunDummySimulationNia`     # Executes config & scenario
│
└── output/                                              # Simulation result folder (generated successfully)
├── `output_config.xml`                                  # Copy of final config used
├── `output_network.xml.gz`                              # Network used in simulation
├── `output_plans.xml.gz`                                # Final agent plans after iteration
├── `scorestats.txt`                                     # Score per iteration (stability indicator)
├── `travelTimes.txt`                                    # Average mode travel times
├── `logfile.log`                                        # Complete run log (runtime messages)
└── ITERS/it.0/
  ├── `0.events.xml.gz`                                  # All events during simulation (core for analysis)
  ├── `0.linkstats.txt.gz`                               # Link-level stats (volume, delay, travel time)
  └── `0.plans.xml.gz`                                   # Iteration 0 plans snapshot

UPCOMING LEARNING FOCUS
- Learn to interpret and analyze simulation output files (scorestats.txt, linkstats.txt.gz, events.xml.gz)
- Visualize agent movements
- Run experiments by modifying input parameters
- Study the real data pipeline structure
- Develop the core research concept
