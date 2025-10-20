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
  - How can the integration of a private shuttle service within an existing GTFS-based public transport system be effectively simulated and analyzed using MATSim?
  - How can scenario results be interpreted in terms of accessibility, equity, and system performance within an agent-based modeling framework?
 
**Research Gap**
- Most studies using MATSim focus on simulation behavior, but not on systematic integration of GTFS data as a representation of real public transport operations. There is a lack of clear workflow on how GTFS-based routes and timetables can be accurately transformed into MATSim’s network and schedule formats without information loss.
- MATSim, while effective in simulating agent-based travel behavior, relies on static plans and preset rules that limit its ability to reflect real-time changes and dynamic variations in transport systems, creating gaps in adaptiveness and realism due to its dependence on pre-collected data and scenario-based simulations (Huang et al, 2022)

**Points to Clarify**
1. The key challenges that I should anticipate when converting GTFS data into MATSim formats, and how might these impact the realism of my model?
2. For adaptiveness, since I am not sure if my research will involve dynamic travel behavior or real-time changes, how should I discuss the possible limitations of MATSim in representing changes or variations in travel patterns? 
