# Data

`datasets/` holds the 13 CSV files provided to participants by the MTA Data Series 2026 organizers: Formula 1 race history from 1950 through the 2026 São Paulo GP, plus the competition price list (`prices.csv`).

| File | Contents |
|---|---|
| results.csv | Race results, one row per driver per race |
| races.csv | Race calendar (year, round, circuit, date) |
| drivers.csv | Driver names and codes |
| constructors.csv | Teams |
| status.csv | Finishing status labels |
| prices.csv | Competition price per driver ($M) |
| qualifying.csv | Qualifying positions and lap times |
| lap_times.csv | Race lap times |
| pit_stops.csv | Pit stop durations |
| driver_standings.csv | Championship standings after each race |
| constructor_standings.csv | Team standings after each race |
| circuits.csv | Circuits |
| sprint_results.csv | Sprint results (not used: the case scores main races only) |

The only outside data is the official 2026 Las Vegas GP starting grid and qualifying order, which are typed into the notebook.
