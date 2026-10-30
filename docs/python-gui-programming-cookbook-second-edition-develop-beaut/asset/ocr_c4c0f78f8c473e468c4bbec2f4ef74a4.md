Now, we can control the logging by adding logging levels to our logging statements and setting the level we wish to output. Let's add this capability to our **New York** button callback method in the `Callbacks_Refactored.py` module, which is the `getDateTime` method.

We change the previous `print` statements to `log` statements using different debug levels.

In the `GUI_Refactored.py`, we import both the new classes from our `Logger` module:

```
```python
from Ch08_Code.Logger import Logger, LogLevel
```
```

Next, we create local instances of those classes:

```
```python
# create Logger instance
fullPath = path.realpath(__file__)
self.log = Logger(fullPath)

# create Log Level instance
self.level = LogLevel()
```
```

As we are passing an instance of the GUI class to the `Callbacks_Refactored.py` initializer, we can use logging level constraints according to the `LogLevel` class we created:

```
```python
# Format local US time with TimeZone info
def getDateTime(self):
    fmtStrZone = "%Y-%m-%d %H:%M:%S %Z"
    # Get Coordinated Universal Time
    utc = datetime.now(timezone('UTC'))
    self.oop.log.writeToLog(utc.strftime(fmtStrZone),
                            self.oop.level.MINIMUM)

    # Convert UTC datetime object to Los Angeles TimeZone
    la = utc.astimezone(timezone('America/Los_Angeles'))
    self.oop.log.writeToLog(la.strftime(fmtStrZone),
                            self.oop.level.NORMAL)

    # Convert UTC datetime object to New York TimeZone
    ny = utc.astimezone(timezone('America/New_York'))
    self.oop.log.writeToLog(ny.strftime(fmtStrZone),
                            self.oop.level.DEBUG)

    # update GUI label with NY Time and Zone
    self.oop.lbl2.set(ny.strftime(fmtStrZone))
```
```