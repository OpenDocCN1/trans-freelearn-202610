# Crow's feet

So we have two lines connecting the album table and the photo table.

For a given album, what's so different about the results of these two queries?

```
SELECT pid
FROM photo
WHERE aid = «album ID»
```

and

```
SELECT pid
FROM photo
WHERE pid = «album's cover_pid»
```

The answer is, the first may return several photos' IDs, while the second will only ever return one. We can clarify this in the wording of our relationship sentences like so:

"The album contains **zero or more** photos"

"The album's cover is **one and exactly one** photo"

Note I've had to reverse the sentences to start with "the album" rather than "the photo" for these to make sense.

We can also represent this in the diagram; we use what's called **crow's foot notation**:

![](bbox=[0.35, 0.52, 0.65, 0.66])

The three prongs ("**crow's foot**") on the right of the top line means "many" and the circle means "zero," so this reads as "zero to many" or "zero or more". The pair of dashes across the right of the bottom line means "one and exactly one".