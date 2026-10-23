```
<div class="number">100</div>
</li>
</ul>
<ul id="tertiaryconsumers">
<li class="tertiaryconsumerlist">
<div class="name">lion</div>
<div class="number">80</div>
</li>
<li class="tertiaryconsumerlist">
<div class="name">tiger</div>
<div class="number">50</div>
</li>
</ul>
</body>
</html>
```

The preceding HTML is a simple representation of the ecological pyramid. To find the first producer, primary consumer, or secondary consumer, we can use Beautiful Soup search methods. In general, to find the first entry of any tag within a BeautifulSoup object, we can use the find() method.

# Finding the first producer

In the case of the ecological pyramid example of the HTML content, we can easily recognize that the producers are within the first <ul> tag. Since the producers come as the first entry for the <ul> tag within the whole HTML document, it is easy to find the first producer using the find() method. The HTML tree that represents the first producer is shown in the following diagram:

![](bbox=[0.32, 0.59, 0.68, 0.79])