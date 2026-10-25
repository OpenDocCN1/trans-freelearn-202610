Let's cover a quick implementation of an Object Pooling System.

An important feature of this system is to allow the pooled object to decide how to recycle
itself when the time comes. The following Interface Class called `IPoolableObject` will
satisfy this requirement nicely:

```
```
public interface IPoolableObject{
    void New();
    void Respawn();
}
```
```

This Interface Class defines two methods: `New()` and `Respawn()`. These should be called
when the object is first created and when it has been respawnd, respectively.

The following `ObjectPool` class definition is a fairly simple implementation of the Object
Pooling concept:

```
```
using System.Collections.Generic;

public class ObjectPool<T> where T : IPoolableObject, new() {
    private Stack<T> _pool;
    private int _currentIndex = 0;

    public ObjectPool(int initialCapacity) {
        _pool = new Stack<T>(initialCapacity);
        for(int i = 0; i < initialCapacity; ++i) {
            Spawn(); // instantiate a pool of N objects
        }
        Reset();
    }

    public int Count {
        get { return _pool.Count; }
    }

    public void Reset() {
        _currentIndex = 0;
    }

    public T Spawn() {
        if (_currentIndex < Count) {
            T obj = _pool.Peek();
            _currentIndex++;
            IPoolableObject po = obj as IPoolableObject;
            po.Respawn();
            return obj;
        } else {
```
```