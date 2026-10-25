The getChapters() function calls the getChaptersForTitle() function, which calls the BookDatabase class and gets the chapter titles for the clicked book. The function also inserts the voting button for authors, and shows the current situation of votes, and shows the number of authors who have voted among the total.

The showBook() function retrieves a rendered book from the BookDatabase and shows it in the HTML page.

The function voteforPublish() takes the user's vote and updates the voting status of the specified book. The refreshChapterIfVisible() function updates the user's screen if changes happen in the book content, and the clearChapterViewIfVisible() function clears the contents of the book if a user is viewing it and the book is voted to be published.

The notifyBookPublished() function notifies the user that a book has been published and clears the book content if it is visible.

The getChapterContent() function shows the chapter content in the editable text area where the user can edit a chapter. If someone else is currently editing the chapter, the read-only text area is shown to the user, and the content of the area is the latest content that has been saved by the author.

The cancelChapterContentEdit(), closeChapterContentEdit(), and the saveChapterContent() functions correspond to the buttons with the same name in the chapter edit page. The save function also calls the setFeedback() function to show the user the time of the save event.

The startNewBook() and addChapter() functions are used to add a book or a chapter in a book. The refreshBookLists() function just calls the getBookLists() function to get the current books in the user interface. The last function, clearContentAreas(), clears the book content.

The rest of the page is HTML code, and the content includes the headlines and the place holders, and div elements, that are used by JavaScript functions.

# Configuring the Web Application

Before getting to the Java code, we need to set up DWR for our web application. We copy the dwr.jar file to the WEB-INF | lib directory and set up the web.xml file as shown:

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_
```