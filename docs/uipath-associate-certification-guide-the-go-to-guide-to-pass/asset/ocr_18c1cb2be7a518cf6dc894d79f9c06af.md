11. Set the Set Name value to idx (1) and the Set Value value to 1 (2). Then, click OK (3):

![](bbox=[0.135, 0.15, 0.865, 0.59])

Figure 8.25 – Parameterized values

12. The selector will look similar to the following and will be ready to select the values dynamically. However, we must change the values accordingly:

```
<html app='chrome.exe' title='All Tech Books & eBooks | Programming & Tech Learning | Packt' />
<webctrl css-selector='body&gt;div&gt;main&gt;div&gt;div&gt;div&gt;
div&gt;div&gt;div&gt;div&gt;div&gt;div&gt;div&gt;div&gt;div&gt;
a&gt;h5&gt;b idx='{{idx}}' />
```

Figure 8.26 – Parameterized selectors