![](bbox=[0.18, 0.15, 0.81, 0.37])

3. This will create the Content, fonts, and Scripts folders and add the necessary Bootstrap files to each.

# Improving your site performance with bundling and minification

Bundling and minification is a feature in ASP.NET that allows you to increase the speed at which your site loads. This is accomplished by limiting the number of requests to CSS and JavaScript files your site needs to make by combining these types of files into one large file and removing all unnecessary characters, such as comments, white spaces, and new line characters from the files.

> Most modern browsers have a limit of six concurrent connections per hostname. This means that if you reference more than six CSS or JavaScript files on a page, the browser will only download six files at a time and queue the rest. Limiting the number of CSS and JavaScript files is always a good idea.