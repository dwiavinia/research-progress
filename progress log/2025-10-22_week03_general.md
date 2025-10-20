**Introduction**
- Context of Modelling Approach
  - Agent-Based Model (ABM) are used to represent individual travelers as autonomous agents
  - Unlike activity-based models that focus on daily shedules, ABM emphasizes interactions and emergent behavior at the system level
  - MATSim is ABM framework widely used for transport simulation and policy analysis 
- Current Learning Focus
  - Understanding how MATSim agents perform replanning through iterations to reach a behavioral equilibrium
  - Exploring how GTFS data (routes, stops, timetables) can be converted and used to build realistic MATSim network
  - Examining technical mechanism such as multi-threading and scenario iteration to improve computational efficiency
- Research Direction
  - Integrating GTFS-based public transport data with a private shuttle service (e.g., Micron Shuttle) inside MATSim simulation.
  - Comparing system performance across different integration scenarios.
  - Developing a methodological framework for evaluating the integration of public and private transport services using agent-based simulation
  - Quantifying accessibility and equity outcomes to identify which integration strategy provides the most balanced benefits

**Problem Statements**
- Background Challenges
  - Existing agent-based transport simulations such as MATSim are widely used to represent traveler behavior and system dynamics (Horni et al., 2016). while they provided detailed behavioral patterns, these models require complex data preparation and calibration processes.
  - Public transport networks are often represented using GTFS data, which must be converted into MATSim-compatible formats, a technically demanding process for users with limited experience (Roth et al., 2019; Ziemke et al., 2021).
- Problem Focus
  - How can the integration of a private shuttle service (e.g., Micron Shuttle) within an existing GTFS-based public transport system be effectively simulated and analyzed using MATSim?
  - How can scenario results be interpreted in terms of accessibility, equity, and system performance within an agent-based modeling framework?
 
**Research Gap**

- 
