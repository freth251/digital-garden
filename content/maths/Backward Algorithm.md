#maths 
The backward algorithm is a mirror of the [[Forward Algorithm]], where instead of computing the likelihood of being in a certain state given the observations that came before it, we compute the likelihood of being in a certain state knowing future observations.

$$
\begin{align*}
\text{Initialization:} \quad & \beta_T(X_i) = 1, & 1 \leq i \leq N \\
\text{Recursion:} \quad & \beta_{t-1}(i) = \sum_{j=1}^{N} \beta_t(X_j) P(X_j|X_i) P(Y^t|X_j), & 1 \leq t \leq T-1, 1 \leq i \leq N \\
\text{Termination:} \quad & P(O) = \sum_{j=1}^{N}\pi[j] P(Y^0|X_j)\beta_{1}(j), &
\end{align*}
$$

## Example

Let's say we got observer $O=\{😫,😃\}$. To compute the likelihood of this observation with the backward algorithm, we will have: 

$$
\begin{align*}
\underline{Initialization:} \\
\\
t=1: \ & \beta_2(☀️)= 1, \\ 
& \beta_2(☁️)= 1,\\
& \beta_2(⛈)= 1 \\
\\
\underline{Recursion:} \\
\\
t=2 : \ & \beta_1(☀️)=[\beta_2(☀️)P(☀️|☀️)+\beta_2(☁️)P(☀️|☁️) +\beta_1(⛈)P(☀️|⛈)]P(😃|☀️) \\
& \beta_1(☁️)=[\beta_2(☀️)P(☁️|☀️)+\beta_2(☁️)P(☁️|☁️) +\beta_2(⛈)P(☁️|⛈)]P(😃|☁️) \\
& \beta_1(⛈)=[\beta_2(☀️)P(⛈|☀️)+\beta_2(☁️)P(⛈|☁️) +\beta_2(⛈)P(⛈|⛈)]P(😃|⛈) \\
\ \\
\underline{Termination:} & \\
\\
P(O)= &\beta_1(☀️)\pi[☀️]P(😫|☀️)\\ +  &\beta_1(☁️)\pi[☁️]P(😫|☁️) \\ + &\beta_1(⛈)\pi[⛈]P(😫|⛈)

\end{align*}
$$
