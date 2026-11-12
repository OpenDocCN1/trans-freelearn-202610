# 3. Similarity calculation:

+   I. Measure similarity between users or items.

# 4. User-based collaborative filtering:

+   I. Identify users with similar preferences.
+   II. Recommend items liked by users similar to the target user. For instance, if user A and user B have similar movie preferences, recommend movies liked by user B to user A.

# 5. Item-based collaborative filtering:

+   I. Focus on similarities between items.
+   II. Recommend items similar to those that the user has already engaged with. If user A enjoyed *The Matrix*, suggest other sci-fi movies

# 6. Embeddings and automatic learning:

+   I. Assign scalar values (embeddings) to movies and users.
+   II. These embeddings capture characteristics (for example, child versus adult content).
+   III. The product of movie and user embeddings predicts user preferences.
+   IV. Learn embeddings automatically without manual feature engineering.

# 7. Deployment and recommendations:

+   I. Integrate the collaborative filtering system into an application or platform.
+   II. Continuously update user profiles and monitor system performance.
+   III. Provide personalized recommendations to users based on similarity and embeddings.

Let's look at the pros and cons of collaborative filtering.

# The pros of collaborative filtering

+   * Improved recommendation accuracy:
+     * Collaborative filtering leverages user behavior patterns to make accurate recommendations. By analyzing past interactions, it identifies similar users or items, leading to better predictions.
+   * Robustness and adaptability:
+     * Collaborative filtering adapts well to changes in user preferences and item availability
+     * It doesn't rely on explicit domain knowledge or feature engineering, making it versatile across different domains