# Swarm Intelligence & Nature-Inspired Optimization Algorithms

> **What is Swarm Intelligence?**
> Swarm intelligence is the collective behavior of simple agents (like ants, bees, or birds) that produces intelligent global outcomes — without any central control. Think of how a colony of ants finds the shortest path to food, even though no single ant "knows" the whole map.

These notes cover four key swarm/population-based metaheuristic algorithms:
1. [Ant Colony Optimization (ACO)](#1-ant-colony-optimization-aco)
2. [Particle Swarm Optimization (PSO)](#2-particle-swarm-optimization-pso)
3. [Artificial Bee Colony (ABC)](#3-artificial-bee-colony-abc)
4. [Cuckoo Search Algorithm (CSA)](#4-cuckoo-search-algorithm-csa)

---

## What is a Metaheuristic Algorithm?

A **metaheuristic** is a high-level, problem-independent strategy used to find near-optimal solutions to complex problems where exact methods are too slow or impossible.

| Property | Description |
|---|---|
| Does NOT guarantee the exact optimal | Finds a "good enough" solution |
| Works for large search spaces | Efficient even with millions of possibilities |
| Iterative | Improves the solution over many cycles |

**Two broad types:**

- **Single-solution based** — works on one solution at a time (e.g., Hill Climbing, Simulated Annealing)
- **Population-based** — works on many solutions simultaneously (e.g., ACO, PSO, ABC, Genetic Algorithms)

All four algorithms in these notes are **population-based**.

---

## 1. Ant Colony Optimization (ACO)

### Overview

- **Proposed by:** Marco Dorigo (1992)
- **Inspired by:** How real ants find the shortest path to food using pheromone trails
- **Type:** Population-based, cooperative metaheuristic
- **Communication:** Indirect (via environment — no direct signaling between ants)

### Real-Ant Inspiration

Real ants solve the shortest-path problem naturally:

1. Ants wander randomly, searching for food.
2. When food is found, the ant returns to the nest, leaving a **pheromone trail**.
3. Other ants detect the pheromone and prefer stronger (more pheromone) paths.
4. Shorter paths get reinforced faster (more round trips = more pheromone per unit time).
5. Longer paths lose pheromone due to **evaporation** and are eventually abandoned.
6. The **shortest path emerges** automatically — no ant planned it!

> This is **emergent behavior**: complex, intelligent results from simple local rules.

### Core Components

| Component | Role |
|---|---|
| **Ants** | Agents that construct candidate solutions |
| **Pheromone (τ)** | Numerical value on each path edge — represents how desirable/promising that path is |
| **Heuristic Information (η)** | Problem-specific knowledge (e.g., shorter distance = better) |
| **Pheromone Evaporation** | Reduces pheromone over time, preventing the algorithm from getting stuck early |

### Key Formulas

**Heuristic value** (for shortest-path problems):

$$\eta_{ij} = \frac{1}{d_{ij}}$$

where $d_{ij}$ is the distance between nodes $i$ and $j$. Shorter distances → higher heuristic value.

**Probability of choosing path (i → j):**

$$P_{ij} = \frac{[\tau_{ij}]^{\alpha} \cdot [\eta_{ij}]^{\beta}}{\sum_{k} [\tau_{ik}]^{\alpha} \cdot [\eta_{ik}]^{\beta}}$$

| Symbol | Meaning |
|---|---|
| $\tau_{ij}$ | Pheromone on edge (i, j) — "past experience" |
| $\eta_{ij}$ | Heuristic value — "current knowledge" |
| $\alpha$ | How much pheromone matters (higher = follow trails more) |
| $\beta$ | How much heuristic matters (higher = prefer shorter edges more) |

![ACO Formulas](images/aco_formulas.png)

**Pheromone update after each iteration:**

*Evaporation (forget old trails):*
$$\tau_{ij} \leftarrow (1 - \rho) \cdot \tau_{ij}$$

*Deposition (reinforce good trails):*
$$\tau_{ij} \leftarrow \tau_{ij} + \Delta\tau_{ij}$$

where $\rho \in (0, 1)$ is the evaporation rate and $\Delta\tau_{ij}$ is added by ants that used edge (i, j).

![ACO Steps](images/aco_steps.png)

### Algorithm Steps

1. **Initialize** — Set pheromone values on all edges to a small constant. Place ants randomly.
2. **Construct Solutions** — Each ant builds a complete solution (e.g., a tour in TSP) using the probability formula above.
3. **Evaluate Solutions** — Calculate the fitness (quality) of each ant's solution.
4. **Update Pheromones** — Apply evaporation, then let ants deposit pheromone proportional to their solution quality.
5. **Repeat** — Go back to step 2 until a stopping condition is met (max iterations or convergence).
6. **Output** — Return the best solution found.

### ACO Flowchart

![ACO Flowchart](images/aco_flowchart.png)

### Key Concepts to Remember

- **Positive feedback:** Good paths get more pheromone → more ants follow them → even more pheromone.
- **Negative feedback:** Evaporation removes pheromone from unused/bad paths, preventing premature convergence.
- **No central control:** The optimal solution emerges from local interactions.

### ACO Variants

| Variant | Key Idea |
|---|---|
| Best-Worst AS | Only best and worst ants update pheromone |
| MAX-MIN AS | Pheromone bounded between a min and max value |
| Rank-Based AS | Ants are ranked; higher-ranked ants deposit more pheromone |
| Ant-Q | Combines ACO with Q-learning (reinforcement learning) |

---

## 2. Particle Swarm Optimization (PSO)

### Overview

- **Proposed by:** Kennedy & Eberhart (1995)
- **Inspired by:** Flocking behavior of birds and schooling of fish — collective social intelligence
- **Type:** Population-based, stochastic optimization
- **Key idea:** Each particle (solution) moves through the search space influenced by its own best experience AND the swarm's best experience

### Nature Analogy

Imagine a flock of birds searching for the best feeding spot. Each bird:
- Remembers the best spot it personally visited (**personal best**).
- Knows the best spot any bird in the flock has found (**global best**).
- Adjusts its flying direction based on both.

Over time, the whole flock converges toward the best feeding spot.

### Core Concepts

| Term | Meaning |
|---|---|
| **Particle** | One candidate solution |
| **Position (x)** | The current solution value |
| **Velocity (v)** | The rate and direction of change of position |
| **pBest** | Best position the particle has personally found (cognitive component) |
| **gBest** | Best position found by any particle in the swarm (social component) |

### Key Formulas

**Velocity update:**

$$v(t+1) = w \cdot v(t) + c_1 r_1 (pBest - x(t)) + c_2 r_2 (gBest - x(t))$$

| Term | Meaning |
|---|---|
| $w \cdot v(t)$ | **Inertia** — keeps the particle moving in the same direction (exploration vs exploitation trade-off) |
| $c_1 r_1 (pBest - x)$ | **Cognitive component** — pull toward personal best |
| $c_2 r_2 (gBest - x)$ | **Social component** — pull toward global best |
| $w$ | Inertia weight (larger = more exploration; smaller = more exploitation) |
| $c_1, c_2$ | Cognitive and social coefficients |
| $r_1, r_2$ | Random numbers in [0, 1] — add stochasticity |

**Position update:**

$$x(t+1) = x(t) + v(t+1)$$

### Algorithm Steps

1. **Initialize** — Generate particles at random positions with random velocities. Set each particle's pBest = its initial position.
2. **Evaluate Fitness** — Calculate how good each particle's current position is.
3. **Update pBest** — If a particle's current position is better than its personal best, update pBest.
4. **Update gBest** — If any particle's pBest is better than the global best, update gBest.
5. **Update Velocity** — Apply the velocity formula above.
6. **Update Position** — Move each particle using the position formula.
7. **Repeat** — Go to step 2 until stopping condition (max iterations or convergence).
8. **Output** — Return gBest as the optimal solution.

### PSO Flowchart

![PSO Flowchart](images/pso_flowchart.png)

### Advantages vs Limitations

| Advantages | Limitations |
|---|---|
| Simple to implement | Can get stuck in local optima (premature convergence) |
| Few parameters to tune | Weak at fine-grained local search |
| Fast convergence | Sensitive to parameter choices |
| No gradient needed | High cost for very large swarms |
| Works for complex, nonlinear problems | Poor performance on discrete problems |

### Key Insight

The velocity equation balances three forces: **momentum** (keep going), **personal learning** (return to your best), and **social learning** (follow the swarm's best). This balance drives convergence to good solutions.

---

## 3. Artificial Bee Colony (ABC)

### Overview

- **Inspired by:** Foraging behavior of honeybee colonies
- **Type:** Population-based, swarm intelligence algorithm
- **Key idea:** Different bees play different roles — exploitation of known sources, exploration of new ones, and probabilistic selection

### Two Pillars of Swarm Intelligence (relevant to ABC)

**1. Self-Organization:** Global order arises from local interactions (no central control).

**2. Division of Labor:** Different agents perform specialized tasks in parallel — improving efficiency and adaptability.

> Both are necessary for swarm intelligence to work!

### Bee Roles

| Bee Type | Role in Algorithm |
|---|---|
| **Employed Bees** | One per food source. Exploit known food sources using local search. |
| **Onlooker Bees** | Wait in the hive. Observe employed bees' dances. Select food sources probabilistically based on quality. |
| **Scout Bees** | When a food source is abandoned (too poor), a scout randomly finds a new source (exploration). |

### Biological Mapping

| Biology | Algorithm |
|---|---|
| Food source | Candidate solution |
| Nectar amount | Fitness (quality of solution) |
| Bee | Agent/search entity |
| Best food source | Optimal solution |
| Waggle dance | Information sharing mechanism |

### Waggle Dance

In a real hive, bees communicate using the **waggle dance** — a movement that encodes the direction, distance, and quality of a food source. Onlooker bees watch multiple dances and choose which source to visit based on how energetic (profitable) the dance is. Better sources attract more bees → **positive feedback**.

### Key Formulas

**Local search (Employed Bee update):**

$$v_{ij} = x_{ij} + \phi_{ij}(x_{ij} - x_{kj})$$

where $x_{ij}$ is the current solution, $x_{kj}$ is a randomly chosen neighbor solution, and $\phi_{ij}$ is a random number in [-1, 1]. This generates a nearby candidate solution.

**Selection probability (Onlooker Bee):**

$$P_i = \frac{fit_i}{\sum fit_i}$$

Higher fitness → higher probability of being selected by onlookers. This is **fitness-proportionate selection**.

### Algorithm Steps

1. **Initialize** — Generate food sources (solutions) randomly.
2. **Employed Bee Phase** — Each employed bee performs local search around its source. If better, it replaces the old source (greedy selection).
3. **Onlooker Bee Phase** — Onlookers select food sources based on probability $P_i$. They also perform local search, focusing effort on better sources.
4. **Scout Bee Phase** — Any source that hasn't improved after a set number of trials (the "limit") is abandoned. The employed bee becomes a scout and randomly generates a new source.
5. **Memorize Best** — Track and store the best solution found so far.
6. **Repeat** — Until stopping condition is met.
7. **Output** — Best solution found.

### ABC Flowchart

![ABC Flowchart](images/abc_flowchart.png)

### Feedback Mechanisms Summary

| Mechanism | Effect |
|---|---|
| **Positive Feedback** | Good sources attract more bees (via waggle dance) → exploitation |
| **Negative Feedback** | Poor sources are abandoned → prevents stagnation |
| **Fluctuations (Randomness)** | Scout bees explore randomly → prevents getting stuck in local optima |

### Key Insight

The three bee roles naturally balance **exploration** (scouts finding new areas) and **exploitation** (employed + onlookers refining known solutions) — a fundamental challenge in all optimization.

---

## 4. Cuckoo Search Algorithm (CSA)

### Overview

- **Proposed by:** Xin-She Yang & Suash Deb (2009)
- **Inspired by:** The brood parasitism of cuckoo birds
- **Type:** Population-based metaheuristic
- **Key mechanism:** Lévy flights for exploration + probabilistic abandonment of poor solutions

### Biological Inspiration: Cuckoo Brood Parasitism

Cuckoos are fascinating — they **never raise their own chicks**. Instead:

1. A cuckoo lays its egg in another bird's nest (the host).
2. The cuckoo egg mimics the host's eggs to avoid detection.
3. The cuckoo chick hatches early and pushes other eggs out of the nest.
4. If the host detects the intruder egg, it ejects it (or abandons the nest entirely).

This creates an evolutionary arms race: hosts evolve better detection, cuckoos evolve better mimicry.

**Algorithm mapping:** Eggs that pass detection survive → "best solutions survive."

### Three Rules of CSA

1. Each cuckoo lays **one egg** in a **randomly chosen** nest.
2. The **best nests** (highest quality eggs) are **carried forward** to the next generation.
3. A fraction $P_a$ of the worst nests are **discovered and abandoned** — new ones are built (random new solutions generated).

### Biological-to-Algorithm Mapping

| Biology | Algorithm |
|---|---|
| Nest | Candidate solution (a point in search space) |
| Egg | New generated solution |
| Egg fitness / mimicry quality | Solution quality (fitness value) |
| Host detection & rejection | Evaluation + discarding worse solutions |
| $P_a$ (discovery probability) | Probability that the worst solutions are removed |

### Lévy Flight — The Core Exploration Mechanism

Instead of taking uniform random steps (like a simple random walk), CSA uses **Lévy flights** to generate new solutions.

**What is a Lévy flight?**
- A random walk where most steps are small (local search), but occasionally there are very long jumps (global exploration).
- This "heavy-tailed" behavior avoids getting stuck in local optima.

**Lévy distribution formula:**

$$\text{Lévy} \sim t^{-\lambda}, \quad 1 < \lambda \leq 3$$

**Why Lévy flights are powerful:**

| Normal Random Walk | Lévy Flight |
|---|---|
| Steps are uniformly sized | Steps are mostly small, with rare large jumps |
| Explores locally | Explores both locally and globally |
| May miss distant optima | Can escape local minima |

### Algorithm Steps

1. **Initialize** — Generate $n$ host nests (solutions) randomly in the search space.
2. **Generate New Solution** — Pick a cuckoo and generate a new solution using a Lévy flight.
3. **Evaluate Fitness** — Compute the quality of the new solution.
4. **Compare & Replace** — Randomly pick an existing nest $j$. If the new solution is better than nest $j$, replace it.
5. **Abandon Worst Nests** — With probability $P_a$, discard the worst nests and generate new ones via Lévy flight.
6. **Keep Best** — Always preserve the best solutions found so far.
7. **Repeat** — Until max iterations or convergence.
8. **Output** — The best solution (the "finest nest").

### CSA Flowchart

![CSA Flowchart](images/csa_flowchart.png)

### Key Insight

Lévy flights provide a natural balance of exploration (rare long jumps to discover new regions) and exploitation (frequent small steps to refine current solutions). Combined with probabilistic abandonment of poor nests, CSA efficiently navigates complex search spaces.

---

## Comparison of All Four Algorithms

| Feature | ACO | PSO | ABC | CSA |
|---|---|---|---|---|
| **Year** | 1992 | 1995 | ~2005 | 2009 |
| **Inspired by** | Ant foraging | Bird flocking | Bee foraging | Cuckoo parasitism |
| **Communication** | Indirect (pheromones) | Direct (shared gBest) | Waggle dance | None (fitness comparison) |
| **Exploration mechanism** | Random ant paths | Inertia + randomness | Scout bees | Lévy flights |
| **Exploitation mechanism** | Pheromone trails | pBest + gBest pull | Employed + Onlooker bees | Greedy replacement |
| **Memory** | Pheromone matrix | pBest per particle | Food source quality | Best nest retained |
| **Handles discrete problems?** | ✅ Excellent (e.g., TSP) | ❌ Poor | ✅ Yes | ✅ Yes |
| **Key parameter** | $\alpha, \beta, \rho$ | $w, c_1, c_2$ | Limit (abandonment threshold) | $P_a$ (abandonment probability) |

---

## Summary: The Big Picture

All four algorithms share a common philosophy:

- **No central control** — intelligence emerges from local interactions
- **Balance exploration & exploitation** — search new areas while refining known good ones
- **Iterative improvement** — solutions get better over time through repeated cycles
- **Stochastic** — randomness is a feature, not a bug — it enables diverse exploration

They differ mainly in *how* they implement exploration and exploitation, drawing inspiration from different aspects of nature.

---

*Notes compiled from: ACO (Dorigo, 1992), PSO (Kennedy & Eberhart, 1995), ABC, and CSA (Yang & Deb, 2009)*
