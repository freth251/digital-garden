#maths 
Given a [[maths/Hidden Markov Model|Hidden Markov Model]] with known parameters (stationary distribution/ transition probability/ emission probability) we can calculate the likelihood of seeing a sequence observations, efficiently using the forward algorithm. 

![[hmm_full.png]]


Let's say we observe 😫,😃. What is the likelihood of observing these states, given our hidden markov model? 

The naive approach is to enumerate all possible hidden states and sum their probabilities. That is: 

$$ P(O) = \sum_{\forall q} P(O|q)P(q) $$
Where: 
- $q = q_1, q_2, \ldots, q_T$ represents a sequence of states through the HMM that generates the observations $O$. 
- $P(O|q)$ is the probability of observing the sequence $O$ given the state sequence $q$, which is the product of the observation probabilities for each observed symbol at each step in the sequence given the state at that step: $\prod_{t=1}^{T} P(o_t|q_t)$. 
- $P(q)$ is the probability of the state sequence $q$, which includes the initial state probability and the product of the transition probabilities for each transition in the sequence: $P(q_1) \prod_{t=1}^{T-1} P(q_{t+1}|q_t)$. 
- The sum is taken over all possible sequences of states $q$ of length $T$.

We can expand the equation to: 

$$ P(O) = \sum_{\forall q} [(\prod_{t=1}^{T} P(o_t|q_t))*(P(q_1) \prod_{t=1}^{T-1} P(q_{t+1}|q_t))] $$
For $O=\{😫,😃\}$ the [[Permutations vs Combinations|permutations]] of states $q$ that can give rise to $O$ are 

$$
q=\{
\{☀️, ☀️\}, \{☀️, ☁️\}, \{☀️, ⛈\}, 
\{☁️, ☀️\}, \{☁️, ☁️\}, \{☁️, ⛈\}, 
\{⛈, ☀️\}, \{⛈, ☁️\}, \{⛈, ⛈\}\}
$$
The number of possible hidden states can be calculated with: 
$$
P(n,r)= n^r
$$
where $n$ is the number of states and $r$ is the number of observations. This is an exponential function that grows with the number of observations we make. 


The probability of making observation $O$ is: 

$$ 
\begin{align*}
P(O=\{o_1=😫,o_2=😃\})&=  \sum_{\forall q} [(\prod_{t=1}^{T} P(o_t|q_t))*(P(q_1) \prod_{t=1}^{T-1} P(q_{t+1}|q_t))]\\
&=  \sum_{\forall q}  P(q_1)*P(o_1|q_1)* P(q_2|q_1)* P(o_2|q_2) \\
&=  \sum_{\forall q}  P(q_1)*P(😫|q_1)* P(q_2|q_1)* P(😃|q_2) \\
&= P(☀️)*P(😫|☀️)* P(☀️|☀️)* P(😃|☀️) \\
&+P(☀️)\ *P(😫|☀️) *\ P(☁️|☀️)* P(😃|☁️) \\ 
&+ P(☀️)\ *P(😫|☀️)* P(⛈|☀️)* P(😃|⛈) \\
&+ P(☁️)\ *P(😫|☁️)* P(☀️|☁️)* P(😃|☀️) \\
&+ P(☁️)\ *P(😫|☁️)* P(☁️|☁️)* P(😃|☁️) \\
&+ P(☁️)\ *P(😫|☁️)* \ P(⛈|☁️)* P(😃|⛈) \\
&+ P(⛈)*P(😫|⛈)* P(☀️|⛈)* P(😃|☀️) \\ 
&+ P(⛈)*P(😫|⛈)* P(☁️|⛈)* P(😃|☁️) \\
&+ P(⛈)*P(😫|⛈)* P(⛈|⛈)* P(😃|⛈) \\
\end{align*}
$$

Writing it out we can see that we are recomputing certain parts of the equation. For example $P(⛈)*P(😫|⛈)$ is computed 3 times. We can make the computation of $P(O)$ much more efficient by computing it recursively. This is where the **forward algorithm** comes in. 

$$
\begin{align*}
\text{Initialization:} \quad & \alpha_1(X_i) = \pi[i] P(Y^0|X_i), & 1 \leq i \leq N \\
\text{Recursion:} \quad & \alpha_{t+1}(i) = [\sum_{j=1}^{N} \alpha_t(X_j) P(X_i|X_j)] P(Y^t|X_i), & 1 \leq t \leq T-1, 1 \leq i \leq N \\
\text{Termination:} \quad & P(O) = \sum_{i=1}^{N} \alpha_T(i), &
\end{align*}
$$

Let's recompute our example using the forward algorithm: 
$$
\begin{align*}
\underline{Initialization:} \\
\\
t=1: \ & \alpha_1(☀️)= \pi[☀️]P(😫|☀️), \\ 
& \alpha_1(☁️)= \pi[☁️]P(😫|☁️),\\
& \alpha_1(⛈)= \pi[⛈]P(😫|⛈) \\
\\
\underline{Recursion:} \\
\\
t=2 : \ & \alpha_2(☀️)=[\alpha_1(☀️)P(☀️|☀️)+\alpha_1(☁️)P(☀️|☁️) +\alpha_1(⛈)P(☀️|⛈)]P(😃|☀️) \\
& \alpha_2(☁️)=[\alpha_1(☀️)P(☁️|☀️)+\alpha_1(☁️)P(☁️|☁️) +\alpha_1(⛈)P(☁️|⛈)]P(😃|☁️) \\
& \alpha_2(⛈)=[\alpha_1(☀️)P(⛈|☀️)+\alpha_1(☁️)P(⛈|☁️) +\alpha_1(⛈)P(⛈|⛈)]P(😃|⛈) \\
\ \\
\underline{Termination:} & \\
\\
P(O)= &\alpha_2(☀️) +  \alpha_2(☁️) + \alpha_2(⛈)

\end{align*}
$$

The complexity to compute the forward algorithm is $O(N^2T)$. This is because for each of the observations $T$, we compute the transition from each state to every other state leading to $N*N$. 

In comparison the complexity for the naive approach is $O(N^T)$, which is infeasible for even moderate sized $T$. 


Further Reading: 
- [Forward Algorithm Clearly Explained | Hidden Markov Model | Part - 6](https://www.youtube.com/watch?v=9-sPm4CfcD0)
