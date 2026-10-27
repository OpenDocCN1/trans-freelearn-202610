# The Bootstrap grid system

Many websites are reporting an increasing amount of mobile traffic and this trend is expected to increase over the coming years. The Bootstrap grid system is mobile-first, which means it is designed to target devices with smaller displays and then grow as the display size increases.

Fortunately, this is not something you need to be too concerned about as Bootstrap takes care of most of the heavy lifting.

# Bootstrap grid options

Bootstrap 3 introduced a number of predefined grid classes in order to specify the sizes of columns in your design. These class names are listed in the following table:

| Class name | Type of device | Resolution | Container width | Column width |
|---|---|---|---|---|
| col-xs-* | Phones | Less than 768 px | Auto | Auto |
| col-sm-* | Tablets | Larger than 768 px | 750 px | 60 px |
| col-md-* | Desktops | Larger than 992 px | 970 px | 1170 px |
| col-lg-* | High-resolution desktops | Larger than 1200 px | 78 px | 95 px |

The Bootstrap grid is divided into 12 columns. When laying out your web page, keep in mind that all columns combined should be a total of 12. To illustrate this, consider the following HTML code:

```
<div class="container">
    <div class="row">
        <div class="col-md-3" style="background-color:green;">
            <h3>green</h3>
        </div>
        <div class="col-md-6" style="background-color:red;">
            <h3>red</h3>
        </div>
        <div class="col-md-3" style="background-color:blue;">
            <h3>blue</h3>
        </div>
    </div>
</div>
```