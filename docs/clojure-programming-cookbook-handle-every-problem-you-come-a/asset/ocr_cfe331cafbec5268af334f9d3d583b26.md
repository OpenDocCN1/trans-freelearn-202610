case, the first parameter function of map receives multiple inputs. map will then be called step by step until one of the 2...n parameters runs out of elements:

```
(map # (hash-map %1 %2)
    (take 5 (range 0 10))
    (range 1 10))

; ({0 1} {1 2} {2 3} {3 4} {4 5})
```

# OK!

The last part is quite overwhelming but very interesting. We want to impose a constraint on the value of each lvar. This is done through project, to work on the value, and everyg, to apply a constraint on a vector of variables:

```
(project [vs]
    (everyg
        (fn [[v n]] (fd/!= v n))
        (map
            vector
            vs
            (range 1 10))))
```

With all the elements in mind, we realize this means that we want to make sure that each logic variable does not have a value equal to its position. So c1 cannot take value 1, c2 cannot take value 2, and so on.

Running the preceding code is somewhat easy to generate, but complicated to replicate with other techniques. Let's enjoy:

```
;   ([4 3 1 8 9 2 6 7 5])
```

Phew!! What a ride again! But at last, that hardcore piece of code now finally looks readable to us.

# There's more...