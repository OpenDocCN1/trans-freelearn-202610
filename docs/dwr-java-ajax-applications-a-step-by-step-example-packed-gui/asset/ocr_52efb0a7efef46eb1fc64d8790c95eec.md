```
</tr>
</thead>
<tbody id="usersOnline">
</tbody>
</table>

<input id="logoutButton" type="button" value="Logout" onclick="logout();return false;"></div>

</div>

<div id="footer">Stylesheet by
<a href="http://www.dynamicdrive.com/style/">Dynamic Drive CSS Library</a></div>

<script type="text/javascript">
getPreviousMessages();
</script>

</body>
</html>
```

The functionality at the beginning of the page is the same as in the mainpage.jsp page in the previous DWRBookAuthoring sample.

The first chat-room-specific JavaScript function is getPreviousMessages(). This function is called at the end of mainpage.jsp, and it retrieves previous chat messages for this chat room.

The newMessage() function is called by the server-side Java code when a new message is posted to the chat room. The function also scrolls the chat area automatically to show the latest message.

The sendMessageIfEnter() and sendMessage() functions are used to send user messages to the server. There is the input field for the message text in the HTML code, and the sendMessageIfEnter() function listens to onkeyup events in the input field. If the user presses enter, the sendMessage() function is called to send the message to the server.

The HTML code includes the chat area of specified size and with automatic scrolling.

# Developing the Java Code

The Java code for the chat room sample uses the same classes as the DWRBookAuthoring sample. The Login, UserDatabase and Util classes are the same except that the package name is different. The logic for the server-side chat room functionality is in the ChatRoomDatabase class. The source code for the ChatRoomDatabase is as follows: