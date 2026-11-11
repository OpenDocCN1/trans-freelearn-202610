Buy it on Amazon

Once we encounter the end `Errors` and `SmallImage` element, we are no longer in any sort of special state. We clear these two flags here in `endElement`.

```
$this->tagName = "";
```

And we clear the `tagName` property to make sure the next element that we are not interested in is not captured.

```
    }
    }
?>
```

Our form is ready to be used at this point. If you load `UPCForm.php` into your web browser, you can enter a UPC number and hit the submit button to see it in action.

![](bbox=[0.18, 0.36, 0.82, 0.57])

![](bbox=[0.21, 0.62, 0.26, 0.66])

> If you enter a book's UPC number, they will probably not appear in the Internet UPC Database call results. Books usually use a special UPC number that begins with 978, followed by the ISBN number. This is a special subset of UPC and EAN numbers that the Internet UPC Database does not support.