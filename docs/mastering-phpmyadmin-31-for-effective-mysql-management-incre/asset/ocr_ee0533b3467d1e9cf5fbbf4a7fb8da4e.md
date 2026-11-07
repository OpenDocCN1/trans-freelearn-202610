To help choose a suitable theme, the color palette icon next to **Theme/Style** brings us screenshots of the available themes. We can then click on **take it** under the theme we want. The chosen theme is remembered in a cookie. By default, the remembered theme applies to all servers we connect to. To make phpMyAdmin remember one theme per MySQL server, we set `$cfg['ThemePerServer']` to TRUE.

# The color picker

Here is a feature—the custom color picker— that may seem frivolous but has its utility. We'll discuss first the *how* and then the *why*.

Located on the homepage, the small rainbow icon gives access to a picker, which controls the background color of both panels. The phpMyAdmin team merged the MooRainbow code from Djamil Legato (http://moorainbow.woolly-sheep.net) to offer this feature.

![](bbox=[0.207, 0.393, 0.792, 0.697])

You can operate the picker in many ways—moving the slider, moving the mouse in the big square, or directly entering values in the fields. When you are done, you click **Select** to clear the picker.