# User Accounts

```
```csharp
if(!string.IsNullOrEmpty(NewPassword))
    account.Password = NewPassword.
    Encrypt(NewPassword);

account.FirstName = FirstName;
account.LastName = LastName;

account.Zip = ZipCode;
account.BirthDate = BirthDate;

_accountRepository.SaveAccount(account);
_view.ShowMessage(true, "Your account has been
updated!", account.UserName);
}
else
{
    _view.ShowMessage(false, "The password you entered
doesn't match your current password! Please try
again.", account.UserName);
}
}
}
}
}
```
```

One thing to notice with the presenter is that when it is first initialized, it loads the current user's account details and passes that data to the view for initial display. Then once the user edits their data, there are several validation steps that occur. The most important is that of the password and the email.

If the password is not changed, we want to make sure that we do not store an empty value to the system!

For the email, if a user changes it, we want to make sure that we resend the verification email again and flag the account as not having a validated email address.

Beyond that we are simply updating the account object via the AccountRepository.Save() method.

# Implementing security

Now that we have all of our plumbing in place, we are at a point that we can lock down our site. Up until now someone could go wherever they wanted to on the site and we would not be able to stop them at all!