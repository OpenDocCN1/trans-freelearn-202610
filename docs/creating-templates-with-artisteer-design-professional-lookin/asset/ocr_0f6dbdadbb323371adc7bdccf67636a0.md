We have overwritten the width and color of the border, and have also set the background color as a delicate shade of yellow. Save the file and refresh the page. You should see something like the following screenshot:

![](bbox=[0.175, 0.207, 0.825, 0.522])

Our block on the right looks quite different from its twin brother on the left

6. So far, so good. Let's perform the next step and change the font of the text. Add another line of code to our class and make it look like the following:

```
.clearfix_SpecialModule
{
  border: 1pt solid red;
  background-color: #FFFF99;
  color: blue;
}
```

7. Save the changes, refresh the page, and... nothing happened! Something is wrong. Revert the changes (delete the last definition from our class) and then let's look at the source code of the page. The piece of code that displays our block looks like the following:

```
<div class="art-block clearfix_SpecialModule">
  <div class="art-blockheader">
    <h3 class="t">Second Block</h3>
  </div>
  <div class="art-blockcontent">
    <div class="custom_SpecialModule"  >
```