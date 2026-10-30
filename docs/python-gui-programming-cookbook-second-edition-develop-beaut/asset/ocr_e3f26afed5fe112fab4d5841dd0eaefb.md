The preceding screenshot shows that the command downloaded the .whl format. If you have not done so, you might have to install the Python wheel module first.

This installed the Python pytz module into the site-packages folder, so now we can import this module from our Python GUI code.

We can list all the existing time zones by running the following code, which will display the time zones in our ScrolledText widget. First, we add a new Button widget to our GUI:

```
import pytz
class OOP():
    # TZ Button callback
    def allTimeZones(self):
        for tz in pytz.all_timezones:
            self.scr.insert(tk.INSERT, tz + '\n')

    def createWidgets(self):
        # Adding a TZ Button
        self.allTZs = ttk.Button(self.widgetFrame,
                                text=self.i18n.timeZones,
                                command=self.allTimeZones)
        self.allTZs.grid(column=0, row=9, sticky='WE')
```

Clicking our new Button widget results in the following output:

GUI.py

![](bbox=[0.22, 0.6, 0.75, 0.71])

After we install the tzlocal Python module, we can print our current locale by running the following code:

```
# TZ Local Button callback
def localZone(self):
    from tzlocal import get_localzone
    self.scr.insert(tk.INSERT, get_localzone())

def createWidgets(self):
```