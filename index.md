---
title: Ansatz Analytica Points System
description: A modern, context-aware performance rating and prediction model for football.
layout: default
---

# Ansatz Analytica Points System

The *Ansatz Analytica Points System (AAPS)* is an alternative method for evaluating team performance and predicting outcomes in football (soccer), combining statistical smoothing, probabilistic inference, and customized scoring rules.

## Overview

Unlike traditional point systems that use static values (e.g., 3 points for a win, 1 for a draw), AAPS introduces weighted and probabilistic approaches to measuring performance. It combines **Bayesian smoothing**, **exponential smoothing**, and **negative binomial modeling** to estimate team form and scoreline probabilities.

## Core Methodology

### Exponential Smoothing

Used to emphasize recent performance:

```
Smoothed Value = α × current_value + (1 − α) × previous_smoothed_value
```

### Bayesian Smoothing

Incorporates prior knowledge with sample data:

- For rate (e.g., goals/game):  
```
smoothed = (prior_mean × prior_weight + sample_mean × sample_size) / (prior_weight + sample_size)
```

- For binary outcomes (e.g., clean sheets):  
  Similar logic using success counts.

## Metrics Tracked

- Goals scored/conceded
- Games with goals (binary)
- Clean sheets (binary)
- Total games played

## Venue-Aware Points

| Outcome | Home | Away |
|--------|------|------|
| Win    | 3    | 4    |
| Draw   | 1    | 2    |
| Loss   | -1   | 0    |

## Performance Ratio

```
Performance Ratio = Smoothed Points / Smoothed Max Points
```

Calculated separately for home, away, and total.

## Expected Goals (λ)

AAPS uses:

- Scoring & conceding rates
- Scoring probabilities
- Clean sheet tendencies
- League multipliers

## Goal Prediction

### Negative Binomial Model

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
