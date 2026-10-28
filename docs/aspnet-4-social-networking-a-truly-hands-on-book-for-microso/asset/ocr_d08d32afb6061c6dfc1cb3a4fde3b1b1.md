# Messaging

And here is what will be seen after replying to the message:

![](bbox=[0.224, 0.176, 0.779, 0.465])

As usual, as all the leg work is done in the presenter, let's jump straight to it.

```
```csharp
//Fisharoo/Web/Mail/Presenters/ReadMessagePresenter.cs
public void Init(ReadMessage view)
{
    _view = view;
    _view.LoadMessage(_messageService.GetMessageByMessageID(_webContext.MessageID, _userSession.CurrentUser.AccountID));
}
```
```

As you can see here, we are populating the UI by getting the message based on the ID and then passing it to the view for display.

For the **Reply** button, we will bubble up the click event into the presenter. The presenter then makes a call to the Redirector class that passes the current MessageID to the NewMessage page.