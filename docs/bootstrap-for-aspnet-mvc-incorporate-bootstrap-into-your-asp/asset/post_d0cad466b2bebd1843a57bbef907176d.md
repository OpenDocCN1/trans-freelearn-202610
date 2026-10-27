
## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/support, selecting your book, clicking on the errata submission form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website, or added to any list of existing errata, under the Errata section of that title.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.

## Getting Started with ASP.NET MVC and Bootstrap

As developers, we can find it difficult to create great-looking user interfaces from scratch when using HTML and CSS. This is especially hard when developers have years of experience developing Windows Forms applications. Microsoft introduced Web Forms to abstract the complexities of building websites away for Windows Forms developers and to ease the switch from Windows Forms to the Web. This, in turn, made it very hard for Web Forms developers to switch to ASP.NET MVC and even harder for Windows Forms developers.

Twitter Bootstrap is a set of stylized components, plugins, and a layout grid that takes care of the heavy lifting. Microsoft included Bootstrap in all ASP.NET MVC project templates since 2013. In the sample project, we'll start by creating a new ASP.NET MVC project either by using the standard Visual Studio MVC project template or by starting with an empty MVC project and adding the necessary files as we need them.

In this chapter, we will cover the following topics:

- The files included in the Bootstrap distribution
- How to create an ASP.NET MVC site using the standard Visual Studio project template and Bootstrap
- How to create an empty ASP.NET MVC site and add the Bootstrap files manually
- How to create a Layout file that references the Bootstrap files
- Adding Bootstrap files using NuGet
- Improving site performance with bundling and minification

### Bootstrap JavaScript files (the js folder)

The js folder contains two files. All the Bootstrap plugins are contained in the bootstrap.js file. The bootstrap.min.js file is simply a minified version of the aforementioned file. Before including the file in your project, make sure that you have a reference to the jQuery library because all Bootstrap plugins require jQuery.

The default project template automatically adds the jQuery library to your project and creates a bundle for it. The jQuery bundle will be included in your pages if you have the following line of code inside your view:

```
@Scripts.Render("~/bundles/jquery")
```

### Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

### The Bootstrap folder structure

The unzipped folder structure for Bootstrap will look something like the following screenshot:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.min.css
│   ├── bootstrap-theme.css
│   └── bootstrap-theme.min.css
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    └── glyphicons-halflings-regular.woff
```

### Using Bootstrap with a site created with the standard Visual Studio project template

From Visual Studio 2013, when creating an ASP.NET project, you only have one project template to choose from, that is, the ASP.NET Web Application project template, as shown in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_22_0.png)

### Examining the default MVC project layout

The default project template adds all the necessary Bootstrap files we discussed earlier, although it does not use the same folder naming convention as the default Bootstrap distribution. The default project layout will look similar to the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_24_0.png)

### The Content folder

The Content folder contains both the bootstrap.css and bootstrap.min.css files as well as a style sheet called Site.css. This file is used to apply any additional styling on top of the default styles provided by Bootstrap, and it is also used to specify the styles to use for the jQuery validation plugin required by ASP.NET MVC for form validation. For example, the following CSS highlights any input element with a reddish color and draws a border around the element if the validation for that field failed:

```
.field-validation-error {
    color: #b94a48;
}

.field-validation-valid {
    display: none;
}

input.input-validation-error {
    border: 1px solid #b94a48;
}

input[type="checkbox"].input-validation-error {
    border: 0 none;
}

.validation-summary-errors {
    color: #b94a48;
}

.validation-summary-valid {
    display: none;
}
```

### The fonts folder

The fonts folder contains the Glyphicons font in all the necessary formats.

### The Scripts folder

The Scripts folder contains a number of scripts. Most notably for this book, it contains the bootstrap.js and bootstrap.min.js JavaScript files. The default ASP.NET MVC project template also adds both minified and normal files for the following JavaScript libraries and plugins:

- jQuery
- jQuery validation plugin
- jQuery and jQuery validation support library for unobtrusive validation
- Modernizr
- Respond.js

Visual Studio enables IntelliSense for jQuery, Bootstrap, and Modernizr, as well as Respond.js, by adding the _references.js file to the Scripts folder. This is a very useful feature when working with JavaScript and well worth using when working with the Bootstrap components.

Most of these libraries and files are beyond the scope of this book, but we will touch on some of them as we progress.

### Creating an empty ASP.NET MVC site and adding Bootstrap manually

The default project layout is a good start for any ASP.NET MVC project, but for the sample project we'll be building throughout this book, we'll create an empty ASP.NET MVC site and add the necessary files manually. This is done by performing the following steps:

1. Start by creating a new ASP.NET web application project in Visual Studio and name the project Northwind.Web.
2. This time, select the Empty template in the New ASP.NET Project dialog and make sure the MVC checkbox is selected, as shown in the following screenshot:

   ![](img/371b9a67300a03b7b335eb8a6fe55d94_27_0.png)

3. An empty project layout will be created for you and you'll notice that we do not have the Content, Fonts, or Scripts folder—we'll add them ourselves!

### Adding the Bootstrap style sheets

To add the Bootstrap style sheet files to your project, complete the following steps:

1. Create a new folder by right-clicking on the new project's name inside Visual Studio's Solution Explorer and navigating to Add | New Folder, name the new folder css.
2. Next, right-click on the newly created css folder and navigate to Add | Existing Item... from the context menu.
3. Browse to the folder in which you've extracted the Bootstrap distribution files and select the bootstrap.css file that you can locate in the css folder.
