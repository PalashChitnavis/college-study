# Soft Computing — Complete Notes

## 1. Soft Computing vs Hard Computing

| Parameter | Soft Computing (SC) | Hard Computing (HC) |
|-----------|--------------------|--------------------|
| **Data Handling** | Imprecise, incomplete, fuzzy, missing, etc. | Exact, complete, no missing info, precise (always) |
| **Output** | Approximate (helpful for real life) | Exact |
| **Adaptability** | High | Very low |
| **Logic** | Probabilistic, fuzzy, continuous logic | Discrete |
| **Real World Usability** | Used more | Very less usable |

> **When to use SC:** When data is uncertain, hidden, missing, probabilistic, continuous, or fuzzy.

> **Key History:** Initially feedforward models were used, but they weren't sufficient, so backpropagation type models were introduced.

### Complexity Classes
- **P (Polynomial):** As input size grows, computation goes in polynomial time.
- **NP (Non-deterministic Polynomial):** As input size grows, computation becomes *deterministic* polynomial time.
- **NP-Hard:** Problems that are at least as hard as NP problems.

---

## 2. Models of a Neuron

Three main models:
1. **MP Model** (McCulloch-Pitts)
2. **Rosenblatt's Perceptron Model**
3. **ADALINE** (Adaptive Linear Element)

### 2.1 MP (McCulloch-Pitts) Neuron

**Structure:**
- Inputs: $x_1, x_2, \ldots, x_n$ with weights $w_1, w_2, \ldots, w_n$
- Activation: $\sum w_i x_i - \theta$ → step function $f$ → output

**Decision Rule:**
$$\text{output} = \begin{cases} 1 & \text{if } S \geq 0 \\ 0 & \text{otherwise} \end{cases}$$

where $S = \sum w_i x_i$

**Hard Inhibitory (Veto) Rule:** If any inhibitory input $= 1$, output $= 0$. Else calculate weighted sum of excitatory inputs.

---

### 🔢 MP Neuron — Worked Example

**Problem:** 6 inputs $x_1$ to $x_6$. Excitatory: $x_1$ to $x_4$. Inhibitory: $x_5, x_6$.

Weights: $w_1=2, w_2=1, w_3=3, w_4=2$. Threshold $\theta = 5$.

Weighted sum of excitatory = $2x_1 + x_2 + 3x_3 + 2x_4$

Decision: if $S \geq \theta$ → output $= 1$, else $= 0$

| Case | $x_1$ | $x_2$ | $x_3$ | $x_4$ | $x_5$ | $x_6$ | S | o/p |
|------|-------|-------|-------|-------|-------|-------|---|-----|
| A    | 1     | 1     | 0     | 1     | 0     | 0     | 5 | 1   |
| B    | 1     | 0     | 1     | 0     | 0     | 0     | 5 | 1   |
| C    | 1     | 1     | 0     | 1     | 0     | 0     | 6 | 1   |
| D    | 0     | 1     | 1     | 1     | 0     | 1(inh)| — | 0   |
| E    | 0     | 0     | 1     | 1     | 0     | 0     | 5 | 1   |
| F    | 1     | 0     | 1     | 1     | 1(inh)| 0    | — | 0   |
| G    | 1     | 1     | 0     | 0     | 0     | 0     | 3 | 0   |
| H    | 0     | 1     | 0     | 1     | 1(inh)| 0    | — | 0   |

---

### 2.2 ADALINE Model

**Structure:**
- Inputs $x_1 \ldots x_n$ with weights $w_i$
- Activation: $\alpha = \sum w_i x_i - \theta$
- Output: $s = f(\alpha) = \alpha$ (linear)

**Learning Rule (LMS/Widrow-Hoff):**
$$\Delta w_i = \eta \cdot \delta \cdot x_i$$
$$\Delta \theta_i = \eta \delta$$

where $\delta = b - s = b - \alpha$ (error), $b$ = target output, $\eta$ = learning rate.

---

### 🔢 ADALINE — Worked Example

**Given:** $x_1 = 1,\ x_2 = -1,\ b = +1,\ w_1 = 0.2,\ w_2 = -0.1,\ \theta = 0.05,\ \eta = 0.1$

**Iteration 1:**

$$\alpha = \sum w_i x_i - \theta = (0.2)(1) + (-0.1)(-1) - 0.05 = 0.2 + 0.1 - 0.05 = 0.25$$

$$\delta = b - \alpha = 1 - 0.25 = 0.75$$

$$\Delta w_1 = \eta \cdot \delta \cdot x_1 = 0.1 \times 0.75 \times 1 = 0.075 \Rightarrow w_1' = 0.275$$

$$\Delta w_2 = \eta \cdot \delta \cdot x_2 = 0.1 \times 0.75 \times (-1) = -0.075 \Rightarrow w_2' = -0.175$$

$$\Delta \theta = \eta \delta = 0.1 \times 0.75 = 0.075 \Rightarrow \theta' = 0.125$$

**Iteration 2:**

$$\alpha' = 0.275 + 0.175 - 0.125 = 0.325$$

$$\delta' = 1 - 0.325 = 0.675$$

$$\Delta w_1' = 0.1 \times 0.675 \times 0.275 = 0.01856 \approx 0.0165$$

---

### 2.3 Rosenblatt's Perceptron Model

**Structure:** Sensory units → Association units → Response unit

**Activation (Hard Limiter):**
$$y = \begin{cases} +1 & \text{if } (w_1 x_1 + w_2 x_2 - b) \geq 0 \\ -1 & \text{otherwise} \end{cases}$$

**Weight Update Rule:**
$$w_i' = w_i + \eta \cdot \delta \cdot x_i, \quad b' = b + \eta \cdot \delta$$

where $\delta = \text{target} - \text{output}$

---

### 🔢 Perceptron — Worked Example

**Given:** 2 inputs $(x_1, x_2)$, initial $w_1 = w_2 = 0$, $b = 0$, $\eta = 1$

| Pattern | $x_1$ | $x_2$ | Target |
|---------|-------|-------|--------|
| P1      | 1     | 1     | 1      |
| P2      | 1     | -1    | -1     |
| P3      | -1    | 1     | -1     |
| P4      | -1    | -1    | -1     |

**Pass 1:**

P1: $\sum = 0 - 0 = 0 \geq 0 \Rightarrow y = 1$ ✓ (target = 1, no update)

P2: $x_1=1, x_2=-1$: $\sum = 0 \Rightarrow y = 1$, target = -1 ✗ → Update:
$$w_1' = 0 + 1\times(-1-1)\times1 = -2,\quad w_2' = 0 + 1\times(-2)\times(-1) = 2,\quad b' = -2$$

P3: $w_1=-2, w_2=2$: $\sum = (-2)(-1) + (2)(1) - (-2) = 2+2+2 = 6 \geq 0 \Rightarrow y=1$, target=-1 ✗ → Update:
$$w_1'' = -2 + 1\times(-2)\times(-1) = 2,\quad w_2'' = 0,\quad b'' = -4$$

P4: $\sum = (2)(-1) + 0 - (-4) = -2+4 = 2 \geq 0 \Rightarrow y=1$, target=-1 ✗ → update continues...

> Training continues until all patterns are correctly classified.

---

### Supervised Neural Networks
Types: Adaline, MLP, M-Adaline, Backpropagation, Feedforward

---

## 3. Fuzzy Systems

### 3.1 Introduction

**Fuzzy Logic** deals with degrees of truth rather than strict true/false.

In traditional (crisp) sets, an element either belongs (1) or doesn't (0). In fuzzy sets, membership is a value in $[0, 1]$.

### 3.2 Soft Computing vs Hard Computing — Fuzzy Angle

- **Soft Computing** handles data that is: uncertain, hidden, missing, probabilistic, continuous, or fuzzy.
- **Hard Computing** requires exact, well-defined problems.

### 3.3 Fuzzy Set Operations

For two fuzzy sets A and B:

- **Complement:** $\overline{A \cap B} = \overline{A} \cup \overline{B}$ (De Morgan's Law)
- **Intersection (AND):** min operator
- **Union (OR):** max operator

---

### 🔢 Fuzzy Set Operations — Example

**Given:**
$$A \cap B = \{(0,30), (0.1,40), (0.2,50), (0.5,60), (0.2,65), (0.1,70), (0,75), (0,80)\}$$

$$\overline{A \cap B} = \{(1,30), (0.9,40), (0.8,50), (0.5,60), (0.8,65), (0.9,70), (1,75), (1,80)\}$$

$$\overline{A} = (1, 30)$$

---

### 3.4 Membership Functions (MF)

A membership function maps each element of a universe of discourse to a value in $[0,1]$.

**Common types:**
- Triangular: tri(a, b, c) — rises linearly from a to b, falls from b to c
- Trapezoidal: flat top between two peaks
- Gaussian

---

### 3.5 Overlap Ratio and Overlap Robustness

**Overlap Ratio:**
$$\text{Overlap ratio} = \frac{|\text{overlap interval}|}{|\text{support of larger set}|}$$

**Overlap Robustness:**
$$\text{Overlap robustness} = \frac{\sum \mu_1(x) \cdot \Delta x}{\sum (\mu_1 + \mu_2) \cdot \Delta x}$$

---

### 🔢 Overlap — Example 1

Two fuzzy sets with MFs $\mu_1$ and $\mu_2$ defined over $[2, 10]$.

From the graph: $\mu_1 + \mu_2 = 1$ everywhere (they sum to 1).

Overlap robustness $= \frac{1}{2}$

---

### 🔢 Overlap — Example 2

**Given intervals and $\mu_1 + \mu_2$ values:**

| Interval | $\mu_1 + \mu_2$ |
|----------|----------------|
| 0 – 2    | 0.6            |
| 2 – 4    | 1.2            |
| 4 – 6    | 0.8            |

$$\text{Overlap robustness} = \frac{0.6 \times 2 + 1.2 \times 2 + 0.8 \times 2}{2 \times (6 - 0)} = \frac{2 \times (0.6 + 1.2 + 0.8)}{2 \times 6} = \frac{2 \times 2.6}{12} = 0.43$$

---

### 🔢 Overlap — Example 3 (Overlap Ratio & Robustness)

**Given:** Fuzzy set 1: support $[0, 10]$; Fuzzy set 2: support $[6, 16]$

Overlap interval $= [6, 10]$, length $= 4$

Assume $\mu_1(x) + \mu_2(x) \leq 1$

$$\text{Overlap ratio} = \frac{4}{16} = \frac{1}{4}$$

$$\text{Overlap robustness} = \frac{2 \times 4}{2 \times 4} \times \frac{1 \times (10-6)}{2 \times (10-6)} = 0.5$$

---

### 3.6 Defuzzification

Converting a fuzzy output set to a single crisp value.

**Methods:**
1. **Centroid / CoA / CoG** — Center of Area/Gravity
2. **MoM** — Mean of Maximum
3. **SoM** — Smallest of Maximum
4. **LoM** — Largest of Maximum
5. **Bisector of Area**

---

#### Centroid (CoA) Formula

**Discrete:**
$$z^* = \frac{\sum z \cdot \mu(z)}{\sum \mu(z)}$$

**Continuous:**
$$z^* = \frac{\int z \cdot \mu(z)\, dz}{\int \mu(z)\, dz}$$

---

### 🔢 Defuzzification — Centroid Example 1 (Discrete)

**Given:** $z = 10, 20, 30, 40, 50$; $\mu(z) = 0.1, 0.4, 0.8, 0.4, 0.1$

$$z^* = \frac{10(0.1) + 20(0.4) + 30(0.8) + 40(0.4) + 50(0.1)}{0.1 + 0.4 + 0.8 + 0.4 + 0.1} = \frac{1 + 8 + 24 + 16 + 5}{1.8} = \frac{54}{1.8} = 30$$

---

### 🔢 Defuzzification — Centroid Example 2 (Triangular MF)

**Given:** Triangular fuzzy set, base $= [10, 50]$, peak $= 30$ with $\mu = 1$

$\mu$ rises linearly from 10 to 30, falls from 30 to 50.

**Rising edge:** $\mu_z = \frac{z - 10}{20}$

**Falling edge:** $\mu_z = \frac{50 - z}{20}$

$$z^* = \frac{\int_{10}^{30} z \cdot \frac{z-10}{20}\, dz + \int_{30}^{50} z \cdot \frac{50-z}{20}\, dz}{\int_{10}^{30} \frac{z-10}{20}\, dz + \int_{30}^{50} \frac{50-z}{20}\, dz}$$

After integration (by symmetry): $z^* = 30$

---

### 🔢 Defuzzification — Homework Problem

Fuzzy set: triangular, $a = 10$ (left base), $b = 40$ (peak), $c = 50$ (right base).

Rising: $\mu = \frac{z-10}{30}$, Falling: $\mu = \frac{50-z}{10}$

$$z^* = \frac{N}{D} = \frac{2000/3}{20} = \frac{100}{3} = 33.33$$

---

### 🔢 Defuzzification — CoG (Case II — Bisector/Segment method)

**Trapezoidal fuzzy set:** base $= [0, 100]$, flat top $= [40, 60]$, height $= 1$

Areas: $A_1 = 20$, $A_2 = 20$, $A_3 = 20$ → Total $= 60$

**Bisector of Area (BoA):** The vertical line that divides total area in half.

Half area $= 30$. BoA $= 50°$

---

#### MoM, SoM, LoM

**MoM (Mean of Maximum):** Average of all $z$ values where $\mu$ is maximum.

**SoM:** Smallest $z$ where $\mu$ is maximum.

**LoM:** Largest $z$ where $\mu$ is maximum.

**Physical Interpretation of MoM:** Chooses average of all strongest decisions.

---

### 🔢 MoM, SoM, LoM Example

Maximum $\mu^* = 0.8$ occurs at $z = \{20°, 25°, 30°, 35°\}$

$$\text{MoM} = \frac{20+25+30+35}{4} = 27.5°$$

$$\text{SoM} = 20°, \quad \text{LoM} = 35°$$

---

### 3.7 Steps for Fuzzy Inference System (FIS)

1. Given input and output variables, **create fuzzy sets** for each variable and decide membership functions.
2. **Calculate membership values** for all inputs for all MFs.
3. **Create rules** using fuzzy operators.
4. **Substitute membership values** in rules and calculate value of antecedent.
5. **Apply implication** (fuzzy implication — Mamdani / Sugeno).
6. If multiple outputs, perform **aggregation** for all output rules.
7. **Perform defuzzification**.

---

### 3.8 Fuzzy Implication I(a, b)

**Definition:** The implication operator tells: "If the IF PART is true to degree $a$, how strongly should the THEN PART be applied to degree $b$?"

- $I(a, b)$ gives the **truth value of the whole rule**.

**Fuzzy Relation:**
$$R(x, y) = \sum_i \mu(x_i, y_i) \cdot (x_i; w_i)$$

where $\mu(x_i, y_i)$ = membership degree of pair $(x_i, y_i)$.

$R$ tells how strongly each input-output pair is related.

---

### 3.9 Types of Fuzzy Implication

| # | Name | Formula |
|---|------|---------|
| 1 | **Mamdani** | $I(a,b) = \min(a, b)$ |
| 2 | **Larsen** | $I(a,b) = a \times b$ |
| 3 | **Godel** | $I(a,b) = \begin{cases}1 & a \leq b \\ 0 & \text{otherwise}\end{cases}$ |
| 4 | **Lukasiewicz** | $I(a,b) = \min(1,\ 1-a+b)$ |
| 5 | **Zadeh** | $I(a,b) = \max(1-a,\ \min(a,b))$ |
| 6 | **Kleene-Dienes** | $I(a,b) = \max(1-a,\ b)$ |
| 7 | **Goguen** | $I(a,b) = \begin{cases}1 & a \leq b \\ b/a & a > b\end{cases}$ |
| 8 | **Reichenbach** | $I(a,b) = 1 - a + ab$ |
| 9 | **Yager** | $I(a,b) = b^a$ (exponential) |
| 10 | **Fodor** | $I(a,b) = \begin{cases}1 & a \leq b \\ \max(1-a, b) & a > b\end{cases}$ |
| 11 | **Sugeno** | $I(a,b) = \dfrac{b}{a+b-ab}$ |
| 12 | **Gaines-Rescher** | $I(a,b) = \begin{cases}1 & a \leq b \\ 0 & a > b\end{cases}$ |
| 13 | **Wu** | $I(a,b) = \min(1, b/a)$ — ratio based |
| 14 | **Dubois-Prade** | $I(a,b) = \begin{cases}1 & a \leq b \\ \max(1-a,b) & a > b\end{cases}$ |
| 15 | **Wilmott** | $I(a,b) = \min(1,\ 1-a+ab)$ |
| 16 | **Baldwin** | $I(a,b) = \min(1,\ (1-a+b)/2)$ |
| 17 | **Nilpotent Min** | $I(a,b) = \begin{cases}1 & a \leq b \\ 1-a+b & a > b\end{cases}$ |
| 18 | **Drastic** | $I(a,b) = \begin{cases}b & a=1 \\ 1 & a<1\end{cases}$ |
| 19 | **Algebraic Product** | $I(a,b) = a \times b$ |

---

### 🔢 Lukasiewicz Implication — Cases

$I(a,b) = \min(1,\ 1-a+b)$

| Case | IF      | THEN    | $a$   | $b$   | $I(a,b)$       | Strength   |
|------|---------|---------|-------|-------|----------------|------------|
| 1    | Strong  | Weak    | 0.9   | 0.2   | $\min(1, 0.3) = 0.3$ | Weak |
| 2    | Strong  | Strong  | 0.8   | 0.8   | $\min(1, 1.0) = 1$ | Strongest |
| 3    | Weak    | Strong  | 0.1   | 0.8   | $\min(1, 1.7) = 1$ | Strongest |
| 4    | Weak    | Weak    | 0.2   | 0.2   | $\min(1, 1.0) = 1$ | Strongest |

**Key Observations:**
- If antecedent is **weak**, rule is **always strong**.
- If antecedent is **strong**, consequent needs to be strong for the rule to be strong.

---

### 🔢 Sugeno Implication — Cases

$I(a,b) = \dfrac{b}{a+b(1-a)}$

| Case | Condition |
|------|-----------|
| $b=1$ | $I=1$ |
| $a=0$ | $I=1$ |
| $a=1$ | $I=b$ (truth purely depends on THEN) |
| $b=0$ | $I=0$ |

**Numerical examples:**

1. $a=0.7, b=0.2$: $I = \frac{0.2}{0.7 + 0.2(0.3)} = \frac{0.2}{0.76} = 0.263$
2. $a=0.2, b=0.7$: $I = \frac{0.7}{0.2+0.7(0.8)} = \frac{0.7}{0.76} = 0.921$
3. $a=b=0.5$: $I = \frac{0.5}{0.5+0.5(0.5)} = \frac{0.5}{0.75} = 0.667$

---

### 🔢 Fuzzy Robotic Controller — Full Numerical

**Given:** Angular error $\alpha = +20°$, Goal distance: **near** (membership 1.0 assumed).

**Fuzzy Sets for $\alpha$:**
- more negative: $[-60°, 0°]$; less negative: $[-30°, 0°]$; no: $0$; less positive: $[0°, 30°]$; more positive: $[30°, 60°]$

**For $\alpha = 20°$:**
$$\mu(\text{no}) = \frac{30-20}{30-0} = \frac{10}{30} = 0.33$$
$$\mu(\text{less positive}) = \frac{30-20}{30-0} = 0.33$$
$$\mu(\text{more positive}) = 0, \quad \mu(\text{less negative}) = 0, \quad \mu(\text{more negative}) = 0$$

**Rules:**
- Rule 1: If $\alpha = 0$ AND distance near → steering straight → $\min(0.33, 1.0) = 0.33$
- Rule 2: If $\alpha$ is less positive AND distance near → steering right → $\min(0.33, 1.0) = 0.33$

**Centroid:**
$$c = \frac{0 \times 0.33 + 20 \times 0.33}{0.33 + 0.33} = \frac{6.6}{0.66} = 10°$$

---

## 4. Genetic Algorithms (GA)

### 4.1 Theory

Genetic Algorithms are **evolutionary optimization techniques** inspired by natural selection. They work on a population of candidate solutions (chromosomes).

**Basic flow:**
1. Initialize population (encoding chromosomes)
2. Evaluate fitness
3. Selection
4. Crossover (recombination)
5. Mutation
6. Repeat until convergence

---

### 4.2 Encoding Schemes for GA

| # | Type | Description | Example |
|---|------|-------------|---------|
| 1 | **Continuous Value** | Real numbers, used for continuous solutions | $c_1 = 9846.1002$ |
| 2 | **Binary Encoding** | Bits 0 and 1 | $c_1 = 1001010$ |
| 3 | **Integer Encoding** | Integers | $c_1 = [2, 4, 6, 8, 10, 12]$ |
| 4 | **Value/Real Encoding** | Real-valued genes | $c_2 = \{\text{up, left, down, right}\}$ |
| 5 | **Tree Encoding** | Represents programs/expressions | $(a+b)\times(c-d)$ |
| 6 | **Permutation Encoding** | Ordered sequence, each gene appears once | TSP routes |

**Gene → Chromosome (encoding) → Genotype → (decoding) → Phenotype**

---

### 🔢 GA — Evolutionary Algorithm Example

**Problem:** Maximize $f(x) = \sin(x)$ where $x \in [1, 15]$ using Evolutionary Algorithm.

| Chromosome (binary) | $x$ | $\sin(x)$ |
|---------------------|-----|-----------|
| 0001                | 1   | 0.84      |
| 0010                | 2   | 0.90      |
| 0011                | 3   | 0.14      |
| 0100                | 4   | -0.76     |
| …                   | …   | …         |
| 1110                | 14  | 0.99      |

Best: chromosome 1110 (x=14, sin(14)≈0.99)

---

### 4.3 Fitness Scaling

Maps raw fitness scores to scaled values:

| Raw score | Scaled | Quality |
|-----------|--------|---------|
| -1 → 0    | 0      | Worst   |
| 0 → 50    | 50     | Average |
| 1 → 100   | 100    | Best    |

**Formula:** $\text{Scaled} = (b+1) \times 50$ where $b$ is raw fitness.

---

### 4.4 Crossover Types

#### 1. Uniform Crossover (Binary)

**Rule:** If mask $M = 1$ → take gene from P1; if $M = 0$ → take from P2.

**Example:**

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|----------|---|---|---|---|---|---|---|---|
| Mask M   | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 |
| P1       | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 |
| P2       | 0 | 1 | 0 | 0 | 1 | 1 | 0 | 1 |
| Child C  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |

Child $C_2$ (complement) $= 00000000$

---

#### 2. Uniform Crossover (Real-valued)

$P1 = [6.0, 3.0, 2.0]$, $P2 = [2.0, 5.0, 8.0]$, Mask $M = [1, 0, 1]$

Child $C = [6, 5, 2]$

---

#### 3. Arithmetic Crossover

$$C_1 = \alpha P_1 + (1-\alpha)P_2$$
$$C_2 = (1-\alpha)P_1 + \alpha P_2$$

**Example:** $P1 = (8, 2)$, $P2 = (2, 10)$, $\alpha = 0.3$

$$C_1 = 0.3 \times 8 + 0.7 \times 2 = 2.4 + 1.4 = 3.8$$
$$C_1(y) = 0.3 \times 2 + 0.7 \times 10 = 7.6$$

So $C_1 = (3.8, 7.6)$

$$C_2 = 0.7 \times 8 + 0.3 \times 2 = 6.2$$
$$C_2(y) = 0.7 \times 2 + 0.3 \times 10 = 7.6 \rightarrow C_2 = (6.2, 7.6)$$

---

#### 4. Arithmetic Crossover with Mutation

$P1 = [5.0, 1.5, 9.0, 2.0]$, $P2 = [1.0, 4.5, 3.0, 8.0]$, $\alpha = 0.25$

After crossover, apply mutation to Child 1 by adding $A = [+0.2, -0.1, 0, +0.4]$:

$C_1 = [2.0, 3.75, 4.5, 6.5]$

Final $C_1 = [2.2, 3.65, 4.5, 6.9]$ (after adding mutation)

Final $C_2 = [4.2, 2.15, 7.5, 3.9]$

---

#### 5. Shuffle Crossover

1. Shuffle parents using permutation $\pi$
2. Apply 1-point crossover
3. Unshuffle using mask

**Example:** $P1 = [A, B, C, D, E, F]$, $P2 = [a, b, c, d, e, f]$, $\pi = [3, 6, 1, 5, 2, 4]$

After crossover at position 4 and unshuffling: $C_1 = [A, b, C, d, E, F]$

---

#### 6. Order-Changing Crossover

$P1 = (1, 2, 3, 4, 5, 6)$, $P2 = (2, 3, 4, 6, 7, 8)$

$C = P1 + P2 = (3, 5, 7, 10, 12, 14)$

$C_{\text{order}} = (14, 12, 10, 7, 5, 3)$ (descending)

---

### 4.5 Mutation Types

Starting chromosome: $C = [1, 2, 3, 4, 5, 6, 7]$

| # | Type | Result |
|---|------|--------|
| 1 | **Inversion** | $C_i = [1, 2, 6, 5, 4, 3, 7]$ |
| 2 | **Insertion** | $C_{ins} = [1, 2, 6, 4, 5, 7]$ |
| 3 | **Displacement** | $C_D = [1, 3, 4, 5, 2, 6, 7]$ |
| 4 | **Swap (Reciprocal Exchange)** | $C_S = [1, 2, 5, 4, 3, 6, 7]$ |
| 5 | **Shift right** | $C_{sh} = [4, 1, 2, 3, 7, 5, 6]$ |
| 5 | **Shift left** | $C_{sm} = [5, 1, 2, 3, 4, 7, 5, 6]$ |

---

### 4.6 Schema Theory

A **schema** is a template describing a set of chromosomes with fixed bits at certain positions.

Example: $H = 1 \times 0 \times 1$

- **Order** $o(H)$ = number of fixed (constant) bits $= 3$
- **Length** $l(H)$ = position of last fixed bit − position of first fixed bit $= 5$

**Example 2:** $H_2 = X1X0XX1X$
- Order $= 3$, Length $= 6$

---

### 4.7 Schema Fragility (Probability of Disruption)

For **1-point crossover:**

$$P_d = \frac{l(H)}{L-1}$$

$$P_s = 1 - P_d = \frac{L - 1 - l(H)}{L - 1}$$

where $L$ = chromosome length, $l(H)$ = schema length.

---

### 🔢 Schema Fragility — Example

**Given:** Chromosome length $L = 12$, 1-point crossover.

$H_A = 1 \times 0 \times\times\times\times\times\times\times\times$ → $l = 2$, $P_{dA} = \frac{2}{11}$, $P_{sA} = \frac{9}{11}$

$H_B = \times\times1\times\times\times\times\times\times\times0$ → $l = 9$, $P_{dB} = \frac{9}{11}$, $P_{sB} = \frac{2}{11}$

For **2-point crossover:**

$P_{dA} = \frac{1}{10}$, $P_{dB} = \frac{8}{10}$, $P_{sA} = \frac{9}{10}$, $P_{sB} = \frac{2}{10}$

---

### 4.8 Tree Encoding — Crossover Example

$P1 \Rightarrow (x - y) \times (y - z)$ and $P2 \Rightarrow (x - y) + (4 \times z)$

After subtree crossover (swapping subtrees): new offspring expressions are generated.

---

### 4.9 Permutation Encoding (for TSP)

**Traveling Salesman Problem (TSP):**
- Cities: $\{1, 2, 3, 4, 5\}$
- One gene = one city
- Chromosome = entire route
- Genotype = permutation sequence
- Phenotype = actual travel route and total distance

---

## 5. RBF Networks (Radial Basis Function)

### 5.1 Theory

RBF networks have:
- **Input layer**
- **Hidden layer** with RBF neurons (each with a center and spread)
- **Output layer**

**Gaussian RBF:**
$$\phi(z) = \exp\!\left(\frac{-\|x - c_j\|^2}{2\sigma^2}\right)$$

**Output:**
$$y = \sum_{j=1}^{h} \phi_j \cdot w_j$$

---

### 🔢 RBF — Example 1

**Given:** Input $x = [2, 3]$, 2 Gaussian RBF neurons.
- Neuron 1: centre $c_1 = (1, 2)$, spread $\sigma_1 = 1$, output weight $w_1 = 0.5$
- Neuron 2: centre $c_2 = (3, 4)$, spread $\sigma_2 = 2$, output weight $w_2 = 0.8$

**For Neuron 1:**
$$\phi_1 = \exp\!\left(\frac{-(2-1)^2 + (3-2)^2}{2 \times 1}\right) = e^{-1} \approx 0.3679$$

**For Neuron 2:**
$$\phi_2 = \exp\!\left(\frac{-(2-3)^2 + (3-4)^2}{2 \times 4}\right) = e^{-0.25} \approx 0.776$$

$$y = w_1\phi_1 + w_2\phi_2 = 0.5 \times 0.3679 + 0.8 \times 0.776 = 0.184 + 0.621 = 0.807$$

---

### 🔢 RBF — Example 2 (3 neurons, 3D input)

**Given:** $x = (2, -1, 3)$
- N1: $c_1=(1,0,2)$, $\sigma=1$, $w_1=0.6$
- N2: $c_2=(3,-2,4)$, $\sigma=2$, $w_2=0.3$
- N3: $c_3=(2,-1,1)$, $\sigma=1.5$, $w_3=0.5$

$$\phi_1 = e^{-(1+1+1)/2} = e^{-1.5} = 0.223$$

$$\phi_2 = e^{-(1+1+1)/8} = e^{-3/8} = 0.687$$

$$\phi_3 = e^{-(4)/4.5} = e^{-4/4.5} = 0.411$$

$$y = 0.6 \times 0.223 + 0.3 \times 0.687 + 0.5 \times 0.411 = 0.1338 + 0.2061 + 0.2055 = 0.545$$

---

## 6. Feedforward Neural Network (Backpropagation)

### 6.1 Theory

A feedforward NN with backpropagation:
- Forward pass: compute output
- Compare with target → compute error
- Backward pass: update weights using gradient descent

**Activation function (Sigmoid):**
$$f(x) = \frac{1}{1 + e^{-x}}$$

**Weight update:**
$$\Delta w = \eta \cdot \delta \cdot x_j$$

For **hidden → output:** $\delta = \text{error term} = \text{target} - \text{output}$

For **input → hidden:** $\delta = f'(x) \times (\text{error} \times w_{\text{output}})$

---

### 🔢 Backpropagation — Full Worked Example

**Given:**
- 2 input neurons: $x_1 = 0.6$, $x_2 = 0.1$
- 1 hidden neuron, 1 output neuron
- Input→Hidden weights: $w_1 = 0.2$, $w_2 = 0.3$; hidden bias $= 0.1$
- Hidden→Output weight: $w_3 = 0.4$; output bias $= 0.2$
- Learning rate $\eta = 0.5$; Target $= 0.8$; Activation: sigmoid

**Step 1 — Hidden Layer Output:**

$$\text{net}_h = x_1 w_1 + x_2 w_2 + b_h = 0.6(0.2) + 0.1(0.3) + 0.1 = 0.12 + 0.03 + 0.1 = 0.25$$

Hmm, note: $0.6 \times 0.2 = 0.12$, $0.1 \times 0.3 = 0.03$, so net = $0.12 + 0.03 + 0.1 = 0.25$

Wait — notes show 0.12 but the calculation gives:

$0.6 \times 0.2 = 0.12$ + $0.1 \times 0.3 = 0.03$ + $0.1 = 0.25$

Actually notes show result = $0.16 + 0.03 + 0.1 = 0.25$ ← **(net = 0.25)**

After sigmoid: $h = \frac{1}{1+e^{-0.25}} \approx \frac{1}{1 + e^{0.16}} \approx 0.562$ ← **(notes say ≈ 0.562)**

**Step 2 — Final Output:**

$$\text{net}_o = h \times w_3 + b_o = 0.562 \times 0.4 + 0.2 = 0.225 + 0.2 = 0.425$$

After sigmoid: $o = \frac{1}{1+e^{-0.425}} \approx 0.604 \approx 0.6$

**Step 3 — Error:**

$$\text{error} = \text{target} - \text{output} = 0.8 - 0.6 = 0.2$$

**Step 4 — Update Hidden→Output Weights:**

$$\Delta w_3 = \eta \cdot \delta \cdot h = 0.5 \times 0.2 \times 0.562 = 0.056 \Rightarrow w_3' = 0.456$$

$$\Delta b_o = \eta \cdot \delta = 0.5 \times 0.2 = 0.01 \Rightarrow b_o' = 0.3$$

**Step 5 — Update Input→Hidden Weights:**

Hidden error signal: $\delta_h = \text{error} \times w_3 = 0.2 \times 0.4 = 0.08$ (backpropagated)

Using sigmoid derivative: $f'(x) = f(x)(1-f(x)) \approx 0.562 \times (1-0.562) \approx 0.246$

Wait — notes use a simpler version. From notes directly:

$$\Delta w_1 = \eta \delta x_1 = 0.5 \times 0.2 \times 0.6 = 0.06 \Rightarrow w_1' = 0.26$$

$$\Delta w_2 = 0.5 \times 0.2 \times 0.1 = 0.01 \Rightarrow w_2' = 0.31$$

$$\Delta b_h = \eta \delta = 0.5 \times 0.2 = 0.1 \Rightarrow b_h' = 0.2$$

---

## 7. Self Organizing Maps (SOMs)

### 7.1 Theory

**Proposed by:** Kohonen (also called Kohonen maps)

**Type:** Unsupervised neural network

**Purpose:** Maps high-dimensional data to low-dimensional (usually 2D) output.

**Structure:**
- Input layer → (Euclidean distance calculation) → Competitive layer → Grid of neurons

**Key idea:** Neurons compete to be the "winner" for each input pattern.

**Block diagram:**
```
Input layer → Distance calculation → Competitive layer → Grid of neuron mapped
```

**BMU (Best Matching Unit):** The neuron with the minimum distance to the input.

**Weight Update Equation:**
$$w_i(t+1) = w_i(t) + \alpha(t) \cdot h_{c}(t) \cdot [x - w_i(t)]$$

where:
- $\alpha(t)$ = dynamic learning rate
- $h_c(t)$ = neighbourhood function (usually Gaussian)

**Neighbourhood function:**
$$h_{c,i}(t) = \exp\!\left(\frac{-(y_c - y_i)^2}{2\sigma^2}\right)$$

**Winning neuron** = lowest distance between input and neuron weights.

**Weights** = strength of connection (we calculate distance between input and weights).

**Clusters:** e.g., Cluster 1: 0-10, Cluster 2: 10-20, Cluster 3: >20

**Stopping criteria for SOMs:**
- Neighbourhood shrinks or minimises
- Learning rate decreases
- Max number of iterations
- Weight stabilisation

---

### 🔢 SOM — Worked Example

**Given:** Input vector $X = (0.5, 0.7)$, Neuron weights:
$$w_1 = (0.2, 0.4),\quad w_2 = (0.6, 0.8),\quad w_3 = (0.9, 0.3)$$

Learning rate $= 0.4$

**Step 1 — Compute distances:**

$$d_1 = \|X - w_1\|^2 = (0.5-0.2)^2 + (0.7-0.4)^2 = 0.09 + 0.09 = 0.18$$

$$d_2 = (0.5-0.6)^2 + (0.7-0.8)^2 = 0.01+0.01 = 0.02 \leftarrow \text{min (winner)}$$

$$d_3 = (0.5-0.9)^2 + (0.7-0.3)^2 = 0.16+0.16 = 0.32$$

**Winner** = $w_2$ (least distance). $X$ belongs to cluster of $w_2$.

**Step 2 — Weight update (assume $h_c(t) = 1$):**

$$w_2(t+1)_x = 0.6 + 0.4 \times 1 \times (0.5 - 0.6) = 0.6 - 0.04 = 0.56$$

$$w_2(t+1)_y = 0.8 + 0.4 \times (0.7 - 0.8) = 0.8 - 0.04 = 0.76$$

$$w_2(t+1) = (0.56, 0.76)$$

---

## 8. Learning Vector Quantization (LVQ)

### 8.1 Theory

LVQ is a **supervised** version of SOM that assigns class labels to prototypes.

**Algorithm Flow:**
1. Initialize prototype values
2. Select training sample
3. Compute distance from all prototypes
4. Find winner neuron
5. Check class label
   - **Correct class** → Move prototype **towards** input
   - **Wrong class** → Move prototype **away** from input
6. Update weights
7. Reduce learning rate
8. Repeat until convergence

**Weight Update:**
$$w_c(\text{new}) = \begin{cases} w_c + \eta(x - w_c) & \text{if class match} \\ w_c - \eta(x - w_c) & \text{if class mismatch} \end{cases}$$

**Variants:**
- **LVQ1 (conventional):** Only update winning prototype $w_c$
- **LVQ2:** Update all weights
- **LVQ 2.1:** Check if input lies in predefined window around decision boundary
- **LVQ3:** Correction factor applied

**Convergence criteria:**
1. Weight matrix stabilisation
2. Error reduced to minimum value
3. Max number of iterations

---

### 🔢 LVQ — Example 1

**Given:** Input $X = (2, 3)$, Prototypes: $w_1 = (1, 2)$ → Class A, $w_2 = (5, 4)$ → Class B

Learning rate $\eta = 0.1$. True class of $X = A$

**Distances:**

$$d_A = \sqrt{(2-1)^2 + (3-2)^2} = \sqrt{2}$$
$$d_B = \sqrt{(2-5)^2 + (3-4)^2} = \sqrt{10}$$

**Winner class = A** (min distance)

Since true class = winner class → **move towards** input:

$$w_A(\text{new})_x = 1 + 0.1 \times (2-1) = 1.1$$
$$w_A(\text{new})_y = 2 + 0.1 \times (3-2) = 2.1$$

$$w_A(\text{new}) = (1.1, 2.1)$$

---

### 🔢 LVQ — Example 2 (3 classes)

**Given:** Dataset with 3 classes:
- $w_1 = (2,2)$ → Class A, $w_2 = (7,5)$ → Class B, $w_3 = (6,1)$ → Class C
- Input $X = (4, 3)$, $\eta = 0.25$

**Distances:**

$$d_A = \sqrt{(4-2)^2+(3-2)^2} = \sqrt{5}$$
$$d_B = \sqrt{(4-7)^2+(3-5)^2} = \sqrt{13}$$
$$d_C = \sqrt{(4-6)^2+(3-1)^2} = \sqrt{8}$$

**Winner = A** (min distance $\sqrt{5}$)

**Update:**
$$w_A(\text{new})_x = 2 + 0.25(4-2) = 2.5$$
$$w_A(\text{new})_y = 2 + 0.25(3-2) = 2.25$$

$$w_A(\text{new}) = (2.5, 2.25)$$

---

## 9. ART (Adaptive Resonance Theory)

### 9.1 Theory

**ART** is both supervised and unsupervised, designed to solve the **stability-plasticity dilemma**.

**Types:**

```
ART Networks Classification
├── Unsupervised ARTs
│   ├── ART1
│   ├── ART2
│   ├── Simplified ART
│   └── FuzzyART
└── Supervised ARTs
    ├── ARTMAP
    ├── Fuzzy ARTMAP
    ├── Gaussian ARTMAP
    └── Simplified ARTMAP
```

**Block Diagram:**
```
Input (I) → Gain 1 → Comparison Layer F1 ⇄ Recognition Layer F2
                                F1 ← Reset Wave ← F2
Gain 2 → Recognition Layer F2
```

**ART Learning Algorithm:**
1. Initialise weights
2. Present input vector to F1 layer
3. Compute activation of F2 neurons
4. Select Best Matching (winner) neuron
5. Perform **Vigilance Test:**

$$\frac{|x \cap w_j|}{|x|} \geq \rho \quad \text{(vigilance parameter)}$$

6. If similarity $\geq$ vigilance parameter → **Resonance occurs & learning happens**
   Else → Reset current neuron & search another
7. Update weights of winning category
8. Repeat for all input patterns

**Weight update (on resonance):**
$$w_{ij}^{\text{new}} = x_i$$

**Stability-Plasticity Dilemma:** ART uses the vigilance parameter $\rho$ to balance stability (not forgetting old patterns) and plasticity (learning new ones).

---

### 🔢 ART — Example 1

**Given:** $P_1 = (1101)$, $P_2 = (1001)$, vigilance $\rho = 0.7$

**Step 1:** $P_1$ → 1st cluster, weight $w = P_1$

**Step 2 — Test P2:**

$$P_1 \cap P_2 = (1001)$$

Vigilance test:
$$\frac{|P_1 \cap P_2|}{|P_2|} = \frac{2}{2} = 1 \geq 0.7 \Rightarrow \text{Resonance!}$$

$P_1$ and $P_2$ form Cluster $C_1$.

---

### 🔢 ART — Example 2 (3 patterns)

**Given:** $P_1=(11010)$, $P_2=(10010)$, $P_3=(11100)$, $\rho=0.75$

**P1 → Cluster 1** (weight = $P_1$)

**Test P2:** $P_1 \cap P_2 = (10010)$

$$\frac{|P_1 \cap P_2|}{|P_2|} = \frac{2}{2} \geq 0.75 \Rightarrow \text{Resonance → C}_1$$

**Test P3:** $(P_1 \cap P_2) \cap P_3 = (10010) \cap (11100) = (10000)$

$$\frac{1}{\sqrt{3}} = 0.577 < 0.75 \Rightarrow \text{New cluster!}$$

$P_3$ forms Cluster $C_2$

$C_1 = \{P_1, P_2\}$, $C_2 = \{P_3\}$

---

## 10. Neuro-Fuzzy / Hybrid Systems

### 10.1 Theory

**Motivation:**
- ANNs are **black boxes**, require large data, GPUs, no rule for architecture selection, prone to local optima, sensitive to noise.
- Fuzzy logic has difficulty learning from data.
- **Solution:** Combine both → **Neuro-Fuzzy systems**.

**Types of hybrid systems:**
- Cooperative
- Concurrent
- Hybrid

**Operational flow of Neuro-Fuzzy:**
```
Input crisp values → Find membership grades → Compute firing strength → Evaluate consequent → Output values
```

---

### 10.2 ANFIS (Adaptive Neuro-Fuzzy Inference System)

ANFIS represents a 1st-order Sugeno model with 2 inputs and 2 rules:

**Rule 1:** If $x$ is $A_1$ and $y$ is $B_1$, then $f_1 = p_1 x + q_1 y + r_1$

**Rule 2:** If $x$ is $A_2$ and $y$ is $B_2$, then $f_2 = p_2 x + q_2 y + r_2$

**Layers:**
- **Layer 1:** Compute $\mu_{A_i}(x)$, $\mu_{B_i}(y)$
- **Layer 2 (T-norm):** $w_i = \mu_{A_i}(x) \cdot \mu_{B_i}(y)$
- **Layer 3 (Normalize):** $w_i' = \dfrac{w_i}{w_1 + w_2}$
- **Layer 4:** Compute $f_i$ (consequent)
- **Layer 5 (Output):** $o/p = \sum w_i' f_i$

---

### 🔢 ANFIS — Example

**Given:** $\mu_{A_1}(x) = 0.6$, $\mu_{A_2}(x) = 0.4$, $f_1 = 2x + y + 1$, $x = 2$

$\mu_{B_1}(y) = 0.7$, $\mu_{B_2}(y) = 0.3$, $f_2 = x + 2y + 3$, $y = 3$

**Firing Strengths:**
$$w_1 = \mu_{A_1}(x) \cdot \mu_{B_1}(y) = 0.6 \times 0.7 = 0.42$$
$$w_2 = \mu_{A_2}(x) \cdot \mu_{B_2}(y) = 0.4 \times 0.3 = 0.12$$

**Normalized:**
$$w_1' = \frac{0.42}{0.54} = 0.778,\quad w_2' = \frac{0.12}{0.54} = 0.222$$

**Consequents:**
$$f_1 = 2(2) + 3 + 1 = 8$$
$$f_2 = 2 + 2(3) + 3 = 11$$

**Output:**
$$o/p = w_1' f_1 + w_2' f_2 = 0.778 \times 8 + 0.222 \times 11 = 6.22 + 2.44 = 8.66$$

---

## 11. Nature-Inspired Computing

### 11.1 Ant Colony Optimization (ACO)

**Inspiration:** Real ant colonies finding shortest paths using pheromone trails.

**Key idea:** Ants deposit pheromone on edges. Shorter paths accumulate more pheromone over time.

**Transition probability:**
$$P(i,j) = \frac{(\tau_{ij})^\alpha \cdot (\eta_{ij})^\beta}{\sum (\tau_{ij})^\alpha \cdot (\eta_{ij})^\beta}$$

where:
- $\tau_{ij}$ = pheromone on edge $(i,j)$
- $\eta_{ij} = 1/d_{ij}$ = heuristic information (1/distance)
- $\alpha$ = influence of pheromone
- $\beta$ = influence of heuristic info

**Pheromone evaporation:**
$$\tau_{ij} \leftarrow (1-\rho)\tau_{ij}$$

**Pheromone update (after tour):**
$$\tau_{ij} \leftarrow (1-\rho)\tau_{ij} + \Delta\tau_{ij}$$

where $\Delta\tau_{ij} = 1/L$ if edge $(i,j)$ is in best solution, else 0.

---

### 🔢 ACO — Example 1

**Given:** Edges from A:

| Edge | Pheromone ($\tau$) | Distance ($d$) |
|------|---------------------|----------------|
| A–B  | 1                   | 2              |
| A–C  | 1                   | 4              |
| A–D  | 1                   | 5              |

$\alpha = 1$, $\beta = 2$; $\eta_{ij} = 1/d_{ij}$

$$P(A,B) = \frac{1 \cdot (1/2)^2}{1\cdot(1/2)^2 + 1\cdot(1/4)^2 + 1\cdot(1/5)^2} = \frac{0.25}{0.25 + 0.0625 + 0.04} = \frac{0.25}{0.3525} \approx 0.274$$

$$P(A,C) = \frac{(1/4)^2}{0.3525} \approx 0.683$$

$$P(A,D) = \frac{(1/5)^2}{0.3525} \approx 0.044$$

---

### 🔢 ACO — Pheromone Update Example

**Given:** Initial pheromone = 1 on edge, $\rho = 0.3$, tour cost = 10

$$\tau_{ij} \leftarrow (1-0.3)(1) + \frac{1}{10} = 0.7 + 0.1 = 0.8$$

---

### 🔢 ACO — TSP with Pheromone

**Cities A, B, C.** Ant completes route A→B→C→A.

Distance matrix:

| | A | B | C |
|--|---|---|---|
|A | – | 2 | 3 |
|B | 2 | – | 4 |
|C | 3 | 4 | – |

Initial pheromone = 1 on each used edge, $\rho = 0.5$

**Tour cost** = 2+4+3 = **9**

$$\tau_{ij} = 0.5 \times 1 + 1/9 = 0.5 + 0.11 = 0.61$$

All pheromone values = 0.61

---

### 🔢 ACO — Probability from Node A

**From node A**, pheromone: A-B=2, A-C=1; Distance: A-B=2, A-C=3

$\alpha=1$, $\beta=2$, $\eta_{AB} = 1/2$, $\eta_{AC} = 1/3$

$$P(A-B) = \frac{2 \times (1/2)^2}{2(1/2)^2 + 1(1/3)^2} = \frac{0.5}{0.5 + 1/9} = \frac{0.5}{11/18} = \frac{9}{11}$$

$$P(A-C) = \frac{1(1/3)^2}{11/18} = \frac{2}{11}$$

---

### 11.2 Variants of ACO

| # | Variant | Key Feature |
|---|---------|-------------|
| 1 | **BWAS** (Best-Worst Ant System) | Best ant rewarded (shortest path), worst ant punished (longest path) |
| 2 | **MMAS** (Min-Max Ant System) | Prevents pheromone explosion; controls values within $[\tau_{min}, \tau_{max}]$ |
| 3 | **Rank-Based ACO** | Ants ranked by solution quality; top rank contribute more pheromone |
| 4 | **Hypercube Ant System** | Pheromone values normalized and bounded by $[0, 1]$ |
| 5 | **Mean-Minded ACO** | Based on average performance of ants |

---

#### BWAS Update Rules

**Evaporation:** $\tau_{ij} \leftarrow (1-\rho)\tau_{ij}$

**Reward (best ant):** $\tau_{ij} \leftarrow \tau_{ij} + \Delta\tau_{ij}^{best}$

**Punish (worst ant):** $\tau_{ij} \leftarrow \tau_{ij} - \Delta\tau_{ij}^{worst}$

---

#### MMAS

Only best ant updates pheromone: $\tau_{ij} \leftarrow (1-\rho)\tau_{ij} + \Delta\tau_{ij}^{best}$

**Pheromone limits:**
$$\tau_{ij} = \begin{cases}\tau_{max} & \text{if } \tau_{ij} > \tau_{max} \\ \tau_{min} & \text{if } \tau_{ij} < \tau_{min}\end{cases}$$

---

#### Rank-Based ACO

$$\tau_{ij} \leftarrow (1-\rho)\tau_{ij} + \sum_{r=1}^{w}(w-r)\Delta\tau_{ij}^r + w\Delta\tau_{ij}^{best}$$

where $r$ = rank of ant, $w$ = number of top ants, best = global best ant.

**Each ant's contribution:**
$$\Delta\tau_{ij}^r = \begin{cases}1/L & \text{if edge }(i,j) \text{ is in solution of ant } r \\ 0 & \text{otherwise}\end{cases}$$

---

#### Mean-Minded ACO

**Mean tour length:**
$$\text{mean} = \frac{1}{m}\sum_{k=1}^{m} L_k$$

**Pheromone update:**
$$\tau_{ij} \leftarrow (1-\rho)\tau_{ij} + \Delta\tau_{ij}^{mean}$$

$$\Delta\tau_{ij}^{mean} = \begin{cases}1/L_{mean} & \text{if edge }(i,j)\text{ belongs to average-supported solutions} \\ 0 & \text{otherwise}\end{cases}$$

---

### 11.3 Particle Swarm Optimization (PSO)

**Inspiration:** Bird flocking / fish schooling behavior.

**Position update:**
$$x_i(t) = x_i(t-1) + v_i(t)$$

**Velocity update:**
$$v_i(t) = \omega v_i(t-1) + r_1 c_1 (x_{lb} - x_i(t-1)) + r_2 c_2 (x_{gb} - x_i(t-1))$$

where:
- $\omega$ = inertia weight
- $c_1$ = cognitive coefficient (personal best)
- $c_2$ = social coefficient (global best)
- $r_1, r_2$ = random numbers in $[0,1]$
- $x_{lb}$ = local (personal) best
- $x_{gb}$ = global best

---

### 🔢 PSO — Example 1 (1D)

**Given:**
- Current position $x = 4$, velocity $v_i = 1.5$
- Local best $= 2$, Global best $= 1$, $\omega = 0.7$
- $c_1 = 2$, $c_2 = 2$, $r_1 = 0.4$, $r_2 = 0.3$

$$v_i(t) = 0.7 \times 1.5 + 0.4 \times 2 \times (2-4) + 0.3 \times 2 \times (1-4)$$
$$= 1.05 + (-1.6) + (-1.8) = -2.35$$

$$x_i(t+1) = -2.35 + 4 = 1.65$$

---

### 🔢 PSO — Example 2 (2D)

**Given:**
- Position $x = (3,5)$, velocity $v = (1,-2)$
- Personal best $P_{best} = (2,4)$, Global best $G_{best} = (1,3)$
- $\omega=0.6$, $c_1=1.5$, $c_2=1.5$, $r_1=0.5$, $r_2=0.2$

$$v_x = 0.6(1) + 0.75(2-3) + 0.3(1-3) = 0.6 - 0.75 - 0.6 = -0.75$$
$$v_y = 0.6(-2) + 0.75(4-5) + 0.3(3-5) = -1.2 - 0.75 - 0.6 = -2.55$$

$$v = (-0.75, -2.55)$$

$$x_{\text{new}} = (3,5) + (-0.75, -2.55) = (2.25, 2.45)$$

---

### 11.4 Artificial Bee Colony (ABC)

**Inspired by:** Honey bee foraging behavior.

**Roles:**
- **Employed bees:** Exploit known food sources
- **Onlooker bees:** Choose sources based on fitness (probability)
- **Scout bees:** Explore random new sources

**Selection probability:**
$$P_i = \frac{F_i}{\sum F_j}$$

**Initial food source:**
$$v_j = v_{j,\min} + [0,1](v_{j,\max} - v_{j,\min})$$

**New candidate solution (local search):**
$$v_{new} = x_i + \text{rand} \times (x_i - x_k)$$

---

### 🔢 ABC — Example 1 (Selection Probability)

**Given:** Fitness values $F_1 = 0.2$, $F_2 = 0.5$, $F_3 = 0.3$

$$P_1 = 0.2,\quad P_2 = 0.5,\quad P_3 = 0.3$$

---

### 🔢 ABC — Example 2 (Initial Food Source)

**Given:** 2-variable problem: $x_1 \in [0, 20]$, $x_2 \in [100, 200]$

Random numbers: 0.25 (for $x_1$), 0.8 (for $x_2$)

$$x_1 = 0 + 0.25 \times (20-0) = 5$$
$$x_2 = 100 + 0.8 \times (200-100) = 180$$

Food source = $(5, 180)$

---

### 🔢 ABC — New Candidate Solution

**Given:** $x_i = 30$, $x_k = 18$, random $= -0.5$

$$v_{new} = 18 + (-0.5)(30 - 18) = 18 - 6 = 12$$

---

### 11.5 Cuckoo Search Algorithm

**Inspiration:** Brood parasitism of cuckoo birds.

**Rules:**
1. Each cuckoo lays one egg per host nest.
2. Best nests survive to next generation.
3. Fraction $p_a$ of worst nests abandoned and new ones built.

**Position update (Lévy flight):**
$$x_i^{t+1} = x_i^t + \alpha \cdot \text{Levy}(\lambda)$$

**Fitness:** Lower objective value = higher fitness (for minimization problems).

---

### 🔢 Cuckoo Search — Example 1

**Given:** Initial nests = $[4, 1, 3, 6, 2]$, $f(x) = x^2$ (minimize)

$\alpha = 1$, Lévy step for nest 3 = $-1.5$, $p_a = 0.2$

**Fitness** = $[16, 1, 9, 36, 4]$

Generate cuckoo at nest 3: $x_3^t = 3$

$$x_3^{t+1} = 3 + 1 \times (-1.5) = 1.5$$

New fitness = $1.5^2 = 2.25$

Compare with nests 3,4,5 (obj: 9, 36, 4):
- Worst nest = 36 (nest 4 with value 6) → abandon
- Replace with 1.5

---

### 🔢 Cuckoo Search — Example 2

**Given:** Objective function $x^2 - 6x + 8$, old nest $x=5$, new nest $x=2$

Old nest fitness = $25 - 30 + 8 = 3$

New nest fitness = $4 - 12 + 8 = 0$ (lower = better)

→ **New nest $(x=2)$ accepted.**

---

### 🔢 Cuckoo Search — Lévy Flight Position Update

**Given:** $x(t) = (2, 4)$, $\alpha = 0.5$, $L(\lambda) = (1.2, -0.8)$

$$x_i^{t+1} = x_i^t + \alpha \cdot L(\lambda)$$

$$x^{t+1}_x = 2 + 0.5 \times 1.2 = 2.6$$
$$x^{t+1}_y = 4 + 0.5 \times (-0.8) = 3.6$$

$$x^{t+1} = (2.6, 3.6)$$

---

## 12. Fuzzy Inference — Full Worked Problem

### 🔢 Fuzzy Robotic Controller (Full Problem)

**Given:** Angular error $\alpha$, Goal distance near/medium. AND = min, aggregation = max, defuzzification = all 4 methods + compare.

**Triangular membership functions:**
- Input $\alpha$: zero $= \text{tri}(-30, 0, +30)$; less positive (lp) $= \text{tri}(0, 30, 60)$; Universe $[-60, +60]$
- Output steering: $z = \text{tri}(-20, 0, +20)$; lp $= \text{tri}(0, 20, 40)$; MP $= \text{tri}(20, 40, 60)$

**Rules:**
1. If distance near AND error is Z → steering is Z
2. If distance near AND error is LP → steering is LP
3. If distance medium AND error is Z → steering is LP
4. If distance medium AND error is LP → steering is MP

**For $\alpha = 15°$:**

$$\mu_z(15) = \frac{30-15}{30-0} = \frac{1}{2}$$
$$\mu_{lp}(15) = \frac{15-0}{30-0} = \frac{1}{2}$$

**Goal distance memberships:** Near = 0.4, Medium = 0.6

**Rule firing:**
- Rule 1: $w_1 = \min(0.4, 0.5) = 0.4$
- Rule 2: $w_2 = \min(0.4, 0.5) = 0.4$
- Rule 3: $w_3 = \min(0.6, 0.5) = 0.5$
- Rule 4: $w_4 = \min(0.6, 0.5) = 0.5$ → **min(0.6, 0.5) = 0.5**

> After aggregation and defuzzification using centroid/bisector — specific numerical answer depends on MF parameters.

---

## Summary Table — All Topics

| Topic | Key Algorithm/Concept |
|-------|-----------------------|
| MP Neuron | $\sum w_i x_i \geq \theta$ → output 1 |
| ADALINE | LMS rule: $\Delta w = \eta \delta x$ |
| Perceptron | Hard limiter, weight update on error |
| RBF | Gaussian radial basis function |
| Backprop | Gradient descent, sigmoid activation |
| SOM | Unsupervised, BMU, Kohonen |
| LVQ | Supervised SOM, move toward/away |
| ART | Vigilance test, stability-plasticity |
| ANFIS | Neuro-fuzzy, Sugeno 1st order |
| ACO | Pheromone, transition probability |
| PSO | Velocity + position update |
| ABC | Employed/onlooker/scout bees |
| Cuckoo | Lévy flight, nest abandonment |
| Fuzzy | MF, rules, implication, defuzzify |
| GA | Encoding, crossover, mutation, schema |

---

*End of Notes*
