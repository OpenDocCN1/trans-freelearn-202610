```
```csharp
mr.IsApproved = false;
results.Add(mr);
}
}
if(!string.IsNullOrEmpty(txtGagDate.Text))
{
    _presenter.GagUserUntil(Convert.ToInt32
        (litAccountID.Text),
        litAccountUsername.Text,
        DateTime.Parse(txtGagDate.Text),
        txtReason.Text);
}
}
}
}
if(results.Count() > 0)
    _presenter.SaveModerationResults(results);
}
```
```

There appears to be a lot going on here, but a fair amount of the code at the top of the method is dedicated to locating and loading the controls from the UI. Once we have the controls in hand, we can interrogate the state of each flagged item. When we find something with a checkbox selected for approving or denying, we add the item to our List of ModerationResults (recall that we created this earlier in the repository in the previous section).

Once we have worked our way through the data, we then send our list of alterations through to the ModerationRepository where it is saved in bulk.

# Gagging

In the moderation pages UI you can notice that we have two text box controls. We have one text box for capturing the reason why we have decided to gag a user. And the other text box is for providing the date, which indicates when the gag would ultimately be revoked for a user.

In the **btnSubmit_Click()** method (discussed previously), you will also see some code that interrogates the state of those text boxes. If we find something in the Repeaters control collection, then we call into the GagUserUntil() method of our presenter and effectively gag the user. The presenter creates a new Gag object and persists it out to our database.

```
```csharp
//Fisharoo/AdminConsole/Moderations/Presenters/DefaultPresenter.cs
public void GagUserUntil(int AccountID, string AccountUsername,
                         DateTime GagTillDate, string Reason)
{
```
```