For example, we can find all the li tags that come after the first div tag using the following code:

```
first_div = soup.div
all_li_tags = first_div.find_all_next("li")

#output
[<li class="producerlist">
    <div class="name">plants</div>
    <div class="number">100000</div>
</li>, <li class="producerlist">
    <div class="name">algae</div>
    <div class="number">100000</div>
</li>, <li class="primaryconsumerlist">
    <div class="name">deer</div>
    <div class="number">1000</div>
</li>, <li class="primaryconsumerlist">
    <div class="name">rabbit</div>
    <div class="number">2000</div>
</li>, <li class="secondaryconsumerlist">
    <div class="name">fox</div>
    <div class="number">100</div>
</li>, <li class="secondaryconsumerlist">
    <div class="name">bear</div>
    <div class="number">100</div>
</li>, <li class="tertiaryconsumerlist">
    <div class="name">lion</div>
    <div class="number">80</div>
</li>, <li class="tertiaryconsumerlist">
    <div class="name">tiger</div>
    <div class="number">50</div>
</li>]
```

# Searching for previous

Searching for previous is the opposite case of next, where we can find the previous object associated with a particular object. We can use the find_all_previous() method to find all the previous objects associated with the current object and find_previous() to find the previous object associated with the current object.