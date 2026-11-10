# Mention two examples of Server API methods used for lifecycle management in a Docbase.

Two examples of Server API methods used for lifecycle management in a Docbase are: `install` and `uninstall`.

# What does the get API method do?

The get API method retrieves the value of the specified attribute of an object.

# Write a Server API command for linking an object to a folder abc residing within a cabinet xyz in a Docbase.

The Server API command in this case would be: `link,c,<object ID>,'/xyz/abc'`

# Which Server API command shows all the contained attributes in an object along with their respective values?

The Server API command dump shows all attributes and their respective values for the object in question. Usage: `dump,c,<object ID>`

# Can the destroy Server API method be used for deleting an object that is currently checked-out (and hence locked) in the Docbase?

No. In the case of locked objects in Docbase, running the destroy API method throws an error.