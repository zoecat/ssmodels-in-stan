
# Parameter Estimation

## Log log-likelihood

If the the system matrices and initial conditions are known, the log likelihood is
$$
\begin{aligned}[t]
\log L(\mat{Y}_n) &= \log p(\vec{y}_1, \dots, \vec{y}_n) = \sum_{t = 1}^n \log p(\vec{y}_t | \mat{Y}_{t - 1}) \\
&= - \frac{np}{2} \log 2 \pi - \frac{1}{2} \sum_{t = 1}^n \left( \log \left| \mat{F}_t \right| + \vec{v}\T \mat{F}_t^{-1} \vec{v}_t \right)
\end{aligned} .
$$
The log-likelihood only requires running the filter to calculate $\mat{F}_t$ and $\mat{V}_t$.

See [@DurbinKoopman2012, Sec. 7.2.1]

## Integrated Sampler

$$
p(\vec{y} | \mat{H}, \mat{\Psi}) = \int p(\vec{y} | \vec{\alpha}, \mat{H}) p(\vec{\alpha} | \mat{\Psi})\,d\vec{\alpha},
$$

The log-likelihood of a state-space function can be calculated analytically and expressed up to an integrating constant marginally of the state vector $\vec{\alpha}$,
$$
\log p(\vec{y} | \mat{H}, \mat{\Psi}) = \text{const} - 0.5 \left( \sum_{t = 1}^n \sum_{i = 1}^p \log f_{t, i} - v_t^2 f^{-1}_{t,i} \right) .
$$
Thus, parameters of the state space model can be sampled as,
$$
p(\mat{H}, \mat{\Psi} | \vec{y}) \propto p(\vec{y} | \mat{H}, \mat{\Psi}) p(\mat{H}) p(\mat{\Psi}) .
$$

## Diagnostic Checking

The *standardized prediction errors* are,
$$
\vec{v}^*_t = \mat{G}_t \vec{v}_t ,
$$
where $\mat{G}_t \mat{G}_t\T = \mat{F}_t^{-1}$.
See Koopmans JSS Sec 3.3.
These residuals should satisfy independence, homoskedasticity, and normality.

- independence: Box-Ljung test statistic
- normality: Bowman and Shenton test statistic
- homoskedasticity: compare variance of standardized prediction errors of the 1st third to that of the last third. See Harvey (1989), Durbin and Koopman (2012), and Commandeur and Koopman (2007).

The *auxiliary residuals* are the standardized smoothed observation and state disturbances,
$$
\begin{aligned}[t]
e^*_t &= \frac{\hat{\varepsilon}_t}{\sqrt{\Var(\hat{varepsilon}_t)}} , \\
r^*_t &= \frac{\hat{\eta}_t}{\sqrt{\Var{\hat{\eta}_t}}} ,
\end{aligned}
$$
for $t = 1, \dots, n$.
The standardized smoothed observation disturbances allows for the detection of *outliers*,
while the standardized smoothed state disturbances allows for the detection of *structural breaks*.
Each auxiliary residual is a $t$-test that there was no outlier (or structural break).
