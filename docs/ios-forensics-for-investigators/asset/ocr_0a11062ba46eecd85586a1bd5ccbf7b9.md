user's browsing history are not stored and the websites that have been visited are not synced to iCloud.

> As stated in Apple's official documentation, *"Safari won't remember the pages you visited, your search history, or your AutoFill information AFTER you close a tab in private browsing mode."*

Theoretically, this poses a problem from a forensic perspective as both `history.db` and `KnowledgeC.db` do not store this data. There are, however, other options:

+   - You can analyze the cookies that are stored on the device. Safari's web cache could also reveal traces of websites that were browsed with private browsing active.
+   - The `BrowserState.db` database stores details of active tabs, **including private tabs**. For each tab, there is an entry in the `tab_sessions` table that contains a binary PLIST. Converting this PLIST into XML format will allow the investigator to parse through the file, which contains the history of the pages that were browsed within that tab. This will only work if the private tab is still open in Safari.
+   - As we learned in the previous chapters, deleted data is not permanently removed from a SQLite database – it is merely marked as deleted. By using **forensic tools**, it is possible to recover deleted records from the `BrowserState.db` database, which may include details of tabs that have been opened in private mode. To learn how to attempt data recovery from SQLite databases, please refer to *Chapter 4, Working with Common iOS Artifacts*.

Internet browsing artifacts are an important part of any mobile forensics investigation. Analyzing the browsing history may provide some insight, but accessing the data that's relevant to private internet browsing can be far more enlightening, revealing any activity that the user wanted to keep hidden.

# Summary