# carbonaware-football-logistics

## Multi-Objective Optimisation for Carbon-Aware Football Travel

A Python library for optimising team travel logistics in football, balancing carbon footprint reduction with player recovery and performance under uncertainty.

## Overview

This library provides tools to:
- Calculate carbon emissions from team travel using ICAO/DEFRA methodologies
- Model player fatigue and recovery from travel characteristics (distance, time zones, altitude)
- Optimise itineraries using two-stage stochastic programming and NSGA-II
- Generate explainable recommendations with counterfactual explanations
- Visualise Pareto fronts for carbon-recovery trade-offs

## Installation

```bash
# Clone the repository
git clone https://github.com/sebasgj821018/r1-logistics-public.git
cd r1-logistics-public

# Install dependencies
pip install -r requirements.txt
```

## Quick Start

### Calculate Carbon Emissions

```python
from carbonaware import calculate_emissions

# Example: Porto Alegre → La Paz (direct flight)
emissions = calculate_emissions(
    origin="POA",
    destination="LPB",
    distance_km=3400,
    num_passengers=25,
    transport_mode="commercial"
)

print(f"CO2 emissions: {emissions:.2f} tonnes")
```

### Optimise Itinerary

```python
from carbonaware import optimize_itinerary

result = optimize_itinerary(
    origin="POA",
    destination="LPB",
    altitude_m=3640,
    kickoff_date="2026-10-15",
    kickoff_time="20:00",
    constraints={
        "min_recovery_hours": 48,
        "max_carbon_tonnes": 50
    }
)

print(f"Optimal arrival: T-{result.arrival_hours}h before kickoff")
print(f"CO2 savings: {result.co2_savings:.1f} tonnes vs. baseline")
```

### Visualise Pareto Front

```python
from carbonaware import plot_pareto_front

plot_pareto_front(
    results_list,
    title="Brasileirão 2025: Carbon-Recovery Pareto Front",
    xlabel="Carbon Emissions (tCO2e)",
    ylabel="Recovery Score"
)
```

## Features

### Core Functionality
- **Carbon Calculation**: ICAO methodology + UK DEFRA emission factors
- **Fatigue Modeling**: Multi-factor recovery model (distance, time zones, altitude, arrival time)
- **Stochastic Optimisation**: Two-stage programming with scenario trees
- **Multi-Objective NSGA-II**: Pareto-optimal solutions for carbon, recovery, and equity

### Advanced Modeling
- **Bayesian GAM Surrogates**: Glass-box models for recovery→performance mapping
- **Chance Constraints**: Probabilistic guarantees on recovery thresholds
- **Altitude Acclimatisation**: Discrete windows (≤24h or ≥5-7 days; avoid 24-72h)
- **Explainable AI**: Additive shape functions with credible intervals

### Optimization
- **Integer Programming**: MIP-based routing with real flight constraints
- **Equity Metrics**: Gini coefficient of rest hours across squad
- **Backtesting**: Compare optimised vs. historical itineraries

## Project Structure

```
r1-logistics-public/
├── src/
│   └── carbonaware/
│       ├── __init__.py
│       ├── emissions.py
│       ├── fatigue_model.py
│       ├── optimizer.py
│       ├── xai.py
│       └── visualization.py
├── data/
│   └── synthetic/
├── notebooks/
│   └── demo_optimization.ipynb
├── tests/
│   └── test_emissions.py
├── requirements.txt
├── LICENSE
└── README.md
```

## Data Sources

### Aviation Data
- **OurAirports** (airport coordinates, elevation) - Public Domain
- **ANAC Microdata** (Brazilian flight delays) - Open Government Data

### Emission Factors
- **UK DEFRA Conversion Factors** - Open Government Licence
- **ICAO Carbon Emissions Calculator** methodology

### Football Data (for demos only)
- Synthetic fixture data included in `data/synthetic/`
- Real fixture data requires separate acquisition (see private repository)

## Testing

```bash
pytest tests/
pytest tests/ --cov=carbonaware
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## License

MIT License. See [LICENSE](LICENSE) file for details.

## Citation

If you use this library in academic work, please cite:

```bibtex
@software{gaviria2027carbonaware,
  author = {Gaviria Jaramillo, Sebastián},
  title = {carbonaware-football-logistics},
  year = {2027},
  url = {https://github.com/sebasgj821018/r1-logistics-public}
}
```

## Author

**Sebastián Gaviria Jaramillo**
- Independent Researcher
- MSc Candidate, EAFIT University (Medellín, Colombia)
- Former Head of Sports Analytics, Independiente del Valle (Ecuador)
- 10+ years experience in sports data science

## Related Research

This library supports the research project:

> *"Carbon-aware and recovery-aware travel logistics in South American club football: a multi-objective stochastic optimisation of fixture congestion"*

Submitted to: **World Congress on Science and Football (WCSF) 2027**, Liverpool, UK.
