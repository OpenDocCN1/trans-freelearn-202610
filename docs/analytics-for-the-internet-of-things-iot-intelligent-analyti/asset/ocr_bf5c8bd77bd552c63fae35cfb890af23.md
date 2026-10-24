The R-tree index is built from the MBRs so that larger bounding boxes contain groups of smaller bounding boxes with the goal of optimizing the search time for the lowest level MBRs. The resulting set of bounding boxes forms a tree hierarchy, which is stored as an index. The index tree is then used to quickly find the matching geometry by working down the hierarchy. The following image helps to visualize an example R-tree index:

![](bbox=[0.19, 0.28, 0.82, 0.62])

R-tree index, Source: Wikipedia