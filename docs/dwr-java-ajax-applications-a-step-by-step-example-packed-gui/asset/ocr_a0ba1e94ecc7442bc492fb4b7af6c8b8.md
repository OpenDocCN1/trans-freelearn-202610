The input element, where we enter the license name, listens to the onkeyup event which calls the showPopupMenu() JavaScript function. Clicking the Input button calls the showLicenseText() function (the JavaScript functions are explained shortly). Finally, the two div elements are place holders for the pop-up menu and the iframe element that shows license content.

For the pop-up box functionality, we use existing code and modify it for our purpose (many thanks to http://www.jtricks.com). The following is the popup.js file, which is located under the WebContent | js directory.

```
//<script type="text/javascript"><!--
/* Original script by: www.jtricks.com
 * Version: 20070301
 * Latest version:
 * www.jtricks.com/javascript/window/box.html
 *
 * Modified by Sami Salkosuo.
 */
// Moves the box object to be directly beneath an object.
function move_box(an, box)
{
    var cleft = 0;
    var ctop = 0;
    var obj = an;

    while (obj.offsetParent)
    {
        cleft += obj.offsetLeft;
        ctop += obj.offsetTop;
        obj = obj.offsetParent;
    }

    box.style.left = cleft + 'px';

    ctop += an.offsetHeight + 8;

    // Handle Internet Explorer body margins,
    // which affect normal document, but not
    // absolute-positioned stuff.
    if (document.body.currentStyle &&
        document.body.currentStyle['marginTop'])
    {
        ctop += parseInt(
            document.body.currentStyle['marginTop']);
    }
}
```