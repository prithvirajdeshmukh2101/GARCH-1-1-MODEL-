S&P 500 Volatility Forecasting with GARCH(1,1)

A hands-on quantitative finance project that fits a GARCH(1,1) model to ~18 years of real S&P 500 daily returns (2007–2026) and visualizes the resulting conditional volatility band. Built in Python with an accompanying interactive browser dashboard.

Show Image Show Image Show Image


Overview

Financial markets do not have constant risk. Calm periods are followed by calm periods, and turbulent periods cluster together — a phenomenon called volatility clustering. A single GARCH(1,1) model captures this behavior remarkably well, which is why it remains one of the most widely used tools in risk management, option pricing, and Value-at-Risk estimation.

This project downloads real S&P 500 index data, fits a GARCH(1,1) model, and produces a chart of daily returns wrapped in a ±2σ conditional volatility band that widens during crises (2008, 2020) and contracts during calm years.

What is GARCH(1,1)?

GARCH stands for Generalized Autoregressive Conditional Heteroskedasticity. The (1,1) version estimates tomorrow's variance from three components:

σ²(t+1) = ω + α · ε²(t) + β · σ²(t)

TermNameMeaningω (omega)BaselineA constant long-run risk floor that is always present.α · ε²(t)News / reactionHow strongly yesterday's market shock feeds into today's risk.β · σ²(t)Memory / persistenceHow strongly yesterday's volatility carries forward.

The sum α + β measures persistence — how long a volatility shock lingers. For equity indices it is typically close to (but below) 1.0, meaning both calm and stormy regimes tend to last. On this dataset the fitted value is roughly 0.97, confirming the S&P 500's well-known "sticky" volatility.

Features


Pulls real S&P 500 data directly from Yahoo Finance via yfinance.
Fits GARCH(1,1) using the industry-standard arch library, with a Student's-t distribution to better capture fat-tailed crash days.
Exports a high-resolution static chart of returns with the ±2σ volatility band.
Renders an animated GIF of a rolling 120-day window sweeping across the full history.
Includes a standalone, dependency-free interactive HTML dashboard that simulates the GARCH process live, with a β "memory" slider and a −5σ shock injector.


Repository contents

FileDescriptiongarch_animation_colab.pyMain script — paste into a single Google Colab cell to fit the model and export the chart + animation.garch_dashboard.htmlStandalone interactive dashboard. Open in any browser, no installation required.README.mdThis file.

Getting started

Option A — Google Colab (recommended, zero setup)


Open a new notebook at colab.research.google.com.
Copy the entire contents of garch_animation_colab.py into one cell.
Run the cell (Shift + Enter). It installs dependencies, fits the model, and downloads garch_animation.gif and sp500_garch_static.png.


Option B — Run locally

bashpip install yfinance arch matplotlib pandas numpy
python garch_animation_colab.py


Note: the google.colab download lines at the bottom of the script only run inside Colab. Remove them when running locally — the output files are still saved to the working directory.



Interactive dashboard

Just double-click garch_dashboard.html, or open it in any modern browser. Drag the β memory slider to change how persistent volatility is, and press inject −5σ shock to watch the risk band balloon and slowly decay.

How it works (pipeline)


Download S&P 500 (^GSPC) daily prices.
Compute returns as the percentage change of the closing price, scaled by 100.
Fit arch_model(returns, vol="Garch", p=1, q=1, mean="Constant", dist="t").
Extract the day-by-day conditional_volatility series.
Plot returns with a shaded ±2σ band (≈95% of daily moves should fall inside it under the model).


Sample interpretation

A typical fit on this dataset yields:

omega    ~ small positive   (baseline variance floor)
alpha[1] ~ 0.10             (reaction to shocks)
beta[1]  ~ 0.87             (persistence of volatility)
alpha+beta ~ 0.97          (highly persistent — shocks fade slowly)
nu       ~ 6-8             (fat tails: extreme days more common than a normal curve predicts)

Tech stack


Python — yfinance, arch, pandas, numpy, matplotlib
HTML / JavaScript / Canvas — for the interactive dashboard (no external dependencies)


Disclaimer

This project is for educational purposes only. It is not financial advice and should not be used for live trading or investment decisions. Past volatility is not a guarantee of future risk.

License

Released under the MIT License. Feel free to fork, adapt, and build on it.
