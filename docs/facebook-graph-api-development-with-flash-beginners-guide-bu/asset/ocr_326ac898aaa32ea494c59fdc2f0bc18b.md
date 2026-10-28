...with the reverse sentence being exactly the same. So far, so good. It gets trickier when we try to draw it, though. Do either of these work?

![](bbox=[0.35, 0.19, 0.64, 0.32])

![](bbox=[0.35, 0.34, 0.64, 0.47])

No. And this is a basic rule of the database model: you can't have "many-to-many" relationships between tables. Here we tried it where both tables were the same, but this rule applies even if that's not the case.

I guess we'd better give up then. Not really, of course. Instead, we use what's called a "link table":

![](bbox=[0.31, 0.6, 0.69, 0.73])

Labels for these relationships are tricky; since the link table is not a tangible thing, the proper label has to be something like, "is assigned to," which is not helpful. Really, the link table itself expresses the relationship, so you can often omit the labels altogether. Alternatively, label them as if they were connecting through the link table, like "is friends with".