# # Obtain Components using the fastest method

There are several variations of the `GetComponent()` method, and they each have a different performance cost, so it becomes prudent to call the fastest possible version of this method. The three overloads available are `GetComponent(string)`, `GetComponent<T>()`, and `GetComponent(typeof(T))`. It turns out that the fastest version depends on which version of Unity we are running since several optimizations have been made to these methods through the years; however, in all later versions of Unity 5 and the initial release of Unity 2017, it is best to use the `GetComponent<T>()` variant.

Let's prove this with some simple testing:

```
```
int numTests = 1000000;
TestComponent test;
using (new CustomTimer("GetComponent(string)", numTests)) {
 for (var i = 0; i < numTests; ++i) {
 test = (TestComponent)GetComponent("TestComponent");
 }
}
using (new CustomTimer("GetComponent<ComponentName>", numTests)) {
 for (var i = 0; i < numTests; ++i) {
 test = GetComponent<TestComponent>();
 }
}
using (new CustomTimer("GetComponent(typeof(ComponentName))", numTests)) {
 for (var i = 0; i < numTests; ++i) {
 test = (TestComponent)GetComponent(typeof(TestComponent));
 }
}
```
```

The preceding code tests each of the `GetComponent()` overloads a million times. This is far more tests than would be sensible for a typical project, but it helps make the relative costs clear.

Here is the result we get when the tests complete:

```
```
! GetComponent(string) finished: 6413.00ms total, 0.006413ms per test for 1000000 tests
UnityEngine.Debug:Log(Object)
! GetComponent<ComponentName> finished: 89.00ms total, 0.000089ms per test for 1000000 tests
UnityEngine.Debug:Log(Object)
! GetComponent(typeof(ComponentName)) finished: 95.00ms total, 0.000095ms per test for 1000000 tests
UnityEngine.Debug:Log(Object)
```
```