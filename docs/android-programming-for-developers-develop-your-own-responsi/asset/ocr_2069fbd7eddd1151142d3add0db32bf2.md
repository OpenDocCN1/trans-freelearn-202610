Enter this code just after the call to setContentView in the onCreate method:

```
// Get a reference to all our widgets
RadioGroup radioGroup = (RadioGroup) findViewById(R.id.radioGroup);
RadioButton rbBeijing = (RadioButton) findViewById
(R.id.radioButtonBeijing);
RadioButton rbLondon = (RadioButton) findViewById
(R.id.radioButtonLondon);
RadioButton rbNewYork = (RadioButton) findViewById
(R.id.radioButtonNewYork);
final EditText editText = (EditText) findViewById(R.id.editText);
final Button button = (Button) findViewById(R.id.button);
final TextClock tClock = (TextClock) findViewById(R.id.textClock);
final CheckBox cbTransparency = (CheckBox) findViewById
(R.id.checkBoxTransparency);
final CheckBox cbTint = (CheckBox) findViewById(R.id.checkBoxTint);
final CheckBox cbReSize = (CheckBox) findViewById
(R.id.checkBoxReSize);
final ImageView imageView = (ImageView)
findViewById(R.id.imageView);
Switch switch1 = (Switch) findViewById(R.id.switch1);
final WebView webView = (WebView) findViewById(R.id.webView);
```

Now we can create an anonymous class to listen for and handle clicks on the
checkboxes. Each of the next three blocks of code implement an anonymous
class for each of the checkboxes in turn. What is different in each of the next
three blocks of code, however, is the way in which we respond to a click, and we
will discuss each of them in turn.

The first checkbox is labeled Transparency, and we use the setAlpha method
on imageView to change how transparent (see-through) it is. The setAlpha
method takes a floating point value between 0 and 1 as an argument. Here, 0 is
invisible and 1 is no transparency at all. So, when this checkbox is checked, we
set the alpha to .1, so the image is barely visible, and when it is unchecked, we
set it to 1, which is completely visible with no transparency. The boolean
isChecked parameter of onCheckedChanged contains true or false as to whether
the checkbox is checked or not.

Add this code after the previous block of code in onCreate:

```
/*
```