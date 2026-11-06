This is analogous to packing everything into boxes for a move. If you don’t take the time to organize, group, and label the boxes, then finding a specific item later means looking through everything. This isn’t very efficient, but it does save you time when packing. Database indexes work the same way. You have to decide whether it’s more important to spend time organizing and labeling items when packing (writing) or whether you can afford to take extra time when searching for an item (reading). In most cases, the smart move is to spend a bit of time up front so that finding things is easier later.

# What is an index?

In the database world, an index is a data structure that allows you to find records quickly without scanning the full collection. MongoDB uses a type of data structure called a **B-tree** for most of its indexes.

A B-tree is a self-balancing tree data structure that maintains data in sorted order and allows various operations in logarithmic time, which is *O(log n)*. This means that as your data grows, the time to find a document increases much more slowly compared to a collection scan, which scales linearly with the size of your data, or *O(n)* time.

Let’s take a peek under the hood. If we create an index on our orderId field, the simplified B-tree might look something like Figure 3.1:

![](bbox=[0.22, 0.6, 0.82, 0.7])

*Figure 3.1: Simplified view of B-tree on the orderId field*

When searching for the order ID 295, MongoDB would follow these steps:

+   1. Start at the root node, [200].
+   2. Compare 295 to 200. Since 295 > 200, go right.
+   3. Arrive at node [300].
+   4. Compare 295 to 300. Since 295 < 300, go left.
+   5. Arrive at node [250].
+   6. Compare 295 to 250. Since 295 > 250, go right.
+   7. Continue right until 295 is found or determine that it doesn’t exist.