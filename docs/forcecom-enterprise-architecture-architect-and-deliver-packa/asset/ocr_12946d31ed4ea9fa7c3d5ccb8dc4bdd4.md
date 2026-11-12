```
```java
public Boolean initialiseCalled = false;
public Boolean updateRPMsCalled = false;
public Integer updateRPMsCalledWith = null;
public void initialise() { initialiseCalled = true; }
public void updateRPMs(Integer rpms) {
    updateRPMsCalled = true;
    updateRPMsCalledWith = rpms;
}
public void off() { }
}```
```

```
```java
private class MockEngine implements IEngine {
    public Boolean startCalled = false;
    public void start() { startCalled = true; }
    public void stop() { }
    public Boolean isRunning() { return true; }
}```
```

```
```java
private class MockDisplay implements IDisplay {
    public Boolean showMessageCalled = false;
    public String showMessageCalledWithMessage = null;
    public void backlight(Boolean onOff) { }
    public void showMessage(Integer positionX, Integer positionY, String message) {
        showMessageCalled = true;
        showMessageCalledWithMessage = message;
    }
    public String getMessageShowAt(Integer positionX, Integer positionY) { return null; }
    public Boolean isVisible() { return false; }
}```
```

> You can keep the implementation of these mock classes contained and scoped within your Apex test classes or a single as class, as inner classes, if needed.

After making the preceding changes and introduction of the mocking classes, the implementation of the Car class object model now supports unit testing through CDI.