# Practical Deep Reinforcement Learning for Stock Trading

Python implementation of a **Deep Deterministic Policy Gradient (DDPG)** framework for dynamic portfolio allocation across the 30 constituents of the **Dow Jones Industrial Average (DJIA)**, with performance comparison against a **long-only mean-variance portfolio** and the **DJIA index benchmark**.

## Project Overview

This project investigates the use of **deep reinforcement learning** for portfolio optimization in a high-dimensional stock trading setting. The trading process is modeled as a **Markov Decision Process (MDP)**, where the agent observes current stock prices, holdings, and cash balance, and outputs continuous trading actions for each asset. The objective is to learn a policy that maximizes cumulative portfolio value over time. 

Traditional dynamic programming methods often become computationally infeasible in large financial markets. To address this, the project applies **DDPG**, an actor-critic reinforcement learning algorithm designed for continuous action spaces. The learned trading strategy is compared with a **mean-variance benchmark solved using Portfolio Safeguard (PSG)** and with a passive **DJIA benchmark**. 

## Methodology

The project uses a **DDPG actor-critic architecture**:

- **Actor network**: maps the current state to a continuous action vector  
- **Critic network**: evaluates the quality of the selected action  
- **Replay buffer**: stores past transitions for randomized minibatch training  
- **Target networks**: stabilize learning through soft parameter updates
  
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/a34ad8fe-f3ca-4b9b-aaa7-ac7d9b20cde1" />

The state contains:
- current stock prices,
- current holdings,
- remaining cash balance,

So the total state dimension is 2d+1. For the DJIA universe with d=30, the state dimension is **61** and the action dimension is **30**. Actions are continuous values, interpreted as buy, sell, or hold signals, subject to feasibility constraints on holdings and available cash.

The benchmark portfolio is a **long-only minimum-variance allocation** based on the sample covariance matrix of training-period returns, under the full-investment and nonnegativity constraints. 

## Data and Experimental Setup

The dataset consists of the **30 DJIA constituent stocks** over the period from **January 2, 2009 to September 28, 2018**, downloaded using `yfinance`. The data are split into:

- **Training period:** January 2009 to December 2015  
- **Testing period:** January 2016 to September 2018

The DDPG hyperparameters in the report are:

- Minibatch size: **64**
- Exploration noise standard deviation: **0.05**
- Actor learning rate: **1e-4**
- Critic learning rate: **1e-3**
- Replay buffer size: **100,000**
- Number of training episodes: **100**
- Initial portfolio value: **$10,000**
- Risk-free rate: **1.5% annualized** 

## Performance Metrics

Strategy performance is evaluated using:

- Final portfolio value
- Annualized return
- Annualized volatility
- Sharpe ratio
- Maximum drawdown
- Average drawdown
- Sortino ratio 

## Main Findings

The empirical results in the report show the following overall patterns:

- In the **in-sample period**, the **DDPG strategy outperforms** both the mean-variance portfolio and the DJIA benchmark in final portfolio value, annualized return, and Sharpe ratio. It also achieves the lowest average drawdown among the three strategies.
- In the **out-of-sample period**, DDPG still achieves the **highest final portfolio value**, **highest annualized return**, **highest Sharpe ratio**, and **lowest maximum and average drawdowns** among the compared strategies, though the advantage over the benchmarks becomes much smaller than in-sample.
- The reduced gap between in-sample and out-of-sample performance suggests possible **overfitting** to historical market data.
- The DDPG results vary across runs because of **random network initialization**, **random minibatch sampling**, and **exploration noise**, which makes the method sensitive to hyperparameters and random seeds. 

## Results

- in-sample portfolio value comparison
- in-sample drawdown comparison 
- out-of-sample portfolio value comparison
- out-of-sample drawdown comparison
<img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/adba6b74-1bc7-422b-8679-61baf470c20e" />

<img width="1000" height="400" alt="image" src="https://github.com/user-attachments/assets/f7b50e95-f1d0-43d4-84b9-77b72368d6a1" />



## References

1. Alexei Chekhlov, Stanislav P. Uryasev, and Michael Zabarankin. *Portfolio optimization with drawdown constraints*. SSRN Electronic Journal, 2000.  
2. Xiao-Yang Liu, Zhuoran Xiong, Shan Zhong, Hongyang Yang, and Anwar Walid. *Practical deep reinforcement learning approach for stock trading*. arXiv preprint arXiv:1811.07522, 2022.  
3. Harry Markowitz. *Portfolio selection*. The Journal of Finance, 1952.  
4. William F. Sharpe. *Mutual fund performance*. The Journal of Business, 1966.  
5. Frank A. Sortino and Robert van der Meer. *Downside risk*. The Journal of Portfolio Management, 1994.
