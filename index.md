---
title: Ansatz Analytica Points System
description: A modern, context-aware performance rating and prediction model for football.
layout: default
---

# Ansatz Analytica Points System

The *Ansatz Analytica Points System (AAPS)* is an alternative method for evaluating team performance and predicting outcomes in football (soccer), combining statistical smoothing, probabilistic inference, and customized scoring rules. Designed to enhance predictive accuracy and account for contextual variability across matches and leagues, AAPS provides a dynamic and adaptive framework for understanding team strength over time.

## Overview

Unlike traditional point systems that use static values (e.g., 3 points for a win, 1 for a draw), AAPS introduces weighted and probabilistic approaches to measuring performance. It combines **Bayesian smoothing**, **exponential smoothing**, and **negative binomial modeling** to estimate team form and scoreline probabilities.

## Core Methodology

### Exponential Smoothing

AAPS uses exponential smoothing to give more weight to recent match results. This is useful for capturing momentum or decline:

```
Smoothed Value = α × current_value + (1 − α) × previous_smoothed_value
```

### Bayesian Smoothing

Bayesian smoothing incorporates prior knowledge with sample data. It accounts for sample size and prior expectations. It is applied in two forms:

Rate smoothing (e.g., goals per game): a weighted average of observed values and league priors.

Binary smoothing (e.g., games scored, clean sheets): calculates smoothed probabilities from binary outcomes using league-wide priors.

- For rate (e.g., goals/game):  
```
smoothed = (prior_mean × prior_weight + sample_mean × sample_size) / (prior_weight + sample_size)
```

- For binary outcomes (e.g., clean sheets):  
  Similar logic using success counts.

## Metrics Tracked
For each team, the system analyzes:
- Goals scored/conceded
- Games with goals (binary)
- Clean sheets (binary)
- Total games played
These statistics are smoothed using either exponential or Bayesian techniques, depending on the data size and configuration.

## Venue-Aware Points
AAPS modifies the classic point allocation by factoring in match venue, assigning different weights to wins, draws, and losses:

| Outcome | Home | Away |
|--------|------|------|
| Win    | 3    | 4    |
| Draw   | 1    | 2    |
| Loss   | -1   | 0    |

This design penalizes underperformance at home and rewards overperformance away, enhancing competitive context.

## Performance Ratio
Each match is assigned a max possible point value based on its venue. A team’s smoothed points total is normalized by its smoothed maximum to yield:

```
Performance Ratio = Smoothed Points / Smoothed Max Points
```

Separate performance ratios are computed for home, away, and overall records.

## Expected Goals (λ)

AAPS estimates the expected number of goals using a compound lambda (λ) formulation:

- Team's smoothed goal scoring rate
- Opponent's smoothed conceding rate
- Probability of team scoring
- Probability of opponent not keeping a clean sheet
- League-specific multiplier and venue-based adjustments

This  value feeds into a probabilistic goal model.

## Probabilistic Goal Modeling

### Negative Binomial Model
AAPS uses the Negative Binomial Distribution to model the number of goals scored. This distribution accounts for overdispersion, a common phenomenon in football data where variance exceeds the mean.

```




```

For goals `k`:
```
P(X = k) = C(k+r-1, k) × p^r × (1−p)^k
```
Fallback to Poisson if unstable:
```
P(X = k) = (λ^k × e^−λ) / k!
```

Probabilities are calculated for 0–10 goals.

## League Calibration

- League-specific priors
- Venue-specific scoring multipliers

## Comparison

| Feature | Traditional | Elo | FIFA | AAPS |
|--------|-------------|-----|------|------|
| Venue-aware | No | No | Partial | Yes |
| Time-weighted | No | Yes | No | Yes |
| Probabilistic | No | No | No | Yes |
| League adaptive | No | Partial | Yes | Yes |

## Practical Example

**Team A** is great at home, poor away.  
**Team B** is consistent everywhere.

They’re equal on the traditional table.

But AAPS shows:

- Team A (away): 0.35 performance ratio  
- Team B (home): 0.70 performance ratio

**Outcome:** AAPS reveals Team B is more reliable—valuable insight for bettors or predictors.

## Use Cases

- Sports betting analytics  
- Power rankings  
- AI commentary/simulation

## Summary

AAPS is a modern, data-driven system that rewards consistency, accounts for context, and models match outcomes probabilistically. It's well-suited to sports tech, AI, and performance evaluation platforms.
