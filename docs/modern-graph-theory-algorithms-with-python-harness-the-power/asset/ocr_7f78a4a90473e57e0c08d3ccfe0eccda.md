**Bayes’ Theorem** provides a formula for calculating conditional probability by relating prior events’ probabilities to the event of interest. We can compute the probability of an event, *A*, given event *B* through the following formula:

P(A | B) = (P(A) * P(B | A)) / P(B)

Here, *P(A|B)* is the probability of event *A* occurring given event *B* occurring, *P(A)* is the probability of event *A* happening, *P(B|A)* is the probability of event *B* occurring given event *A* occurring, and *P(B)* is the probability of event *B* occurring.

To make this more concrete, let’s go back to our drug use example. We’ll call event *A* trying the new drug and event *B* being present where the drug is used. The probability of event *B* is 100%, as is the probability of event *B* given event *A*. The probability of event *A* is 30%. Let’s plug the values into our formula:

P(A | B) = (0.3 * 1) / 1

This gives us our expected 30%. However, within probability theory, not all events have a probability of 100%, and many calculations will be more complicated as conditional events are added. Within real-world data, we may need to estimate these probabilities with an algorithm given the data we’ve collected. Let’s move on to event chains such as our drug use example.

# Causal pathways

Conditional probability can involve more than two events of interest with conditional relationships. **Causal pathways** involve chains of conditional probability that can be relatively short, such as our drug dependence example, or very, very long and complex, such as protein activation pathways contributing to disease risk.

Thanks to Bayes’ Theorem, we can chain conditional probabilities together in a piecewise fashion until all conditional probabilities are linked into a final pathway. This works quite well for estimating effect sizes and progression rates for well-known causal pathways. However, many times, we don’t know the exact sequence of events leading to an outcome of interest and simply collect a lot of data we think is related to the outcome. To analyze this data, we’ll need an algorithm. Fortunately, one exists. Let’s dive into Bayesian networks and their application to causal pathway mining.

# Bayesian networks

A **Bayesian network** depicts a set of variables and their conditional probabilities as a **directed acyclic graph (DAG)**. Vertices that are not connected by an edge are conditionally independent (not dependent on each other). Vertices connected by an edge are conditionally dependent.