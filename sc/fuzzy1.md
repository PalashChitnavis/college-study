# 🧠 Fuzzy Logic – Complete Short Notes

> These notes cover **Operations with Fuzzy Sets**, **Properties**, **Hedges**, **Fuzzy Rules**, **Fuzzy Inference Systems (FIS)**, and **Applications**. Suitable for first-time readers.

---

## 📌 What is Fuzzy Logic?

Fuzzy logic is an **extension of ordinary (Boolean) logic**. In ordinary logic, everything is either `0` (false) or `1` (true). In fuzzy logic, truth values are **continuous** — any value between 0 and 1 is allowed.

A **fuzzy set** assigns each element a **degree of membership** (μ) between 0 and 1.

---

## 🔧 Operations with Fuzzy Sets

Since fuzzy logic extends ordinary logic, the same set operations apply — but adapted for partial membership.

---

### 1. Complement (NOT)

The complement of a fuzzy set `A` (written `¬A`) gives the "opposite" set.

**Formula:**
$$\mu_{\neg A}(x) = 1 - \mu_A(x)$$

**Example:**

| Age | Old people (μ) | Not Old people (μ) |
|-----|----------------|---------------------|
| 30  | 0.0            | 1.0                 |
| 40  | 0.2            | 0.8                 |
| 50  | 0.4            | 0.6                 |
| 60  | 0.6            | 0.4                 |
| 70  | 0.8            | 0.2                 |
| 80  | 1.0            | 0.0                 |

> 💡 The graph of `¬A` is a mirror image of `A` — where `A` is high, `¬A` is low and vice versa.

---

### 2. Containment (Subset)

Fuzzy set `A` is a **subset** of fuzzy set `B` if every element of `A` has a membership degree **less than or equal to** that in `B`.

**Formula:**
$$\mu_A(x) \leq \mu_B(x), \quad \text{for all } x \in X$$

**Example:**
- "Very old people" ⊆ "Old people" — the degree of membership of every person in "very old" is ≤ their degree in "old".

> 💡 On a graph, set B's curve lies entirely **below** set A's curve when B ⊆ A.

---

### 3. Intersection (AND)

In fuzzy logic, the intersection takes the **minimum** membership value of an element across two sets.

**Formula:**
$$\mu_{A \cap B}(x) = \min\{\mu_A(x),\, \mu_B(x)\}, \quad \text{for all } x \in X$$

**Example (Old ∩ Average people):**

| Age | Old (μ) | Average (μ) | Intersection (μ) |
|-----|---------|-------------|------------------|
| 30  | 0.0     | 0.1         | 0.0              |
| 40  | 0.1     | 0.2         | 0.1              |
| 50  | 0.2     | 0.6         | 0.2              |
| 60  | 0.6     | 0.5         | **0.5**          |
| 65  | 0.7     | 0.2         | **0.2**          |
| 70  | 0.8     | 0.1         | **0.1**          |
| 75  | 0.9     | 0.0         | 0.0              |
| 80  | 1.0     | 0.0         | 0.0              |

> 💡 Intersection is the **overlapping region** — the area where both sets are active simultaneously.

---

### 4. Union (OR)

The union takes the **maximum** membership value of an element across two sets.

**Formula:**
$$\mu_{A \cup B}(x) = \max\{\mu_A(x),\, \mu_B(x)\}, \quad \text{for all } x \in X$$

**Example (Old ∪ Average people):**

| Age | Old (μ) | Average (μ) | Union (μ) |
|-----|---------|-------------|-----------|
| 30  | 0.0     | 0.1         | **0.1**   |
| 40  | 0.1     | 0.2         | **0.2**   |
| 50  | 0.2     | 0.6         | **0.6**   |
| 60  | 0.6     | 0.5         | **0.6**   |
| 65  | 0.7     | 0.2         | **0.7**   |
| 70  | 0.8     | 0.1         | **0.8**   |
| 75  | 0.9     | 0.0         | **0.9**   |
| 80  | 1.0     | 0.0         | **1.0**   |

> 💡 Union is the **combined coverage** — Union is always ≥ both individual sets.

---

### 5. Equality

Two fuzzy sets `A` and `B` are **equal** if they have the same membership degree at every point.

**Formula:**
$$\mu_A(x) = \mu_B(x), \quad \text{for all } x \in X$$

---

### 6. Algebraic Product

**Formula:**
$$\mu_{AB}(x) = \mu_A(x) \cdot \mu_B(x), \quad \text{for all } x \in X$$

---

### 7. Algebraic Sum

**Formula:**
$$\mu_{A+B}(x) = \mu_A(x) + \mu_B(x), \quad \text{for all } x \in X$$

---

## 🏗️ Properties of Fuzzy Sets

Fuzzy sets obey the same algebraic properties as ordinary sets.

---

### 1. Associativity

Grouping does not matter when applying AND or OR repeatedly.

$$A \cup (B \cup C) = (A \cup B) \cup C$$
$$A \cap (B \cap C) = (A \cap B) \cap C$$

**Example:** `young ∪ (average ∪ old)` = `(young ∪ average) ∪ old`

---

### 2. Distributivity

$$A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$$
$$A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$$

---

### 3. Commutativity

Order doesn't matter for AND / OR.

$$A \cup B = B \cup A$$
$$A \cap B = B \cap A$$

---

### 4. Transitivity

$$\text{IF } (A \subset B) \text{ AND } (B \subset C) \text{ THEN } (A \subset C)$$

**Example:** IF (old ⊂ very old) AND (very old ⊂ extremely old) THEN (old ⊂ extremely old)

---

### 5. Idempotency

Applying a set operation to itself yields the same set.

$$A \cup A = A \qquad A \cap A = A$$

---

### 6. Identity

Given the **empty set** ∅ (all memberships = 0) and the **universal set** X (all memberships = 1):

$$A \cup \emptyset = A \qquad A \cap \emptyset = \emptyset$$
$$A \cup X = X \qquad A \cap X = A$$

---

### 7. Involution (Double Negation)

$$\neg(\neg A) = A$$

**Example:** NOT(NOT young people) = young people

---

### 8. De Morgan's Laws

$$\neg(A \cap B) = \neg A \cup \neg B$$
$$\neg(A \cup B) = \neg A \cap \neg B$$

**Example:**
- ¬(young ∩ tall) = ¬young ∪ ¬tall
- ¬(young ∪ tall) = ¬young ∩ ¬tall

---

## 🎚️ Hedges (Linguistic Modifiers)

**Hedges** are adjectives or adverbs (like "very", "slightly", "somewhat") that **modify the membership function** of a fuzzy set — either concentrating or dilating it.

> General rule: **Power > 1** → reduces (concentrates); **Power < 1** → increases (dilates)

---

### Hedges that REDUCE truth value (Concentration)

| Hedge       | Formula                                  | Effect on μ = 0.6 |
|-------------|------------------------------------------|-------------------|
| **very**    | $\mu_{A\_very}(x) = \mu_A(x)^2$         | 0.6² = **0.36**   |
| **extremely** | $\mu_{A\_extremely}(x) = \mu_A(x)^3$  | 0.6³ = **0.216 ≈ 0.21** |
| **very very** | $\mu_{A\_veryvery}(x) = \mu_A(x)^4$   | 0.6⁴ = **0.13 ≈ 0.12** |

---

### Hedges that INCREASE truth value (Dilation)

| Hedge        | Formula                                          | Effect on μ = 0.6 |
|--------------|--------------------------------------------------|-------------------|
| **somewhat** | $\mu_{A\_somewhat}(x) = \sqrt{\mu_A(x)}$         | √0.6 = **0.77**   |
| **slightly** | $\mu_{A\_slightly}(x) = \sqrt[3]{\mu_A(x)}$      | ∛0.6 = **0.84**   |

---

### Hedges that INTENSIFY truth value

| Hedge     | Formula | Effect |
|-----------|---------|--------|
| **indeed** | $\mu_{A\_indeed}(x) = \begin{cases} 2 \cdot (\mu_A(x))^2, & \text{if } 0 \leq \mu_A(x) \leq 0.5 \\ 1 - 2(1 - \mu_A(x))^2, & \text{if } 0.5 < \mu_A(x) \leq 1 \end{cases}$ | If μ > 0.5: increases; if μ < 0.5: decreases |

**Examples for "indeed":**
- μ = 0.6 → `1 - 2(1-0.6)² = 1 - 0.32 = 0.68`
- μ = 0.3 → `2 × 0.3² = 0.18`

> 💡 Think of "indeed" as a **contrast enhancer** — strong memberships get stronger, weak ones get weaker.

---

## 📏 Fuzzy Rules

Fuzzy rules are **linguistic IF-THEN constructs**:

```
IF  <premise/antecedent>
THEN <consequence>
```

Where premises and consequences use **linguistic variables** (fuzzy sets).

**Example conversion from crisp to fuzzy:**

| Ordinary Logic | Fuzzy Logic |
|----------------|-------------|
| IF temp = -5 THEN cold | IF temp is **low** THEN weather is **cold** |
| IF temp = 15 THEN warm | IF temp is **medium** THEN weather is **warm** |
| IF temp = 35 THEN hot | IF temp is **high** THEN weather is **hot** |

Rules can have **multiple antecedents** joined by AND / OR:
- **AND** → takes the **minimum** (intersection)
- **OR** → takes the **maximum** (union)
- **NOT** → complement: `μ¬A(x) = 1 - μA(x)`

---

## ⚙️ Fuzzy Inference System (FIS)

A Fuzzy Inference System processes crisp inputs through fuzzy logic to produce crisp outputs. The pipeline is:

```
Crisp Input
    ↓
[FUZZIFICATION]        ← Convert crisp values to fuzzy degrees
    ↓
[EVALUATE FUZZY RULES] ← Apply IF-THEN rules
    ↓
[COMBINE INFORMATION]  ← Aggregate rule outputs
    ↓
[DEFUZZIFICATION]      ← Convert fuzzy output back to crisp value
    ↓
Crisp Output
```

---

### Step 1: Fuzzification

- Crisp inputs are **mapped onto membership functions**
- Each input gets a degree of membership (μ) for each fuzzy label
- The universe of discourse is divided into fuzzy sets (e.g., low, below average, average, above average, high)

**Key design rules for membership functions:**
- Every point in the universe must belong to **at least one** membership function
- No two functions can share the **same peak (maximal meaningfulness)**
- When two functions overlap, the sum of memberships at any overlapping point must be **≤ 1**
- The overlap must **not cross** the peak of either function

**Overlap Metrics:**

$$\text{Overlap ratio} = \frac{\text{overlap scope}}{\text{adjacent membership function scope}}$$

$$\text{Overlap robustness} = \frac{\int_L^U (\mu_1 + \mu_2)\,dx}{2(U - L)}$$

---

### Step 2: Rule Evaluation

Each rule fires based on the fuzzified inputs. For a rule like:

```
IF x is A AND y is B  →  truth = min(μA(x), μB(x))
IF x is A OR y is B   →  truth = max(μA(x), μB(x))
```

The resulting truth value **clips or scales** the output membership function.

---

### Step 3: Aggregation

All activated rule outputs are **combined** (typically using union/max) into a single fuzzy output set.

---

### Step 4: Defuzzification

Converts the aggregated fuzzy output into a **single crisp value**.

**5 Common Methods:**

| Method | Description | Formula |
|--------|-------------|---------|
| **COA** (Centroid of Area) | Most popular; finds the center of gravity | $COA = \dfrac{\sum \mu_A(z) \cdot z}{\sum \mu_A(z)}$ |
| **BOA** (Bisector of Area) | Splits area into two equal halves | $\int_\alpha^{BOA} \mu_A(z)\,dz = \int_{BOA}^\beta \mu_A(z)\,dz$ |
| **MOM** (Mean of Maximum) | Average of all max-membership points | $MOM = \dfrac{\int_{Z'} z\,dz}{\int_{Z'} dz}$ where $Z' = \{z \mid \mu_A(z) = \mu^*\}$ |
| **SOM** | Smallest value at maximum membership | — |
| **LOM** | Largest value at maximum membership | — |

---

## 🔬 Fuzzy Inference Models

### Mamdani Fuzzy Model (1975)

- Most widely accepted
- Both inputs AND outputs are **fuzzy sets**
- Rule consequents are **fuzzy regions** (triangular, trapezoidal shapes)
- Defuzzification uses **COG (Center of Gravity)**
- More **intuitive** and human-understandable
- **Higher computational cost**

**Rule format:**
```
IF x is A AND y is B  THEN z is C
```

**4 Steps:**
1. **Fuzzification** — compute μ for each input
2. **Rule Evaluation** — fire all rules, clip output sets
3. **Aggregation** — combine all clipped output sets
4. **Defuzzification** — compute COG

**Worked Example (Weather System):**

Rules:
```
Rule 1: IF temp is LOW AND wind is STRONG → weather is COLD
Rule 2: IF temp is MEDIUM OR wind is GENTLE → weather is AVERAGE
Rule 3: IF temp is HIGH OR wind is GENTLE → weather is HOT
```

Given: temperature = 25°C, wind = 35 km/h

After fuzzification: `μA1=0, μA2=0.4, μA3=0.15, μB1=0.8, μB2=0`

Rule results: `μC1=0, μC2=0.4, μC3=0.15`

Defuzzification (COG):
$$COG = \frac{(0{+}10{+}20{+}30) \times 0.0 + (40{+}50{+}60{+}70) \times 0.4 + (80{+}90{+}100) \times 0.15}{0+0+0+0+0.4+0.4+0.4+0.4+0.15+0.15+0.15} = \frac{128.5}{2.05} = \mathbf{62.68}$$

> **Interpretation:** The weather is hot with 62.68% intensity.

---

### Sugeno Fuzzy Model (1985)

Also called **Takagi-Sugeno-Kang (TSK)** model.

- Output membership functions are **singletons (spikes)** or linear/polynomial functions — NOT fuzzy regions
- Much more **computationally efficient**
- Works well with **optimization and adaptive control**
- Better for **dynamic nonlinear systems**

**Rule format (zero-order):**
```
IF x is A AND y is B  THEN z = k   (k is a constant)
```

**Rule format (first-order):**
```
IF x is A AND y is B  THEN z = ax + by + c
```

**Defuzzification** uses **Weighted Average (WA)**:

$$WA = \frac{\mu(k_1) \times k_1 + \mu(k_2) \times k_2 + \mu(k_3) \times k_3}{\mu(k_1) + \mu(k_2) + \mu(k_3)}$$

**Worked Example (same weather problem, Sugeno):**

$$\text{Crisp output} = \frac{0 + 0.4 \times 0.55 + 0.15 \times 0.75}{0 + 0.4 + 0.15} = \frac{0.33}{0.55} = \mathbf{0.6}$$

---

### Mamdani vs. Sugeno — Quick Comparison

| Feature | Mamdani | Sugeno |
|---------|---------|--------|
| Output type | Fuzzy set | Singleton / linear function |
| Defuzzification | COG (integration) | Weighted Average |
| Computational cost | Higher | Lower (efficient) |
| Interpretability | More intuitive | Less intuitive |
| Best for | Human expert systems | Control & optimization |
| Supports adaptive techniques | No | Yes |

---

## 🔗 Fuzzy Relations and Implication

**Fuzzy relation** R(A, B) connects two fuzzy sets over universes X and Y:
$$\mu_{R(x,y)} : X \times Y \rightarrow [0,1]$$

**Fuzzy Implication** (A → B) can be computed in multiple ways:

| Method | Formula |
|--------|---------|
| **Mamdani** | $R(x_i, y_i) = \sum \mu_A(x_i) \wedge \mu_B(y_i) / (x_i, y_i)$ |
| **Larson** | $R(x_i, y_i) = \sum \mu_A(x_i) \times \mu_B(y_i) / (x_i, y_i)$ |
| **Bounded Difference** | $R(x_i, y_i) = \sum 0 \vee (\mu_A(x_i) + \mu_B(y_i) - 1) / (x_i, y_i)$ |
| **Goguen** | $R(x,y) = 1$ if $\mu_A \leq \mu_B$; else $\mu_A(x)/\mu_B(x)$ |
| **Kurt Gödel** | $R(x,y) = 1$ if $\mu_A \leq \mu_B$; else $\mu_B(x)$ |

**Two Inference Procedures:**
- **GMP** – Generalized Modus Ponens (affirms): "if P then Q; P is true → Q is true"
- **GMT** – Generalized Modus Tollens (denies): "if P then Q; Q is false → P is false"

---

## 🌍 Applications of Fuzzy Logic

### Financial
- Banknote transfer control
- Fund management
- Stock market predictions

### Industrial
- Cement kiln control
- Heat exchanger control
- Water purification plant control
- Wastewater treatment

### Defense
- Underwater target recognition
- Thermal infrared image recognition
- NATO decision-making modeling

### Medical
- Diagnostic support systems
- Anesthesia pressure control
- Alzheimer's neuropathology modeling
- Diabetes and prostate cancer diagnosis

### Electronics
- Auto-exposure in video cameras
- Air conditioning systems
- Washing machine timing
- Microwave ovens, vacuum cleaners

### Robotics
- Flexible-link manipulator control
- Robot arm control

### Marine
- Ship autopilot
- Autonomous underwater vehicle control

### Manufacturing & Chemical
- Cheese production optimization
- pH control, distillation process control

### Business & Securities
- Decision-making support
- Personnel evaluation
- Securities trading systems

---

## 📝 Summary of Key Formulas

| Operation | Formula |
|-----------|---------|
| Complement | $\mu_{\neg A}(x) = 1 - \mu_A(x)$ |
| Intersection | $\mu_{A \cap B}(x) = \min(\mu_A, \mu_B)$ |
| Union | $\mu_{A \cup B}(x) = \max(\mu_A, \mu_B)$ |
| Algebraic Product | $\mu_{AB}(x) = \mu_A(x) \cdot \mu_B(x)$ |
| Algebraic Sum | $\mu_{A+B}(x) = \mu_A(x) + \mu_B(x)$ |
| Hedge: very | $\mu^2$ |
| Hedge: extremely | $\mu^3$ |
| Hedge: very very | $\mu^4$ |
| Hedge: somewhat | $\sqrt{\mu}$ |
| Hedge: slightly | $\sqrt[3]{\mu}$ |
| COG Defuzzification | $\dfrac{\sum \mu_A(z) \cdot z}{\sum \mu_A(z)}$ |
| Sugeno WA | $\dfrac{\sum \mu(k_i) \cdot k_i}{\sum \mu(k_i)}$ |

---

*End of Notes — Fuzzy Logic*
