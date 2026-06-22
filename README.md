S&P 500 Volatility Forecasting with GARCH(1,1)

What's going on here?

GARCH(1,1) is a way to model how risky markets feel day to day. The core idea is simple: today's volatility depends on two things — how big yesterday's price shock was, and how volatile things already were. There's also a baseline level of risk that's always there, no matter what.

The model has three moving parts:


ω (omega) — a floor. Markets are never perfectly calm, and this captures that.
α · ε²(t) — how much yesterday's shock rattles today. Big move yesterday? Expect more uncertainty today.
β · σ²(t) — how much yesterday's volatility carries over. Markets tend to stay nervous for a while after a scare.


Add α and β together and you get a sense of how "sticky" volatility is. For the S&P 500, that number is usually around 0.97 — meaning once things get choppy, they stay choppy for a while. Calm periods also tend to last. It cuts both ways.


What this project does


Pulls live S&P 500 data straight from Yahoo Finance
Fits the GARCH model using a Student's-t distribution (better at handling the wild swings you get on crash days)
Saves a static chart showing daily returns with a ±2σ volatility band around them
Creates an animated GIF of a rolling 120-day window moving through the full history
Includes a self-contained HTML dashboard you can open in any browser — no install, no dependencies — with a slider to adjust volatility memory and a button to simulate a market shock



What's in the repo

FileWhat it isgarch_animation_colab.pyThe main script. Drop it into a Google Colab cell and run it.garch_dashboard.htmlThe interactive dashboard. Just open it in a browser.README.mdThis file.


Running it

The easy way — Google Colab


Go to colab.research.google.com and open a blank notebook
Paste the contents of garch_animation_colab.py into one cell
Hit Shift + Enter — it'll install what it needs, fit the model, and download the outputs


Locally

bashpip install yfinance arch matplotlib pandas numpy
python garch_animation_colab.py

One thing to note: the file-download lines at the bottom of the script are Colab-specific. If you're running locally, just delete those lines — the files still get saved to whatever folder you're in.


The interactive dashboard

Double-click garch_dashboard.html and it opens in your browser. Drag the β slider to control how long volatility sticks around, and hit the shock button to watch what a −5σ event looks like as it slowly fades.


How it actually works


Download daily closing prices for ^GSPC
Compute daily returns (percentage change × 100)
Fit the GARCH(1,1) model with a constant mean and Student's-t errors
Pull out the day-by-day volatility estimate
Plot returns with a shaded ±2σ band — on a well-fitted model, about 95% of days should land inside it



What the numbers usually look like

A typical fit gives you something like:


omega — small positive number (the baseline floor)
alpha — around 0.10 (how much each shock matters)
beta — around 0.87 (how long it lingers)
alpha + beta — around 0.97 (shocks fade, but slowly)
nu — somewhere between 6 and 8 (fat tails; big days happen more often than a normal distribution would predict)



Tech

Python (yfinance, arch, pandas, numpy, matplotlib) for the model and charts. Plain HTML, JavaScript, and Canvas for the dashboard — nothing to install.


This is a learning project, not financial advice. Don't trade off it.
