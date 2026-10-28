```
}
)

setMethod(f="SetLength",
          signature="Ant",
          definition=function(antie,newLength)
          {
              if(newLength>0.0) {
                  antie@Length = newLength
              } else {
                  warning("Error - invalid length passed");
              }

              return(antie)
          }
)
```

When setting the length, the new object must be set using the object that is passed back from the function:

```
> ant2 <- new("Ant",Length=4.5)
> ant2@Length
[1] 4.5
> ant2 <- SetLength(ant2,6.25)
> ant2@Length
[1] 6.25
```