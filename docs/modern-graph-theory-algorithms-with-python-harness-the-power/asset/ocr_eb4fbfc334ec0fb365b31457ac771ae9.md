reduces training time, and reduces the computational resources needed to fit a good model. Now that we understand a bit about neural network architecture, let's turn to a type of network that integrates information about hierarchy and layering within its structure.

# Hierarchical networks

In Chapter 11, we encountered ontologies and phylogenies within the linguistic and genetic context. A hierarchical network builds on the intuition within ontologies and phylogenies; hierarchical networks are tree-like networks that can include multiple sources and sinks flowing from the top of the tree to the bottom of the tree. Gene regulatory networks often follow this hierarchical network structure rather than strictly a tree structure, where multiple sources and sinks do not exist in the network, or an ontology structure, where parent vertices don't typically exist. Figure 14.6 shows an example of a hierarchical network:

![](bbox=[0.22, 0.39, 0.75, 0.6])

Figure 14.6 – A hierarchical network example where the second layer of the network contains the parents of a vertex in the third layer

Studying gene regulation provides insight into myriad areas of research, including the evolutionary pathways within organ systems or species, the pathologies associated with gene dysregulation, and the discernment of cellular functions. Gene regulation isn't as simple as understanding the gene pathways themselves. Environmental conditions can change the structure of proteins associated with DNA or RNA to make DNA/RNA less or more accessible to the proteins involved in gene transcription. This, in turn, usually modifies the structures of those proteins further, either upregulating or downregulating the rate of transcription through accessibility to DNA/RNA.

Recent advances in the fields of genomics and epigenomics (which combines genetic analysis with environmental conditions) include measurement tools for gene expression and the structure of proteins around genes (which reflect local environmental conditions). By measuring these levels of expression under different conditions and different genetic mutation lines, it's possible to discern regulatory pathways within organisms.