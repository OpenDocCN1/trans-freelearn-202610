```
(let [layout (VerticalLayout.)]
    (.addComponent layout (Label. "I am getting sleepy ~~~"))
    (.start (Thread. (runme)))
    layout
)
```

The code reads by doing a simple layout, starting a thread, and, after 3 seconds, displaying the notification. Running the code by yourself... You will notice very quickly that...the thread starts, waits for 3 seconds and...does nothing. As you read it properly, it doesn't work. What's wrong?

The reason behind the lack of visual update is that the thread that was spawned is not the UI thread anymore, so the refresh does not link to existing UI objects and the update fails.

To perform an async update, we will implement a new Runnable, and ask the UI thread to perform the async update. This is done with UI/getCurrent to retrieve the UI thread, and .access to perform the async update.

So, with those few updates, you now have the following code:

```
(def async-ui-update
    (reify Runnable
      (run [_]
        (Notification/show "Let's sleep"))))

(defn runme []
    (reify Runnable
      (run [_]
        (Thread/sleep 3000)
        ; (Notification/show "Does not work"))))
    (-> (UI/getCurrent) (.access async-ui-update))))
```

And finally, the notification shows the following: