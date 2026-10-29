```
(conso 1 [2 3] q)
; ((1 2 3))
```

Note that this is one result, and that result is (1 2 3).

# resto: check for the rest of a vector

Here we look for a var, such that this var equals the rest of vector [1 2 3 4]:

```
(run* [q]
  (resto [1 2 3 4] q))
; ((2 3 4))
```

Note that this is one result, and that result is (2 3 4).

# membero

Keep going on. We want to find a value q, such that 7 is a member of [1 3 8 q]:

```
(run* [q]
  (membero 7 [1 3 8 q]))
; (7)
```

Of course, we can have multiple statements while looking for a result. Let's say we want to find q such that q is a member of [1 2 3] and a member of [2 3 4]. This is pretty much a vector intersection:

```
(run* [q]
  (membero q [1 2 3])
  (membero q [2 3 4]))
; (2 3)
```

That gives us two solutions: q can either be 2 or 3.

Statements can be mixed, of course. Say we want to find members of [1 2 3] that are not 2:

```
(run* [q]
  (!= q 2)
  (membero q [1 2 3]))
; (1 3)
```