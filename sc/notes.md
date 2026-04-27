# Soft Computing — Complete Notes

> **Subject:** Soft Computing

## 1. Introduction — Soft vs Hard Computing

### What is Soft Computing?
Soft Computing (SC) finds **good-enough solutions** for complex problems where data may be **imprecise, incomplete, fuzzy, or missing**.

### Soft Computing vs Hard Computing

| Parameter         | Soft Computing (SC)                          | Hard Computing (HC)                        |
|-------------------|----------------------------------------------|--------------------------------------------|
| Data Handling     | Imprecise, incomplete, fuzzy, missing        | Exact, complete, no missing info, precise  |
| Output            | Approximate (helpful for real life)          | Exact                                      |
| Adaptability      | High (more adaptive)                         | Very low                                   |
| Logic             | Probabilistic, fuzzy, continuous logic       | Discrete                                   |
| Real World Use    | Used more                                    | Very less usable                           |

> **When to use SC:** When data is uncertain, hidden, missing, probabilistic, continuous, or fuzzy.

### Computational Complexity
- **P (Polynomial):** As input size grows, computation goes in polynomial time.
- **NP (Non-deterministic Polynomial):** As input size grows, computation becomes **deterministically** polynomial time.
- **NP-Hard:** Problems at least as hard as NP problems.

> Initially, feedforward models were tried but weren't sufficient → **Back Propagation** models were introduced.

---

## 2. Fuzzy Systems

### Fuzzy Set Basics
A fuzzy set assigns a **membership degree** μ(x) ∈ [0, 1] to each element.

### Complement
$$\Gamma(A \cap B) = \Gamma A \cup \Gamma B$$

**Example:**  
Given A ∩ B = {(0,30), (0.1,40), (0.2,50), (0.5,60), (0.2,65), (0.1,70), (0,75), (0,80)}

Γ(A ∩ B) = {(1,30), (0.9,40), (0.8,50), (0.5,60), (0.8,65), (0.9,70), (1,75), (1,80)}

ΓA = {(1,30)}

---

### Overlap Ratio & Robustness

**Overlap Ratio** = (overlap interval length) / (total support length)

**Overlap Robustness** = Weighted average of (μ₁ + μ₂) over intervals

**Example 1:** Two MFs on [2, 10], crossing in the middle  
→ Overlap ratio = 0 (MFs don't overlap), Robustness = 1/2

**Example 2:** Two MFs on [0, 18] — overlap robustness = 0.7

**Example 3 (Numerical):**

| Interval | μ₁ + μ₂ |
|----------|---------|
| 0–2      | 0.6     |
| 2–4      | 1.2     |
| 4–6      | 0.8     |

$$\text{Overlap Robustness} = \frac{0.6 \times 2 + 1.2 \times 2 + 0.8 \times 2}{2 \times (6-0)} = \frac{2(0.6+1.2+0.8)}{12} = \frac{5.2}{12} \approx 0.43$$

---

### Centroid (Defuzzification Method)

$$z^* = \frac{\int \mu(z) \cdot z \, dz}{\int \mu(z) \, dz}$$

**Discrete version:**
$$z^* = \frac{\sum z_i \cdot \mu(z_i)}{\sum \mu(z_i)}$$

**Example:** Fuzzy controller with z = 10, 20, 30, 40, 50 and μ(z) = 0.1, 0.4, 0.8, 0.4, 0.1

$$z^* = \frac{1+8+24+16+5}{0.1+0.4+0.8+0.4+0.1} = \frac{54}{1.8} = 30$$

---

### Overlap Ratio & Robustness (Given F1, F2)

**Example:** F1 support = [0,10], F2 support = [6,16], overlap = [6,10], μ₁(x)+μ₂(x) ≤ 1

$$\text{Overlap Ratio} = \frac{4}{16} = \frac{1}{4}$$

$$\text{Overlap Robustness} = \frac{2 \times 4}{2 \times 4} \times \frac{1 \times (10-6)}{2 \times (10-6)} = 0.5$$

---

## 3. Fuzzy Implication I(a, b)

The **implication operator** answers: *"If the IF-PART is true to degree 'a', how strongly should the THEN-PART be applied to degree 'b'?"*

- **I(a,b)** gives the truth value of the whole rule.
- **Fuzzy Relation** R(x,y) = Σ μ(xᵢ,yᵢ) / (xᵢ; wᵢ)  
  where μ(xᵢ,yᵢ) = membership degree of pair (xᵢ, yᵢ)
- R tells how strongly each I/p–o/p pair is related.

### Types of Fuzzy Implication

| # | Name | Formula |
|---|------|---------|
| 1 | **Mamdani** | I(a,b) = min(a, b) |
| 2 | **Larsen** | I(a,b) = a × b |
| 3 | **Gödel** | I(a,b) = 1 if a ≤ b; 0 otherwise |
| 4 | **Łukasiewicz** | I(a,b) = min(1, 1−a+b) |
| 5 | **Zadeh** | I(a,b) = max(1−a, min(a,b)) |
| 6 | **Kleene-Dienes** | I(a,b) = max(1−a, b) |
| 7 | **Goguen** | I(a,b) = 1 if a ≤ b; b/a if a > b |
| 8 | **Reichenbach** | I(a,b) = 1−a+ab (blends negation & addition) |
| 9 | **Yager** | I(a,b) = b^a (exponential) |
| 10 | **Fodor** | I(a,b) = 1 if a ≤ b; max(1−a, b) if a > b |
| 11 ⭐ | **Sugeno** | I(a,b) = b / (a+b−ab) |
| 12 | **Gaines-Rescher** | I(a,b) = 1 if a ≤ b; 0 if a > b |
| 13 | **Wu** | I(a,b) = min(1, b/a) — ratio-based |
| 14 | **Dubois-Prade** | I(a,b) = 1 if a ≤ b; max(1−a, b) if a > b |
| 15 | **Wilmott** | I(a,b) = min(1, 1−a+ab) |
| 16 | **Baldwin** | I(a,b) = min(1, (1−a+b)/2) |
| 17 | **Nilpotent Min** | I(a,b) = 1 if a ≤ b; 1−a+b if a > b |
| 18 | **Drastic** | I(a,b) = b if a=1; 1 if a<1 |
| 19 | **Algebraic Product** | I(a,b) = a × b |

### Łukasiewicz — 4 Cases

| Case | IF    | THEN  | I(a,b)         | Interpretation  |
|------|-------|-------|----------------|-----------------|
| 1    | Strong (a=0.9) | Weak (b=0.2) | min(1, 1−0.9+0.2) = 0.3 | Rule is weak |
| 2    | Strong (a=0.8) | Strong (b=0.8) | min(1, 1−0.8+0.8) = 1 | Strongest |
| 3    | Weak (a=0.2) | Strong (b=0.8) | min(1, 1−0.2+0.8) = 1 | Strongest |
| 4    | Weak (a=0.3) | Weak (b=0.2) | min(1, 1−0.3+0.2) = 0.9 | Strong |

> **Rule:** If antecedent is weak, rule is always strong. If antecedent is strong, consequent must be strong for the rule to be strong.

### Sugeno Implication — Special Cases

$$I(a,b) = \frac{b}{a+b(1-a)}$$

| Case | Condition | Result |
|------|-----------|--------|
| 1 | b = 1 | I = 1 |
| 2 | a = 0 | I = 1 |
| 3 | a = 1 | I = b (truth purely dependent on THEN) |
| 4 | b = 0 | I = 0 |

**Numerical Examples (Sugeno):**
1. a = 0.7, b = 0.2 → I = 0.2 / (0.7 + 0.2×0.3) = 0.2/0.76 ≈ **0.263**
2. a = 0.2, b = 0.7 → I ≈ **0.921**
3. a = b = 0.5 → I = 0.5 / (0.5 + 0.5×0.5) = 0.5/0.75 ≈ **0.667**

---

## 4. Defuzzification Methods

Defuzzification converts a fuzzy set back into a crisp output.

| # | Method | Description |
|---|--------|-------------|
| 1 | **Centroid / CoA / CoG** | Center of area — most common |
| 2 | **MoM (Mean of Max)** | Average of all x where μ is maximum |
| 3 | **SoM (Smallest of Max)** | Smallest x where μ is maximum |
| 4 | **LoM (Largest of Max)** | Largest x where μ is maximum |
| 5 | **Bisector** | Vertical line dividing area into two equal halves |

### Centroid — Triangular MF Example

Triangular fuzzy set: base = [10, 50], peak = 30, μ = 1 at peak, rises/falls linearly.

$$\mu_{\text{rising}}(z) = \frac{z-10}{20}, \quad \mu_{\text{falling}}(z) = \frac{50-z}{20}$$

$$z^* = \frac{\int_{10}^{30} z \cdot \frac{z-10}{20}\,dz + \int_{30}^{50} z \cdot \frac{50-z}{20}\,dz}{\int_{10}^{30} \frac{z-10}{20}\,dz + \int_{30}^{50} \frac{50-z}{20}\,dz} = 30$$

### Centroid — Another Example (a=10, b=40 peak, c=50)

$$\mu_{\text{rising}} = \frac{z-10}{30}, \quad \mu_{\text{falling}} = \frac{50-z}{10}$$

$$z^* = 33.33$$

### Bisector Example

Trapezoidal fuzzy set: base [0,100], flat top [40,60], height = 1.  
A₁ = 20, A₂ = 20, A₃ = 20 → Total area = 60, **BoA = 50°**

### MoM, SoM, LoM Example

Given μ* = 0.8 occurs at z = {20, 25, 30, 35}:
- **MoM** = (20+25+30+35)/4 = **27.5** → *Physical meaning: average of all strong decisions*
- **SoM** = 20
- **LoM** = 35

### CoA — Continuous MF Example (Case II)

Membership function defined piecewise:

| Segment | Range       | μ(x)               |
|---------|-------------|---------------------|
| 1       | 0 ≤ x ≤ 20 | 0.1                 |
| 2       | 20 ≤ x ≤ 30 | ramp 0.1 → 0.2     |
| 3       | 30 ≤ x ≤ 60 | 0.2                 |
| 4       | 60 ≤ x ≤ 70 | ramp 0.2 → 0.5     |
| 5       | 70 ≤ x ≤ 100 | 0.5                |

**Answer:** z* = N/D = 1833.33/28 ≈ **56.75**

---

## 5. Steps for a Fuzzy Inference System (FIS)

1. Given input & output variables → create fuzzy sets & decide membership functions.
2. Calculate membership values for all inputs across all membership functions.
3. Create rules using fuzzy operators.
4. Substitute membership values in rules → calculate antecedent value.
5. Apply implication (Mamdani / Sugeno).
6. If multiple outputs → perform **aggregation** for all output rules.
7. Perform **defuzzification**.

---

## 6. Fuzzy Robotic Controller — Numerical

**Given:** Angular error α = +20°, Goal distance = near (membership = 1.0), AND = min, Aggregation = max, Defuzzification = Centroid.

**Input fuzzy sets** (angle, UoD = [-90, 90]):
- more_n: [-60, -30]
- less_n: [-30, 0]
- no: 0
- less_p: [0, 30]
- more_p: [30, 60]

**For α = 20°:**
- μ(no) = (30−20)/30 = **0.33**
- μ(less_p) = (30−20)/30 = **0.33**
- μ(more_p) = 0, μ(less_n) = 0, μ(more_n) = 0

**Rules:**
- Rule 1: If α=0 AND distance=near → steering=straight → min(0.33, 1.0) = **0.33**
- Rule 2: If α=less_p AND distance=near → steering=right → min(0.33, 1.0) = **0.33**

**Centroid:**
$$c = \frac{0 \times 0.33 + 20 \times 0.33}{0.33 + 0.33} = \frac{6.6}{0.66} = 10$$

---

## 7. Encoding Schemes for Genetic Algorithms (GA)

| # | Type | Example |
|---|------|---------|
| 1 | Continuous Value | C1 = 9846.1002 |
| 2 | Binary Encoding | C1 = 10010101 |
| 3 | Integer Encoding | C1 = 2 4 6 8 10 12 |
| 4 | Value/Real Encoding | C1 = 11.465, 10.269, ... |
| 5 | Tree Encoding | Expression trees |
| 6 | Permutation Encoding | For TSP-type problems |

**Gene → Chromosome (Genotype) → [decoding] → Phenotype**

---

## 8. Crossover Methods in GA

### 1. Uniform Crossover (Binary)

**Rule:** If mask M = 1 → take gene from P1; if M = 0 → take from P2.

**Example:**
```
P1     = 1 0 1 1 0 0 1 0
P2     = 0 1 0 0 1 1 0 1
Mask M = 1 0 1 1 0 0 1 0
Child  = 1 1 1 1 1 1 1 1
C2 (complement) = 0 0 0 0 0 0 0 0
```

### 2. Uniform Crossover (Real-valued)

P1 = [6.0, 3.0, 2.0], P2 = [2.0, 5.0, 8.0], Mask = [1 0 1]  
Child C = [6, 5, 2]

### 3. Arithmetic Crossover

$$C_1 = \alpha P_1 + (1-\alpha)P_2, \quad C_2 = (1-\alpha)P_1 + \alpha P_2$$

**Example:** P1 = (8, 2), P2 = (2, 10), α = 0.3  
- C1 = 0.3×8 + 0.7×2 = **3.8**, yC1 = 0.7×2 + 0.3×10 = **4.4** → C1 = **(3.8, 4.4)**
- C2 = 0.7×8 + 0.3×2 = **6.2**, yC2 = 0.7×10 + 0.3×2 = **7.6** → C2 = **(6.2, 7.6)**

### 4. Shuffle Crossover

1. Shuffle both parents with permutation π.
2. Apply 1-point crossover on shuffled parents.
3. Unshuffle (using mask).

### 5. Order Changing Crossover

C = P1 + P2 (element-wise sum), then sort in descending order.

**Example:**  
P1 = (1,2,3,4,5,6), P2 = (2,3,4,5,6,7,8)  
C = (3,5,7,10,12,14) → C_order = **(14,12,10,7,5,3)**

---

## 9. Mutation Types

Given chromosome C = [1,2,3,4,5,6,7]:

| # | Type | Result |
|---|------|--------|
| 1 | **Inversion** | [1,2,6,5,4,3,7] |
| 2 | **Insertion** | [1,2,6,4,5,7] |
| 3 | **Displacement** | [1,3,4,5,2,6,7] |
| 4 | **Swap (Reciprocal Exchange)** | [1,2,5,4,3,6,7] |
| 5 | **Shift Right** | [4,1,2,3,4,7,5,6] |

---

## 10. Schema Theory

A **schema** H is a template using {0, 1, X} where X = don't care.

- **Order o(H)** = number of fixed (non-X) positions
- **Length δ(H)** = (last fixed position − first fixed position)

**Example 1:** H = 1 X 0 X 1 → o=3, length=4  
**Example 2:** H = X 1 X 0 X X 1 X → o=3, length=6

### Fragility of Schema

For chromosome length ℓ, one-point crossover:

$$P_d = \frac{\delta(H)}{\ell - 1}, \quad P_s = 1 - P_d$$

**Example:** ℓ=12, single-point crossover  
- HA = 1×0×××××××× → δ=2, Pd_A = 2/11, Ps_A = 9/11  
- HB = ××1×××××××0 → δ=10, Pd_B = 10/11, Ps_B = 2/11 → **HB is more fragile**

For 2-point crossover:  
- Pd_A = 1/10, Pd_B = 8/10

---

## 11. Evolutionary Algorithms — Example

**Maximize f(x) = sin(x), x ∈ [1, 15] using GA (binary encoding)**

| Chromosome | x | sin(x) |
|------------|---|--------|
| 0001       | 1 | 0.84   |
| 0010       | 2 | 0.90   |
| 0011       | 3 | 0.14   |
| 0100       | 4 | −0.76  |
| 1110       | 14 | 0.99  |

→ Best: chromosome **1110** → x=14 → sin(14) ≈ **0.99**

### Fitness Scaling

| Fitness | Scale |
|---------|-------|
| −1 → 0  | worst |
| 0 → 50  | avg   |
| 1 → 100 | best  |

Formula: `f_scaled = (b+1) × 50`

---

## 12. Models of Neuron

### 1. MP (McCulloch-Pitts) Model
$$\text{Net input} = \sum w_i x_i - \theta, \quad \text{Output} = f(\text{net})$$

### 2. Rosenblatt's Perceptron Model
Three units:
- **Sensory unit** (input layer with A₁, A₂, ..., Aₙ)
- **Association unit**
- Summation: Σ xᵢwᵢ − θ → activation function f → output s = f(x)

### 3. ADALINE (Adaptive Linear Element)
$$x = \sum w_i x_i - \theta \quad (\text{activation})$$
$$\text{Output } s = f(x) = x$$
$$\text{Error } \delta = b - s = b - x \quad (b = \text{target})$$
$$\Delta w_i = \eta \delta x_i, \quad \Delta\theta_i = \eta \delta$$

---

## 13. MP Neuron — Numerical

**Problem:** 6 inputs x₁–x₆; Excitatory: x₁–x₄ (w=2,1,3,2); Inhibitory: x₅,x₆; θ=5  
**Hard inhibitory rule:** If any inhibitory input = 1 → output = 0  
**Else:** Decision = 1 if weighted sum ≥ 0; else 0

Weighted sum = 2x₁ + x₂ + 3x₃ + 2x₄

| Case | x₁ | x₂ | x₃ | x₄ | x₅ | x₆ | S | o/p |
|------|----|----|----|----|----|----|---|-----|
| A    | 1  | 1  | 0  | 1  | 0  | 0  | 5 | 1   |
| B    | 1  | 0  | 1  | 0  | 0  | 0  | 5 | 1   |
| C    | 1  | 0  | 1  | 0  | 0  | 0  | 6 | 1   |
| D    | 0  | 1  | 1  | 1  | 0  | No | — | 0   |
| E    | 0  | 0  | 1  | 1  | 0  | 0  | 5 | 1   |
| F    | 1  | 0  | 1  | 1  | 1  | 0  | Na| 0 (inhibited) |
| G    | 1  | 1  | 0  | 0  | 0  | 0  | 3 | 0   |
| H    | 0  | 1  | 0  | 1  | N  | 0  | — | 0   |

---

## 14. ADALINE — Numerical

**Given:** x₁=1, x₂=−1, b=+1, w₁=0.2, w₂=−0.1, θ=0.05, η=0.1

**Iteration 1:**
1. Σwᵢxᵢ = 0.2×1 + (−0.1)×(−1) = 0.3 → x = 0.3 − 0.05 = **0.25**
2. δ = 1 − 0.25 = **0.75**
3. Δw₁ = 0.1×0.75×1 = **0.075** → w₁' = 0.275
4. Δw₂ = 0.1×0.75×(−1) = **−0.075** → w₂' = −0.175
5. Δθ = 0.1×0.75 = 0.075 → θ' = 0.125 (Δbᵢ = ηδ)

**Iteration 2:**
1. Σ = 0.275×1 + 0.175×1 − 0.125 = **0.325**
2. δ' = 1 − 0.325 = **0.675**
3. Δw₁' = 0.1×0.675×0.275 = **0.01856** (approx)

---

## 15. Rosenblatt's Perceptron — Numerical

**Setup:** 2 inputs (x₁, x₂), bias b, η=1, initial w₁=w₂=0, b=0  
**Activation (hard limiter):** y = +1 if (w₁x₁+w₂x₂−b) ≥ 0; else y = −1

**Training data:**

| Pattern | x₁ | x₂ | Target |
|---------|----|----|--------|
| P₁      | 1  | 1  | 1      |
| P₂      | 1  | −1 | −1     |
| P₃      | −1 | 1  | −1     |
| P₄      | −1 | −1 | −1     |

**Epoch 1:**
- P₁: Σ = 0 → o/p = +1 ✓ (matches target=1)
- P₂: x₁=1, x₂=−1 → Σ = 0 → o/p=+1, target=−1 ✗
  - Update: w₁' = 0 + 1×(−1−1)×1 = **−2**, w₂' = 0 + 1×(−1−1)×(−1) = **2**, b' = **−2**

**Epoch 2 (w₁=−2, w₂=2, b=−2):**
- P₁: Σ = −2+2−(−2) = 2 > 0 → o/p=1, target=1 ✓
- P₂: x₁=1, x₂=−1 → −2−2−(−2) = −2 < 0 → o/p=−1, target=−1 ✓
- P₃: x₁=−1, x₂=1 → +2+2−(−2) = 6 > 0 → o/p=1, target=−1 ✗
  - Update: w₁'' = −2+1×(−2)×(−1) = **0**, w₂'' = 2+1×(−2)×1 = **0**, b'' = −2+1×(−2) = **−4**
- P₄: x₁=−1, x₂=−1 → 0+0−(−4) = 4 > 0 → o/p=1, target=−1 ✗ (continues training...)

---

## 16. Gaussian RBF Networks

### Architecture
- Input layer → Hidden RBF neurons → Output neuron
- Each hidden neuron has: **center cⱼ** and **spread σⱼ**

### Gaussian Activation Function
$$\phi(z) = e^{-\|z - c_j\|^2 / (2\sigma^2)}$$

### Output
$$y = \sum_{j=1}^{h} \phi_j w_j$$

### Numerical Example

**x = [2,3], Neuron 1:** c₁=(1,2), σ₁=1, w₁=0.5

$$\phi_1 = e^{-\frac{(2-1)^2+(3-2)^2}{2 \times 1}} = e^{-1} \approx 0.3679$$

**Neuron 2:** c₂=(3,4), σ₂=2, w₂=0.8

$$\phi_2 = e^{-\frac{(2-3)^2+(3-4)^2}{2 \times 4}} = e^{-0.25} \approx 0.776$$

$$y = 0.5 \times 0.3679 + 0.8 \times 0.776 \approx \mathbf{0.807}$$

---

### Example 2: x=(2,−1,3), 3 RBF neurons

| Neuron | Center | σ | w |
|--------|--------|---|---|
| N1 | (1,0,2) | 1 | 0.6 |
| N2 | (3,−2,4) | 2 | 0.3 |
| N3 | (2,−1,1) | 1.5 | 0.5 |

$$\phi_1 = e^{-(1+1+1)/2} = e^{-1.5} \approx 0.223$$
$$\phi_2 = e^{-(1+1+1)/8} = e^{-3/8} \approx 0.687$$
$$\phi_3 = e^{-(0+0+4)/4.5} = e^{-4/4.5} \approx 0.411$$

$$y = 0.6 \times 0.223 + 0.3 \times 0.687 + 0.5 \times 0.411 \approx 0.545$$

---

## 17. Feedforward Neural Network — Backpropagation

### Setup

- 2 inputs: x₁=0.6, x₂=0.1
- 1 hidden neuron, 1 output neuron
- Input→hidden weights: w₁=0.2, w₂=0.3; hidden bias bₕ=0.1
- Hidden→output weight: w₃=0.4; output bias bₒ=0.2
- Target = 0.8, η=0.5
- Activation: sigmoid f(x) = 1/(1+e⁻ˣ)

### Forward Pass

**Hidden layer:**
$$\text{net}_h = x_1w_1 + x_2w_2 + b_h = 0.6×0.2 + 0.1×0.3 + 0.1 = 0.12+0.03+0.1 = 0.25$$
$$h = \sigma(0.25) = \frac{1}{1+e^{-0.25}} \approx 0.562$$

**Output layer:**
$$\text{net}_o = h \times w_3 + b_o = 0.562×0.4 + 0.2 = 0.225 + 0.2 = 0.425$$
$$\hat{y} = \sigma(0.425) \approx 0.605 \approx 0.6$$

**Error term:**
$$\delta_o = \text{target} - \hat{y} = 0.8 - 0.6 = 0.2$$

### Weight Update (Hidden → Output)

$$\Delta w_3 = \eta \times \delta_o \times h = 0.5 \times 0.2 \times 0.562 = 0.056$$
$$w_3' = 0.4 + 0.056 = 0.456$$

$$\Delta b_o = \eta \times \delta_o = 0.5 \times 0.2 = 0.01$$
$$b_o' = 0.2 + 0.01 = 0.3$$

### Weight Update (Input → Hidden)

$$\delta_h = \delta_o \times w_3 \times h(1-h) \approx 0.2 \times 0.4 \times 0.562×0.438 \approx 0.2$$

$$\Delta w_1 = \eta \times \delta_h \times x_1 = 0.5 \times 0.2 \times 0.6 = 0.06 \implies w_1' = 0.26$$
$$\Delta w_2 = \eta \times \delta_h \times x_2 = 0.5 \times 0.2 \times 0.1 = 0.01 \implies w_2' = 0.31$$
$$b_h' = 0.1 + 0.1 = 0.2$$

---

## 18. Self-Organizing Maps (SOMs)

- Proposed by **Kohonen**
- **Unsupervised** neural network
- Maps high-dimensional data to low-dimensional grid (usually 2D)
- Output layer = Kohonen space grid (e.g., 5×5)

### Block Diagram Flow
> Input layer → Euclidean distance calculation → Competitive layer → Grid of neurons mapped

### BMU (Best Matching Unit)
The neuron with the **minimum distance** to the input is the winner.

### Weight Update Equation
$$W_i(t+1) = W_i(t) + \alpha(t) \cdot h_{ci}(t)[X - W_i(t)]$$

Where:
- α(t) = dynamic learning rate
- h_{ci}(t) = neighbourhood function (usually Gaussian):
$$h_{ci}(t) = \exp\left(\frac{-(y_c - y_i)^2}{2\sigma^2}\right)$$

### Stopping Criteria
- Neighbourhood shrinks / minimizes
- Learning rate decreases
- Max iterations reached
- Weight stabilisation

### SOM Numerical

**Given:** x=(0.5, 0.7), Neurons: w₁=(0.2,0.4), w₂=(0.6,0.8), w₃=(0.9,0.3), η=0.4

Distances:
- d₁² = (0.5−0.2)²+(0.7−0.4)² = 0.09+0.09 = 0.18
- d₂² = (0.5−0.6)²+(0.7−0.8)² = 0.01+0.01 = 0.02 ← **winner (w₂)**
- d₃² = (0.5−0.9)²+(0.7−0.3)² = 0.16+0.16 = 0.32

**Winner = w₂.** X belongs to cluster of w₂. Assume h_{ci}(t) = 1.

$$w_{2x}(t+1) = 0.6 + 0.4 \times (0.5−0.6) = 0.6 − 0.04 = 0.56$$
$$w_{2y}(t+1) = 0.8 + 0.4 \times (0.7−0.8) = 0.8 − 0.04 = 0.76$$
$$w_2' = (0.56, 0.76)$$

---

## 19. Learning Vector Quantization (LVQ)

### Flow
Start → Initialize prototype values → Select training sample → Compute distance from all prototypes → Find winner neuron → Check class label → (Correct class: move prototype **towards** input | Wrong class: move prototype **away** from input) → Update weights → Reduce learning rate → Repeat until convergence.

### Weight Update Rules
- If class(X) = class(winner): `W_c(new) = W_c + η(X − W_c)`
- Else: `W_c(new) = W_c − η(X − W_c)`

### LVQ Variants
| Version | Description |
|---------|-------------|
| LVQ1 | Only update winning prototype Wc |
| LVQ2 | Update all weights |
| LVQ2.1 | Check if input lies in predefined window around decision boundary |
| LVQ3 | Correction factor applied |

### Convergence Criteria
1. Weight matrix stabilization
2. Error reduced to minimum value
3. Max number of iterations reached

### LVQ Numerical Example 1

**x=(2,3), w₁=(1,2) class A, w₂=(5,4) class B, η=0.1, True class=A**

Distances: Class A = √2 ← winner, Class B = √10

**Winner = Class A.** Same class → move toward:
$$w_{Ax}' = 1 + 0.1(2−1) = 1.1, \quad w_{Ay}' = 2 + 0.1(3−2) = 2.1$$
$$w_A' = (1.1, 2.1)$$

### LVQ Numerical Example 2

**3 classes:** w₁=(2,2) A, w₂=(7,5) B, w₃=(6,1) C; x=(4,3), η=0.25

Distances: A→√5 ← **winner**, B→√13, C→√8

**Winner = Class A → move toward:**
$$w_{Ax}' = 2 + 0.25(4-2) = 2.5, \quad w_{Ay}' = 2 + 0.25(3-2) = 2.25$$
$$w_A' = (2.5, 2.25)$$

---

## 20. ART (Adaptive Resonance Theory)

### Overview
- For classification using neural networks
- Two types: **Unsupervised ART** (ART1, ART2, FuzzyART) and **Supervised ART** (ARTMAP, FuzzyARTMAP, SimplifiedARTMAP)

### Block Diagram
```
Input (I) → Gain 1 → Comparison Layer F1 ↔ Recognition Layer F2
                                           ↑↓
                                      Reset Wave
                     Gain 2 → F2
```

### ART Learning Algorithm
1. Initialize weights
2. Present input vector to F1 layer
3. Compute activation of F2 neurons
4. Select Best Matching neuron (winner)
5. Perform **Vigilance Test**: |X ∩ W| / |X| ≥ ρ (vigilance parameter)
6. If similarity ≥ ρ → **Resonance occurs** → learning happens; Else → reset current neuron & search another
7. Update weights of winning category
8. Repeat for all I/p patterns

### Principle of Stability-Plasticity Dilemma
The network must be plastic enough to learn new patterns but stable enough to retain old ones.

### Weight Update
$$w_{ij}^{\text{new}} = X_i \quad (\text{when resonance occurs})$$

### ART Vigilance Test Numerical

**P₁=(1101), P₂=(1001), ρ=0.7**

- P₁ → 1st cluster, w = P₁
- P₁ ∩ P₂ = (1001), |P₁ ∩ P₂| = 2, |P₂| = 2 → 2/2 = 1 ≥ 0.7 → **Resonance!**
- P₁ & P₂ → Cluster C₁

### ART Example 2

**P₁=(11010), P₂=(10010), P₃=(11100), ρ=0.75**

- P₁ → 1st cluster
- P₁ ∩ P₂ = (10010), |P₁∩P₂|/|P₂| = 3/3 = 1 ≥ 0.75 → **Resonance** → C₁ = {P₁,P₂}
- (P₁∩P₂) ∩ P₃ = (10000) → 1/√3 < 0.75 → **No Resonance** → new cluster C₂ = {P₃}

---

## 21. Neuro-Fuzzy Hybrid System (ANFIS)

### Why Hybrid?
- **Limitations of ANN:** Black box, requires large data, needs GPUs/HPC, no rule for selecting architecture, local optima, sensitive
- **Limitations of Fuzzy Logic:** Manual rule creation, scalability issues

### Types of Hybrid Systems
- Cooperative, Concurrent, **Hybrid** (most powerful)

### Operational Flow of Neuro-Fuzzy
```
I/p crisp values → Find membership grades → Compute firing strength → Evaluate consequent Eqs → Output values
```

### ANFIS — 2 inputs, 2 rules, 1st order Sugeno model

**Rules:**
- Rule 1: If x is A1 AND y is B1 → f1 = p₁x + q₁y + r₁
- Rule 2: If x is A2 AND y is B2 → f2 = p₂x + q₂y + r₂

**Layers:**
| Layer | Function |
|-------|----------|
| 1 | μ_A₁(x), μ_B₁(y) |
| 2 (Π) | w₁ = μ_A₁(x) × μ_B₁(y); w₂ = μ_A₂(x) × μ_B₂(y) |
| 3 (N) | w₁' = w₁/(w₁+w₂); w₂' = w₂/(w₁+w₂) |
| 4 | f₁, f₂ |
| 5 | o/p = Σ wᵢ'fᵢ |

### ANFIS Numerical

**Given:** μ_A₁(x)=0.6, μ_A₂(x)=0.4, μ_B₁(y)=0.7, μ_B₂(y)=0.3  
f₁ = 2x+y+1, f₂ = x+2y+3, x=2, y=3

- w₁ = 0.6×0.7 = **0.42**, w₂ = 0.4×0.3 = **0.12**
- w₁' = 0.42/0.54 = **0.78**, w₂' = 0.12/0.54 = **0.22**
- f₁ = 2(2)+3+1 = **8**, f₂ = 2+6+3 = **11**
- **Output = 0.78×8 + 0.22×11 = 6.24 + 2.42 = 8.66**

---

## 22. Ant Colony Optimization (ACO)

### Concept
Inspired by how ants find shortest paths using **pheromone trails**.

### Probability of Moving from i to j

$$P(i,j) = \frac{(\tau_{ij})^\alpha (\eta_{ij})^\beta}{\sum (\tau_{ij})^\alpha (\eta_{ij})^\beta}$$

Where:
- τᵢⱼ = pheromone on edge (i,j)
- ηᵢⱼ = heuristic info = 1/distance
- α = influence of pheromone, β = influence of heuristic

### ACO Numerical Example

**Edges from A:** A-B (τ=1, d=2), A-C (τ=1, d=4), A-D (τ=1, d=5); α=1, β=2

η_{AB}=1/2, η_{AC}=1/4, η_{AD}=1/5

$$P(A,B) = \frac{1 \cdot (1/2)^2}{1\cdot(1/2)^2 + 1\cdot(1/4)^2 + 1\cdot(1/5)^2} = \frac{1/4}{1/4+1/16+1/25} = \frac{0.25}{0.3150} \approx \mathbf{0.274}$$

$$P(A,C) \approx 0.683, \quad P(A,D) \approx 0.044$$

### Pheromone Update

$$\tau_{ij} \leftarrow (1-\rho)\tau_{ij} + \Delta\tau_{ij}$$

Where Δτᵢⱼ = 1/L (if edge used) or 0.

**Example:** τ=1, ρ=0.3, tour cost=10  
$$\tau' = 0.7 \times 1 + 1/10 = 0.7 + 0.1 = 0.8$$

---

## 23. ACO Variants

| # | Name | Key Feature |
|---|------|------------|
| 1 | **BWAS** (Best-Worst AS) | Best ant rewarded, worst ant punished |
| 2 | **MMAS** (Min-Max AS) | Pheromone bounded [τ_min, τ_max] to prevent explosion |
| 3 | **Rank-based AS** | Ants ranked by solution quality; top ranks deposit more |
| 4 | **Hypercube AS** | Pheromone normalized to [0,1] |
| 5 | **Mean-Minded ACO** | Based on average performance of ants |

### MMAS Rules
1. Only best ant updates pheromone
2. Pheromone bounded: if τᵢⱼ > τ_max → set to τ_max; if τᵢⱼ < τ_min → set to τ_min

### Rank-Based ACO Update Formula
$$\tau_{ij} \leftarrow (1-\rho)\tau_{ij} + \sum_{r=1}^{w}(w-r)\Delta\tau_{ij}^r + w\Delta\tau_{ij}^{\text{best}}$$

### Ant-Q Algorithm
Combines ACO + Reinforcement learning.

$$P_{ij} = \frac{|AQ(i,j)|^\alpha |\eta_{ij}|^\beta}{\sum |AQ(i,j)|^\alpha |\eta_{ij}|^\beta}$$

**Update:** AQ(i,j) ← (1-κ)AQ(i,j) + κ(r + γ·max(AQ(i,j)))

### TSP Numerical Example

Cities A, B, C; tour A→B→C→A; initial pheromone=1, ρ=0.5  
Tour cost = 2+4+3 = **9**

$$\tau' = (1-0.5)\times1 + 1/9 = 0.5+0.111 = 0.611$$

→ All pheromone values = **0.61**

---

## 24. Particle Swarm Optimization (PSO)

### Update Equations
$$v_i(t) = \omega v_i(t-1) + r_1c_1(x_{lb} - x_i(t-1)) + r_2c_2(x_{gb} - x_i(t-1))$$
$$x_i(t) = x_i(t-1) + v_i(t)$$

Where:
- ω = inertia weight
- c₁, c₂ = cognitive and social coefficients
- r₁, r₂ = random numbers
- x_{lb} = local (personal) best, x_{gb} = global best

### PSO Numerical Example 1

**1D:** x=4, v=1.5, p_best=2, g_best=1, ω=0.7, c₁=c₂=2, r₁=0.4, r₂=0.3

$$v(t) = 0.7 \times 1.5 + 0.8 \times (2-4) + 0.6 \times (1-4)$$
$$= 1.05 - 1.6 - 1.8 = -2.35$$
$$x(t+1) = 4 + (-2.35) = 1.65$$

### PSO Numerical Example 2

**2D:** x=(3,5), v=(1,−2), p_best=(2,4), g_best=(1,3), ω=0.6, c₁=1.5, c₂=1.5, r₁=0.5, r₂=0.2

$$v_x = 0.6×1 + 0.75(2-3) + 0.3(1-3) = 0.6−0.75−0.6 = -0.75$$
$$v_y = 0.6×(-2) + 0.75(4-5) + 0.3(3-5) = -1.2−0.75−0.6 = -2.55$$

$$x(t+1) = (3+(-0.75),\ 5+(-2.55)) = (2.25, 2.45)$$

---

## 25. Cuckoo Search Algorithm

### Concept
- Cuckoos lay eggs in other birds' nests
- **Lévy flights** for position updates
- Discovery probability pₐ controls abandonment

### Update Equation
$$x_i^{t+1} = x_i^t + \alpha \cdot \text{Levy}(\lambda)$$

### Cuckoo Search Numerical

**Nests = [4,1,3,6,2], f(x)=x², α=1, Lévy step for nest 3 = −1.5, pₐ=0.2**

Fitness = [16, 1, 9, 36, 4]  
→ Generate cuckoo at nest 3: x₃=3, x₃^{t+1} = 3−1.5 = **1.5**  
→ f(1.5) = 2.25 (lower value = higher fitness since we want minimum)

Compare with nests 3,4,5 (values: 9,36,4) → worst = 36  
→ Abandon nest 4 (value 36), replace with 1.5

### Lévy Flight Update (Position)

**x(t)=(2,4), α=0.5, L(λ)=(1.2,−0.8)**

$$x^{t+1} = (2+0.5×1.2,\ 4+0.5×(-0.8)) = (2.6, 3.6)$$

---

## 26. Artificial Bee Colony (ABC) System

### Roles
- **Employed bees:** Exploit known food sources
- **Onlooker bees:** Choose food sources based on probability
- **Scout bees:** Discover new food sources randomly

### Selection Probability
$$P_i = \frac{f_i}{\sum f_k}$$

### Initial Food Source
$$v_i = v_{i,\min} + [0,1](v_{i,\max} - v_{i,\min})$$

### Local Search (New Candidate)
$$v_{\text{new}} = x_k + \text{rand} \times (x_i - x_k)$$

### ABC Numericals

**Q1:** f₁=0.2, f₂=0.5, f₃=0.3 → P₁=0.2, P₂=0.5, P₃=0.3

**Q2:** 2-variable problem, x₁∈[0,20], x₂∈[100,200], rand=0.25, 0.8  
$$x_1 = 0 + 0.25×(20-0) = 5$$
$$x_2 = 100 + 0.8×(200-100) = 180$$
→ Food source = **(5, 180)**

**Q3:** xᵢ=30, xₖ=18, rand=−0.5  
$$v_{\text{new}} = 18 + (-0.5)(30-18) = 18-6 = 12$$

---

*End of Notes — Soft Computing*
