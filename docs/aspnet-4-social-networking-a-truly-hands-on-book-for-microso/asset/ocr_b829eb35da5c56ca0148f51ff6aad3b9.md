# # Implementing the application layer

This layer can be called as application or business logic or components or services layer. In our code, we have called this as **Components**. They are all one and the same. This layer should be relatively thin and lightweight. It is not supposed to hold any business logic or data access logic. It is more of a working layer that is responsible for keeping the business layer easier and cleaner to use. Often, it will combine several items from the business layer and several methods from the data layer to present an easy-to-use interface for a complex task.

An example of this would be our AccountService. The AccountService provides a few simple methods that utilize several of our more infrastructure-oriented classes. Here is the code:

```
```csharp
//Fisharoo/Components/AccountService.cs
namespace Fisharoo.Components
{
    [Export(typeof(IAccountService))]
    public class AccountService : IAccountService
    {
        [Import]
        private IAccountRepository _accountRepository;
        [Import]
        private IPermissionRepository _permissionRepository;
        [Import]
        private IUserSession _userSession;
        [Import]
        private IRedirector _redirector;
        [Import]
        private IEmail _email;
        public AccountService()
        {
            MEFManager.Compose(this);
        }
        public bool UsernameInUse(string Username)
        {
            Account account =
                _accountRepository.GetAccountByUsername(Username);
            if(account != null)
                return true;
            return false;
        }
    }
}
```
```