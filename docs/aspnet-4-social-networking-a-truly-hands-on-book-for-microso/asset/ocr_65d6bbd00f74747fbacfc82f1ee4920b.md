The **GetQueryStringValue()** method takes in a key value and retrieves the item from the query string. The **SetInSession()** method allows you to pass in a key and an object. The object is then stored in the session under that key name. **GetFromSession()** does just that—it takes a key and retrieves that corresponding object. **UpdateInSession()** is very similar to **SetInSession()** with the exception that it assumes that a key already exists and updates the value that is currently stored there. This method will throw an error if a key does not exist. Therefore prior to using this method, you should check that your key exists in the session collection!

# Redirection

From an OOP point of view, Response.Redirect is about as useful as the session object is (we're starting to see a trend here). It simply provides a way of sending you from one place to another. It would be nice if we could work with it using methods. It would be even better if we could hide some logic in those methods if need be. Our initial implementation is very easy.

```
```csharp
//Fisharoo/Components/Redirector.cs
using System.ComponentModel.Composition;
using System.Web;
using Fisharoo.Interfaces;

namespace Fisharoo.Components
{
    [Export(typeof(Redirector))]
    public class Redirector : IRedirector
    {
        public void GoToHomePage()
        {
            Redirect("~/Default.aspx");
        }

        public void GoToErrorPage()
        {
            Redirect("~/Error.aspx");
        }

        private void Redirect(string path)
        {
            HttpContext.Current.Response.Redirect(path);
        }
    }
}
```
```