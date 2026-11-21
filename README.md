# Mean-Variance vs CVaR Portfolio Optimisation

Code for the numerical experiment in the final report for MATH11111 Fundamentals of Optimisation (University of Edinburgh).

The experiment compares two portfolio models on simulated data:

- **Mean-variance (Markowitz):** minimises portfolio variance for a target return.
- **CVaR (Rockafellar and Uryasev, 2000):** minimises the 95% Conditional Value at Risk for the same target return, solved as a linear programme.

## What the code does

1. Simulates 5,000 equally likely scenarios of net returns for four assets (seed 42).
2. **Case 1:** all four assets have normally distributed returns.
3. **Case 2:** asset 4 is replaced by a crash-risk asset with the same mean (7%) and variance as before: it loses 47% with probability 10% and otherwise gains 13%.
4. For each case, solves both models (long-only, fully invested, 6% target return) with `cvxpy`.
5. Evaluates both portfolios on the same scenarios and reports expected return, 95% VaR and 95% CVaR.

The data are artificial. They are chosen to isolate one effect (a tail risk that variance cannot see), not to represent real markets.

## Main results

| Case | Model | Weights (assets 1 to 4) | Return | VaR | CVaR |
|---|---|---|---|---|---|
| 1 (normal) | Mean-variance | 0.235, 0.173, 0.369, 0.222 | 0.060 | 0.098 | 0.1352 |
| 1 (normal) | CVaR | 0.230, 0.180, 0.363, 0.227 | 0.060 | 0.097 | 0.1351 |
| 2 (crash asset) | Mean-variance | 0.195, 0.138, 0.399, 0.268 | 0.060 | 0.093 | 0.1435 |
| 2 (crash asset) | CVaR | 0.268, 0.168, 0.402, 0.162 | 0.060 | 0.089 | 0.1244 |

With normal returns the two models choose almost the same portfolio. With the crash asset, mean-variance holds more of it and carries about 15% more CVaR for the same expected return.

## How to run

```bash
pip install -r requirements.txt
jupyter notebook mv_vs_cvar.ipynb
```

Then choose **Restart and Run All**. Results are reproducible because the random seed is fixed.

## Files

- `mv_vs_cvar.ipynb`: the experiment (code and output).
- `requirements.txt`: Python packages needed (`numpy`, `scipy`, `pandas`, `matplotlib`, `cvxpy`).

## Limitations

Portfolios are optimised and evaluated on the same scenarios (no out-of-sample test), results come from a single simulation, and the model is single-period and long-only with no transaction costs.
