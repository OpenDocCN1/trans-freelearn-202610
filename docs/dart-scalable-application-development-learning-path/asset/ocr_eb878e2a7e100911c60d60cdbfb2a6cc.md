![](bbox=[0.323, 0.148, 0.678, 0.411])

# Proxy

The last annotation is proxy. Proxy is a well-known pattern used when we need to call a real class's methods through the instance of another class. Let's assume that we have the following Car class:

```
part of cars;

// Class Car
class Car {
    int _speed = 0;
    // The car speed
    int get speed => _speed;

    // Accelerate car
    accelerate(acc) {
        _speed += acc;
    }
}
```