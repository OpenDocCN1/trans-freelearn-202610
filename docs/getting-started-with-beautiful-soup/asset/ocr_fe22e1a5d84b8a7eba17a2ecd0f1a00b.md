# Search Using Beautiful Soup

Now, we can change to the Soup directory using the following command:

```
```bash
cd Soup.
```
```

We can save the following code as `ecologicalpyramid.py` and use python `ecologicalpyramid.py` to run it, or we can run the code from Python interpreter. Using the following code, we will create a BeautifulSoup object using the `ecologicalpyramid.html` file:

```
```python
from bs4 import BeautifulSoup
with open("ecologicalpyramid.html","r") as ecological_pyramid:
    soup = BeautifulSoup(ecological_pyramid,"lxml")
producer_entries = soup.find("ul")
print(producer_entries.li.div.string)

#output
plants
```
```

Since producers come as the first entry for the `<ul>` tag, we can use the `find()` method, which normally searches for only the first occurrence of a particular tag in a BeautifulSoup object. We store this in `producer_entries`. The next line prints the name of the first producer. From the previous HTML diagram, we can understand that the first producer is stored inside the first `<div>` tag of the first `<li>` tag that immediately follows the first `<ul>` tag, as shown in the following code:

```
```html
<ul id="producers">
    <li class="producerlist">
        <div class="name">plants</div>
        <div class="number">100000</div>
    </li>
</ul>
```
```

So, after running the preceding code, we will get `plants`, which is the first producer, as the output.

# Explaining find()

At this point, we know that `find()` is used to search for the first occurrence of any items within a BeautifulSoup object. The signature of the `find()` method is as follows:

```
```text
find(name,attrs,recursive,text,**kwargs)
```
```