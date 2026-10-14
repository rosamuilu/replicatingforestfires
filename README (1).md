# Replicating forest fires

A replication of the self-organised critical (SOC) forest-fire model by Drossel & Schwabl (1992), built in Python with AgentPy. The goal is to demonstrate self-organised critical dynamics in a simple model. The replication used a forest fire simulation by Foramitti (2021) as a starting point.

Complex Systems — Classic Model Replication & Critical Review (Group 7)
Rosa Muilu, Binti Dekker, Martijn Schotte, Noah Zuijderwijk

![Forest fire animation](forest_fire_animation.gif)

## The model

Each cell on a 2D lattice is empty, a tree, or burning. Every time step:
- a burning tree becomes an empty site
- a tree next to a burning tree catches fire
- an empty site grows a tree with probability `p`
- a tree catches fire from lightning with probability `f`

The system is expected to self-organise into a critical state when growth is much faster than lightning (`f/p → 0`). In that state, fire sizes follow a power law: many small fires, a few medium ones, and rare catastrophic ones.

Simulation settings: 200 × 200 grid, 2,000 time steps, full initial tree cover, random seed 42, with `f` in {10⁻³, 10⁻⁵, 10⁻⁶} and `p/f` varied (10 to 10,000).

## Main findings

**Forest density approaches, but doesn't reach, the critical value.** Drossel & Schwabl report a steady-state density of ρ ≈ 0.39. The closest we got was ρ ≈ 0.34, at `f` = 10⁻⁵ and `p/f` = 1,000. The gap is most likely due to the limited number of time steps.

| `f` | `p/f` | Mean density ρ |
|---|---|---|
| 10⁻⁶ | 10 | 0.066 |
| 10⁻⁶ | 1,000 | 0.297 |
| 10⁻⁶ | 10,000 | 0.523 |
| 10⁻⁵ | 10 | 0.111 |
| 10⁻⁵ | 1,000 | **0.339** |
| 10⁻⁵ | 10,000 | 0.282 |
| 10⁻³ | 10 | 0.228 |
| 10⁻³ | 1,000 | 0.778 |
| 10⁻³ | 10,000 | 0.780 |

**Power-law fire sizes appear for low `f` and `f/p`.** The fire-size distribution follows the scaling trend reported in the original paper. However, the cumulative distribution is still dominated by small fires, which suggests the system had not fully reached the critical regime.

**Fire size had to be approximated.** Once a cell burns out, the model loses track of which fire it belonged to. We therefore measured each fire as the size of the connected tree cluster at the lightning strike, marked with an extra "ignited" cell state. This works well when lightning is rare, because fires seldom overlap. As `f` increases, it overestimates fire sizes.

**The critical-to-non-critical transition was not reproduced.** At higher `f/p` the original model shows an exponential cutoff, and large fires become nearly impossible. Our results still looked power-law-like at these settings. Two things explain this: the cluster-size approximation, and finite-size effects from the small lattice and short runs.

**The original power law has been questioned.** Grassberger (1993, 2002) found that on very large lattices (65,536 × 65,536) the apparent scaling breaks down. Our model might lose its power law at larger sizes too.

## Takeaways

- **Generalisation:** SOC explains how small local interactions can produce rare large-scale events without fine-tuning. Similar dynamics have been described for earthquakes and disease spreading.
- **Boundaries:** true SOC requires infinite size and time and a strict separation of timescales between lightning, fire spread and growth. Real forests are finite, may have frequent ignitions, and fire spreads unevenly (wind, vegetation), none of which the model captures.
- **Policy relevance:** under SOC, catastrophic events can't be eliminated. Letting small, controlled fires happen keeps fuel from building up. Policy should therefore focus on resilience and recovery rather than suppressing every fire.

## Contents

```
replicatingforestfires/
├── forest_fire_animation.gif   # Animation of dynamics in the model
├── replicateforestfires.ipynb  # Jupyter notebook with the model and plots
├── requirements.txt            # List of dependencies
├── forest_fire_results.csv     # Saved results
└── README.md                   # Project description and instructions
```

## Installation

Clone the repository and install the required packages:

```bash
git clone https://github.com/rosamuilu/replicatingforestfires.git
cd replicatingforestfires
pip install -r requirements.txt
```

## Running the model

Run the model directly from `replicateforestfires.ipynb`. You can set the lightning probability, the ratio of regrowth to lightning, the grid size and the number of time steps.

## References

Drossel, B. & Schwabl, F. (1992). Self-Organized Critical Forest-Fire Model. *Physical Review Letters*, 69(11), 1629–1632. https://doi.org/10.1103/PhysRevLett.69.1629

Foramitti, J. (2021). Forest fire model example (AgentPy). https://agentpy.readthedocs.io/en/latest/agentpy_forest_fire.html

Grassberger, P. (1993). On a self-organized critical forest-fire model. *Journal of Physics A*, 26(9), 2081–2089. https://doi.org/10.1088/0305-4470/26/9/007

Grassberger, P. (2002). Critical behaviour of the Drossel-Schwabl forest fire model. *New Journal of Physics*, 4, 17. https://doi.org/10.1088/1367-2630/4/1/317
