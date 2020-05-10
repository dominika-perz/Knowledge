# Probabilistic programming and Bayesian Interferance for Hackers

## Chapter 1
Probability P(A) it is our believe that A will happen. This is prior prabability
Probability P(A|X) is a believe that A will happen now that we know that X happend. This is posterior probability
\begin{align}
 P( A | X ) = & \frac{ P(X | A) P(A) } {P(X) } \\\\[5pt]
& \propto P(X | A) P(A)\;\; (\propto \text{is proportional to })
\end{align}

After we see N exampples we can calculate the observed probability, but as N -> inf we get closer to the true probability.

**Expenceted value** is the true mean of the random variable (so the mean value we will get with N -> inf examples of this variable).
It is not the value that is most likely to be seen. 
E.g. the expected value of flipping th ecoin is 0.5, althought we will never see this result. 

### Discrete
Categorical, binary or integer variable, e.g. population, yes/no, 

Poiss
