Another possibility is the 'horizontalflipped' choice, which rotates the column headers by 90 degrees. If we try this choice, another parameter, $cfg['HeaderFlipType'], plays a role. Its default value, css, displays true rotated headers. Not every browser supports this; however, Internet Explorer 7 does support this feature and produces:

![](bbox=[0.185, 0.24, 0.815, 0.37])

On other browsers, it seems the best we can achieve is by setting $cfg['HeaderFlipType'] to fake.

![](bbox=[0.215, 0.44, 0.785, 0.63])

We are currently using a table containing a small number of rows. With larger tables, we could see a more complete set of navigation buttons. To simulate this, let's use the Show dialog to change the default number of rows from 30 to 1; we then click Show. We can see that the navigation bar adapts itself:

![](bbox=[0.18, 0.73, 0.82, 0.82])