Now that we have our network, let’s apply our max-flow min-cut algorithm and find where we can replace our first traffic light by adding to Script 4.4:

```
#find cut-points
partition = nx.minimum_cut(G1, "a", "g")
print(partition)
```

Our partition suggests that our best option cuts the connection between intersections "c" and "d". Some commuters may be disadvantaged, but this preserves movement for the largest number of commuters overall.

# Summary

In this chapter, we explored transportation problems and routing problems, which come up often in real-world industries. We found the shortest paths between stores with the Dijkstra and A* algorithms on two example store networks. We then considered the traveling salesman problem for optimal route planning on our two store networks to see how connectivity impacts route length. Finally, we considered optimal cuts to maximize flow on a small city network. In the next chapter, we’ll tackle clustering on networks by examining ecological data.

# References

Chen, L., Kyng, R., Liu, Y. P., Peng, R., Gutenberg, M. P., & Sachdeva, S. (2022, October). Maximum flow and minimum-cost flow in almost linear time. In 2022 IEEE 63rd Annual Symposium on Foundations of Computer Science (FOCS) (pp. 612-623). IEEE.

Johnson, D. B. (1973). A note on Dijkstra’s shortest path algorithm. Journal of the ACM (JACM), 20(3), 385-388.

Kang, N. K., Son, H. J., & Lee, S. H. (2018). Modified A-star algorithm for modular plant land transportation. Journal of Mechanical Science and Technology, 32, 5563-5571.

Little, J. D., Murty, K. G., Sweeney, D. W., & Karel, C. (1963). An algorithm for the traveling salesman problem. Operations research, 11(6), 972-989.

Liu, S., Münch, F., & Peyerimhoff, N. (2018). Bakry-Émery curvature and diameter bounds on graphs. Calculus of Variations and Partial Differential Equations, 57, 1-9.

Rondinelli, D., & Berry, M. (2000). Multimodal transportation, logistics, and the environment: managing interactions in a global economy. European Management Journal, 18(4), 398-410.

Rosenthal, W., & Lehner, S. (2008). Rogue waves: Results of the MaxWave project.