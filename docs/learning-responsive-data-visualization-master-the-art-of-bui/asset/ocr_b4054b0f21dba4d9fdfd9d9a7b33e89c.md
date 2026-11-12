# # Including Bootstrap

Make sure you have the Bower package manager installed; this can be done by running the following command from the terminal:

```
npm install -g bower
```

More information about the Bower package manager can be found on its website at http://bower.io/.

Throughout this book, we will make use of only the CSS components of Bootstrap and thus we only need to include the bootstrap.css file to our project. There are multiple ways of doing so:

+   - Use the package manager called bower (recommended) – First, fetch the library by running the following command from the terminal—bower install bootstrap#3.3.5. Then, link the minified CSS file from the bower_components/ directory in the head section of the web page: <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">.

+   - Use a Content Delivery Network (CDN), such as MaxCDN (this requires an active Internet connection while developing). Simply include the minified CSS file from the CDN in the head section of the web page: <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">.

+   - Manually, download the 3.3.5 release from GitHub (https://github.com/twbs/bootstrap/releases) and unzip it into the libs/ directory. Then, link the minified CSS file from the libs/ directory in the head section of the web page: <link rel="stylesheet" href="libs/bootstrap/dist/css/bootstrap.min.css">.

I strongly recommend using the bower package manager to manage your frontend dependencies (or similar ones, such as npm or component). However, in the examples of this book, we will link Bootstrap directly from MaxCDN for simplicity reasons; therefore, all examples in the source of the book require an active Internet connection.

# # Finding additional resources

The ultimate resource for Bootstrap is the documentation that can be found at http://getbootstrap.com/getting-started/. Here, you will also find examples and templates using the Bootstrap framework.