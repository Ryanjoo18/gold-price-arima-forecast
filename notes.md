# ARIMA Model

## Introduction

The Auto Regressive Integrated Moving Average (ARIMA) model is used for forecasting a time series using its past values.

A stochastic process is a collection of random variables indexed by time, $\{X_t\}_{t\in T}$.
Time series is the realisation of the stochastic process.
But we often use the two terms interchangeably.

- AR: Recall that in a regression model, we forecast the variable of interest using a linear combination of predictors.
    In an autoregression model, we forecast the variable of interest using a linear combination of the past values of the variable (hence the name; we regress the target variable on its own past values).
    
    An **autoregression model** of order $p$, denoted by $\text{AR}(p)$, uses the past $p$ values of time series to forecast the value of the time series:
    $$X_t=\phi_1 X_{t-1}+\cdots+\phi_p X_{t-p}+\epsilon_t=\sum_{i=1}^{p}\phi_i X_{t-i}+\epsilon_t$$
    where $\epsilon_t$ is white noise, $\phi_1,\dots,\phi_p$ are model parameters.
    Intuitively, the value of $p$ determines how far back in time we go when trying to predict the current observation.

    White noise represents a completely unpredictable, purely random shock or error term in a model at a given time $t$.
    We write $\epsilon_t \sim WN(0, \sigma^2)$.
    - Expected value of the noise is zero.
    - Variance is constant.
    - The value of the noise at any specific point in time has zero correlation with its value at any other point in time.

    For example, in $\text{AR}(0)$, there is no dependence between the terms, so $\text{AR}(0)$ is white noise.
    $\text{AR}(1)$ can be given by $X_t=\phi X_{t−1}+\epsilon_t$.
    - If $|\phi|$ is close to 0, then the process still looks like white noise.
    - If $\phi<0$, $X_t$ tends to oscillate between positive and negative values.
    - If $\phi=1$, then the process is equivalent to random walk.

- I: A **stationary** time series is one with constant mean and variance through time.
    In general, a stationary time series will have no predictable patterns in the long-term. 
    Time plots will show the series to be roughly horizontal, with constant variance.

    **Differencing** a time series means computing the differences between consecutive values (subtract the previous value from the current value), so that the time series becomes stationary.
    $$X_t^\prime=X_t-X_{t-1}$$
    Note that the differenced series will have only $T-1$ values.

    If the differenced time series is still not stationary, we continue differencing it until it becomes stationary.
    Let $d$ denote the minimum number of differencing needed to make the series stationary.

    To mathematically check for stationarity, we use the Augmented Dickey-Fuller test to see if our data is stationary (i.e., has constant mean and variance).

    Suppose a time series can be modelled by $\text{AR}(p)$:
    $$X_t=\phi_1 X_{t-1}+\cdots+\phi_p X_{t-p}+\epsilon_t.$$
    Let $B$ denote the lag operator, so $BX_t=X_{t-1}$.
    Then the terms in the summation above become
    $$(1-\phi_1B-\cdots-\phi_p B^p)X_t=\epsilon_t.$$
    Define the **characteristic polynomial** of $\text{AR}(p)$ as the term in the brackets:
    $$\Phi(B)=1-\phi_1B-\cdots-\phi_p B^p.$$
    We replace $B$ with $z$, and equate $\Phi(z)=0$.
    Performing a change of variables $z=\frac{1}{\lambda}$ gives
    $$\lambda^p-\phi_1\lambda^{p-1}-\cdots-\phi_p=0$$
    called the **characteristic equation**.
    We say that the time series has a **unit root** if $\lambda=1$ is a root of the characteristic equation.

    > Example: The first order autoregressive model $X_t=\phi X_{t-1}+\epsilon_t$ has a unit root when $\phi=1$, since the characteristic equation is $\lambda-\phi=0$.

    **Theorem:** If the process has a unit root, then it is a non-stationary time series.

    To illustrate the effect of a unit root, consider the first order case $X_t=X_{t-1}+\epsilon_t$. 
    By repeated substitution, we can write $X_t=X_0+\sum_{i=1}^{t}\epsilon_i$.
    Assuming $X_0=0$ for simplicity, the variance of $X_t$ is $\text{Var}(X_t)=\sum_{i=1}^{t}\sigma^2=t\sigma^2$, which diverges to infinity with $t$.

    There are various tests to check for the existence of a unit root, one of which is the **Dickey-Fuller test**.
    Consider $\text{AR}(1)$ given by $X_t=\phi X_{t-1}+\epsilon_t$.
    - Null hypothesis $H_0$: $\phi=1$ (the series is non-stationary)
    - Alternative hypothesis $H_1$: $\phi<1$ (the series is stationary)
    Subtract $X_{t-1}$ from both sides, and define $\Delta X_t=X_t-X_{t-1}$, so that
    $$\Delta X_t=(\phi-1)X_{t-1}+\epsilon_t.$$
    Let $\gamma=\phi-1$, so that
    $$\Delta X_t=\gamma X_{t-1}+\epsilon_t.$$
    - Null hypothesis $H_0$: $\gamma=0$
    - Alternative hypothesis $H_1$: $\gamma<0$

    The **augmented Dickey-Fuller test** includes lags of order $p$:
    $$\Delta X_t=\gamma X_{t-1}+\delta_1\Delta X_{t-1}+\cdots+\delta_p\Delta X_{t-1}$$
    where $\delta_i$ are some coefficients in terms of $\phi_i$.

- MA: Rather than using past values of the forecast variable in a regression, a **moving average model** of order $q$, denoted by $\text{MA}(q)$, uses the past $q$ forecast errors (i.e., random shocks) in a regression-like model:
    $$X_t=\epsilon_t+\theta_1\epsilon_{t-1}+\cdots+\theta_q\epsilon_{t-q}=\epsilon_t+\sum_{j=1}^{q}\theta_j\epsilon_{t-j}$$
    where $\epsilon_t$ is white noise.

    > Motivation for MA model: Recall that in AR models, current observation $X_t$ is regressed using the previous observations $X_{t-1},\dots,X_{t-p}$, plus error term $\epsilon_t$ at current time point. One problem of AR model is the ignorance of correlated noise structures (which is unobservable) in the time series. In other words, the imperfectly predictable terms in current time $\epsilon_t$ and previous steps $\epsilon_{t−1},\dots,\epsilon_{t-q}$ are also informative for predicting observations.

    > For example, your sales has a mean of 1000. There will be white noise or "shocks" to the sales every day, so your sales won't be exactly $\$1000$ but around there. Sometimes shocks are introduced by poor weather, good weather, promotion, etc which affect the sales. Some shocks have instantaneous effect, some not, like coupon issue. Suppose we introduce shock to the system by issuing coupon. More people claim on the first day, then less people claim on the next day on the leftover coupons, and then less and less on the next day. In this case we see the shock today attributable to coupon issue is a proportion of the shock yesterday ($\theta_1\epsilon_{t-1}$) and maybe, plus a proportion of the shock two days ago ($\theta_2\epsilon_{t-2}$). If say, that proportion of the shock from 3 days ago ($\theta_3\epsilon_{t-3}$) is insignificant, i.e., $\theta_3=0$, then we have a $\text{MA}(2)$ here.

    Note: The term "moving average" is somewhat misleading; it has nothing to do with the usual moving average calculations, which is simply an arithmetic average of a fixed number of terms in a sequence by moving the terms forward or backward.

If we combine differencing with autoregression and a moving average model, we obtain a (non-seasonal) **ARIMA model**, denoted by $\text{ARIMA}(p,d,q)$:
$$X_t^\prime=c+\phi_1 X_{t-1}^\prime+\cdots+\phi_p X_{t-p}^\prime+\theta_1\epsilon_{t-1}+\cdots+\theta_q\epsilon_{t-q}+\epsilon_t$$
where $X_t^\prime$ is the differenced time series.

In particular, the $\text{ARIMA}(0,1,0)$ represents a random walk, since $X_t^\prime=\epsilon_t$, so $X_t=X_{t-1}+\epsilon_t$, which means the next value is the current value plus randomness.

## Determining p, d and q

**Autocorrelation** measures the correlation between time series with a lagged version of itself, i.e., $X_t$ and $X_{t-k}$ for $k=0,1,2,\dots$.
An ACF plot shows the autocorrelations, where $x$-axis is the lag value, $y$-axis is the correlation coefficient.
For example, if lag = 0, then current value has perfect correlation with itself, so correlation coefficient = 1.

> Motivation for partial correlation:
> Suppose we want to find correlation between $X_t$ and $X_{t-2}$. But if $X_t$ and $X_{t-1}$ are correlated, $X_{t-1}$ and $X_{t-2}$ are correlated, then $X_t$ and $X_{t-2}$ might be correlated simply because they are connected to $X_{t-1}$, rather than because of any new information containing in $X_{t-2}$ that could be used in forecasting $X_t$.

**Partial autocorrelation** measures the correlation between $X_t$ and $X_{t-k}$ after removing the effects of lags $X_{t-1},\dots,X_{t-(k-1)}$.
Intuitively, it is the amount of correlation with each lag that is not accounted for by more recent lags.
A PACF plot shows the partial correlations between time series with a lagged version of itself.

Note: 1st partial autocorrelation equals 1st autocorrelation, because there are no lags between them to remove.

Correlations which are within the blue bands (confidence bands) are not statistically significant.
The number of lags where ACF cuts off is $q$, and where PACF cuts off is $p$ (exclude lag = 0).

My reasoning: Partial autocorrelation removes the effect of in-between lags, so we can use it to determine "actual" correlation between current value and past value, since the effect of shocks over time is removed.
Thus from here, we determine number of past values that have significant correlation, $p$.
On the other hand, autocorrelation includes the effect of shocks, so we use it to determine number of shock terms, $q$.

Pmdarima tests various combinations $p$ and $q$.
To choose the best model, it minimises information criteria such as AIC and BIC, which aim to balance fit and complexity of a model.
- Akaike information criterion (AIC) rewards a model that has good fit and few parameters (to avoid overfitting to the training data); the lower the better.
- Bayesian information criterion (BIC) is very similar to the AIC, but imposes a stronger penalty for model complexity i.e. using too many parameters.

## References

- [Forecasting: Principles & Practice](https://otexts.com/fpp2/arima.html)
- [Kaggle](https://www.kaggle.com/code/iamleonie/time-series-interpreting-acf-and-pacf/notebook)
- [Datacamp](https://www.datacamp.com/tutorial/arima)
- [CS 3750  Advanced Topics in Machine Learning, University of Pittsburgh](https://people.cs.pitt.edu/~milos/courses/cs3750/lectures/class16.pdf)