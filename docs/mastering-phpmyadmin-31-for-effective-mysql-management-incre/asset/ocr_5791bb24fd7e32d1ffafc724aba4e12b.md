In extreme situations (thousands of databases), a user who installs his own copy of phpMyAdmin should put `false` in this parameter. This would block any database names fetching, and would require to populate the `$cfg['Servers'][$i]['only_db']` parameter with this user's database list.

Another parameter that can help is `$cfg['Servers'][$i]['CountTables']`, which has a default value of `true`. Setting it to `false` would instruct phpMyAdmin to avoid counting the tables when producing the list of databases and tables in the navigation panel (in Full mode).

# Right panel (main)

The right panel is the main working area, and all the possible views for it are explained in the following sections. Its appearance can be customized. The background color is defined in `$cfg['RightBgColor']`, and the default color is #F5F5F5. We can also select a background image by setting the URI of the image we want (for example, http://www.domain.com/images/clouds.jpg) in `$cfg['RightBgImage']`.

# Homepage

The homepage may contain a varying number of links depending on the login mode and the user's rights. A normal user may see it as:

![](bbox=[0.2, 0.53, 0.8, 0.825])