
The `toUTC` option gives you the time in **Coordinated Universal Time (UTC)** format, which is for all practical purposes identical to **Greenwich Mean Time (GMT)**. Use `timeZoneName` to get an abbreviated name of the time zone for the `DateTime` object. The difference between UTC and the time zone of a `DateTime` object is calculated by calling `timeZoneOffset`.

To find the last day of the month, giving a day value of zero for the next month returns the previous month's last day.

## There's more...

Be aware that a `DateTime` object is always in the local time zone, unless explicitly created in the UTC time zone with the `DateTime.utc` constructor; this can be done as follows:

```
DateTime moonLanding = new DateTime.utc(1969, DateTime.JULY, 20);
```

In this constructor, only the year is required, all the other date and time parts are optional.

If you have other `Datetime` questions, look up the API docs at `https://api.dartlang.org/apidocs/channels/stable/dartdoc-viewer/dart-core.DateTime`.

## Improving performance in numerical computations

Unlike Java and C#, who have dedicated 8, 16, 32, and 64 bit signed and unsigned integer types, and 32-bit and 64-bit floats, Dart does not have bounded integer types or 32-bit floating point number types; it only has two numeric types, `int` (an arbitrarily sized integer) and `double` (conforming to the IEEE-754 spec), and their supertype `num`. This was done to make the language more dynamic and easier to learn and use. However, the Dart VM does a good job of inferring the range of integers, and optimizes whenever possible. Here, we provide a number of discussions and tips to give your code the highest performance possible when it involves numerical computing.

## How to do it...

The VM uses three integer types internally and switches between them behind the scenes as numbers grow and shrink in size. They are as follows:

- `smi` (small integer): You can think of this integer type as being 32 bit on a 32-bit machine and 64 bit on a 64-bit machine
- `mint` (medium integer): This integer type is always 64 bit
- `bigint` (big integer): The machine's RAM is the limit for this integer type

## Working with Data Types

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

## How it works...

Advice mostly arises from the way the Dart VM can optimize working with the different numerical types. Because JavaScript can't use the exact same optimizations, dart2js sometimes works differently; refer to the next section for special advice in this case.

Small machine integers (smis) fit in a register and can be loaded and stored directly in a field instead of being fetched from memory because they never require memory allocation. That's why they are fast. However, their range depends on the CPU architecture, so assume a 31-bit range unless you require more bits. Operations with bigints cannot be optimized by the VM. Working with doubles is very efficient because they are unboxed, that is, they don't need to be put on the heap.

Dart-typed lists can only store numbers and not null values. They behave as an array of bounded integer and float values, each entry containing its value, and the VM optimizes their usage. Typed lists are most of the time much more compact, providing better memory and CPU cache usage; for example, if you need only 8 bits of precision, use Int8List. They are also faster to process when Garbage Collection (GC) occurs because they never store object references. So they don't have to be scanned by the GC.

## There's more...

From a language design point of view, using the int and num types is the best; the Dart VM experts would probably want us to work only with double because that is where most optimizations can be done.

## Working with JavaScript

In JavaScript, every number is represented as double (it only knows the IEEE-754 type).

![](img/52c05b53a56abd53584e4fed738ed509_619_0.png)

> If your app will be used in the compiled JavaScript form, it is better to use only the num type. Also, use is num instead of testing is int or is double because the Dart VM and dart2js behave differently here too. If you insist on using the int type, stay within the smi range.
> In any case, use Dart-typed lists as they map trivially to JavaScript-typed arrays. However, avoid Int64List or Uint64List because dart2js does not support 64-bit integers due to lack of support of typed data in Internet Explorer 9 (causing a runtime exception if used).

## Parsing numbers

Avoid extensive use of double.parse(String s) or even int.parse(String s) as at the time of writing their performance was not optimized.

## See also

- Consult http://dartogreniyorun.blogspot.be/2013/05/performance-optimization-and-dart.html, where the use of typed data results in a performance increase by two times
- You might also want to consult the *Benchmarking your app* recipe in Chapter 2, *Structuring, Testing, and Deploying an Application*

## Using SIMD for enhanced performance

A lot of modern CPUs and GPUs provide **Single Instruction Multiple Data (SIMD)** support. Four 32-bit data values (integers or floats) can be processed in parallel with the help of 128-bit special registers. This provides a potential speedup of 400 percent for image processing, 3D graphics, audio processing, and other numeric computation algorithms. Also, machine-learning algorithms (such as for automatic speech recognition) that use a **Gauss Mixture Model (GMM)** benefit from SIMD.

## How to do it...

Dart lets you work with this feature by using the special SIMD x types from the `typed_data` library. It offers the following four types:

- `Int32x4`, which represents four 32-bit integer values
- `Float32x4`, which represents four single-precision floating point values
- List structures to contain the 32-bit integer values, such as `Int32x4List`
- `Float32x4List`, list structure to contain the 32-bit floating point values

Let's see some examples of SIMD operations in simd.dart; the different types are highlighted in the following snippet:

```
import 'dart:typed_data';

void main() {
  var a = new Float32x4(14.1, 6.7, 56.3, 78.41);
  var b = new Float32x4(12.3, 5.4, 81.7, 13.43);
  Float32x4 sum = new Float32x4.zero(); //
  print(sum); // [0.000000, 0.000000, 0.000000, 0.000000]
  sum = a + b;
  print(sum); // [26.400002, 12.100000, 138.000000, 91.840004]
  print(sum.z); // 138.0
  // b.y = 3.14;  // --> NoSuchMethodError
  b = b.withY(3.14);
  print(b); // [12.300000, 3.140000, 81.699997, 13.430000]
  b = b.shuffle(Float32x4.WYXZ);
  print(b); // [13.430000, 3.140000, 12.300000, 81.699997]
  // a < b; // There is no such operator in Float32x4
  Int32x4 mask = a.greaterThan(b);  // Create selection mask.
  Float32x4 c = mask.select(a, b);   // Select.
  print(c); // [14.100000, 6.700000, 56.299999, 81.699997]
  // selectively applying an operation:
  Float32x4 v = new Float32x4(22.0, 33.0, 44.0, 55.0);
  // mask = [0xFFFFFFFF, 0xFFFFFFFF, 0xFFFFFFFF, 0x0]
  mask = new Int32x4.bool(true, true, false, true);
  // r = [4.0, 9.0, 16.0, 25.0].
  Float32x4 r = v - v;
  v = mask.select(r, v);
  print(v); // [0.000000, 0.000000, 44.000000, 0.000000]
}
```

If your application needs a list of Float32 objects and you can deploy them on an SIMD platform, then be sure to use Float32x4List instead of List<Float32x4> to get better performance.
