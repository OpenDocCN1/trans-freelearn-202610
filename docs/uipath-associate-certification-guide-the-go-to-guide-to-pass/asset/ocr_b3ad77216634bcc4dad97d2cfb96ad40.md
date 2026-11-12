# SubString function

There are scenarios where you need to extract only a particular portion of a string from a longer string to process it further. For example, from all the text in a **Portable Document Format (PDF)** file, you might want to extract only the invoice number to use it in another application. The `SubString` function comes in handy in such scenarios as it allows extraction of text by providing the starting index and the length of the substring. It uses the following expression to extract a portion of a text from a longer string:

```
StrVariable.Substring(<starting index>, <length of substring>)
```

Applying our knowledge of indexes, let's use the same working solution and try to extract the name of the person from the string variable, as follows:

+   1. Let's open the same working solution we worked on previously.

+   2. The `CleansedValue` string variable contains the name "John Doe". Let's count the starting index of the word "John" in the string. The word "John" starts from the 47th position.

+   3. Now, we have to check how many characters we need to extract. The name "John Doe" includes eight characters, including the space in the middle. Hence, the length of the string we require for extraction is 8.

+   4. Using the starting index and the length we calculated, let's configure the last **Message Box** activity to show the name. The following code example illustrates the configuration to apply in the **Message Box** activity:

```
CleansedValue.Substring(47, 8)
```

We can try to get the same output using the `IndexOf` function without hardcoding the positions. As we explored several `IndexOf` functions, let's explore a bit how we can combine these two functions together to get the same output.

+   5. Start the program and see the output of the last **Message Box** activity. It should show the name "John Doe".

As a quick recap, this section focused on extracting the desired part of a string from a larger string by providing start and end index points. This function comes in very handy on many occasions where you want to extract certain parts of a string based on a specific string pattern. Apart from this function, we also have the ability to split a string into multiple elements and access each part separately. Let's now see how we can split a text and retrieve individual elements after splitting.