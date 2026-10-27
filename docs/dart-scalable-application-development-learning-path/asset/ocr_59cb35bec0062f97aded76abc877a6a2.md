```
function process_js() {
    var container = $('#container');
    // Create 10000 DIV elements
    for (var i = 0; i < 10000; i++) {
        $('<div>Test</div>').appendTo(container);
    }
    // Find and update classes of all DIV elements
    $('#container > div').css("color","red");
    // Remove all DIV elements
    $('#container > div').remove();
}
```

The main code registers the click event listeners and the call function run_dart_js_test. The first parameter of the run_dart_js_test function must be a function which we will investigate. The second and third parameters are used to pass the selector of the result element and test the title:

```
void main() {
    querySelector('#run_jproxy').onClick.listen((event) {
        run_dart_js_test(process_jproxy, '#result_jproxy', 'JProxy');
    });
    querySelector('#run_dart').onClick.listen((event) {
        run_dart_js_test(process_dart, '#result_dart', 'Dart');
    })
}
```

```
run_dart_js_test(Function fun, String el, String title) {
    var startTime = new DateTime.now();
    fun();
    var diff = new DateTime.now().difference(startTime);
    querySelector(el).text = '$title tooks ${diff.inMilliseconds} ms to process 10000 HTML elements.';
}
```

Here is the Dart solution based on JProxy and dart:js:

```
process_jproxy() {
    var container = $('(#container');
    // Create 10000 DIV elements
    for (var i = 0; i < 10000; i++) {
        $('<div>Test</div>').appendTo(container.object);
    }
    // Find and update classes of all DIV elements
    $('(#container > div').css("color","red");
    // Remove all DIV elements
    $('(#container > div').remove();
}
```