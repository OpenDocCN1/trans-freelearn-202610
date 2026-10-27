# Summary

To summarize, we will highlight some important facts about advanced storage space.

The concept of cookies was introduced for the first time on the Netscape web browser and was later migrated to Internet Explorer. It still remains relevant today. In order to have stateful communication, web browsers provide an area in their subdirectories where state information can be stored and accessed. The area and information stored in this area is called a cookie. The domain of cookies tells the browser the domain where the cookie should be sent. It allows a cookie to cross subdomains but does not allow it to cross domains. The path is the directory in the root of the domain where the cookie is active. Cookies are the universal mechanism that helps persist user-specific data in web browsers.

Web Storage represents a mechanism for better and a more secured way to persist data on the client than cookies. There are two Web Storage objects (Session and Local) that can be used to persisting user data in a web browser for the length of a session and indefinitely. Both of them have a similar simple API declared via a Storage interface.

Web SQL was introduced by Apple and is based on a free database SQLite. It will be discontinued very soon, and we used it here only to see how it could be used and how migration to other advanced technologies can be done painlessly.

In IndexedDB, we can store large amount of structured data, images, arrays, and whole objects, and just index them with a key. IndexedDB doesn't have any limits on a single database item's size, but it might impose a limit on each database's total items.

In the next chapter, we will demonstrate how different HTML5 features can be used in Dart.