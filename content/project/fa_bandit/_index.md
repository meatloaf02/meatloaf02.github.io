# Multi-Armed Bandit Simulation: Optimizing Financial Aid Discount Rates

## Overview

Build a contextual multi-armed bandit simulation in a **Jupyter notebook** that models a tuition-dependent institution optimizing institutional aid (discount rate) offers to maximize **expected net tuition revenue** per admitted student. The bandit must learn that the revenue-maximizing discount is not necessarily the yield-maximizing discount.

## Environment

### Notebook Setup
- Single Jupyter notebook: `financial_aid_bandit.ipynb`
- Dependencies: `numpy`, `matplotlib`, `scipy` (for logistic function), `pandas` (for results tables)
- Set random seed for reproducibility
- Use clear markdown headers to separate sections: Setup, Simulation Parameters, Algorithm Implementations, Experiment Runner, Results & Visualization

### Arms (Discount Tiers)
Five arms representing institutional discount rates applied to a **base tuition of $50,000**:

| Arm | Discount Rate | Net Revenue if Enrolled |
|-----|--------------|------------------------|
| 0   | 20%          | $40,000                |
| 1   | 30%          | $35,000                |
| 2   | 40%          | $30,000                |
| 3   | 50%          | $25,000                |
| 4   | 60%          | $20,000                |

### Student Segments (Context)

Three segments with different price sensitivities. Enrollment probability follows a logistic curve:

```
P(enroll) = floor + (ceiling - floor) / (1 + exp(-k * (discount - midpoint)))
```

**High-Need Students (40% of admit pool)**
- floor = 0.05, ceiling = 0.85, midpoint = 0.45, k = 15
- Very price-sensitive; without significant aid they can't attend
- Expected revenue per offer at each arm:
  - 20%: ~$3,200 | 30%: ~$5,250 | 40%: ~$11,400 | **50%: ~$18,000** | 60%: ~$16,400
- **True optimal arm: 50% discount**

**Merit-Focused Students (35% of admit pool)**
- floor = 0.15, ceiling = 0.70, midpoint = 0.35, k = 10
- Moderately sensitive; weigh fit, reputation, program quality alongside aid
- **Compute and display the true optimal arm for this segment**

**Low-Need Students (25% of admit pool)**
- floor = 0.25, ceiling = 0.55, midpoint = 0.30, k = 8
- Relatively price-inelastic; choosing based on brand, location, program
- **Compute and display the true optimal arm for this segment**

### Reward Function

```
reward = enrolled * tuition * (1 - discount_rate)
```

Where `enrolled` is a Bernoulli draw with probability `P(enroll | discount, segment)`.

- Reward is **0** if student does not enroll
- Reward is `tuition * (1 - discount_rate)` if student enrolls

This creates the key tension: higher discounts increase yield but decrease per-student revenue.

## Algorithms to Implement

### 1. Epsilon-Greedy (baseline)
- Epsilon = 0.1 (explore 10% of the time)
- Track estimated reward per arm (per segment for contextual version)
- Simple and interpretable

### 2. UCB1 (Upper Confidence Bound)
- Uses deterministic confidence intervals to balance exploration/exploitation
- `UCB = mean_reward + c * sqrt(ln(total_pulls) / arm_pulls)` where c = 2
- Per-segment tracking for contextual version

### 3. Thompson Sampling
- Bayesian approach using reward distribution posteriors
- Since rewards are not binary (they're 0 or a dollar amount), use a Normal distribution posterior:
  - Track running mean and variance of rewards per arm (per segment)
  - Sample from Normal(mean, variance/n) for each arm and pick the highest sample
- Alternatively, you can normalize rewards to [0, 1] and use Beta posteriors — implementer's choice, but document the approach

## Experiment Design

### Simulation Parameters
- **Total admits**: 5,000 (roughly two recruiting cycles for a mid-size institution)
- **Segment distribution**: 40% high-need, 35% merit, 25% low-need
- Each "round" represents one admitted student arriving with a segment label; the bandit picks which discount arm to pull
- **Number of independent runs**: 30 (to compute confidence intervals on results)

### Non-Contextual vs. Contextual Comparison
Run two versions of each algorithm:
1. **Non-contextual**: Ignores segment info, learns a single policy across all students
2. **Contextual**: Maintains separate bandit statistics per segment, learns segment-specific policies

This comparison demonstrates why context matters — the optimal arm differs by segment.

### Metrics to Track
- **Cumulative regret** over time: difference between the oracle (always picking the true optimal arm per segment) and the algorithm's choices
- **Cumulative net revenue** over time
- **Arm selection frequency** per segment (at convergence)
- **Learned policy vs. true optimal policy** per segment

## Visualizations (produce all of these)

### 1. Enrollment Probability Curves
- Single plot with three logistic curves (one per segment) showing P(enroll) vs. discount rate
- X-axis: discount rate (0.2 to 0.6), Y-axis: enrollment probability
- Color-coded by segment with clear legend

### 2. Expected Revenue Curves
- Single plot with three curves showing expected revenue per offer vs. discount rate
- `E[revenue] = P(enroll) * tuition * (1 - discount)`
- Mark the true optimal discount for each segment with a vertical line or star marker
- This is the key insight plot — the peak is NOT at the highest discount

### 3. Cumulative Regret Comparison
- Line plot of cumulative regret over 5,000 admits
- One line per algorithm (3 algorithms × 2 versions = 6 lines)
- Use solid lines for contextual, dashed for non-contextual
- Include shaded confidence intervals from the 30 runs
- Title: "Cumulative Regret: Contextual vs. Non-Contextual Bandits"

### 4. Cumulative Net Revenue Comparison
- Same structure as regret plot but showing total revenue captured
- Include a horizontal dashed line for the oracle's expected total revenue
- Include a horizontal dashed line for a "naive strategy" (offering everyone 40% discount)

### 5. Learned Policy Table
- At the end of the simulation, display a table showing:
  - For each segment: the arm each contextual algorithm converges to vs. the true optimal
  - Arm pull distribution (what % of the time each arm was selected in the final 1,000 rounds)

### 6. Arm Selection Over Time (optional but nice)
- For the best-performing algorithm, show a stacked area chart of arm selection proportions over time, faceted by segment
- Shows the algorithm converging from exploration to exploitation

## Code Organization Within the Notebook

Structure the notebook with these sections:

1. **Imports & Configuration** — all parameters in a config dict for easy tuning
2. **Enrollment Model** — functions defining the logistic curves and reward calculation
3. **Insight Plots** — plots 1 and 2 above (show these before running bandits to build intuition)
4. **Bandit Algorithms** — clean class-based implementations (base class with Epsilon-Greedy, UCB1, Thompson Sampling subclasses)
5. **Experiment Runner** — function that runs one full simulation, returns metrics; outer loop for 30 runs
6. **Results & Visualization** — plots 3, 4, 5, 6 and summary statistics
7. **Discussion** — markdown cells interpreting results: which algorithm performed best, how quickly each converged, why contextual outperforms non-contextual, implications for enrollment management practice

## Style Notes
- Use clean, publication-quality matplotlib styling (increase font sizes, use a professional color palette)
- Add descriptive markdown cells between code cells explaining what's happening and why
- Print intermediate sanity checks (e.g., verify enrollment probabilities match expectations at each arm)
- All dollar values should be formatted with commas and dollar signs in output
