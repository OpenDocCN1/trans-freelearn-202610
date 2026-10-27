JavaScript, which had all but lost its original appeal, made a surprise comeback and soon enough the web world was abuzz with the release of various leading JavaScript packages for web development and visualization, such as D3, Angular, Ember, and others ever since 2010-2011.

But these were mostly used by seasoned JavaScript developers, few of whom were also proficient in R. Developing a solution that would help bridge the gap between JavaScript web-based application development and R programming became a necessity for R developers to showcase and share their work with a broader audience.

# R Shiny platform for developers

R Shiny introduced a platform for R developers to create JavaScript-based web applications without having to get involved, or, for that, matter even be proficient in JavaScript.

In order to build our application, we will leverage R Shiny and create an interface to connect to the CMS Open Payments data we set up in the prior section.

If you are using your own R installation (locally), you'll need to install a few R packages. Note that if you are using a Linux workstation, you may need to install some additional Linux packages. For example, in Ubuntu Linux, you'll need to install the following. You may already have some of the packages, in which case you'll receive a message indicating that no further changes were needed for the respective package:

```
sudo apt-get install software-properties-common libssl-dev libcurl4-openssl-dev gdebi-core rlwrap
```

![](bbox=[0.07, 0.63, 0.15, 0.69])

If you are using the Packt Data Science VM, you can proceed directly to developing the application as these Linux packages have already been installed for you.

The Shiny application requires a few additional R packages to provide all its functionalities. Note that R packages are different from the Linux packages described previously. R packages, which number in the thousands, provide specialized functions for specific subject areas. For the web application, we will install a few R packages that will let us leverage some of the features in the web-based application.