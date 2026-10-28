# Access a dictionary element

You can specify a dictionary key to access its corresponding value. Type in and run the following code:

```
contactList["Shah"]
```

This returns the value for the key "Shah", and +60123456789 is displayed in the Results area.

If you attempt to specify a key that does not exist in the dictionary, a nil value will be returned.

Imagine that one of your contacts has a new phone, so you must update the phone number for that contact. You can modify the key-value pairs stored in a dictionary. You’ll learn how to do this in the next section.

# Assign a new value to an existing key

You can assign a new value to an existing key. Type and run the following code:

```
contactList["Shah"] = "+60126789345"
contactList
```

This assigns a new value to the key "Shah". The contactList dictionary now contains "Shah" : "+60126789345", "Sohini" : "+0223456789", and "Nithya" : "+0229876543". You can see this in the Results area.

Imagine that you must remove a contact from your app. Let’s see how you can remove elements from a dictionary in the next section.

# Remove an element from a dictionary

To remove an element from a dictionary, assign nil to an existing key. Type in and run the following code:

```
contactList["Sohini"] = nil
contactList
```

This removes the element with the key "Sohini" from the contactList dictionary, and it now contains "Shah" : "+60126789345" and "Nithya" : "+0229876543". You can see this in the Results area.