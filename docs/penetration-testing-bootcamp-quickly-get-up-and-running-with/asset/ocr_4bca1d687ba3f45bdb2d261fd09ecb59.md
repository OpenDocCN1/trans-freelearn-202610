Rainbow tables are typically very good for harder passwords, while hash tables tend to work better for simple, more common passwords. Wordlists are just that: a list of passwords that will be tried against the system in real time. Because of this, wordlists are typically used in online attacks.

Dictionary attacks are the type of attack we are trying to accomplish with these tools. A dictionary attack uses some sort of precomputed dictionary file to try and guess the password. The attack is typically limited to the size of the dictionary file it will use. There are also brute-force attacks, which are similar to dictionary attacks but try every single combination possible, and can therefore take a long time to finish.

Next, I will talk about some utilities that I use for generating rainbow tables and wordlists, as well as some online resources for grabbing other tables and wordlists that others have already created and have found useful. Tables can take a long time to generate based on the table size, so if someone has already generated some good lists, be sure to keep those lists handy to save you some time.

# Creating rainbows with RainbowCrack

RainbowCrack is a software package that contains many tools, including one that is used to generate rainbow tables called rtgen. There are also utilities that can convert tables back and forth between different rainbow table formats, as well as rcrack, which is used with the rainbow tables to crack passwords. Here is the syntax for rtgen:

```
rtgen hash_algorithm charset plaintext_len_min plaintext_len_max
table_index chain_len chain_num part_index
```