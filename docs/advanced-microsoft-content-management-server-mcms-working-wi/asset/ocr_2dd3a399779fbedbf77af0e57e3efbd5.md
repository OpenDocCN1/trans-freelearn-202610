```
// The event handler that saves the contents of the TextBox to the posting's
// DisplayName property
private void saveDisplayName(object sender, WebAuthorPostingEventArgs e)
{
    e.Posting.DisplayName = txtDisplayName.Text.Trim();
}
```

The control is complete. To use the control, simply drag and drop it onto any template file that requires it. In this example, we have chosen to save only the display name of the posting. You could easily extend the control to work with all other properties and custom properties of the posting.

> If you need to re-use the code across projects, it is more efficient to use a server control instead of a web user control. Here's an example of such a control:
> 
> http://www.gotdotnet.com/Community/UserSamples/Details.aspx?SampleGuid=2A85D442-286F-4AD6-8A03-82436BF956A5

# Tip #7: How to Re-write Unfriendly URLs as Friendly URLs

MCMS, as you probably know, uses an internal system of "unfriendly URLs" in order to retrieve both the template file and the posting. Visitors to the site typically don't see these unfriendly URLs, except:

+   - When toggling between Edit Site and Live Site views.
+   - After performing a postback to the server. For example, the template file could have a button that refreshes certain labels on the page. When the button is clicked, a postback occurs and the URL on the address bar changes from a friendly URL to the unfriendly version.
+   - When the site has been configured to use unique ID-based URLs.
+   - When displaying a page within a frameset.

Unfriendly URLs are a composition of the URL of the template file and information about the posting appended in its querystring:

```
http://localhost/TropicalGreen/Templates/Plant.aspx?NRMODE=Published&NRORIGINALURL=%2fNR%2fexeres%2f0E0047A0-1560-4BA8-859F-C012826391F0%2cframeless%2ehtm%3fNRMODE%3dPublished&FRAMELESS=true&NRNODEGUID=%7b0E0047A0-1560-4BA8-859F-C012826391F0%7d&NRCACHEHINT=ModifyLoggedIn
```

Sometimes, it could appear in a slightly simpler form:

```
http://localhost/NR/exeres/0E0047A0-1560-4BA8-859F-C012826391F0,frameless.htm?NRMODE=Published
```

But behind the scenes, MCMS still sees the complex version.

Both URLs really point to the same page, say

```
http://localhost/tropicalgreen/plantcatalog/coconut.htm
```

but you can't tell just by looking at them.