# Cosine similarity

Given two embedding vectors, A and B, cosine similarity is defined as follows:

$similarity(\mathbf{A},\mathbf{B}) = \frac{\mathbf{A}\cdot\mathbf{B}}{\|\mathbf{A}\|\|\mathbf{B}\|} = \frac{\sum_{i=1}^n A_i B_i}{\sqrt{\sum_{i=1}^n A_i^2}\sqrt{\sum_{i=1}^n B_i^2}}$

Here, the numerator (the top part of the fraction) is a dot product of the two vectors and the denominator (the bottom part of the fraction) is the product of magnitudes of each vector.

The resulting similarity score is a value between -1 and 1. A value of -1 indicates that the vectors are the exact opposite of each other. A value of 0 indicates that the vectors are orthogonal, meaning there is no correlation. A value of 1 indicates the vectors are similar.

For example, if we compared two embedding vectors representing two customers' music tastes where they rated different songs, we would have the following:

+   - *similarity*(**A**, **B**) = 1; means that the customers rated the same exact songs exactly the same.
+   - *similarity*(**A**, **B**) = −1; means that the customers rated the same songs in the opposite way.
+   - *similarity*(**A**, **B**) = 0; means that there is no correlation between the customers' music tastes, meaning they probably agreed on some songs, but disagreed on other.

You should now have a high-level understanding of the role and value of embeddings in NLP. It's now time to put the theory into practice and get some hands-on experience with working with embeddings with Flair.

# The "king – man ≈ queen – woman" analogy

Well-designed and trained embeddings with a high enough dimensionality not only represent or uniquely identify a certain word or a sentence but also contain abstract information about the meaning of the word and its semantic properties. Surprisingly enough, we can use arithmetic operations on word vectors to form new words. This means that, in theory, if we add two word vectors together, we should get a new word that is the sum of the meanings of both of those words. Similarly, if we subtract one embedding vector from another, we get a new embedding that contains the meaning of the first word minus the meaning of the second word. This brings us to the king – man ≈ queen – woman analogy.