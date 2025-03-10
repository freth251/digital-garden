#maths 
Given a [[maths/Hidden Markov Model|Hidden Markov Model]] with known parameters, we can deduct the most likely sequence of hidden states given a sequence of observation. The Viterbi algorithm gives us a sequence while the forward algorithm gives us a probability. The Viterbi algorithm is similar to the forward algorithm where instead of summing the likelihoods, we take the max. We can write it as the following: 


$$
\begin{align*}
\text{Initialization:} \quad & \alpha_1(X_i) = \pi[i] P(Y^0|X_i), & 1 \leq i \leq N \\
\text{Recursion:} \quad & \alpha_{t+1}(i) = \max_{1 \leq i \leq N}[ \alpha_t(X_j) P(X_i|X_j)] P(Y^t|X_i), & 1 \leq t \leq T-1, 1 \leq i \leq N \\
\text{Termination:} \quad & P(O) = \max_{1 \leq i \leq N} \alpha_T(i), &
\end{align*}
$$

Further reading: 
- [Viterbi Algorith](https://www.youtube.com/watch?v=6JVqutwtzmo)
