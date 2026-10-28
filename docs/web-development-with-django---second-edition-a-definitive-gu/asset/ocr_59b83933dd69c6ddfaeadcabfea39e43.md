![](bbox=[0.115, 0.095, 0.85, 0.505])

Figure 1.23 – The views.py default content

+   2. Remove this placeholder text from views.py and instead insert this content:

```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world!")
```

First, the HttpResponse class needs to be imported from django.http. This is what is used to create the response that goes back to the web browser. You can also use it to control things such as the HTTP headers or status code. For now, it will just use the default headers and the 200 Success status code. Its first argument is the string content to send as the body of the response.