![](bbox=[0.18, 0.15, 0.82, 0.35])

If using relative paths results in HTTP 404 errors when accessing the style sheet, how then, did the style get successfully applied to the posting? The secret lies in the RobotMetaTag. In MCMS websites, the RobotMetaTag has a RenderBaseHref property, which when set to true (its default value), will generate a <base> tag between the <head> tags of the page. Search the generated HTML source of the posting, and you will find the <base> tag that looks like the following:

```
<base href="http://tropicalgreen/tropicalgreen/Templates/Plant.aspx">
```

When evaluating links on a page, the web browser automatically prefixes all relative links with the value stored in the href attribute of the <base> tag (instead of the requesting URL for the page) such that a relative path like ../App_Themes/Sunny/Sunny.css becomes http://tropicalgreen/tropicalgreen/Templates/../App_Themes/Sunny/Sunny.css.

The converted URL does lead to the style sheet, so we don’t get any HTTP 404 errors.

By default, the RenderBaseHref property of the RobotMetaTag control stores the Boolean true. So usually, the <base> tag is generated and you won’t have to worry about paths to style sheets and other relative links being inaccessible. However, depending on the site setup, there may be special situations where generating a <base> is not desirable.

Consider the case of a hosting scenario where a firewall or proxy forwards the request to an internal web server. The trouble with the MCMS-generated <base> tag is that it contains the full URL, which may include the server’s name, especially when the "map channels to host headers" option has been turned off. When the internal server returns the page back to the proxy, the <base> tag of the returned page will contain the name of the internal server instead of the site’s domain name as entered in the browser.