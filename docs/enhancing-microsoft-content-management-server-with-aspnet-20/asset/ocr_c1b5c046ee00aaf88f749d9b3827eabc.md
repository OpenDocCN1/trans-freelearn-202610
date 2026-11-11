# 5

# Applying Themes

Most websites are designed with a common look and feel in mind. After all, if you went to a site that had pages that are distinctively different from one another, you might start to wonder (probably after the second page) if you were still surfing the same website.

In this chapter, we will see how a common look and feel can be applied efficiently to an MCMS site by using themes. We will create skins and cascading style sheets and demonstrate how they work together to define the appearance of a site. Finally, we will discuss an essential customization required for themes to work correctly in an MCMS site.

# Themes versus Cascading Style Sheets

The challenge of maintaining a consistent look and feel across a website is not a new one. Not too long ago, developers used cascading style sheets (CSS) to define how the site should look. The use of CSS was a step towards separating design elements from HTML. Graphic designers could work independently on the CSS file, providing information such as color schemes, font types, spacing, margins, and other visual effects. Should the need arise to change any of these attributes, all that the designer needed to do was to amend the CSS file, and voilà, the look and feel of the site would be instantly refreshed!

Nevertheless, despite the advantages of using CSS, modifications to a site’s design still required a fair bit of work. Just think about the last time a complex control such as a new calendar control was introduced to a site that you worked on. In order to give a desired look and feel to the control, several styles had to be applied: One for the title, another for the selected date, and a couple more for the navigation arrows. Also, the same task had to be repeated for each calendar control on the site. You could wrap the "styled" calendar in a custom control and reuse it across the site, but that would mean writing code with design elements in mind, which makes for a