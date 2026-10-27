# Working with Data Types

You can't use these classes in the Dart code; the VM automatically promotes an integer from smi to mint, and to bigint when necessary. However, you can get better performance if you ensure that your integer values stay in smaller ranges. For example, refer to the integer_promotion.dart file:

```
import 'dart:math';

void main() {
  int points = 42; // starts as a smi
  print(points);  // 42
  points = pow(points, 10); // becomes a mint
  print(points); // 17080198121677824
  points = pow(points, 3); // becomes a bigint
  print(points);  // 49828603059827911309997192674768286219621357322
}
```

> Don't use the bigint range unless you really have to because this will result in a performance hit. If possible, stay in the smi range; if not, use the mint range.

Working with lists of numbers, numbers can be stored in an object List(), or in a generic type List<int> or List<double>. While the latter is better than the first, it is best to use Dart typed lists, which is available in the dart:typed_data package such as Uint8List, Int64List, or Float32List. So instead of using List<double>, which is always slower, use Float32List or Float64List. Be careful when using web apps; not every browser supports typed_data yet. For example, Internet Explorer 10 supports it, but version 9 does not.

> Use typed lists when computing; calculations will run more efficiently.

Taking into account these considerations, it is advisable to benchmark a few versions of your app with different number of type usages (for example, comparing Float32List with Float64List, or even with a normal list), in order to see which one performs best in the specific context of your app.