```
// Filter - Angular 1 <br/><td>{{movie.price | currency}}</td> <br/><br/>// Pipes in Angular 4 <br/><td>{{movie.price | currency:'USD':true}} <br/></td>
```

Due to performance reasons, filter and orderBy filters have been removed from Pipes, although you can at anytime build a custom pipe if similar code is reused in multiple templates.