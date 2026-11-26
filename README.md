# F1 Driver Selection with XGBoost — MTA Data Series 2026

Team entry for the **MTA Data Series 2026** case competition (3rd place). The task: pick exactly three drivers for the 2026 Las Vegas Grand Prix to maximize the **point-to-cost ratio** (total points ÷ total price) under a fixed price list.

## Approach

Prices are fixed, so the only thing to forecast is each driver's points in the race.

1. **Clean the data** (1950–2026 race history): match drivers by `driverId` (car numbers differ between files), treat any non-numeric finishing position as a DNF, re-score every race with the 2026 points system (25-18-15-12-10-8-6-4-2-1, main race only, no sprints), remove red-flag pit-stop outliers.
2. **Build pre-race features**: grid slot, qualifying position before penalties, grid penalty, recent form (points, finishing position, places gained, DNF rate over the last 3–10 races), qualifying gap to pole, championship position, team pit-stop pace. Every rolling feature is lagged one race (`shift(1)`), so no row sees its own result.
3. **Train XGBoost** on 2024–2026 (1,398 driver-races, 68 races, 13 features), with the training window chosen by a walk-forward backtest on 2026 Rounds 6–20. Final predictions average 10 random seeds.
4. **Score every lineup**: expected points ÷ price for each driver, then all 1,330 three-driver combinations by team ratio.

## Results

| Validation | RMSE (points) | R² |
|---|---|---|
| Walk-forward backtest | 4.13 | 0.65 |
| Random 80/20 split | 4.12 | 0.65 |
| Chronological 80/20 split | 4.00 | 0.67 |

Same races, same features: last-10-race average 4.86, points for grid slot 4.79, linear regression 4.50, **XGBoost 4.13**.

| Lineup | Price | Predicted ratio | Actual ratio |
|---|---|---|---|
| Antonelli + Lindblad + Lawson (model's #1) | $4M | 6.46 | 3.0 |
| Antonelli + Lindblad + Colapinto (final lineup, model's #2) | $4M | 5.98 | 5.0 |

The key insight: prices track reputation, not 2026 form. Antonelli had won 9 of 20 races and qualified fastest, yet cost $2M; his grid penalty moved him back but did not slow the car.

**Limitations.** Features, model settings and the training window were all tuned on the same backtest races, so the backtest error is somewhat optimistic. A single race is one draw from a noisy outcome, so the actual ratio says little about model quality on its own.

## Repository

```
notebooks/MTA_2026_F1_XGBoost.ipynb   full pipeline, with outputs
data/datasets/                        the 13 competition CSVs
data/README.md                        what each file contains
requirements.txt
```

## Run it

```bash
pip install -r requirements.txt
jupyter notebook notebooks/MTA_2026_F1_XGBoost.ipynb
```

Runs in about 30 seconds on a laptop. On Google Colab, the notebook asks you to upload the datasets as a zip.

## Team

- **Crystal** — XGBoost model and pipeline (this notebook)
- **Jessica Yuan** — Monte Carlo risk analysis, report and presentation
