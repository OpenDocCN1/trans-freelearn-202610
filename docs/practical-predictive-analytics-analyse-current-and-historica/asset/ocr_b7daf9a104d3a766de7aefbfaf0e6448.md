| Word | Count |
|---|---|
| RETROSPOT | 7155 |
| PACK | 6861 |
| VINTAGE | 6204 |
| HEART | 4799 |
| HANGING | 4457 |
| DOORMAT | 4175 |
| REGENCY | 3452 |

Similarly, print the popular last words of the description:

```
kable(head(as.data.frame(sort(table(OnlineRetail$lastword[]), dec
10)))
sort(table(OnlineRetail$lastword[]), decreasing = TRUE)
```

18376

| Word | Count |
|---|---|
| DESIGN | 12713 |
| HOLDER | 6792 |