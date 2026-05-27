# 2D Autonomous Driving AI

A **neural network agent** trained to navigate a 2D track simulation without any hard-coded rules. The agent learns purely through reinforcement signals from its environment.

📄 **[Read the Paper](https://github.com/woodyhoko/2D-auto-driving-AI/blob/main/2D%20auto%20driving%20AI.pdf)**

---

## Overview

The project implements a self-driving AI in a top-down 2D simulation:

- The **car agent** uses proximity sensors (raycasts) as input to a neural network
- The **network output** controls steering angle and throttle
- Training uses an evolutionary / neuroevolution approach — no gradient backprop; populations of agents compete and mutate over generations
- A browser-based HTML track (`track_example.html`) demonstrates the simulation environment

---

## Key Features

- 🧬 **Evolutionary training** — population-based optimization without labeled data
- 📡 **Sensor-based perception** — raycast distance sensors around the car
- 🌐 **Browser visualization** — the simulation runs in `track_example.html`
- 📊 **Full methodology** documented in the accompanying paper

---

## Files

| File | Description |
|---|---|
| `track_example.html` | Interactive 2D simulation environment |
| `2D auto driving AI.pdf` | Research paper — architecture, training, results |
| `Artificial_life_proposal_.pdf` | Initial project proposal |

---

## Run the Simulation

```bash
# Open the browser simulation
open track_example.html
# or serve it:
python -m http.server 8000
```

