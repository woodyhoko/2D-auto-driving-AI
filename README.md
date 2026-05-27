# Learning-Based Neuroevolution (LBNE)

**Ho Ko** — ECE, University of Waterloo

📄 **[Read the Paper](https://github.com/woodyhoko/2D-auto-driving-AI/blob/main/2D%20auto%20driving%20AI.pdf)** | **[▶ Run the Simulation](https://woodyhoko.github.io/2D-auto-driving-AI/track_example.html)**

---

## Abstract

> *Most neuroevolution methods use Genetic Algorithms to evolve neural network topologies, but discard the power of backpropagation entirely. This paper proposes LBNE — a hybrid algorithm that evolves both architecture and weights via GA, while also using backpropagation as a secondary learning signal — demonstrated on an autonomous driving task.*

Neural networks excel at learning non-linear mappings; genetic algorithms excel at exploring unknown solution spaces. Existing approaches use one or the other. **LBNE (Learning-Based Neuroevolution)** combines both: the GA evolves the population topology and parameter space, while backpropagation fine-tunes each individual — analogous to how humans inherit biological traits from parents but also learn from experience.

---

## Algorithm: LBNE

### Core Loop

```
1. Initialize population of N agents (random topology + weights)
2. For each new individual:
   └── Backprop on sample I/O from the current best agent
       (learn from the "role model" before entering the world)
3. Evaluate each agent's fitness in the environment
4. Eliminate underperformers
5. Replenish population via crossover + mutation
6. Agents that perform well stop learning (save compute)
7. Repeat
```

### Key Innovations vs. Prior Work

| Method | GA Topology | GA Weights | Backprop |
|---|---|---|---|
| Standard GA | ✅ | ✅ | ❌ |
| NEAT | ✅ | ✅ | ❌ |
| EPNet / MA | ✅ | ✅ | ✅ (fixed task) |
| **LBNE (ours)** | **✅** | **✅** | **✅ (any RL task)** |

LBNE differs from EPNet and MA in that it doesn't require a pre-defined supervised learning task — the backprop signal comes from imitating the current best-performing individual, making it applicable to arbitrary reinforcement learning problems.

### Genetic Encoding

Each individual is represented by **two chromosomes**:
1. **Topology chromosome** — array of integers specifying hidden layer sizes
2. **Parameter chromosome** — weights organized *by layer*, so feature-extraction knowledge at each abstraction level can be inherited hierarchically

Crossover is applied per-layer (not per-neuron) to preserve learned feature detectors.

---

## Experiment: Autonomous Driving

The algorithm is validated on a 3D circular race track simulation built with **Three.js + TensorFlow.js** (GPU-accelerated, runs in browser).

**Agent perception:**
- 5 ultrasonic distance sensors at different forward angles
- 1 speed indicator
- Total input size: 6

**Agent output:**
- Steering angle in range [−π/8, π/8]
- Acceleration in range [−1, 1]

**Fitness function:**

```
Score_t = Score_(t-1) + Speed × 2 − |Direction| × 3 − 1
```

- Rewards sustained speed
- Penalizes zigzag driving (angle offset)
- The constant −1 eliminates stalling agents

**Comparison:** LBNE vs. pure GA baseline — LBNE converges faster and achieves higher fitness per generation.

---

## Files

| File | Description |
|---|---|
| `track_example.html` | Interactive browser simulation (Three.js + TF.js) |
| `2D auto driving AI.pdf` | Full paper (8 pages) — IEEE format |
| `Artificial_life_proposal_.pdf` | Initial project proposal |

---

## Run the Simulation

```bash
# Open directly in browser (or serve for full functionality):
python -m http.server 8000
# then open http://localhost:8000/track_example.html
```

The simulation runs in your browser using WebGL (Three.js) for rendering and TensorFlow.js for neural network inference — no Python or server required for the demo.

---

## Related Work

| Paper | Relevance |
|---|---|
| Stanley & Miikkulainen — NEAT (2002) | Topology-evolving GA baseline |
| Yao — EPNet (1999) | Early hybrid GA + backprop, fixed task |
| Such et al. — ES for RL (2017) | Large-scale neuroevolution |
| Clune et al. (2019) — Nature | Motivating hybrid NE + deep learning |
