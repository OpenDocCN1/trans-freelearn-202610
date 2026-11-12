# Including D3.js

There are multiple ways to include D3.js to your project:

+   Using the package manager called bower (recommended), fetch the library by running the bower install d3#3.5.6 command from the terminal. Then, link the minified CSS file from the bower_components/ directory in the head section of the web page: <link rel="stylesheet" href="bower_components/d3/d3.min.css">.

+   Using a CDN, such as cdnjs (this requires an active Internet connection while developing), include the minified CSS file from the CDN in the head section of the web page: <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.6/d3.min.js">.

+   Manually download the 3.5.6 release from GitHub (https://github.com/mbostock/d3/releases) and unzip it into the libs/ directory. Then, link the minified CSS file from the libs/ directory in the head section of the web page <link rel="stylesheet" href="libs/d3/d3.min.css">.

I strongly recommend using the package manager called bower to manage your frontend dependencies. However, in the examples of the book, we will link D3.js directly from cdnjs for simplicity reasons; therefore, all the examples in the source of the book require an active Internet connection.

# Finding additional resources

The ultimate resource for D3.js is the WIKI page of the GitHub project, which can be found at https://github.com/mbostock/d3/wiki. Here, you will also find examples and a detailed description of the API.

Another great resource for examples is Mike Bostock's page at http://bl.ocks.org/mbostock and his blog at http://bost.ocks.org/mike/.

For more detailed information and examples, I recommend reading the Mastering D3.js book by Pablo Navarro Castillo.