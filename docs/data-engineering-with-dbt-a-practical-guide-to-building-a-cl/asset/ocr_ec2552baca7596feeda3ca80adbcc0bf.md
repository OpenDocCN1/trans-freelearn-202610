This would be the case if a country has multiple codes to represent a “generic box 30x30x5 cm” product because they are sourcing them from different vendors or producing them in different plants, but the MDD has only one such product.

This also shows that a mapping table might use multiple columns to map one code across domains. Using the box as an example, we would need the MD code and the production plant code to map an MD code to the local domain.

It might be surprising to you, but the mapping tables are the ones that are needed, whereas having the master data dimension for all or even most concepts is not common. It is very welcome, but not a must.

Even when we just have the mapping table, we implicitly have also a minimal MDD that’s made by all the MD codes that are present in the mapping table. In such cases, the dimension used in data marts is derived from the local dimensions, along with some business rules to decide what data to collect from each local dimension and the eventual precedence when data can be alternatively sourced by multiple dimensions.

# A light MDM approach with DBT

When we discussed modularity earlier in this chapter, we introduced the two possible extreme situations that we will find when making data interoperable across domains.

One possibility is to progressively convert the data from one domain to the next, while the other extreme is to convert everything directly to the higher-level domain, which is often considered the target master data system of reference.

Reality is hardly black and white, and you will find yourself in situations where, for some data, you have to go through a few conversion steps, while for others, you just have to take a single step. This mixed approach is often the case as some entities do not have the same semantics. Besides this, with master data code conversions, some rules are needed to have the same definition and build a more widely applicable version of the concept.

Let’s make a simple graphical example to better clarify one step of the MD conversion process. In our example, we have two systems that have Order LINES data to be converted into some master data domain to make it interoperable:

![](bbox=[0.26, 0.73, 0.75, 0.87])

Figure 13.3: Desired result of the master data conversion – interoperable Order LINES from SYS1 and SYS2