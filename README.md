# QUANT-PREP-GUIDE
This is a repository that I have build as a complete in depth road map of 7-8 months for preparing for quant.


Python for Quant Finance — The Complete Pre-MFE Roadmap

Who this is for: You've finished Python basics (data types, mutability, loops, conditionals, functions, recursion, break/continue/pass). This roadmap takes you from there to "walks into an MFE program already dangerous."

Total duration: ~7–9 months at 15–20 hrs/week. Compress or stretch as needed, but do NOT skip phases — each builds on the last.

The one rule: Never move to the next phase until you've completed the checkpoint project of the current one. Reading ≠ knowing. Only building is knowing.

PHASE 0 — Professional Setup (Week 1)

You'll be judged on your workflow as much as your code.

Git & GitHub: init, add, commit, branch, merge, push/pull, writing good commit messages, .gitignore. Every single thing you build from now on gets committed. Your commit history IS your proof of work for admissions and recruiters.
Environments: venv or conda, pip, requirements.txt. Understand why isolating project dependencies matters.
Tools: VS Code (with Python + Jupyter extensions), Jupyter notebooks for exploration, .py files for real code. Learn the discipline: explore in notebooks, ship in modules.
The terminal: cd, ls, mkdir, running scripts, reading error tracebacks bottom-up.
Checkpoint: Push your two existing projects (stat-arb engine, pricing lab) to GitHub with clean commits. If you can't, redo this phase.

PHASE 1 — Intermediate → Advanced Python (Weeks 2–6)

The gap between "knows Python" and "engineers in Python." Quant interviews at top firms test this layer hard.

1.1 Object-Oriented Programming, properly

Classes, __init__, instance vs class attributes, methods.
Dunder (magic) methods: __repr__, __str__, __eq__, __lt__, __len__, __getitem__, __add__. Exercise: build a Portfolio class where portfolio1 + portfolio2 merges holdings and len(portfolio) returns position count.
Inheritance and super(); when to prefer composition over inheritance.
Abstract base classes (abc module) — how real pricing libraries define an Instrument interface that Option, Bond, Swap all implement.
@property, @staticmethod, @classmethod — know the difference cold.
dataclasses — the modern way to define data containers (a Trade, a Quote).
1.2 Functional & idiomatic Python

List/dict/set comprehensions (nested ones too), generator expressions.
Generators and yield — process a 10GB tick-data file without loading it into memory. This is a real quant-dev interview question.
lambda, map, filter, sorted(key=...), zip, enumerate, any/all.
Decorators — write your own @timer and @memoize decorators. Understand closures first.
*args, **kwargs, unpacking, keyword-only arguments.
Context managers (with, writing your own via __enter__/__exit__).
1.3 Robustness & correctness

Exceptions: try/except/else/finally, raising your own exception classes (class InvalidPriceError(ValueError)).
Type hints: def price(S: float, K: float) -> float:, Optional, list[float], and running mypy. MFE group projects will thank you.
Testing with pytest: write tests FIRST for a function, then implement it. Learn assert, fixtures, pytest.approx for floats, parametrized tests. Non-negotiable: from now on, every module you write gets a test file.
Logging (logging module) instead of print for anything serious.
1.4 Under the hood (interview favorites)

Mutability revisited: why def f(x, lst=[]) is a famous bug. Shallow vs deep copy.
How Python variables are references; is vs ==.
Time complexity of core operations: list append O(1), list insert O(n), dict/set lookup O(1). Big-O basics.
Iterators vs iterables; what for actually does.
Resources: Fluent Python (Ramalho) — read selectively; Corey Schafer's OOP YouTube series; official Python docs on dataclasses/typing.

Checkpoint project: Refactor your derivatives-pricing-lab into an OOP library: abstract PricingEngine base class, BlackScholesEngine, BinomialEngine, MonteCarloEngine subclasses, an Option dataclass, full type hints, and a pytest suite with ≥15 tests (including the cross-validation checks). Commit it. This single refactor teaches you more than a month of tutorials.

PHASE 2 — NumPy Mastery (Weeks 7–9)

NumPy is the bedrock. Quants who loop over arrays get rejected.

ndarray fundamentals: shape, dtype, why NumPy is 100× faster (contiguous memory, C loops).
Creation: array, zeros, ones, arange, linspace, full, eye.
Indexing & slicing: basic, boolean masks (prices[returns < 0]), fancy indexing. Views vs copies — a silent-bug factory; know when a slice shares memory.
Vectorization: rewrite any loop as array ops. Exercise: compute a 20-day moving average three ways (pure loop, np.convolve, cumsum trick) and time all three.
Broadcasting — the rules, cold. Exercise: build a full option-price surface over a strike grid × maturity grid with zero loops.
axis semantics on 2-D/3-D arrays: mean(axis=0) vs axis=1. Simulate 10,000 GBM paths as a matrix and compute per-path and per-date statistics.
Aggregations, argmax/argmin/argsort, where, clip, cumsum/cumprod, diff.
Linear algebra (np.linalg): matrix multiply @, inv, solve (and why solve beats inv), eigenvalues (→ PCA on yield curves later), Cholesky decomposition (→ correlated Monte Carlo — a genuine desk technique).
Random: default_rng, seeding, standard_normal, multivariate_normal, choice (→ bootstrap resampling).
Numerical hygiene: floating-point error, np.isclose, never == on floats; nan propagation.
Checkpoint project: Correlated Monte Carlo engine. Simulate 5 correlated assets via Cholesky, price a basket option, verify the sample correlation matrix matches the input. Fully vectorized — a single Python loop disqualifies it.

PHASE 3 — pandas & Time Series Mastery (Weeks 10–13)

Where quants live day-to-day. Aim for fluency, not familiarity.

Series & DataFrame anatomy: the index is everything. loc vs iloc (and why chained indexing df[a][b] = x is a bug).
I/O: read_csv (parse_dates, dtypes, chunksize for big files), Parquet (and why it beats CSV).
Selection & filtering: boolean masks, query, isin, between.
Cleaning: dropna, fillna (and why ffill is right for prices but forward-filling returns is a crime), duplicates, string methods, astype.
Transformations: apply vs vectorized ops (know why apply is slow), assign, pipe for clean pipelines, map, replace.
GroupBy — split-apply-combine: aggregate returns by sector/month, transform (z-score within groups — cross-sectional signals!), agg with multiple functions.
Merging: merge (inner/left/outer — and how a bad join silently duplicates rows), concat, join; merge_asof for as-of joins of trades to quotes — a real market-microstructure tool.
Time series (the quant core):
DatetimeIndex, timezone handling, business-day calendars.
resample — daily→monthly returns done correctly (compound, don't average!).
rolling / expanding / ewm — moving stats, EWMA volatility (RiskMetrics style).
shift / pct_change / diff — and the look-ahead-bias discipline you already know from your stat-arb project.
MultiIndex: panel data (dates × tickers), stack/unstack, pivot_table.
Performance: vectorize first, itertuples if you must, never iterrows in production.
Resource: Python for Data Analysis (Wes McKinney — pandas' creator). Work the exercises.

Checkpoint project: Equity data pipeline. Download 10 years of prices for ~30 tickers (yfinance), clean them (survivorship notes, missing data policy documented), compute daily/monthly returns, rolling 60-day volatility and correlations, per-sector aggregates, and output a tidy Parquet dataset + a one-page summary notebook. This dataset feeds every later phase.

PHASE 4 — Probability, Statistics & Econometrics in Python (Weeks 14–19)

MFE coursework assumes this. Arriving with it already coded = massive advantage. Do the math on paper AND in code — the combination is what builds the "sharp mind."

4.1 Probability (with scipy.stats + simulation)

Distributions: normal, lognormal, Student-t, chi-square, Poisson, exponential — pdf, cdf, ppf, rvs for each. KNOW why asset returns are fat-tailed (compare fitted normal vs t on real returns; Q-Q plots).
Law of Large Numbers & Central Limit Theorem — demonstrate both by simulation.
Conditional probability & Bayes — code the classic interview problems (dice games, coin sequences, Monty Hall) as simulations.
Moments: skewness, kurtosis of real return data.
4.2 Statistics

Estimation: MLE — fit a distribution to returns by writing the negative log-likelihood and minimizing with scipy.optimize.minimize yourself (don't just call .fit).
Hypothesis testing: t-tests, p-values (and their misuse), confidence intervals via both formulas and bootstrap (rng.choice).
Linear regression deeply (statsmodels): OLS assumptions, reading the full summary table, R², heteroskedasticity, autocorrelation of residuals, Newey-West errors. Application: estimate a stock's beta via CAPM regression; test if alpha is significant.
Multiple regression, multicollinearity (VIF), factor models (regress returns on Fama-French factors — data is free online).
4.3 Time-series econometrics

Stationarity, unit roots, ADF test (you've met it — now understand it).
Autocorrelation: ACF/PACF plots on returns vs |returns| (volatility clustering — a stylized fact you should discover yourself).
AR, MA, ARMA models — fit, forecast, evaluate.
GARCH(1,1) with the arch package: fit to real returns, forecast volatility, compare to realized vol. This is the single most-used time-series model in risk.
Cointegration revisited: now you fully understand your own stat-arb project.
Resources: Introduction to Statistical Learning (free PDF) chapters 2–3; Hilpisch, Python for Finance; MIT OCW 18.S096 (Topics in Mathematics with Applications in Finance — watch the lectures, it's literally a mini pre-MFE).

Checkpoint project: Stylized-facts report. A polished notebook proving on real data: fat tails, volatility clustering, near-zero return autocorrelation, leverage effect. Fit normal vs t vs GARCH, compare with proper statistical tests. Write it like a research note — this doubles as SOP/interview material.

4.4 Parallel math track (paper + Python, ongoing through all phases)

Linear algebra: matrix ops, eigendecomposition, positive-definiteness (covariance matrices!), Cholesky. Do 3Blue1Brown's Essence of Linear Algebra, then implement PCA from scratch on the yield curve (level/slope/curvature — a famous result you can reproduce).
Calculus: partial derivatives (Greeks!), Taylor expansion (delta-gamma P&L approximation), Lagrange multipliers (portfolio optimization), basic ODEs.
Stochastic calculus preview (don't master it — MFE will — but preview): random walks → Brownian motion, Itô's lemma at intuition level, why the −σ²/2 appears. Shreve Stochastic Calculus for Finance I (the binomial book) is very readable NOW and maps directly to your binomial tree code.
PHASE 5 — Core Quant Finance Domains (Weeks 20–28)

Now finance proper. For each domain: learn the theory, implement from scratch, THEN check against a library.

5.1 Portfolio theory & asset allocation

Returns math: simple vs log returns, annualization, geometric vs arithmetic mean.
Markowitz mean-variance optimization: implement the efficient frontier with scipy.optimize.minimize (with constraints: long-only, fully-invested), plot it, find max-Sharpe and min-variance portfolios.
CAPM, beta, security market line. Sharpe/Sortino/Information ratios.
Why naive Markowitz fails in practice (estimation error in the covariance matrix) — demonstrate with a rolling out-of-sample test; touch on shrinkage (Ledoit-Wolf via scikit-learn).
Risk parity as an alternative — implement inverse-vol and equal-risk-contribution weighting.
5.2 Fixed income (MFE-critical, most self-taught quants skip it — don't)

Time value of money, compounding conventions, discount factors.
Bond pricing from cash flows; clean vs dirty price; yield-to-maturity via root-finding (Brent — you know it).
Duration and convexity: derive, implement, verify numerically by bumping yields. Price-yield curve plot.
Yield curve: bootstrapping zero rates from par bonds/swap rates (a genuinely great coding exercise), forward rates, Nelson-Siegel curve fitting via least squares.
PCA on historical yield-curve moves (connects to your linear algebra).
5.3 Derivatives (extend your pricing lab)

Re-derive everything in your lab so you own it: put-call parity (test it numerically!), binomial → Black-Scholes convergence, risk-neutral pricing intuition.
Add: finite-difference PDE solver (explicit + implicit Crank-Nicolson) as a 4th engine; Longstaff-Schwartz least-squares Monte Carlo for American options; Greeks by finite differences vs analytic; delta-hedging simulation (hedge daily, watch P&L variance shrink — the most instructive experiment in derivatives).
Volatility: realized vol estimators, implied vol surfaces on real option chain data (yfinance provides chains), the smile/skew.
5.4 Risk management

VaR three ways: historical, parametric (variance-covariance), Monte Carlo. Implement all three on a multi-asset portfolio.
Expected Shortfall (CVaR) and why regulators moved to it (VaR isn't subadditive — construct the classic counterexample).
Backtesting VaR: Kupiec exceedance test.
Stress testing and scenario analysis; EWMA & GARCH-based dynamic VaR.
Drawdown analytics (you have these — generalize them).
5.5 Backtesting & strategy research (level up your stat-arb work)

Event-driven vs vectorized backtesting architectures — build a small event-driven engine (Strategy / Portfolio / ExecutionHandler classes: your OOP phase pays off).
The seven deadly sins: look-ahead bias, survivorship bias, overfitting, data snooping, ignoring costs/slippage/borrow, unrealistic fills, no out-of-sample test. Be able to give a concrete example of each.
Walk-forward analysis and parameter-sensitivity heatmaps.
Cross-sectional strategies: momentum and mean-reversion on your Phase-3 dataset; decile portfolios; long-short construction.
Resources: Hull, Options, Futures and Other Derivatives (the bible — read alongside coding); Tuckman, Fixed Income Securities; Ernie Chan, Quantitative Trading; QuantStart articles.

Checkpoint project (pick ONE, do it exceptionally):

Multi-strategy backtester: event-driven engine running momentum + pairs-trading with full cost model, walk-forward validation, and a tear-sheet report generator; or
Fixed-income analytics library: curve bootstrapper + bond pricer + duration/convexity + PCA study; or
Risk engine: portfolio VaR/ES via all three methods, GARCH-dynamic VaR, Kupiec backtest, stress scenarios. This becomes GitHub project #3 — your flagship.
PHASE 6 — Machine Learning for Finance (Weeks 29–33)

ML is now table stakes for MFE grads. But finance-flavored ML ≠ Kaggle ML.

scikit-learn workflow: train/test split, pipelines, StandardScaler, metrics.
Models (know how each works, not just .fit()): linear/logistic regression, regularization (Ridge/Lasso — connect to shrinkage!), decision trees, random forests, gradient boosting (XGBoost/LightGBM), k-means, PCA (again — now the ML view).
The finance-specific dangers (this is what separates you): temporal leakage — why random train/test splits are invalid; walk-forward & purged/embargoed cross-validation; the near-zero signal-to-noise ratio of returns; why 55% directional accuracy can be a great model and 90% means you leaked data.
Feature engineering from market data: lagged returns, rolling stats, technical features, cross-sectional ranks.
Bias-variance tradeoff, overfitting diagnostics, feature importance & SHAP.
Read: Advances in Financial Machine Learning (López de Prado) — chapters on labeling (triple-barrier), sample weights, and cross-validation. Hard but formative.
Optional stretch: a small PyTorch feedforward net for the same prediction task — mostly to learn the framework exists; deep learning depth can wait for the MFE.
Checkpoint project: Return-prediction study done RIGHT. Predict next-day/next-week direction for liquid ETFs with proper purged walk-forward CV, honest benchmarks (always-long, momentum), transaction-cost-aware evaluation, and a written conclusion — even if the conclusion is "no exploitable edge found" (a negative result presented rigorously is more impressive to quants than a fake 80% accuracy).

PHASE 7 — Performance, Data Engineering & Polish (Weeks 34–37)

What makes you "quant developer capable" — a huge differentiator.

Profiling before optimizing: %timeit, cProfile, line_profiler. Find the actual bottleneck; never guess.
Numba @njit: accelerate your Monte Carlo and backtest loops 50–200×; understand what it can/can't compile.
Multiprocessing vs threading vs the GIL (classic interview question); concurrent.futures for parallel simulations.
Memory: dtypes (float32 vs 64), categoricals, chunked processing, Parquet.
SQL (non-negotiable for any quant job): SELECT/WHERE/GROUP BY/HAVING/JOINs/window functions. Practice on SQLite with your own price data via sqlite3 + pandas.read_sql. Window functions (ROW_NUMBER, moving averages in SQL) are a favorite interview topic.
Clean-code habits: project structure (src/, tests/, notebooks/), docstrings, README quality, black formatting, pre-commit hooks, GitHub Actions running your pytest suite on every push (a green CI badge on your repos quietly signals professionalism).
Checkpoint: Take your flagship Phase-5 project: profile it, Numba the hot loop, add CI, document the speedup in the README ("optimized Monte Carlo from 41s to 0.6s" is a resume bullet).

PHASE 8 — Integration, Interview Prep & Pre-Semester Sharpening (final 4–6 weeks)

Brainteasers & probability puzzles: A Practical Guide to Quantitative Finance Interviews (Zhou, "the green book") — one hour daily. Also Heard on the Street (Crack). These are exactly what quant internship interviews (which start EARLY in MFE programs — often within weeks of arrival) will ask.
Mental math: sharpen arithmetic speed (trading-firm tests like Optiver's are pure speed math). Use Zetamac daily, 10 minutes.
Coding drills: easy/medium LeetCode in Python — arrays, hashmaps, two pointers, binary search, simple DP. ~50 problems total is plenty for quant roles.
Re-derive on paper, closed-book: Black-Scholes intuition, duration, VaR definitions, CAPM, OLS estimator. If you can't whiteboard it, you don't own it.
Polish GitHub: 3–4 pinned repos, immaculate READMEs with results tables and plots, your KEYWORDS-style documentation showing you can communicate.
Read the markets 20 min/day (FT/Bloomberg/Matt Levine) — interviews always touch current markets.
Preview your MFE syllabus and front-run the heaviest course (usually stochastic calculus → start Shreve I seriously now).
The Weekly Operating System (applies to every phase)

60% building, 25% reading/watching, 15% math on paper. Tutorials feel productive; only building compounds.
Every concept → a small experiment in code the same day.
Every week → at least one commit to GitHub.
Keep a learning-log.md: what you learned, what confused you, questions to revisit. Reviewing it monthly is where the "deeper sense" you asked for actually forms.
Spaced re-derivation: once a month, pick one old topic and re-implement it from a blank file, closed-book. Painful and priceless.
Master Resource Shelf (in order of use)

Phase	Resource
1	Fluent Python (selective); Corey Schafer OOP videos
2–3	Python for Data Analysis (McKinney)
4	Intro to Statistical Learning (free); MIT OCW 18.S096; 3Blue1Brown LA + Calc
4–5	Hilpisch Python for Finance; Shreve Vol. I
5	Hull OFOD; Tuckman (fixed income); Ernie Chan; QuantStart.com
6	López de Prado AFML (selective); scikit-learn docs
8	Zhou green book; Heard on the Street; Zetamac; LeetCode
Milestone Summary (print this)

 Phase 0: Both existing projects on GitHub, clean history
 Phase 1: OOP refactor of pricing lab + 15 pytest tests
 Phase 2: Correlated (Cholesky) Monte Carlo basket pricer, zero loops
 Phase 3: 10-year, 30-ticker cleaned equity dataset pipeline (Parquet)
 Phase 4: Stylized-facts research note (fat tails, GARCH, clustering)
 Phase 5: Flagship — backtester OR fixed-income library OR risk engine
 Phase 6: Honest ML return-prediction study with purged walk-forward CV
 Phase 7: Profiled + Numba-accelerated flagship, CI badge, SQL fluency
 Phase 8: Green book done, 50 LeetCode, all core derivations closed-book
Finish this and you won't just survive the MFE — you'll be the person classmates form project teams around, and you'll walk into internship interviews with real artifacts while others have coursework.
