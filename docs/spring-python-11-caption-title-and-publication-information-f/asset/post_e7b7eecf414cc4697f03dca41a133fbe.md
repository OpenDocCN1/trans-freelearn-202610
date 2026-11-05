
This command generates a set of files:

| File name | Description |
|---|---|
| booking.py | Runnable CherryPy application. |
| app_context.py | Spring Python application context, that contains the wiring for our components. |
| view.py | Contains all the web parts, including HTML. |
| controller.py | Contains functions that the view accesses to generate dynamic content for the screens. |

This auto-generated application contains pre-wired components, including security, some basic links, and the bootstrapping needed to launch the CherryPy web server engine. It just needs some minor modifications before we start coding our features.

- 2. Remove any licensing comments embedded in the app. While the template files themselves are marked with a license, this application may not be released under the same license
- 
- 3. Ease security restrictions defined in app_context.py, allowing anonymous users access the main parts of site, while subjecting any link underneath /customer/* to full security and requiring ROLE_CUSTOMER

```
@Object
def filterChainProxy(self):
    return CP3FilterChainProxy(filterInvocationDefinitionSource =
    [
        ("/images.*", []),
        ("/html.*", []),
        ("/accessDenied.*", []),
        ("/login.*", ["httpSessionContextIntegrationFilter"]),
        ("/customer/.*", ["httpSessionContextIntegrationFilter",
            "exceptionTranslationFilter",
            "authenticationProcessingFilter",
            "filterSecurityInterceptor"])
    ])
```

```
@Object
def filterSecurityInterceptor(self):
    filter = FilterSecurityInterceptor()
    filter.auth_manager = self.authenticationManager()
    filter.access_decision_mgr = self.accessDecisionManager()
    filter.sessionStrategy = self.cherrypySessionStrategy()
    filter.obj_def_source = [
        ("/customer/.*", ["ROLE_CUSTOMER"])
    ]

    return filter
```

4. Remove the images and CSS code and replace them with a simpler header and footer:

```
def header():
    return """
    <html>
    <head>
    <title>Spring Flight Reservation System</title>
    </head>

    <body>
    """

def footer():
    return """
    <hr>
    <table style="width:100%"><tr>
        <td><A href="/">Home</A></td>
    </tr></table>
    </body>
    """
```

5. Remove all methods from the view layers that were exposed using @cherrypy.expose (except index, login, and logout)

6. Let's launch this web app using Python and then visit it by opening a browser at http://localhost:8080

![](img/bcf886d8b665372ee8afbb620d4273bb_239_0.png)

![](img/bcf886d8b665372ee8afbb620d4273bb_239_1.png)

We now have a clean slate from which we can start fleshing out the features of our system.

## Looking up existing flights

What is the first piece of data that we need for a flight reservation system? Flight listings! Since customers usually need to search based on a date or location, let's fetch some flight data and then build some search features.

- 1. Let's add a link on the main page to take us to a search page.

```
@cherrypy.expose
def index(self):
    """This is the root page for your booking app. Its default
    includes links to all other exposed links automatically."""

    return header() + """
        <H2>Welcome to Spring Flight Reservation System</H2>
        <P>
        <ul>
            <li><a href="flight_listings">Search Flight
                Listings</a></li>
        </ul>
        <P>""" + footer()
```

We have now created an HTML bulleted list with one entry to Search Flight Listings.

![](img/bcf886d8b665372ee8afbb620d4273bb_240_0.png)

2. Before we try to display any flight information, let's define a simple Python class to contain this information:

```
class Flight(object):
    def __init__(self, flight=None, departure=None, arrival=None):
        self.flight = flight
        self.departure = departure
        self.arrival = arrival
```

3. Now that we've created a simple model for our flight data, let's display it on the web page. To do that, let's add a flight_listings function and expose it using @cherrypy.expose:

```
@cherrypy.expose
def flight_listings(self):
    page = header() + """
        <H2>Search Flight Listings</H2>
        <table border="1">
            <tr><th>Flight</th><th>Departure</th>
                <th>Arrival</th></tr>
    """

    for f in self.controller.flights():
        page += "<tr><td>%s</td><td>%s</td><td>%s</td></tr>" % \
                (f.flight, f.departure, f.arrival)

    page += "</table>" + footer()

    return page
```

We haven't created a search box yet. Before we do, let's ask the controller to give us all flight data.

4. We can't click on the link yet because there is no controller, so let's build one and have it supply some sample data

```
from model import *

class BookingController(object):
    def __init__(self):
        self.data = []
        self.data.append(Flight("SpringAir 14", "9:02am Melbourne", "10:45am Nashville"))
        self.data.append(Flight("Python Airways 28", "10:40am Orlando", "2:15pm San Francisco"))

    def flights(self):
        return self.data
```

## Case Study II – Integrating Spring Python with your Java Application

There isn't much there, it's just a simple Python array containing one Flight instance. But we want to build this up from small, simple parts that we can easily visualize. With all these parts in place, let's look at the web page again.

![](img/bcf886d8b665372ee8afbb620d4273bb_242_0.png)

Now we can see this small bit of sample flight data.

- 5. Let's add a search box to provide some minimal filtering capability

```
@cherrypy.expose
def flight_listings(self, criteria=""):
    page = header() + """
        <H2>Search Flight Listings</H2>
        <form method="POST" action="/flight_listings">
            Search: <input type="text"
                        name="criteria"
                        value="%s"
                        size="10"/>
            <input type="submit"/>
        </form>
        <table border="1">
            <tr>
                <th>Flight</th>
                <th>Departure</th>
                <th>Arrival</th>
            </tr>
        """ % criteria

    for f in self.controller.flights(criteria):
        page += "<tr><td>%s</td><td>%s</td><td>%s</td></tr>" % \
            (f.flight, f.departure, f.arrival)

    page += "</table>" + footer()

    return page
```

We added a text input with a button to the top of the screen. The search criterion is now passed into the controller object to do any filtering.

![](img/bcf886d8b665372ee8afbb620d4273bb_243_0.png)

6. How would you utilize the text entered by the user to filter in the controller? Should we be case sensitive or perhaps relax that a bit? Which fields would you want to compare against if you were using it as a customer? Here is one way you could use it:

```
def flights(self, criteria):
    return [f for f in self.data \
        if criteria.lower() in str(f).lower()]
```

This grabs the sample data and applies a filtering list comprehension. The filter applies lower() to both the criteria as well as the stringified version of the flight record.

7. One way to let the customers search is against all the data in the flight record

```
def __str__(self):
    return "%s %s %s" % \
        (self.flight, self.departure, self.arrival)
```

With all these changes, we can now do a simple flight search.

![](img/bcf886d8b665372ee8afbb620d4273bb_244_0.png)

What are some other search options you would consider to make this more sophisticated?

## Moving from sample Python data to real Java data

So far, we are looking at a couple rows of sample data. A more realistic scenario would be tapping some type of a live system, listing real-time flights. What would you do if the data was stored in a backend database and a service was already coded in Java?

- Would you attempt to bypass the Java system and talk directly to the database? What risks are involved with this approach?
- Would you ditch the Python front end we just built and instead embark on a Java-based portal? What if this had been several months of effort?
