#maths
The Baum-Welch algorithm helps us find the parameters of a [[maths/Hidden Markov Model|Hidden Markov Model]] given a sequence of observation. 

Given observations $O={o_1,o_2, ..., o_N}$ and parameters $\lambda=(\pi, A,B)$ we can find $\lambda'$ such that $P(O|\lambda')\leq P(O|\lambda)$ using the Baum-Walch algorithm. It only guarantees a *local minimum*. 

Let's call $\xi(i,j)$ the probability of being in state $S_i$ at time $t$ then state $S_j$ at time $t+1$ given  observation $O$ and parameters $\lambda$. We can write it as such: 

$\xi(i,j)=P(q_t= S_i, q_{t+1}=S_j|O,\lambda)$

We can illustrate is as: 
![[baum_welch_1.png]]

In the [[Forward Algorithm| forward algorithm]] we computed $\alpha$ which captures the probability that we are in state i given the observations that came before it. In the [[Backward Algorithm|backward algorithm]] we computed $\beta$ which captures the probability that we will be in the given state knowing what's going to come in the future. 

We have : 

![[baum_welch_2.png]]

The likelihood that we end up in state $S_i$ at time $t$ given all our observation up until this point is captured by $\alpha_t(i)$. The likelihood that we end up in state $S_j$ at time $t+1$ knowing all our observation after this point is captured by $\beta_{t+1}(j)$.

Now that we know the probability of being in state $i$ and $j$, we just  need to tie them together by multiplying it by the transition probability from state $i$ to $j$, and the probability of being in  state $j$ given our observation at time $t+1$ : 

![[baum_welch_3.png]]

We can rewrite $\xi$ as:

$$
\begin{align*}
\xi(i,j)=&P(q_t= S_i, q_{t+1}=S_j|O,\lambda)\\
&=\frac{\alpha_t(i)P(S_j|S_i)P(O_{t+1}|S_j)\beta_{t+1}(j)}{P(O|\lambda)}\\
&=\frac{\alpha_t(i)P(S_j|S_i)P(O_{t+1}|S_j)\beta_{t+1}(j)}{\sum_{i=1}^N\sum_{j=1}^N \alpha_t(i)P(S_j|S_i)P(O_{t+1}|S_j)\beta_{t+1}(j)}
\end{align*}
$$

*Note that we divide by $P(O|\lambda)$ to normalize our likelihood because we are looking for probabilities.*


We can write the probability of being in state $i$ at time $t$ as: 

$$
\gamma_t(i)= \sum_{j=1}^N \xi(i,j)
$$

Now we have: 

$$
\sum_{t=1}^T \gamma_t(i)= expected\ number\ of \ transitions\ from\ S_i
$$

and 

$$
\sum_{t=1}^T \xi_t(i,j)= expected\ number\ of \ transitions\ from\ S_i\ to\ S_j
$$


Now we can update our parameters. 

To update our stationary distribution: 

$$
\pi= \gamma_1(i) : \ expected \ frequency \ of \ S_i \ at \ time \ t=1
$$

To update our transition matrix:
$$
\begin{align*}
\overline{a}_{ij}= &\frac{expected\ number\ of \ transitions\ from\ S_i\ to\ S_j}{expected\ number\ of \ transitions\ from\ S_i} \\
&= \frac{\sum_{t=1}^{T-1} \xi_t(i,j)}{\sum_{t=1}^{T-1} \gamma_t(i)} \\
\end{align*}
$$

To update our emission probabilities:

$$
\begin{align*}
\overline{b}_{j}= &\frac{expected\ number\ of \ time\ in\ state\ j\ and \ observing \ v_k}{expected\ number\ of \ time\ in\ state\ j} \\
&= \frac{\sum_{t=1}^{T} \gamma_t(j) \quad s.t. \quad O_t=v_k}{\sum_{t=1}^{T} \gamma_t(j)} \\
\end{align*}
$$

*Note $s.t\quad  O_t=v_k$ means such that observation at time $t$ is $v_k$, we only sum terms that meets this condition. 



Further Readings: 
- [Baum-Welch Algorithm](https://en.wikipedia.org/wiki/Baum–Welch_algorithm)
- [Principles of Autonomy and Decision Making](https://ocw.mit.edu/courses/16-410-principles-of-autonomy-and-decision-making-fall-2010/2ebbc8cc4bc9adc3418a572a17331f63_MIT16_410F10_lec21.pdf)
- [A Tutorial on Hidden Markov Models and Selected Applications in Speech recognition](https://www.ece.ucsb.edu/Faculty/Rabiner/ece259/Reprints/tutorial%20on%20hmm%20and%20applications.pdf)
- [Hidden Markov Models 12: the Baum-Welch algorithm](https://www.youtube.com/watch?v=JRsdt05pMoI)
