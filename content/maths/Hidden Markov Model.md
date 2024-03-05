#maths 

[Code](https://github.com/freth251/hmm)

Hidden Markov Models help us model Markov processes whose observations are the only known parameters. 

Let's say we have a friend, called Nahomie, that lives in a country far away from ours. Every Sunday we call Nahomie, and he tells his mood, is he is either sad (😫) or happy (😃). We know Nahomie's mood is caused by the weather in his area but we don't have any more information. The weather can be modeled by a [[Markov Chains|Markov chain]] which means that today's weather only depends on yesterday's weather. 

We can illustrate it as the following: 
![[hmm_full.png]]
A few notes: 
- The observable states are the states of the system we can observe, that is Nahomie's mood.
- The hidden states are the states in the system that we do not directly observe. The hidden states form a Markov chain. 
- The probability of being in a certain mood given the weather is denoted by emission probability.
- The probability of transitioning from one state to another is given by the transition probability. 


Let's say Nahom has been sad (😫) then happy (😃) then sad (😫). 

Now you may ask: 

**What is the likelihood of observing this sequence of moods?**

You can compute it using the [[Forward Algorithm|forward]] or [[Backward Algorithm|backward]] algorithm. 

**What is the likeliest weather that could have led to these moods?**

You can decode the likeliest sequence of hidden states that led to these moods using the [[Viterbi Algorithm|viterbi algorithm]]. 


**Can we learn the parameters (transition probabilities, emission probabilities, and stationnary distribution) of the Markov chain that lead to these moods?**

You can learn the parameters of the Markov Chain using the [[Baum-Welch Algorithm]]. 


An implementation of each algorithm mentioned above can be found [here](https://github.com/freth251/hmm). 