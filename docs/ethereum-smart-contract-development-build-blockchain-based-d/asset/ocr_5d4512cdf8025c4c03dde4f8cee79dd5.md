```
pattern #1 dead code
function q1 (uint y) {
if ( y > 5)
  if ( y*y < 20)
    print("you can't reach me!");
}
```

```
pattern #2 opaque predicate
function q2 (uint y) {
if ( y > 5)
  if ( y > 2)
    print("think out-of-the-case!");
}
```

```
pattern #3
expensive operations of a loop
uint sumout = 0;
function q3 (uint y){
for ( uint j = 0; j < y; j++)
  sumout += j;
}
```

```
pattern #4
constant outcome of a loop
function q4 () returns (uint){
uint sumout = 0;
for ( uint j = 1; j <= 100 ; j++)
  sumout += j;
return sumout;
}
```

```
pattern #7
comparison with unilateral outcome in a loop
function q7 (uint z , uint x)
returns (uint){
for ( int i = 0; i < 100 ; i++)
  if (z > 0)
    x+=z;
return x;
}
```

```
pattern #6
repeated computations in loop
uint y = 1;
uint z = 2;
function q8 (uint j){
uint sumout = 0;
for ( uint k = 1; k <= j ; k++)
  sumout = sumout + y + z;
}
```

```
pattern #5
loop fusion
function q5 (uint y){
uint n = 0;
uint w = 0;
for ( uint j = 0; j < y; j++)
  n += j;
for ( uint k = 0; k < y; k++)
  w -= k;
}
```

# Figure 5.6: Pseudo-code of gas-costly pattern

In pattern 4, the outcome of the loop is a constant value and deterministic in nature. Rather than using a loop, we can just use a statement, such as return 5050, and assign it to sumout, minimizing gas usage. Pattern 5 illustrates the need for a loop fusion where we combine several loops into one to minimize gas usage. We can combine the summation and subtraction step of n and w inside a single loop rather than running them in sequential separate loops. Pattern 6 depicts repeated computation in a loop. The summation of y+z is very expensive due to the use of a SLOAD operation before ADD. This can be avoided by summing up y+z outside the loop and then reusing the result inside the loop.

And, lastly, pattern 7 shows unilateral redundant comparison inside a loop. In simple words, z>0 can safely go outside the loop without loss of functionality and in turn minimize gas cost as we are preventing the same comparison from occurring 100 times, which gives the same result anyway. It is noteworthy, that as we increase the loop counters, the gas