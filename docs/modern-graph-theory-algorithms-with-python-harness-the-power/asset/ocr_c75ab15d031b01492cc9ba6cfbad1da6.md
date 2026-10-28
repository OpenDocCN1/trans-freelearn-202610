# Wrangling Data into Networks with NetworkX and igraph

In this chapter, we will introduce many types of data that are common in analytics projects, including **spatial data**, **temporal data**, and **spatiotemporal data**. Because these data structures do not exist as networks, they must be preprocessed into networks for further analytics. In future chapters, we will detail strategies for optimal preprocessing, but the goal of this chapter is to get familiar with the basics and how preprocessing works with the NetworkX and igraph packages.

We’ll consider many real-world problems in this chapter and the remaining chapters to build intuition around data that can be reformatted and analyzed as a network science problem. Oftentimes, network-based algorithms have lower computational costs than algorithms designed for time series analytics or spatial data. By the end of this chapter, you’ll be able to recognize many types of problems that work well with network analytics, and you’ll walk away equipped to dive into the problems in later chapters.

Specifically, we will cover the following topics in this chapter:

+   - Introduction to different data sources
+   - Wrangling data into networks with igraph
+   - Social network examples with NetworkX

# Technical requirements

To run the practical examples in this chapter, you need to be familiar with Python programming and must install the igraph and NetworkX Python packages.

The code for this chapter is available here: https://github.com/PacktPublishing/Modern-Graph-Theory-Algorithms-with-Python