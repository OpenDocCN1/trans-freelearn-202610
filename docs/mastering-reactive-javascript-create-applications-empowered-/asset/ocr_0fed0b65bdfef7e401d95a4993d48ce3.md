```
var parser = {
    getPageObservable: (url)=>Rx.Observable.fromPromise(request(url)),
    parseContentObservable: (observable)=>observable
        .flatMap((str)=>Rx.Observable
        .fromArray(
            str.replace( /\D+/g, ' ').split(' ')
        )
    ).filter((i)=>i!=''),
    getAndParseUrl: function(url){
    var observable = this.getPageObservable(url);
    return this.parseContentObservable(observable);
    }
};

parser
    .getAndParseUrl('http://www.google.com')
    .subscribe((m)=>console.log(m));
module.exports = parser;
```

In this example, we create a var called parser that contains three methods:

+   - The first one is called getPageObservable(), which fetches a given page.
+   - The second is parseContentObservable(), which replaces all non-number items in the page with a blank space (using a regular expression), and then uses the split method to create an array containing the numbers. We create an Observable from that array using the fromArray() method, and we flatMap() the original Observable to create a new Observable, which propagates each value individually; finally we use filter() to remove all blank strings from the parsed content.
+   - The third method is called getAndParseUrl() and only applies the first two methods. For the last I've added a call to the getAndParseUrl() to print it to the console so we can see the result (uncomment this if you want to see it).

If you run this code you will see all numbers in the http://www.google.com web page printed in the console.

The first thing to notice is how I've created different methods for each step of the program, and now I can test each one individually. If I want to test the first method, I really have to fetch a page to compare the results, but for the other two methods I can create fake observables to compare the results of my tests. A possible test for the parseContentObservable() method can be seen in the following code:

```
var assert = require('assert');
var Rx = require('rx');
```