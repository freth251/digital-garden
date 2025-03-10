#maths 
Permutation and Combination are two concepts from mathematics that deal with the ways objects can be arranged or selected. Their main difference lies in the fact that for permutations the order is important, while it is not the case for combinations. 

### Permutation

In permutations, the order of objects is important. For example, the permutations of the letters A, B, and C taken three at a time are ABC, ACB, BAC, BCA, CAB, CBA. The formula for finding the permutation of $n$ objects taken $r$ at a time is 
$$
P(n,r)= \frac{n!}{(n-r)!}
$$

### Combination

In combinations, the order of objects is not important. For example the combination of the letters A, B, and C taken two at a time are AB, AC, and BC. Whether we chose AB or BA it does not matter, it is considered the same. The formula for finding the permutation of $n$ objects taken $r$ at a time is 
$$
C(n,r)= \frac{n!}{r!(n-r)!}
$$

### Permutations with Repetition

In permutation with repetition, each object can be chose more than once, and the order of selection matters. The formula for $n$ objects taken $r$ at a time is
$$
P(n,r)= n^r
$$

### Combinations with Repetition 

In combination with repetition, each object can be chose more than once, and the order of selection does not matter. The formula for $n$ objects taken $r$ at a time is
$$C(n + r - 1, r) = \frac{(n + r - 1)!}{r!(n - 1)!}$$
