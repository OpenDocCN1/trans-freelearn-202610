At this point, the configuration should be fully loaded, and the application should contain the same models, instances, and settings as at the time of the save operation.

One question remains for the load process: What should we do if parsing the configuration file fails only partially? Maybe a model file was renamed or deleted, or the file was truncated or corrupted and the settings of the last instance are incomplete.

# Strict or relaxed configuration file loading

One option to overcome parsing errors is to discard the entire configuration prior to deleting all current content of the application. This strict loading type is easy to implement; any kind of parsing error invalidates the configuration file at parsing time. We ignore the loading request and present just an error message to the user.

The other option is relaxed parsing. We try our best to load the valid models and fill in the missing configuration parts with default values but also tell the user that parts of the configuration file cannot be loaded.

In both cases, the error message should give detailed hints at which point the parsing failed. Therefore, the exception handling could be extended to know exactly what went wrong, and where. For relaxed handling, as much additional information about affected models, instances, or settings as possible should be presented to the user.

It is up to the creator of an application to decide which strategy fits best. Usually, an attempt to recover as much data as possible should be made. Losing only a small part of the created work is better than losing all data.

# Common errors leading to corrupted files

A couple of factors can render your saved configuration file corrupt. Some common causes are listed here:

+   - Disk or partition is filled up to 100% while writing the file: Even with the large amount of storage we have today, this may happen, and you have only parts of your data saved.
+   - Permission problems: Sometimes, you might have permission to create files but not to write file content. So, your files seem to be saved, but the file has a length of zero bytes.
+   - Connection errors when saving to remote places: Your connection may be interrupted while writing a larger file, resulting in a partially written file.