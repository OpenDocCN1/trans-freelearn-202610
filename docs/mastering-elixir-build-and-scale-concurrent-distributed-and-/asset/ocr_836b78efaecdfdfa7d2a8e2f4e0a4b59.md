```
init(socket, userId) {
  // ...
  let infoArea = document.getElementById("notify_info")
  let successArea = document.getElementById("notify_success")
  userChannel.on("download", ({link, message}) =>
    this.renderDownloadNotification(infoArea, successArea, message,
    link))
  },
  renderDownloadNotification(infoArea, successArea, message, link) {
    infoArea.innerHTML = ""
    successArea.innerHTML = `<a href="${link}">${message}</a>`
  }
}

export default Notification
```

We've changed the application layout to add an ID to the flash message elements so that we can get them in the init function. After getting those elements, we can see how to define a handler for events on our user channel. We're calling the on function on it, passing the name of the event we will handle and a lambda, which will be called whenever we receive this type of event. Note that the lambda will receive as arguments the values of the keys we've defined on the broadcast message—link and message in this example. Inside the lambda, we're simply calling the renderDownloadNotification function, which will display a link in the successArea element. Before that, we're blanking the infoArea element, so that previous flash messages disappear and we just show the notification we're rendering now.

Let's now see all this in action with some screenshots. When we click the "Download File" button, a flash message is displayed, informing us that the download is enqueued and a link will appear soon: