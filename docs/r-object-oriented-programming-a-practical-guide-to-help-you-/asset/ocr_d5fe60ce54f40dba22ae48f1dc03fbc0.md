The following example compares the cumulative distribution functions for two $X^2$ distributions where the first has 30 degrees of freedom and the second has 35 degrees of freedom:

```
```r
> x <- seq(0.0,100.0,by=0.1)
> cdf <- pchisq(x,df=30)
> plot(x,cdf,main="Chi Squared Dist.",xlab='x',ylab='p',col=2,
+      type="l")
> cdfTwo <- pchisq(x,df=35)
> points(x,cdfTwo,col=3,type="l")
```
```