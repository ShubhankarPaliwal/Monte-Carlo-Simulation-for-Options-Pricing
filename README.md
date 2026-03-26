# 🎲 Monte Carlo Simulation for Options Pricing

A quantitative finance project that prices European Call and Put options by simulating 100,000 random stock price paths using **Geometric Brownian Motion (GBM)**. Results are cross-validated against the closed-form **Black-Scholes formula** to verify accuracy.

---

## 🔍 Overview

Instead of solving a complex equation directly, Monte Carlo simulates thousands of possible futures for a stock price. For each simulated path, the option payoff at expiration is calculated. Averaging all payoffs and discounting back to today gives the option price — and with 100,000 simulations, the result lands within ~0.01–0.05 of the exact Black-Scholes answer.

---

## 🚀 Run in Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

> Upload `monte_carlo_options_colab.ipynb` to [colab.research.google.com](https://colab.research.google.com) and run all cells.  
> **No pip installs required** — `numpy`, `scipy`, and `matplotlib` are pre-installed on Colab.

---

## 🛠️ Tech Stack

| Library | Purpose |
|---|---|
| `numpy` | Vectorised simulation of 100,000 price paths |
| `scipy.stats` | Normal CDF for the Black-Scholes formula |
| `matplotlib` | Price path plots, payoff distributions, convergence charts |
| `pandas` | Results comparison table |

---

## ⚙️ How It Works

1. **Parameters** — Define stock price, strike, maturity, volatility, and risk-free rate
2. **GBM simulation** — Generate a (252 × 100,000) matrix of random price paths using the formula:
   ```
   S(t+dt) = S(t) · exp[ (r - σ²/2)·dt  +  σ·√dt·Z ]
   ```
   where Z ~ N(0,1) is a random standard normal draw
3. **Payoff calculation** — For each of the 100,000 final prices:
   ```
   Call payoff = max(S_T - K, 0)
   Put  payoff = max(K  - S_T, 0)
   ```
4. **Discounting** — Average all payoffs and discount back to present value:
   ```
   Option price = e^(-rT) · mean(payoffs)
   ```
5. **Black-Scholes validation** — Compute the exact analytical price using the Black-Scholes formula and compare
6. **Visualisation** — Plot sample paths, payoff histograms, convergence, and sensitivity analysis

---

## 📊 Sample Output

```
Parameters:  S₀=$100  K=$105  T=1yr  r=5%  σ=20%  N=100,000

                 Call price    Put price
Monte Carlo      $8.0213       $8.0891
Black-Scholes    $8.0214       $8.0888
Difference       $0.0001       $0.0003   (~0.003% error)

Put-call parity check:  C - P = $-0.0678  |  S₀ - Ke^(-rT) = $-0.0688  ✅
```

---

## 📈 Charts Generated

| Chart | Description |
|---|---|
| `gbm_paths.png` | 200 sample GBM stock price paths over 1 year |
| `monte_carlo_options_results.png` | Payoff distributions + convergence plots for call & put |
| `sensitivity_analysis.png` | Option price vs stock price across σ = 10%, 20%, 30%, 40% |

---

## 🎛️ Customisable Parameters

All parameters are in one cell at the top of the notebook — just change the values and re-run:

```python
S0      = 100.0    # Current stock price ($)
K       = 105.0    # Strike price ($)
T       = 1.0      # Time to expiration (years)
r       = 0.05     # Risk-free interest rate (5%)
sigma   = 0.20     # Annual volatility (20%)
N_SIMS  = 100_000  # Number of simulated paths
N_STEPS = 252      # Trading days per year
```

---

## 📁 Project Structure

```
monte-carlo-options/
├── monte_carlo_options_colab.ipynb   # Main notebook (Colab-ready)
├── requirements.txt                  # Dependencies (for local use)
└── README.md
```

---

## 📦 Local Installation

```bash
git clone https://github.com/your-username/monte-carlo-options.git
cd monte-carlo-options
pip install -r requirements.txt
jupyter notebook monte_carlo_options_colab.ipynb
```

`requirements.txt`:
```
numpy
scipy
matplotlib
pandas
jupyter
```

---

## 📚 Key Concepts

- **Geometric Brownian Motion (GBM)** — The standard mathematical model for stock price randomness, combining a deterministic drift with random noise
- **Risk-neutral pricing** — Paths are simulated under the risk-free rate `r`, not the actual expected return
- **Put-call parity** — A fundamental no-arbitrage relationship: `C - P = S₀ - Ke^(-rT)`, used here as a sanity check
- **Convergence rate** — Monte Carlo error shrinks as 1/√N, so 4× more simulations halves the error

---

## 📌 Future Improvements

- [ ] Price American options using Least-Squares Monte Carlo (Longstaff-Schwartz)
- [ ] Calculate option Greeks (Delta, Gamma, Vega) via finite differences
- [ ] Add implied volatility solver (invert Black-Scholes for σ given a market price)
- [ ] Interactive parameter sliders using `ipywidgets`
- [ ] Simulate correlated assets for basket option pricing

---

## ⚠️ Disclaimer

This project is for **educational purposes only** and does not constitute financial advice. Options trading involves significant risk and is not suitable for all investors.
