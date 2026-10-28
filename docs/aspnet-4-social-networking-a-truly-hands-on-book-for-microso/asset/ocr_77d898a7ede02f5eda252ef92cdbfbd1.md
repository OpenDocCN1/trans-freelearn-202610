# Friends

Either we can load a **friendInvitation** from the GUID or not. If we can, then we check to see if the user is already a member of the system and logged in. If they are logged in we automatically redirect them to the login screen. Otherwise we prompt them to log in or create an account. If the **friendInvitation** can't be loaded properly then we show an error explaining that.

The real magic occurs for the invitation process in the login and registration pages.

# Login

In the login presenter we have added some logic to the **Init** method to recognize if we have a friendship request or not.

```
```csharp
//Fisharoo/Web/Accounts/Presenters/LoginPresenter.css
public void Init(ILogin view)
{
    _view = view;
    - - -
    if(!string.IsNullOrEmpty(_webContext.FriendshipRequest))
        _view.DisplayMessage("Login to add this friend");
}
```
```

This logic lets the user know that by logging in they will be accepting the friend request.

Then in the AccountService.cs file we have added some additional logic. If the login is a success and if there is a friend request, we confirm the request and make these two users friends, via the **FriendService** we discussed earlier. However, before making friends, we need to validate if the friend request is really meant for the account that has been logged into. If not we display appropriate error message and don't allow the user to log in.

```
```csharp
//Fisharoo/BusinessLogic/AccountService.cs
public string Login(string Username, string Password)
{
    ...
        if (account.EmailVerified)
        {
            _userSession.LoggedIn = true;
            _userSession.Username = Username;
            _userSession.CurrentUser = account;

            if(!string.IsNullOrEmpty(_webContext.
                                      FriendshipRequest))
            {
```
```