Now, we test our internationalized GUI title in two languages and get the following result on running the code:

![](bbox=[0.11, 0.16, 0.97, 0.38])

We ran two unit tests but, instead of an OK, we got a failure. What happened?

Our assertion failed for the German version of our GUI...

While debugging our code, it turns out that in the copy, paste, and modify approach of our unit test code, we forgot to pass German as the language. We can easily fix this:

```
def test_TitleIsGerman(self):
    # i18n = I18N('en')      # <= Bug in Unit Test
    i18n = I18N('de')
    self.assertEqual(i18n.title,
        'Python Grafische Benutzeroberfl' + "u00E4" + 'che')
```

When we rerun our unit tests, we get the expected result of all our tests passing:

UnitTestsFail.py with failure corrected

![](bbox=[0.34, 0.69, 0.74, 0.86])